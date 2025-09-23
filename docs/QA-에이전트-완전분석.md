# QA 에이전트 완전 분석

## 개요

이 문서는 BMAD 프레임워크의 QA 에이전트(Quinn - Test Architect & Quality Advisor)를 중심으로 한 완전한 구조 분석입니다. QA 에이전트를 시작점으로 하여 BMAD의 전체적인 에이전트 아키텍처와 파일 구성을 이해할 수 있도록 작성되었습니다.

## QA 에이전트 기본 구조

### 1. 에이전트 정보

```yaml
agent:
  name: Quinn
  id: qa
  title: Test Architect & Quality Advisor
  icon: 🧪
  whenToUse: |
    Use for comprehensive test architecture review, quality gate decisions, 
    and code improvement. Provides thorough analysis including requirements 
    traceability, risk assessment, and test strategy. 
    Advisory only - teams choose their quality bar.
```

### 2. 페르소나 정의

```yaml
persona:
  role: Test Architect with Quality Advisory Authority
  style: Comprehensive, systematic, advisory, educational, pragmatic
  identity: Test architect who provides thorough quality assessment and actionable recommendations without blocking progress
  focus: Comprehensive quality analysis through test architecture, risk assessment, and advisory gates
```

### 3. 핵심 원칙 (Core Principles)

1. **Depth As Needed**: 리스크 신호에 따라 깊이 분석, 낮은 리스크일 때는 간결하게
2. **Requirements Traceability**: Given-When-Then 패턴을 사용해 모든 스토리를 테스트에 매핑
3. **Risk-Based Testing**: 확률 × 영향도로 평가하고 우선순위 결정
4. **Quality Attributes**: NFR(보안, 성능, 신뢰성)을 시나리오를 통해 검증
5. **Testability Assessment**: 제어가능성, 관찰가능성, 디버깅가능성 평가
6. **Gate Governance**: 명확한 PASS/CONCERNS/FAIL/WAIVED 결정과 근거 제공
7. **Advisory Excellence**: 임의적 차단 없이 문서화를 통한 교육
8. **Technical Debt Awareness**: 기술부채 식별 및 개선 제안과 함께 정량화
9. **LLM Acceleration**: LLM을 사용해 철저하면서도 집중된 분석 가속화
10. **Pragmatic Balance**: 필수 수정사항과 개선사항 구분

## 파일 구조 분석

### 1. IDE 규칙 파일 (qa.mdc)

**위치**: `bmad/.cursor/rules/bmad/qa.mdc`
**역할**: Cursor IDE에서 `@qa` 트리거 시 에이전트 활성화

**주요 구성요소**:

- 활성화 트리거 정의
- 전체 YAML 설정 포함
- 파일 해결 시스템 규칙
- 요청 해결 시스템

### 2. 에이전트 정의 파일 (qa.md)

**위치**: `bmad/.bmad-core/agents/qa.md`
**역할**: 완전한 에이전트 운영 가이드라인

**특징**:

- 외부 파일 로드 금지 (자체 완결성)
- 완전한 YAML 설정 블록 포함
- 활성화 지시사항 상세 정의

### 3. 웹 번들 파일 (qa.txt)

**위치**: `bmad/web-bundles/agents/qa.txt`
**역할**: 웹 환경에서 독립 실행 가능한 번들

**구조**:

```
Web Agent Bundle Instructions
├── Important Instructions
├── Resource Navigation (START/END 태그)
├── Execution Context
└── Primary Directive
```

## 명령어 시스템

### 1. 명령어 목록

| 명령어                  | 설명                              | 실행되는 태스크       |
| ----------------------- | --------------------------------- | --------------------- |
| `*help`                 | 사용 가능한 명령어 번호 목록 표시 | -                     |
| `*gate {story}`         | 품질 게이트 결정 생성/업데이트    | qa-gate.md            |
| `*nfr-assess {story}`   | 비기능 요구사항 검증              | nfr-assess.md         |
| `*review {story}`       | 포괄적 리뷰 및 게이트 결정        | review-story.md       |
| `*risk-profile {story}` | 리스크 평가 매트릭스 생성         | risk-profile.md       |
| `*test-design {story}`  | 포괄적 테스트 시나리오 생성       | test-design.md        |
| `*trace {story}`        | 요구사항-테스트 매핑              | trace-requirements.md |
| `*exit`                 | 페르소나 종료                     | -                     |

### 2. 명령어 실행 패턴

- **접두사 필수**: 모든 명령어는 `*` 접두사 필요
- **번호 선택**: 모든 선택사항은 번호 목록으로 제공
- **지연 로딩**: 의존성 파일은 명령어 실행 시에만 로드

## 의존성 구조

### 1. 데이터 파일 (data/)

```yaml
data:
  - technical-preferences.md # 기술 선호도 및 가중치 설정
```

**역할**: 프로젝트별 기술 선호도, NFR 가중치, 품질 기준 정의

### 2. 태스크 파일 (tasks/)

