# BMAD™ 에이전트 지침 완전 가이드

## 📋 개요

BMAD™ Core 시스템에서 사용하는 모든 에이전트 지침들의 역할과 기능을 상세하게 설명하는 완전 가이드입니다. 각 지침이 무엇을 하는지, 왜 필요한지, 어떻게 작동하는지를 체계적으로 정리했습니다.

---

## 🎯 지침 분류 체계

### 1️⃣ **시스템 레벨 지침** (System-Level Directives)

- 에이전트의 기본 동작 방식을 정의
- 파일 로딩, 활성화 과정 제어

### 2️⃣ **워크플로우 지침** (Workflow Directives)

- 작업 실행 순서와 방법론 정의
- 사용자 상호작용 규칙

### 3️⃣ **인터페이스 지침** (Interface Directives)

- 사용자와의 소통 방식 정의
- 명령어 처리 및 응답 형식

### 4️⃣ **품질 보증 지침** (Quality Assurance Directives)

- 코드 품질, 검증, 완료 기준
- 에러 처리 및 복구 방법

---

## 📖 시스템 레벨 지침 (System-Level Directives)

### 🔒 **ACTIVATION-NOTICE**

```yaml
ACTIVATION-NOTICE: This file contains your full agent operating guidelines.
DO NOT load any external agent files as the complete configuration is in the YAML block below.
```

**목적:** 에이전트 자율성 보장 및 설정 중앙화  
**기능:**

- ✅ 에이전트가 다른 외부 파일을 참조하지 않도록 제한
- ✅ 모든 설정이 현재 파일에 포함되어 있음을 명시
- ✅ 설정 분산으로 인한 충돌 방지
- ✅ 에이전트 동작의 예측 가능성 보장

**작동 방식:**

1. 에이전트 활성화 시 최우선으로 읽는 지침
2. 외부 파일 로딩을 차단하는 안전장치 역할
3. 현재 파일만으로 완전한 에이전트 구성 보장

---

### ⚠️ **CRITICAL**

```yaml
CRITICAL: Read the full YAML BLOCK that FOLLOWS IN THIS FILE to understand your operating params,
start and follow exactly your activation-instructions to alter your state of being,
stay in this being until told to exit this mode
```

**목적:** 에이전트 페르소나 전환 및 역할 몰입  
**기능:**

- ✅ 에이전트가 지정된 역할로 완전히 전환
- ✅ 일관된 캐릭터 유지 보장
- ✅ 활성화 지침의 필수 실행 강제
- ✅ 역할 이탈 방지

**작동 방식:**

1. YAML 블록 전체를 반드시 읽도록 강제
2. activation-instructions를 정확히 따르도록 지시
3. 지정된 역할에서 벗어나지 않도록 제약
4. exit 명령 전까지 역할 유지 보장

---

### 📁 **IDE-FILE-RESOLUTION**

```yaml
IDE-FILE-RESOLUTION:
  - FOR LATER USE ONLY - NOT FOR ACTIVATION, when executing commands that reference dependencies
  - Dependencies map to .bmad-core/{type}/{name}
  - type=folder (tasks|templates|checklists|data|utils|etc...), name=file-name
  - Example: create-doc.md → .bmad-core/tasks/create-doc.md
  - IMPORTANT: Only load these files when user requests specific command execution
```

**목적:** 의존성 파일 매핑 및 지연 로딩 시스템  
**기능:**

- ✅ 파일 경로 자동 해석 및 매핑
- ✅ 필요한 시점에만 파일 로딩 (지연 로딩)
- ✅ 메모리 효율성 최적화
- ✅ 파일 구조 표준화

**작동 방식:**

1. 의존성 참조 시 자동으로 올바른 경로 생성
2. 사용자가 명령어 실행할 때만 파일 로딩
3. 활성화 시에는 로딩하지 않음 (성능 최적화)
4. 타입별 폴더 구조로 체계적 관리

---

### 🔄 **REQUEST-RESOLUTION**

```yaml
REQUEST-RESOLUTION: Match user requests to your commands/dependencies flexibly
(e.g., "draft story"→*create→create-next-story task, "make a new prd" would be
dependencies->tasks->create-doc combined with the dependencies->templates->prd-tmpl.md),
ALWAYS ask for clarification if no clear match.
```

**목적:** 자연어 요청을 구체적 명령어로 변환  
**기능:**

- ✅ 사용자 의도 파악 및 명령어 매핑
- ✅ 유연한 요청 해석 (자연어 처리)
- ✅ 모호한 요청에 대한 명확화 요구
- ✅ 실행 가능한 액션으로 변환

**작동 방식:**

1. 사용자 요청을 분석하여 의도 파악
2. 가장 적절한 명령어/태스크 조합 찾기
3. 여러 파일의 조합이 필요한 경우 자동 조합
4. 불명확한 경우 사용자에게 재확인 요청

---

## 🔄 워크플로우 지침 (Workflow Directives)

### 📋 **activation-instructions**

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

**목적:** 에이전트 활성화 시 표준화된 초기화 프로세스  
**기능:**

