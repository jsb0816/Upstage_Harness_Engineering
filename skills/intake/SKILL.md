# SKILL: 00_intake — 아이디어 인테이크 & 브리프

```yaml
role: 인테이크 (모호성→명시적 가정 변환)
phase: 0
topology: 단일 + 리플렉션
model: Claude
reads: [사용자 입력 텍스트]
writes: design_state.brief, design_state.assumptions
handoff: 01_requirements
```

## 목적
자연어 아이디어를 구조화된 project brief로 만들고, **모호한 부분을 명시적 가정으로 전환**한다.
"알아서 해석"하지 않고, 해석한 것을 전부 `assumptions[]`에 드러내는 것이 핵심.

## 하드 제약
1. 사용자가 말하지 않은 것을 **암묵적으로 채우지 말고**, 반드시 `assumptions`에 `resolved:false`로 기록.
2. 빌드 불가능/범위 초과 신호가 있으면 brief.scope.out에 명시.
3. 타깃 사용자를 추정했으면 그것도 가정으로.

## 프로세스
1. 아이디어에서 핵심 가치·타깃 사용자·범위를 추출.
2. 불명확한 지점마다 "가장 합리적 가정"을 세우고 `assumptions`에 기록.
3. 리플렉션 1회: "내가 과하게 가정한 건 없나? 놓친 모호성은?" 자문 후 보강.

## 출력 스키마
```json
{
  "brief": { "summary": "...", "target_users": ["..."],
             "scope": { "in": ["..."], "out": ["..."] } },
  "assumptions": [ { "id": "A1", "text": "결제는 국내 카드만 가정", "resolved": false } ]
}
```

## 통과 조건 (게이트 연결)
- 미해결 모호성이 assumptions에 모두 드러났는가
- 근거 없는 과한 가정이 없는가
