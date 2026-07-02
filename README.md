# DeepSpec Harness

> 신규 아이디어(CREATE)를 입력받아, 스스로 채점·비판·논쟁하며 심화시킨
> **심층 설계 패키지**를 산출하는 멀티에이전트 하네스. Timely 배포용.

## 이 하네스가 하는 일

사용자가 그냥 Claude에게 "설계해줘" 하면 되는데 **왜 이 하네스를 거쳐야 하는가?**
답은 "더 똑똑한 모델"이 아니라 **모델이 빠뜨릴 수 없게 만드는 구조**다. 이 하네스는
단일 모델 호출로는 구조적으로 불가능한 네 가지를 강제한다.

1. **격리된 다관점 탐색** — 서로의 추론을 못 보는 다모델 에이전트가 병렬·적대적으로 설계
2. **교차모델 검증 게이트** — 생성과 *다른* 모델이 명시적 규칙을 근거와 함께 집행
3. **강제된 보안 방법론** — STRIDE·PII·red-team을 빠짐없이 완주(언급이 아니라 완주)
4. **영속·검증 가능한 상태** — 저장·재개 가능한 단일 진실 소스(`design_state`) + 감사로그

## 폴더 구조

```
deepspec-harness/
├── README.md              # 이 문서
├── HARNESS.md             # 오케스트레이션 스펙 (체이닝 순서·라우팅·체크포인트·모델배정)
├── design_state.md        # 단일 진실 소스 스키마 계약
├── skills/                # 에이전트별 SKILL 정의
│   ├── 00_intake.md
│   ├── 01_requirements.md
│   ├── 02_stack_advocate.md
│   ├── 02_stack_challenger.md
│   ├── 02_stack_judge.md
│   ├── 03_architect_lead.md
│   ├── 03_architect_api.md
│   ├── 03_architect_db.md
│   ├── 04_test_strategist.md
│   ├── 05_project_structurer.md
│   ├── 06_consistency_auditor.md
│   ├── sec_stride.md
│   ├── sec_pii.md
│   ├── sec_redteam.md
│   ├── critic.md          # 범용 비평가 (루브릭 주입형)
│   └── gate_verifier.md   # 교차모델 검증 게이트 (범용)
├── rubrics/               # 단계별 채점 루브릭 (체크리스트)
│   ├── requirements.md
│   ├── stack.md
│   ├── architecture.md
│   ├── test.md
│   ├── security.md
│   └── consistency.md
└── reference/             # 정적 레퍼런스 번들 (실시간 검색 대체)
    ├── stack_reference.md
    ├── security_checklist.md
    └── best_practices.md
```

## Timely 실현 티어

| 티어 | 내용 | 상태 |
|---|---|---|
| **코어** | 체이닝·격리 다관점·교차모델 게이트·영속 상태·체크포인트·critique_log·방법론 강제 | Timely에서 코드 실행 없이 실현 ✅ |
| **툴 의존** | 코드 파싱 게이트·SMT 형식검증·실시간 RAG | 코드실행/툴호출 지원 시 승격 ❓ |
| **로드맵** | DAG 병렬·토큰 텔레메트리·A/B·REVERSE 모드 | 후속 |

## 실행 요약

`HARNESS.md`의 체이닝 순서대로 skills를 호출한다. 각 스킬은 `design_state`의
필요한 슬라이스만 읽고, 결과를 다시 `design_state`에 기록한다. 모든 생성 뒤에는
**교차모델 게이트 + 비평가**가 붙어 통과할 때까지(최대 K회) 반복한다.
