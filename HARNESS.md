# HARNESS — 오케스트레이션 스펙

체이닝 오케스트레이터가 스킬을 엮는 방식, 라우팅 규칙, 체크포인트, 종료조건,
모델 배정을 정의한다. Timely 코어 티어(스킬 체이닝 + 상태 영속 + 멀티모델)만 사용한다.

## 1. 실행 흐름 (체이닝 순서)

```
[입력: 아이디어 텍스트]
  → 00_intake
  → 01_requirements        → GATE+CRITIC(requirements)
  → 02_stack (advocate ∥ challenger → judge)  → GATE+CRITIC(stack)
  → 03_architect (lead → api ∥ db)            → GATE+CRITIC(architecture)
  → sec_stride → sec_pii → sec_redteam        → GATE(security)   [위협→요구 역주입]
  → 04_test_strategist     → GATE+CRITIC(test)
  → 05_project_structurer
  → 06_consistency_auditor → GATE(consistency)
[출력: design_state 전체 = 심층 설계 패키지]
```

`∥` = 격리 병렬(서로의 컨텍스트를 모른 채 독립 실행 후 병합).

## 2. Generate → Critique → Revise 루프

모든 생성 스킬 뒤에 아래 루프를 적용한다.

1. **생성기(모델 A)** 가 아티팩트 산출 → `design_state`에 기록
2. **gate_verifier(모델 B)** 가 정합성 규칙을 근거와 함께 판정 — 위반 시 즉시 재작업
3. **critic(모델 B)** 가 해당 rubric으로 채점 → 결함 목록
4. 임계 통과(권장 8/10) 시 다음 단계, 미달 시 결함만 수정 후 재채점
5. 최대 K회(권장 3회) 초과 시 → `open_risks`에 기록하고 통과 (무한루프 차단)

## 3. 라우팅 규칙 (조건부 심화)

매번 토론·전문가를 소환하지 않는다. 아래 조건에서만 심화 경로를 발동한다.

| 조건 | 경로 |
|---|---|
| 입력 복잡도 낮음(단순 CRUD 등) | 파이프라인만, 토론 생략 |
| 스택 트레이드오프 큼 / 후보 대등 | 02_stack 토론 발동 |
| 아키텍처 컴포넌트 ≥ N개 | 03_architect 전문가 분기(api ∥ db) |
| 생성기 확신도 낮음(self-report) | critic 회차 +1 |
| judge 판정 불일치 | self-consistency 다수결(3회) |

## 4. 모델 배정 (교차 원칙)

**핵심 규칙: 생성자와 판정자는 반드시 다른 모델.** 자기채점을 구조적으로 금지한다.

| 역할 | 권장 모델 | 이유 |
|---|---|---|
| 생성기(각 phase) | Claude | 긴 구조화 생성에 강함 |
| gate_verifier / critic | Gemini 또는 Solar | 생성과 교차 → 진짜 외부 검토 |
| stack judge | Solar | 교차 + Upstage 네이티브 |
| red-team | 생성과 다른 모델 | 공격자 관점 격리 |
| 앙상블 판정(스택 등) | Claude+Gemini+Solar 투표 | 단일 모델 편향 완화 |

## 5. 체크포인트 & 재개

- 각 phase 완료 시 `design_state.checkpoint.last_phase` 갱신 후 상태 영속 저장
- 실패/중단 시 `resumable=true`면 `last_phase` 다음부터 재개(처음부터 X)
- `critique_log`·`trace`도 함께 영속 → 세션을 넘겨도 "왜 이 설계인지" 유지

## 6. 종료조건 & 예산 (프롬프트 로직)

- 노드별 최대 반복 K=3
- 비평 점수가 3회 연속 개선 없으면(진동) 루프 감지 → 강제 종료 + `open_risks` 기록
- (로드맵) 진짜 토큰 미터링·DAG 병렬은 플랫폼 텔레메트리/실행제어 확인 후 승격

## 7. 산출물

`design_state` 전체가 최종 산출물이며, `06_consistency_auditor`가 생성한
**추적 매트릭스(요구→컴포넌트→API→DB→테스트)** 와 `critique_log`가 "깊이의 증거물"이다.
