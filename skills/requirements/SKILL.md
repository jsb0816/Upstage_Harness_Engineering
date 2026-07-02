# SKILL: 01_requirements — 요구사항 (앙상블)

```yaml
role: 요구사항 도출 (다관점 앙상블)
phase: 1
topology: 파이프 + 앙상블 (여러 페르소나 격리 실행 후 병합)
model: Claude (페르소나별 동일 모델, 격리 실행)
reads: design_state.brief, design_state.assumptions
writes: design_state.requirements
handoff: gate_verifier(requirements) → critic
rubric: rubrics/requirements.md
```

## 목적
brief를 기능 요구(FR)와 **비기능 요구(NFR)** 로 분해하고, 각 요구에 ID·출처·우선순위·수용기준을 부여한다.
단일 관점의 누락을 막기 위해 **여러 페르소나가 격리 상태로 각각 도출한 뒤 병합**한다.

## 앙상블 페르소나 (격리 실행)
- **End-user 관점** — 사용자가 실제로 하려는 것
- **Ops/보안 관점** — 운영·보안·규정에서 필요한 것 (NFR 집중)
- **Edge-case 관점** — 실패·경계·악용 시나리오

각 페르소나는 서로의 출력을 보지 않고 독립 도출 → 병합 스킬이 중복 제거·통합.

## 하드 제약
1. 모든 요구에 **고유 ID + source**(brief/assumption/threat) 부여.
2. FR/NFR을 명시적으로 분리. NFR은 성능·보안·확장성·가용성·규정 축을 최소 검토.
3. 각 요구는 **검증 가능**해야 함 → `acceptance`에 Given-When-Then.
4. 우선순위는 MoSCoW(M/S/C/W).

## 출력 스키마
`design_state.requirements[]` (design_state.md 참조).

## 통과 조건
- 완전성(brief의 모든 가치가 요구로), 검증가능성, 원자성, 요구 간 충돌 없음.
