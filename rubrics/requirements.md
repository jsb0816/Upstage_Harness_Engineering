# RUBRIC: requirements

게이트(하드룰)와 비평(품질) 항목. 게이트는 pass/fail, 비평은 0~2점.

## 하드룰 (게이트 — 하나라도 fail이면 재작업)
- [ ] 모든 요구에 고유 ID가 있다
- [ ] 모든 요구에 source(brief/assumption/threat)가 있다
- [ ] FR/NFR이 분리되어 있다
- [ ] 요구 간 직접 충돌이 없다

## 품질 (비평 — 각 0~2점, 임계 80%)
- 완전성: brief의 모든 가치가 요구로 표현됨
- 검증가능성: 각 요구에 Given-When-Then 수용기준
- 원자성: 한 요구가 한 가지만 말함
- NFR 충실도: 성능·보안·확장성·가용성·규정 축 검토
- 우선순위: MoSCoW 부여 및 합리성
