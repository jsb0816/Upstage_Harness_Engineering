# SKILL: 02_stack_judge — 스택 심판

```yaml
role: 토론 판정 + ADR 고정
phase: 2
topology: 토론 judge
model: Solar (advocate/challenger와 교차, Upstage 네이티브)
reads: debate_buffer.advocate, debate_buffer.challenger, design_state.requirements
writes: design_state.stack_decisions
handoff: gate_verifier(stack) → critic
rubric: rubrics/stack.md
```

## 목적
옹호/반론을 종합해 최종 스택을 결정하고, **ADR(결정기록)** 형태로 고정한다.
"왜 이걸 골랐고, 무엇을 포기했는지"가 영구히 남는다.

## 하드 제약
1. advocate/challenger **양쪽을 모두 인용**해 판정 근거를 남긴다.
2. 채택안뿐 아니라 **기각된 대안과 기각 사유**를 반드시 기록.
3. 판정 불일치·확신 낮으면 self-consistency 3회 다수결.

## 출력 스키마
`design_state.stack_decisions[] = { area, choice, alternatives[], rationale, req_refs[] }`
rationale에는 반론을 어떻게 극복/수용했는지 포함.

## 통과 조건 (rubrics/stack.md)
- 모든 선택이 요구에 정당화되나 / 기각 사유가 있나 / 스택에서 누락된 요구 없나
