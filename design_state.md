# design_state — 단일 진실 소스 스키마

모든 스킬은 `design_state`의 필요한 슬라이스만 읽고, 결과를 다시 여기에 쓴다.
노드 간 직접 결합은 없다. 이 상태는 **영속**되며 체크포인트/재개의 기반이다.

## 스키마

```json
{
  "brief":        { "summary": "", "target_users": [], "scope": {"in": [], "out": []} },
  "assumptions":  [ { "id": "A1", "text": "", "resolved": false } ],

  "requirements": [ { "id": "R1", "type": "FR|NFR", "text": "",
                      "source": "brief|assumption|threat", "priority": "M|S|C|W",
                      "acceptance": "Given-When-Then" } ],

  "stack_decisions": [ { "area": "backend|frontend|db|infra",
                         "choice": "", "alternatives": ["", ""],
                         "rationale": "", "req_refs": ["R1"] } ],

  "architecture": {
    "style": "monolith|msa|event-driven",
    "components": [ { "id": "C1", "name": "", "responsibility": "", "req_refs": ["R1"] } ],
    "apis":       [ { "id": "API1", "method": "", "path": "", "auth": "",
                      "req_refs": ["R1"], "errors": [] } ],
    "tables":     [ { "id": "T1", "name": "", "columns": [], "indexes": [], "pii": false } ],
    "sequences":  [ { "flow": "", "steps": [] } ],
    "dataflows":  [ { "from": "", "to": "", "data": "" } ]
  },

  "security": {
    "stride_matrix": [ { "component": "C1", "S": "", "T": "", "R": "",
                         "I": "", "D": "", "E": "" } ],
    "pii_fields":    [ { "table": "T1", "column": "", "policy": "" } ],
    "threats":      [ { "id": "TH1", "desc": "", "severity": "H|M|L", "req_ref": "R9" } ],
    "policies":     [ "인증 없는 엔드포인트 금지", "PII 필수 암호화" ]
  },

  "tests":   [ { "req_ref": "R1", "level": "unit|integration|e2e|load", "cases": [] } ],
  "structure": { "tree": "" },

  "traceability_matrix": [ { "req": "R1", "component": "C1",
                            "api": "API1", "table": "T1", "test": "TC1" } ],

  "critique_log": [ { "phase": "", "judge_model": "", "checklist": [],
                      "defects": [], "revision_n": 0 } ],
  "open_risks":   [ { "phase": "", "issue": "", "reason": "K회 초과|수렴실패" } ],
  "trace":        [ { "node": "", "model": "", "revision_n": 0, "gate_pass": true } ],
  "checkpoint":   { "last_phase": "", "resumable": true }
}
```

## 필드 설계 의도

- **source / req_refs** — 모든 요구·결정이 근거로 역추적된다(추적성).
- **critique_log.judge_model** — 교차모델 판정을 명시적으로 기록(자기채점 아님을 증명).
- **open_risks** — 루프 한도 초과분을 숨기지 않고 드러낸다(정직한 통제).
- **trace / checkpoint** — in-run 관측성 + 세션 재개.

## 쓰기 규칙

- 스킬은 자기 담당 필드만 쓴다(예: `02_stack`은 `stack_decisions`만).
- `security.threats`는 요구사항으로 역주입되므로 `requirements`에 `source:"threat"`로 추가된다.
- 게이트/비평 결과는 항상 `critique_log`에 append(덮어쓰기 금지).