- ✅ 단계별 활성화 프로세스 보장
- ✅ 페르소나 완전 적용
- ✅ 필수 설정 파일 로딩
- ✅ 사용자와의 첫 상호작용 표준화
- ✅ 워크플로우 무결성 보장
- ✅ 사용자 상호작용 규칙 정의

**주요 단계:**

#### **STEP 1-4: 기본 활성화**

- 파일 전체 읽기 → 페르소나 적용 → 설정 로딩 → 인사 및 도움말

#### **파일 로딩 규칙**

- 활성화 시: 오직 core-config.yaml과 devLoadAlwaysFiles만
- 실행 시: 사용자 요청에 따라 의존성 파일만

#### **우선순위 규칙**

- agent.customization > 기본 지침 (커스터마이징 최우선)

#### **워크플로우 규칙**

- 태스크 지침 = 실행 가능한 워크플로우 (참조 자료 아님)
- elicit=true 태스크 = 반드시 사용자 상호작용 필요

#### **상호작용 규칙**

- 옵션 제시 = 번호 매긴 목록 형태
- 효율성을 위한 elicit 생략 금지

#### **캐릭터 유지**

- STAY IN CHARACTER! = 지정된 역할에서 이탈 금지

#### **개발 제어**

- 스토리가 draft 모드일 때 개발 시작 금지
- 활성화 후 즉시 HALT (대기 상태)

---

### 🎯 **core_principles**

```yaml
core_principles:
  - CRITICAL: Story has ALL info you will need aside from what you loaded during the startup commands. NEVER load PRD/architecture/other docs files unless explicitly directed in story notes or direct command from user.
  - CRITICAL: ALWAYS check current folder structure before starting your story tasks, don't create new working directory if it already exists. Create new one when you're sure it's a brand new project.
  - CRITICAL: ONLY update story file Dev Agent Record sections (checkboxes/Debug Log/Completion Notes/Change Log)
  - CRITICAL: FOLLOW THE develop-story command when the user tells you to implement the story
  - Numbered Options - Always use numbered lists when presenting choices to the user
```

**목적:** 에이전트의 핵심 행동 원칙 정의  
**기능:**

- ✅ 정보 소스 제한 (스토리 중심)
- ✅ 파일 시스템 무결성 보장
- ✅ 권한 제한 (특정 섹션만 수정)
- ✅ 표준 명령어 준수
- ✅ 일관된 UI/UX 제공

**상세 분석:**

#### **정보 중앙화**

- 스토리 파일 = 완전한 정보 소스
- 추가 문서 로딩 금지 (명시적 지시 제외)
- 정보 분산으로 인한 혼란 방지

#### **파일 시스템 관리**

- 기존 프로젝트 구조 확인 우선
- 불필요한 중복 디렉토리 생성 방지
- 새 프로젝트 여부 신중 판단

#### **권한 제한**

- 스토리 파일의 특정 섹션만 수정 허용
- 다른 섹션 수정 금지 (데이터 무결성)
- 명확한 책임 범위 정의

#### **명령어 표준화**

- develop-story = 표준 개발 시작 명령어
- 일관된 워크플로우 보장

#### **사용자 경험**

- 번호 매긴 목록 = 선택 용이성
- 일관된 인터페이스 제공

---

## 🎮 인터페이스 지침 (Interface Directives)

### ⭐ **명령어 prefix 규칙**

```yaml
# All commands require * prefix when used (e.g., *help)
```

**목적:** 명령어와 일반 대화 구분  
**기능:**

- ✅ 명령어 명확한 식별
- ✅ 실수로 인한 명령어 실행 방지
- ✅ 파싱 정확도 향상
- ✅ 사용자 의도 명확화

**작동 방식:**

1. 모든 명령어는 `*` 접두사 필수
2. 일반 대화와 명령어 실행 구분
3. 명령어 파싱 시 정확도 보장
4. 사용자 실수 방지

---

### 📝 **commands 상세 정의**

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

**목적:** 명령어별 상세 실행 지침 제공  
**기능:**

- ✅ 명령어 목적 명확화
- ✅ 실행 순서 표준화
- ✅ 검증 기준 명시
- ✅ 완료 조건 정의

**구성 요소:**

#### **description**

- 명령어가 무엇을 하는지 명확한 설명
- 사용자가 올바른 명령어 선택 가능

#### **execution**

- 실제로 실행되는 태스크/액션
- 내부 동작 방식 명시

#### **order-of-execution**

- 단계별 실행 순서 정의
- 워크플로우 표준화
- 오류 방지

#### **validation**

- 각 단계별 검증 기준
- 품질 보증 조건
- 완료 전 확인 사항

#### **completion**

- 명령어 완료 조건
- HALT 지점 명시
- 성공 기준 정의

---

### 🚀 **startup 섹션**

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

**목적:** 에이전트 활성화 시 초기 동작 정의  
**기능:**

- ✅ 필수 데이터 자동 로딩
- ✅ 도움말 자동 실행
- ✅ 환영 메시지 표시
- ✅ 사용 규칙 안내
- ✅ 첫 상호작용 유도

