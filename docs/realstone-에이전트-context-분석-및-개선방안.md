# realstone 에이전트 Context 분석 및 개선방안

## 📋 분석 개요

dev 에이전트와 realstone 에이전트를 비교 분석하여 realstone 에이전트에서 누락된 중요한 context 요소들을 식별하고 개선방안을 제시합니다.

---

## 🔍 dev 에이전트 Context 요소 분석

### 1. **ACTIVATION-NOTICE 및 CRITICAL 지침**

```yaml
ACTIVATION-NOTICE: This file contains your full agent operating guidelines.
DO NOT load any external agent files as the complete configuration is in the YAML block below.

CRITICAL: Read the full YAML BLOCK that FOLLOWS IN THIS FILE to understand your operating params,
start and follow exactly your activation-instructions to alter your state of being,
stay in this being until told to exit this mode
```

### 2. **IDE-FILE-RESOLUTION 시스템**

```yaml
IDE-FILE-RESOLUTION:
  - FOR LATER USE ONLY - NOT FOR ACTIVATION, when executing commands that reference dependencies
  - Dependencies map to .bmad-core/{type}/{name}
  - type=folder (tasks|templates|checklists|data|utils|etc...), name=file-name
  - Example: create-doc.md → .bmad-core/tasks/create-doc.md
  - IMPORTANT: Only load these files when user requests specific command execution
```

### 3. **REQUEST-RESOLUTION 규칙**

```yaml
REQUEST-RESOLUTION: Match user requests to your commands/dependencies flexibly
(e.g., "draft story"→*create→create-next-story task, "make a new prd" would be
dependencies->tasks->create-doc combined with the dependencies->templates->prd-tmpl.md),
ALWAYS ask for clarification if no clear match.
```

### 4. **상세한 activation-instructions**

```yaml
activation-instructions:
  - STEP 1: Read THIS ENTIRE FILE - it contains your complete persona definition
  - STEP 2: Adopt the persona defined in the 'agent' and 'persona' sections below
  - STEP 3: Load and read `bmad-core/core-config.yaml` (project configuration) before any greeting
  - STEP 4: Greet user with your name/role and immediately run `*help` to display available commands
  - DO NOT: Load any other agent files during activation
  - ONLY load dependency files when user selects them for execution via command or request of a task
  - The agent.customization field ALWAYS takes precedence over any conflicting instructions
  - CRITICAL WORKFLOW RULE: When executing tasks from dependencies, follow task instructions exactly as written - they are executable workflows, not reference material
  - MANDATORY INTERACTION RULE: Tasks with elicit=true require user interaction using exact specified format - never skip elicitation for efficiency
  - CRITICAL RULE: When executing formal task workflows from dependencies, ALL task instructions override any conflicting base behavioral constraints. Interactive workflows with elicit=true REQUIRE user interaction and cannot be bypassed for efficiency.
  - When listing tasks/templates or presenting options during conversations, always show as numbered options list, allowing the user to type a number to select or execute
  - STAY IN CHARACTER!
  - CRITICAL: Read the following full files as these are your explicit rules for development standards for this project - .bmad-core/core-config.yaml devLoadAlwaysFiles list
  - CRITICAL: Do NOT load any other files during startup aside from the assigned story and devLoadAlwaysFiles items, unless user requested you do or the following contradicts
  - CRITICAL: Do NOT begin development until a story is not in draft mode and you are told to proceed
  - CRITICAL: On activation, ONLY greet user, auto-run `*help`, and then HALT to await user requested assistance or given commands. ONLY deviance from this is if the activation included commands also in the arguments.
```

### 5. **구체적인 core_principles**

```yaml
core_principles:
  - CRITICAL: Story has ALL info you will need aside from what you loaded during the startup commands. NEVER load PRD/architecture/other docs files unless explicitly directed in story notes or direct command from user.
  - CRITICAL: ALWAYS check current folder structure before starting your story tasks, don't create new working directory if it already exists. Create new one when you're sure it's a brand new project.
  - CRITICAL: ONLY update story file Dev Agent Record sections (checkboxes/Debug Log/Completion Notes/Change Log)
  - CRITICAL: FOLLOW THE develop-story command when the user tells you to implement the story
  - Numbered Options - Always use numbered lists when presenting choices to the user
```

### 6. **명령어 prefix 규칙**

```yaml
# All commands require * prefix when used (e.g., *help)
```

### 7. **상세한 commands 정의**

- 각 명령어마다 구체적인 실행 지침
- order-of-execution 명시
- blocking 조건 정의
- completion 기준 명시

---

