# SKILL: sec_redteam — 적대적 자기공격

```yaml
role: red-team (완성 설계 공격)
phase: 횡단 (보안 마지막)
topology: 격리 전담 에이전트
model: 생성과 다른 모델 (교차 필수)
reads: design_state.* (전체 완성 설계)
writes: design_state.security.threats (추가분) ; requirements 역주입
handoff: gate_verifier(security)
rubric: reference/security_checklist.md
```

## 목적
완성된 설계에 대해 **"공격자라면 어디를 뚫겠나"** 를 별도 컨텍스트에서 수행한다.
OWASP Top-10 실패·접근제어(RLS) 사고를 정면으로 겨냥.

## 하드 제약
1. 설계자 관점이 아니라 **공격자 관점**으로만 사고(격리).
2. `reference/security_checklist.md`의 OWASP 항목을 하나씩 대입.
3. 발견 취약점은 severity와 함께 threats에 추가 + 완화 요구 역주입.

## 프로세스
1. 인증/인가 경로 공격 시나리오.
2. 데이터 노출·주입·권한상승 시나리오.
3. 각 성공 시나리오 → threat + 완화 요구.

## 통과 조건
- OWASP 체크리스트 완주 / 발견 취약점에 완화 요구 매핑.
