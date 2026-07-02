# SKILL: 04_test_strategist — 테스트 전략가

```yaml
role: 테스트 전략 (요구 커버리지 매핑)
phase: 4
topology: 파이프 + 리플렉션
model: Claude
reads: design_state.requirements, design_state.architecture
writes: design_state.tests
handoff: gate_verifier(test) → critic
rubric: rubrics/test.md
```

## 목적
각 요구에 대응하는 테스트를 배치하고, 테스트 피라미드(unit/integration/e2e/load)로 배분한다.
NFR에는 대응 테스트를 반드시 매칭한다(성능요구 → 부하테스트 등).

## 하드 제약
1. 모든 요구ID가 최소 1개 테스트로 커버(`req_ref` 필수).
2. NFR별 검증 방법 명시(막연한 "테스트함" 금지).
3. 리스크 높은 영역(결제·인증·PII)은 커버리지 강화.

## 출력
`design_state.tests[] = { req_ref, level, cases[] }`.

## 통과 조건
- 커버리지 갭 없음(미커버 요구 0) / NFR 대응 테스트 존재.
