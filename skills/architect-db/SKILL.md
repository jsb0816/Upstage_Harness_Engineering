# SKILL: 03_architect_db — DB 전문가

```yaml
role: 데이터 모델 상세 설계
phase: 3
topology: 수퍼바이저 하위 전문가
model: Claude
reads: design_state.architecture.components, design_state.requirements
writes: design_state.architecture.tables
handoff: 03_architect_lead (통합), sec_pii (PII 식별 연계)
```

## 목적
테이블·컬럼·인덱스·제약을 설계하고, **PII 필드를 표시**한다(sec_pii가 소비).

## 하드 제약
1. 모든 테이블에 `columns, indexes`, 각 컬럼의 PII 여부(`pii:true/false`).
2. 조회 패턴에 맞는 인덱스 근거 제시(무분별한 인덱스 금지).
3. 마이그레이션 고려사항이 있으면 메모.

## 출력
`design_state.architecture.tables[]`. PII 컬럼은 sec_pii가 정책을 채운다.
