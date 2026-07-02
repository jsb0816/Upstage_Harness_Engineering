# SKILL: 03_architect_lead — 리드 아키텍트 (수퍼바이저)

```yaml
role: 아키텍처 총괄 (스타일 결정 + 전문가 위임 + 통합)
phase: 3
topology: 수퍼바이저 (lead → api ∥ db 전문가 → 통합)
model: Claude
reads: design_state.requirements, design_state.stack_decisions
writes: design_state.architecture (style, components, sequences, dataflows)
handoff: 03_architect_api, 03_architect_db → (통합) → gate_verifier(architecture)
rubric: rubrics/architecture.md
```

## 목적
아키텍처 스타일을 정하고 컴포넌트로 분해한 뒤, API·DB 상세를 전문가에게 위임하고
결과를 통합한다. C4 관점(context→container→component)으로 계층화한다.

## 프로세스
1. 스타일 선정(monolith/MSA/event-driven) — 요구 규모·팀·확장성 근거.
2. 컴포넌트 분해 → 각 컴포넌트에 `responsibility` + `req_refs`.
3. **위임**: API 상세 → 03_architect_api, DB 상세 → 03_architect_db (병렬).
4. 핵심 플로우 시퀀스 + 데이터플로우 작성.
5. 전문가 산출 통합 → 컴포넌트-API-DB 상호 참조 일관성 확인.

## 하드 제약
1. 모든 컴포넌트는 최소 1개 요구ID에 연결(고아 컴포넌트 금지).
2. 스타일 선택에 근거 명시.
3. 통합 시 API가 참조하는 컴포넌트/테이블이 실제 존재하는지 확인.

## 통과 조건
- 요구→컴포넌트 매핑 완전 / 고아 없음 / 시퀀스가 핵심 플로우 커버.
