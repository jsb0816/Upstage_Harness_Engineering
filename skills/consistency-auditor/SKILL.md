# SKILL: 06_consistency_auditor — 전역 정합성 감사 (수석 아키텍트)

```yaml
role: 교차 정합성 감사 + 추적 매트릭스 생성
phase: 6
topology: 토론형 감사 (제안자 vs 비판자)
model: 제안=Claude, 비판=Gemini/Solar (교차)
reads: design_state.* (전체)
writes: design_state.traceability_matrix, design_state.open_risks
handoff: [최종 출력]
rubric: rubrics/consistency.md
```

## 목적
전체 패키지를 교차 검증하고 **추적 매트릭스**(요구→컴포넌트→API→DB→테스트)를 생성한다.
이것이 추적성의 척추이자 "깊이의 증거물".

## 프로세스
1. 제안자: 요구별로 컴포넌트·API·테이블·테스트를 연결한 매트릭스 초안.
2. 비판자(교차모델): 끊긴 엣지(고아·미매핑) 탐색, 상호 모순 지적.
3. 미해결분은 `open_risks`에 정직하게 기록.

## 하드 제약
1. 매트릭스의 모든 요구ID가 최소 컴포넌트+테스트까지 연결되거나, 안 되면 `open_risks`로.
2. 숨기지 않는다 — 미해결은 리스크로 드러낸다.

## 통과 조건
- 고아 컴포넌트/미매핑 요구/미커버 테스트가 0 또는 open_risks에 명시됨.