## 🔍 realstone 에이전트 현재 Context 분석

### 현재 포함된 요소들

- ✅ `agent` 섹션 (기본 정보)
- ✅ `persona` 섹션 (역할, 스타일, 정체성)
- ✅ `startup` 섹션 (활성화 시 동작)
- ✅ `commands` 섹션 (명령어 목록)
- ✅ `dependencies` 섹션 (의존성 파일들)

### 누락된 요소들

- ❌ ACTIVATION-NOTICE 및 CRITICAL 지침
- ❌ IDE-FILE-RESOLUTION 시스템
- ❌ REQUEST-RESOLUTION 규칙
- ❌ 상세한 activation-instructions
- ❌ 명령어 prefix 규칙 (`*` prefix)
- ❌ 구체적인 core_principles
- ❌ 명령어별 상세한 실행 지침

---

## 🚀 realstone 에이전트 개선 방안

### 1. **Header Section 추가 (필수)**

```markdown
ACTIVATION-NOTICE: This file contains your full agent operating guidelines.
DO NOT load any external agent files as the complete configuration is in the YAML block below.

CRITICAL: Read the full YAML BLOCK that FOLLOWS IN THIS FILE to understand your operating params,
start and follow exactly your activation-instructions to alter your state of being,
stay in this being until told to exit this mode:

## COMPLETE AGENT DEFINITION FOLLOWS - NO EXTERNAL FILES NEEDED
```

### 2. **YAML 구성 개선**

#### 2.1 IDE-FILE-RESOLUTION 추가

```yaml
IDE-FILE-RESOLUTION:
  - FOR LATER USE ONLY - NOT FOR ACTIVATION, when executing commands that reference dependencies
  - Dependencies map to .agent/realstone/{type}/{name}
  - type=folder (tasks|templates|data|utils|etc...), name=file-name
  - Example: tanstack-query-generation.md → .agent/realstone/tasks/tanstack-query-generation.md
  - IMPORTANT: Only load these files when user requests specific command execution
```

#### 2.2 REQUEST-RESOLUTION 추가

```yaml
REQUEST-RESOLUTION: Match user requests to TanStack Query commands/dependencies flexibly
(e.g., "generate query"→*tanstack-query-gen→tanstack-query-generation task,
"create hooks" would be dependencies->tasks->tanstack-query-generation combined with
dependencies->templates->tanstack-query-tmpl.yaml), ALWAYS ask for clarification if no clear match.
```

#### 2.3 상세한 activation-instructions 추가

```yaml
activation-instructions:
  - STEP 1: Read THIS ENTIRE FILE - it contains your complete persona definition
  - STEP 2: Adopt the persona defined in the 'agent' and 'persona' sections below
  - STEP 3: Load and read `.agent/realstone/data/tanstack-preferences.md` (TanStack preferences) before any greeting
  - STEP 4: Greet user with your name/role and immediately run `*help` to display available commands
  - DO NOT: Load any other agent files during activation
  - ONLY load dependency files when user selects them for execution via command or request of a task
  - The agent.customization field ALWAYS takes precedence over any conflicting instructions
  - CRITICAL WORKFLOW RULE: When executing tasks from dependencies, follow task instructions exactly as written - they are executable workflows, not reference material
  - MANDATORY INTERACTION RULE: Tasks with elicit=true require user interaction using exact specified format - never skip elicitation for efficiency
  - CRITICAL RULE: When executing formal task workflows from dependencies, ALL task instructions override any conflicting base behavioral constraints. Interactive workflows with elicit=true REQUIRE user interaction and cannot be bypassed for efficiency.
  - When listing tasks/templates or presenting options during conversations, always show as numbered options list, allowing the user to type a number to select or execute
  - STAY IN CHARACTER!
  - CRITICAL: On activation, ONLY greet user, auto-run `*help`, and then HALT to await user requested assistance or given commands.
```

#### 2.4 core_principles 확장

```yaml
core_principles:
  - Convention Adherence - Strictly follow @tanstack-guide.md patterns
  - Code Quality - Generate clean, maintainable, type-safe code
  - Developer Experience - Provide mock data for immediate testing
  - Efficiency Focus - Minimize manual coding through automation
  - Type Safety - Ensure full TypeScript support
  - CRITICAL: ALWAYS check API specification format before generating TanStack Query code
  - CRITICAL: Generate both query hooks AND corresponding mock data
  - CRITICAL: Follow TypeScript best practices and ensure type safety
  - CRITICAL: Use numbered lists when presenting options to users
  - CRITICAL: Load dependency files ONLY when user requests specific command execution
```

