# SKILL: sec_pii — PII & 데이터 거버넌스

```yaml
role: 개인정보 식별 + 거버넌스 요구 생성
phase: 횡단
topology: 전문가 밴드
model: Gemini 또는 Solar
reads: design_state.architecture.tables
writes: design_state.security.pii_fields ; requirements 역주입
handoff: sec_redteam
```

## 목적
데이터 모델에서 PII 필드를 식별하고 보존정책·암호화·접근통제 요구를 자동 생성한다.
공공·교육(Timely 시장) 특성상 핵심.

## 하드 제약
1. `pii:true`인 모든 컬럼에 정책(암호화·보존기간·접근권한) 부여.
2. 각 정책을 requirements로 역주입(`source:"threat"` 또는 별도 거버넌스 태그).
3. 과수집(minimization 위반) 의심 필드는 플래그.

## 출력
`security.pii_fields[] = { table, column, policy }` + requirements 역주입.
