# SKILL: sec_stride — STRIDE 위협 모델링

```yaml
role: 위협 모델링 (강제 완주)
phase: 횡단 (아키텍처 후)
topology: 전문가 밴드
model: Gemini 또는 Solar (생성과 교차)
reads: design_state.architecture (components, dataflows)
writes: design_state.security.stride_matrix, design_state.security.threats
handoff: sec_pii → sec_redteam ; threats는 requirements로 역주입
rubric: rubrics/security.md
```

## 목적
**모든 컴포넌트**를 STRIDE 6개 렌즈로 빠짐없이 검토한다. 일반 모델은 몇 개 언급하고
말지만, 이 스킬은 **매트릭스를 다 채우기 전엔 통과할 수 없다**.

## STRIDE 렌즈
- **S**poofing (위장) / **T**ampering (변조) / **R**epudiation (부인)
- **I**nformation Disclosure (정보노출) / **D**oS (서비스거부) / **E**levation (권한상승)

## 하드 제약
1. `stride_matrix`는 컴포넌트 × 6렌즈를 **전부** 채운다(해당없음이면 "N/A + 사유").
2. 식별된 위협은 `threats[]`에 severity와 함께 기록.
3. **위협 → 요구 역주입**: 각 위협에 대응하는 완화 요구를 `requirements`에 `source:"threat"`로 추가.

## 출력
`security.stride_matrix[]`, `security.threats[]`, 그리고 requirements 역주입.

## 통과 조건 (게이트)
- 빈 셀 없음 / 모든 H·M 위협에 대응 요구 존재.