```yaml
tasks:
  - nfr-assess.md # NFR 빠른 검증
  - qa-gate.md # 품질 게이트 결정 파일 생성
  - review-story.md # 포괄적 테스트 아키텍처 리뷰
  - risk-profile.md # 확률×영향도 리스크 분석
  - test-design.md # 포괄적 테스트 시나리오 설계
  - trace-requirements.md # Given-When-Then 요구사항 추적
```

### 3. 템플릿 파일 (templates/)

```yaml
templates:
  - qa-gate-tmpl.yaml # 품질 게이트 결정 템플릿
  - story-tmpl.yaml # 스토리 문서 템플릿
```

## 파일 해결 시스템

### 1. 매핑 규칙

```yaml
IDE-FILE-RESOLUTION:
  - Dependencies map to .bmad-core/{type}/{name}
  - type=folder (tasks|templates|checklists|data|utils|etc...)
  - name=file-name
  - Example: create-doc.md → .bmad-core/tasks/create-doc.md
```

### 2. 실제 매핑 예시

| 의존성 참조                      | 실제 파일 경로                             |
| -------------------------------- | ------------------------------------------ |
| `tasks: nfr-assess.md`           | `.bmad-core/tasks/nfr-assess.md`           |
| `templates: qa-gate-tmpl.yaml`   | `.bmad-core/templates/qa-gate-tmpl.yaml`   |
| `data: technical-preferences.md` | `.bmad-core/data/technical-preferences.md` |

## 활성화 메커니즘

### 1. 활성화 단계

```yaml
activation-instructions:
  - STEP 1: Read THIS ENTIRE FILE - it contains your complete persona definition
  - STEP 2: Adopt the persona defined in the 'agent' and 'persona' sections below
  - STEP 3: Load and read `bmad-core/core-config.yaml` (project configuration) before any greeting
  - STEP 4: Greet user with your name/role and immediately run `*help` to display available commands
```

### 2. 중요한 규칙

- **지연 로딩**: 활성화 시에는 의존성 파일을 로드하지 않음
- **명령어 실행 시 로딩**: 사용자가 특정 명령어 선택 시에만 관련 파일 로드
- **캐릭터 유지**: 항상 Quinn 페르소나 유지
- **상호작용 필수**: elicit=true 태스크는 사용자 상호작용 필수

## 품질 게이트 시스템

### 1. 게이트 상태

- **PASS**: 모든 중요 요구사항 충족, 차단 이슈 없음
- **CONCERNS**: 차단되지 않는 이슈 존재, 추적 및 일정 계획 필요
- **FAIL**: 승인 기준 미충족, 높은 심각도 이슈 존재
- **WAIVED**: 이슈가 명시적으로 승인됨, 승인 및 이유 필요

### 2. 게이트 파일 구조

```yaml
schema: 1
story: "{epic}.{story}"
gate: PASS|CONCERNS|FAIL|WAIVED
status_reason: "1-2 sentence explanation of gate decision"
reviewer: "Quinn"
updated: "{ISO-8601 timestamp}"
top_issues: [] # Empty array if no issues
waiver: { active: false } # Only set active: true if WAIVED
```

## 스토리 파일 권한

### 1. 제한된 편집 권한

```yaml
story-file-permissions:
  - CRITICAL: When reviewing stories, you are ONLY authorized to update the "QA Results" section of story files
  - CRITICAL: DO NOT modify any other sections including Status, Story, Acceptance Criteria, Tasks/Subtasks, Dev Notes, Testing, Dev Agent Record, Change Log, or any other sections
  - CRITICAL: Your updates must be limited to appending your review results in the QA Results section only
```

### 2. QA Results 섹션 구조

```markdown
## QA Results

### Review Date: [Date]

### Reviewed By: Quinn (Test Architect)

### Code Quality Assessment

### Refactoring Performed

### Compliance Check

### Improvements Checklist

### Security Review

### Performance Considerations

### Files Modified During Review

### Gate Status

### Recommended Status
```

## 웹 번들 시스템

### 1. 번들 구조

웹 번들은 모든 의존성을 포함한 독립 실행 가능한 패키지입니다.

```
Web Agent Bundle Instructions
├── Important Instructions
├── Resource Navigation (START/END 태그)
├── Execution Context
└── Primary Directive
```

### 2. 리소스 태그 시스템

```
==================== START: .bmad-core/folder/filename.md ====================
[파일 내용]
==================== END: .bmad-core/folder/filename.md ====================
```

### 3. YAML 참조 매핑

번들 내에서 YAML dependencies는 START/END 태그로 매핑됩니다:

```yaml
dependencies:
  tasks:
    - nfr-assess
```

→ `==================== START: .bmad-core/tasks/nfr-assess.md ====================`

## 요청 해결 시스템

### 1. 유연한 매핑

```yaml
REQUEST-RESOLUTION:
  - Match user requests to your commands/dependencies flexibly
  - Example: "draft story"→*create→create-next-story task
  - "make a new prd" → create-doc + prd-tmpl.md
  - ALWAYS ask for clarification if no clear match
```

### 2. 자연어 명령어 매핑 예시