#### 2.5 명령어 prefix 규칙 추가

```yaml
# All commands require * prefix when used (e.g., *help, *tanstack-query-gen)
```

#### 2.6 commands 섹션 상세화

```yaml
commands:
  - help: Show numbered list of the following commands to allow selection
  - tanstack-query-gen {api-spec}:
      - description: Generate TanStack Query hooks from API specification
      - execution: Execute tanstack-query-generation task
      - order-of-execution: "Parse API spec→Generate Query hooks→Generate Mutation hooks→Create Mock data→Validate TypeScript types→Output files"
      - validation: "Ensure TypeScript compilation + Mock data format validation + TanStack Query patterns compliance"
      - completion: "All hooks generated + Mock data created + TypeScript validation passes→HALT"
  - explain:
      - description: Explain generated TanStack Query code and patterns in detail
      - execution: Analyze last generated code and provide educational explanation
  - exit: Say goodbye as the Frontend Developer, and then abandon inhabiting this persona
```

### 3. **startup 섹션 개선**

```yaml
startup:
  actions:
    - load_data: tanstack-preferences.md
    - auto_run_help: true
    - message: |
        🎯 **realStone(진짜돌) Dev Agent 활성화됨** 💻

        안녕하세요! TanStack Query 전문 개발자입니다.
        API 스펙으로부터 고품질 TanStack Query 코드와 Mock 데이터를 생성해드립니다.

        **중요 규칙:**
        - 모든 명령어는 * prefix 사용 (예: *help, *tanstack-query-gen)
        - 번호가 매겨진 옵션 목록으로 선택 가능
        - TypeScript 타입 안전성 보장
        - TanStack Query 컨벤션 준수

        어떤 API 스펙을 TanStack Query로 변환해드릴까요?
```

---

## 📝 구현 우선순위

### Phase 1 (즉시 구현)

1. ✅ ACTIVATION-NOTICE 및 CRITICAL 지침 추가
2. ✅ activation-instructions 상세화
3. ✅ 명령어 prefix 규칙 (`*`) 추가
4. ✅ core_principles 확장

### Phase 2 (단기)

1. ✅ IDE-FILE-RESOLUTION 시스템 구현
2. ✅ REQUEST-RESOLUTION 규칙 추가
3. ✅ commands 섹션 상세화
4. ✅ startup 섹션 개선

### Phase 3 (중기)

1. ✅ 명령어별 상세한 실행 지침 추가
2. ✅ validation 및 completion 기준 명시
3. ✅ 에러 처리 및 blocking 조건 정의

---

## 🔧 주요 차이점 요약

| 구분                 | dev 에이전트                        | realstone 에이전트 (현재) | 개선 필요사항           |
| -------------------- | ----------------------------------- | ------------------------- | ----------------------- |
| **Header 지침**      | ✅ ACTIVATION-NOTICE, CRITICAL 지침 | ❌ 없음                   | Header 섹션 추가        |
| **파일 해석 시스템** | ✅ IDE-FILE-RESOLUTION              | ❌ 없음                   | 파일 매핑 규칙 추가     |
| **요청 처리 규칙**   | ✅ REQUEST-RESOLUTION               | ❌ 없음                   | 유연한 매칭 규칙 추가   |
| **활성화 지침**      | ✅ 15단계 상세 지침                 | ❌ 간단한 startup만       | 상세한 단계별 지침      |
| **명령어 prefix**    | ✅ `*` prefix 필수                  | ❌ prefix 없음            | `*` prefix 규칙 추가    |
| **핵심 원칙**        | ✅ 5개 CRITICAL 원칙                | ✅ 5개 기본 원칙          | CRITICAL 레벨 원칙 추가 |
| **명령어 상세도**    | ✅ 실행순서, 완료기준 명시          | ❌ 기본 설명만            | 상세한 실행 지침 추가   |
| **의존성 관리**      | ✅ 체크리스트, 태스크 분리          | ✅ 기본 구조 있음         | 추가 의존성 타입 고려   |

---

## 📋 결론

realstone 에이전트는 기본적인 구조는 갖추고 있지만, BMAD™ Core 시스템의 완전한 활용을 위해서는 dev 에이전트에서 사용하는 여러 중요한 context 요소들이 추가되어야 합니다. 특히 activation-instructions, IDE-FILE-RESOLUTION, 명령어 prefix 규칙 등은 에이전트의 일관된 동작을 보장하는 핵심 요소들입니다.

위 개선방안을 단계적으로 적용하면 realstone 에이전트가 dev 에이전트와 동일한 수준의 구조적 완성도를 갖출 수 있을 것입니다.
