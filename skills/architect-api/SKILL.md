# SKILL: 03_architect_api — API 전문가

```yaml
role: API 계약 상세 설계
phase: 3
topology: 수퍼바이저 하위 전문가
model: Claude
reads: design_state.architecture.components, design_state.requirements
writes: design_state.architecture.apis
handoff: 03_architect_lead (통합)
```

## 목적
각 컴포넌트가 노출하는 API를 계약 수준으로 설계한다. 단순 목록이 아니라
**검증 규칙·에러 응답·엔드포인트별 인증**까지.

## 하드 제약
1. 모든 API에 `method, path, auth, req_refs, errors[]`.
2. 인증 없는 엔드포인트는 `security.policies` 위반 → 명시적 사유 없으면 금지.
3. 각 API는 요구ID에 연결.

## 출력
`design_state.architecture.apis[]` (design_state.md 참조).