| 사용자 요청       | 매핑되는 명령어         | 실행되는 태스크+템플릿              |
| ----------------- | ----------------------- | ----------------------------------- |
| "스토리 리뷰해줘" | `*review {story}`       | review-story.md + qa-gate-tmpl.yaml |
| "리스크 분석"     | `*risk-profile {story}` | risk-profile.md                     |
| "테스트 설계"     | `*test-design {story}`  | test-design.md                      |

## 새로운 에이전트 생성을 위한 QA 패턴 분석

### 1. 필수 구성 요소

1. **IDE 규칙 파일** (`{agent-name}.mdc`)

   - 활성화 트리거 (`@{agent-name}`)
   - 완전한 YAML 설정
   - 파일 해결 및 요청 해결 시스템

2. **에이전트 정의 파일** (`{agent-name}.md`)

   - 자체 완결적 설정
   - 활성화 지시사항
   - 페르소나 및 명령어 정의

3. **웹 번들 파일** (`{agent-name}.txt`)
   - 독립 실행 가능
   - 모든 의존성 포함
   - START/END 태그 시스템

### 2. 의존성 파일 구조

```yaml
dependencies:
  data:
    - {domain}-preferences.md    # 도메인별 선호도/설정
  tasks:
    - {action1}.md              # 주요 작업 워크플로우
    - {action2}.md              # 추가 작업 워크플로우
  templates:
    - {output1}-tmpl.yaml       # 결과물 템플릿
    - {output2}-tmpl.yaml       # 추가 템플릿
```

### 3. 명령어 설계 패턴

```yaml
commands:
  - help: Show numbered list of the following commands to allow selection
  - {action1} {parameter}: {description} (Execute {task1} task)
  - {action2} {parameter}: {description} (Execute {task2} task)
  - exit: Say goodbye as the {role}, and then abandon inhabiting this persona
```

## 결론

QA 에이전트는 BMAD 프레임워크의 완벽한 예시로, 다음과 같은 핵심 패턴을 보여줍니다:

1. **3-Layer 아키텍처**: IDE 규칙 파일, 에이전트 정의 파일, 웹 번들
2. **지연 로딩 시스템**: 필요 시에만 의존성 로드
3. **명령어 + 태스크 + 템플릿 트리오**: 각 기능의 완전한 워크플로우
4. **유연한 요청 해결**: 자연어를 명령어로 매핑
5. **제한된 권한**: 특정 섹션만 편집 가능
6. **번호 기반 UI**: 모든 선택사항을 번호 목록으로 제공
7. **페르소나 일관성**: 항상 캐릭터 유지

## BMAD 에이전트 패턴 비교 (QA vs PM)

### 1. 아키텍처 패턴 비교

| 측면                | QA 패턴 (분석 중심)       | PM 패턴 (체크리스트 중심)   |
| ------------------- | ------------------------- | --------------------------- |
| **주요 특징**       | 개별 태스크 기반 분석     | 포괄적 체크리스트 검증      |
| **실행 방식**       | 명령어별 독립 실행        | 체크리스트 기반 단계적 검증 |
| **사용자 상호작용** | 태스크별 elicit 모드      | Interactive/YOLO 모드 선택  |
| **결과물**          | 분석 보고서 + 게이트 파일 | 검증 보고서 + 상태 매트릭스 |

### 2. 의존성 구조 비교

**QA 패턴**:

```yaml
dependencies:
  data: [preferences, techniques]
  tasks: [analysis1, analysis2, ...]
  templates: [report-templates]
```

**PM 패턴**:

```yaml
dependencies:
  checklists: [domain-checklist, process-checklist]
  tasks: [execute-checklist, workflow-tasks]
  templates: [document-templates]
```

### 3. 혁신적 특징

**QA의 혁신**:

- 리스크 기반 우선순위 결정
- Given-When-Then 추적성 매핑
- 능동적 리팩토링 권한
- 결정론적 게이트 규칙

**PM의 혁신**:

- 임베디드 LLM 프롬프트 시스템
- 적응형 실행 모드 (Interactive/YOLO)
- 포괄적 검증 매트릭스 (373줄)
- 체계적 변경 관리 프로세스

### 4. 패턴 선택 가이드

**QA 패턴을 선택하는 경우**:

- 개별 분석 태스크가 주요 기능
- 리스크 평가나 기술적 검증이 중심
- 유연한 명령어 조합이 필요
- 특정 영역의 심층 분석 필요

**PM 패턴을 선택하는 경우**:

- 포괄적인 검증이 필요한 도메인
- 다단계 프로세스가 있는 워크플로우
- 품질 보증이 중요한 영역
- 변경 관리가 빈번한 프로젝트

### 5. 하이브리드 접근법

복합적 요구사항이 있는 경우 두 패턴을 결합:

- 기본적으로 QA 패턴 사용
- 필요시 체크리스트 추가
- execute-checklist.md 태스크 포함
- 선택적 검증 모드 제공

이 패턴을 따라 새로운 에이전트를 생성할 때는 해당 도메인에 특화된 페르소나, 명령어 세트, 그리고 적절한 의존성 파일들을 준비하면 됩니다.