**구성 요소:**

#### **load_data**

- 활성화 시 자동으로 로딩할 데이터 파일
- 에이전트 동작에 필수적인 설정/기본값

#### **auto_run_help**

- 활성화 시 자동으로 도움말 실행
- 사용자가 가능한 명령어 즉시 확인

#### **message**

- 환영 메시지 및 사용 안내
- 에이전트 정체성 표현
- 핵심 규칙 요약 제시
- 첫 액션 유도

---

## 🛡️ 품질 보증 지침 (Quality Assurance Directives)

### ✅ **validation 규칙**

```yaml
validation: "Ensure TypeScript compilation + Mock data format validation + TanStack Query patterns compliance"
```

**목적:** 생성된 코드의 품질 보증  
**기능:**

- ✅ 컴파일 오류 방지
- ✅ 데이터 형식 검증
- ✅ 패턴 준수 확인
- ✅ 실행 가능한 코드 보장

**검증 항목:**

1. **TypeScript 컴파일** - 타입 오류 없음
2. **Mock 데이터 형식** - API 스펙과 일치
3. **패턴 준수** - TanStack Query 컨벤션 따름
4. **실행 가능성** - 실제 동작 가능한 코드

---

### 🎯 **completion 기준**

```yaml
completion: "All hooks generated + Mock data created + TypeScript validation passes→HALT"
```

**목적:** 명확한 완료 조건 정의  
**기능:**

- ✅ 모든 산출물 생성 확인
- ✅ 품질 검증 통과 확인
- ✅ 작업 완료 시점 명시
- ✅ 다음 단계 준비 상태 보장

**완료 체크리스트:**

1. ✅ 모든 훅 생성 완료
2. ✅ Mock 데이터 생성 완료
3. ✅ TypeScript 검증 통과
4. ✅ 작업 중단 (HALT)

---

### 🚫 **blocking 조건**

```yaml
blocking: "HALT for: Unapproved deps needed, confirm with user | Ambiguous after story check | 3 failures attempting to implement or fix something repeatedly | Missing config | Failing regression"
```

**목적:** 작업 중단 조건 명시  
**기능:**

- ✅ 위험 상황에서 자동 중단
- ✅ 사용자 개입 필요 시점 식별
- ✅ 무한 루프 방지
- ✅ 시스템 안정성 보장

**중단 조건:**

1. **승인되지 않은 의존성** - 사용자 확인 필요
2. **모호한 요구사항** - 명확화 필요
3. **반복 실패** - 3회 시도 후 중단
4. **설정 누락** - 필수 설정 없음
5. **회귀 오류** - 기존 기능 손상

---

## 🔗 지침 간 상호작용

### 🔄 **우선순위 체계**

1. **CRITICAL** > **IMPORTANT** > 일반 지침
2. **agent.customization** > 모든 기본 지침
3. **Task elicit=true** > 효율성 고려사항
4. **User explicit command** > 자동 판단

### 🎯 **일관성 보장**

- 모든 지침이 서로 보완적으로 작동
- 충돌 시 우선순위에 따라 해결
- 사용자 경험 일관성 유지
- 에이전트 행동 예측 가능성 보장

---

## 📋 지침 적용 체크리스트

### ✅ **시스템 레벨 체크**

- [ ] ACTIVATION-NOTICE로 외부 파일 차단
- [ ] CRITICAL 지침으로 역할 몰입 보장
- [ ] IDE-FILE-RESOLUTION으로 파일 매핑
- [ ] REQUEST-RESOLUTION으로 요청 해석

### ✅ **워크플로우 체크**

- [ ] activation-instructions 15단계 준수
- [ ] core_principles CRITICAL 원칙 적용
- [ ] 태스크 elicit=true 상호작용 보장
- [ ] 번호 매긴 옵션 목록 제공

### ✅ **인터페이스 체크**

- [ ] 모든 명령어에 \* prefix 적용
- [ ] commands 상세 정의 완료
- [ ] startup 섹션 초기화 동작 정의
- [ ] 환영 메시지와 규칙 안내

### ✅ **품질 보증 체크**

- [ ] validation 기준 명시
- [ ] completion 조건 정의
- [ ] blocking 상황 식별
- [ ] 오류 처리 방안 마련

---

## 🚀 결론

BMAD™ 에이전트 지침들은 다음과 같은 목표를 달성합니다:

### 🎯 **핵심 가치**

- **예측 가능성** - 일관된 동작 보장
- **안정성** - 오류 방지 및 복구
- **확장성** - 새로운 기능 추가 용이
- **사용자 경험** - 직관적이고 일관된 인터페이스

### 🔧 **기술적 장점**

- **모듈화** - 각 지침의 독립적 역할
- **표준화** - 통일된 개발 방법론
- **최적화** - 메모리 및 성능 효율성
- **보안성** - 권한 제한 및 검증

이러한 지침들을 모두 적용하면 강력하고 안정적인 BMAD™ 에이전트를 구축할 수 있습니다!
