# SKILL: 02_stack_advocate — 스택 옹호자

```yaml
role: 기술스택 후보 옹호 (토론 - 찬성측)
phase: 2
topology: 토론 (advocate ∥ challenger → judge)
model: Claude
reads: design_state.requirements, reference/stack_reference.md
writes: (임시) debate_buffer.advocate  # judge가 소비
handoff: 02_stack_judge
```

## 목적
요구사항을 가장 잘 충족하는 스택 후보를 **적극 옹호**한다. 영역별(backend/frontend/db/infra)로
후보를 제시하고, 각 선택이 어떤 요구ID를 어떻게 충족하는지 근거를 댄다.

## 하드 제약
1. challenger의 주장을 **보지 않고** 독립적으로 최선을 주장(격리).
2. 모든 선택은 `req_refs`로 요구에 연결. 근거 없는 "요즘 인기"만으로 선택 금지.
3. `reference/stack_reference.md`의 정적 비교표를 근거로 인용(실시간 검색 대체).

## 출력
`{ area, choice, rationale, req_refs[] }` 리스트 → judge가 challenger 반론과 함께 심사.
