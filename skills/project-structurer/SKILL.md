# SKILL: 05_project_structurer — 프로젝트 구조 설계

```yaml
role: 디렉토리/모듈 레이아웃
phase: 5
topology: 단일
model: Claude
reads: design_state.architecture, design_state.stack_decisions
writes: design_state.structure
handoff: 06_consistency_auditor
```

## 목적
스택·아키텍처에 맞는 디렉토리 구조를 제안한다. 모든 컴포넌트/모듈이 들어갈 자리가 있어야 한다.

## 하드 제약
1. 식별된 모든 컴포넌트가 구조 내 위치를 가짐(수용 여부 확인).
2. 선택 스택의 관례(convention)를 따름.

## 출력
`design_state.structure.tree` (트리 텍스트).
