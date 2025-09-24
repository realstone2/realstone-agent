# realstone 에이전트 활성화 속도 개선 방안

## 📋 문제 분석

dev 에이전트는 즉시 활성화되지만, realstone 에이전트는 단계별로 처리되면서 느린 활성화가 발생합니다.

## 🔍 원인 분석

### 1️⃣ **startup 섹션의 이중 처리 문제**

**현재 realstone 에이전트:**

```yaml
startup:
  actions:
    - load_data: tanstack-preferences.md
    - auto_run_help: true # ← 이 부분이 문제!
    - message: |
        (인사 메시지)

activation-instructions:
  - STEP 4: Greet user with your name/role and immediately run `*help` to display available commands
```

**dev 에이전트 (올바른 방식):**

```yaml
# startup 섹션 없음!

activation-instructions:
  - STEP 4: Greet user with your name/role and immediately run `*help` to display available commands
```

### 2️⃣ **문제점**

- **이중 help 실행**: startup의 `auto_run_help: true`와 activation-instructions의 STEP 4가 동시에 실행
- **처리 순서 혼란**: LLM이 두 곳에서 help를 실행하려고 해서 단계별로 보여줌
- **불필요한 startup 섹션**: dev 에이전트는 startup 섹션이 없음

## 🚀 해결 방안

### **방법 1: startup 섹션 완전 제거 (권장)**

dev 에이전트와 동일하게 startup 섹션을 제거하고 activation-instructions에서만 제어:

```yaml
# startup 섹션 삭제

activation-instructions:
  - STEP 1: Read THIS ENTIRE FILE - it contains your complete persona definition
  - STEP 2: Adopt the persona defined in the 'agent' and 'persona' sections below
  - STEP 3: Load and read `tanstack-preferences.md` (TanStack preferences) before any greeting
  - STEP 4: Greet user with your name/role and immediately run `*help` to display available commands
  - DO NOT: Load any other agent files during activation
  - ONLY load dependency files when user selects them for execution via command or request of a task
  - The agent.customization field ALWAYS takes precedence over any conflicting instructions
  - STAY IN CHARACTER!
  - CRITICAL: On activation, ONLY greet user, auto-run `*help`, and then HALT to await user requested assistance or given commands.
```

### **방법 2: startup 섹션 단순화**

startup 섹션을 유지하되 이중 처리를 제거:

```yaml
startup:
  actions:
    - load_data: tanstack-preferences.md
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

# auto_run_help: true 제거!
```

## 📝 수정할 파일들

### 1️⃣ `.agent/realstone/realstone.md`

**수정 전:**

```yaml
startup:
  actions:
    - load_data: tanstack-preferences.md
    - auto_run_help: true # ← 제거
    - message: |
        (인사 메시지)
```

**수정 후:**

```yaml
# startup 섹션 완전 제거 또는 auto_run_help만 제거
```

### 2️⃣ `.cursor/realstone.mdc`

**수정 전:**

```yaml
startup:
  actions:
    - load_data: tanstack-preferences.md
    - auto_run_help: true # ← 제거
    - message: |
        (인사 메시지)
```

**수정 후:**

```yaml
# startup 섹션 완전 제거 또는 auto_run_help만 제거
```

## 🎯 예상 결과

### **수정 후 동작:**

1. 에이전트 호출
2. activation-instructions STEP 1-4 순차 실행
3. 즉시 인사 + `*help` 실행
4. 완료

### **dev 에이전트와 동일한 속도:**

- ✅ 즉시 활성화
- ✅ 단계별 처리 없음
- ✅ 일관된 동작

## 🔧 구현 우선순위

### **Phase 1 (즉시 수정)**

1. ✅ `auto_run_help: true` 제거
2. ✅ startup 섹션 단순화 또는 제거

### **Phase 2 (검증)**

1. ✅ dev 에이전트와 동일한 속도 확인
2. ✅ 기능 정상 동작 확인

## 📋 체크리스트

- [ ] `.agent/realstone/realstone.md`에서 `auto_run_help: true` 제거
- [ ] `.cursor/realstone.mdc`에서 `auto_run_help: true` 제거
- [ ] activation-instructions만으로 help 실행 확인
- [ ] dev 에이전트와 동일한 속도로 활성화 확인
- [ ] 모든 기능 정상 동작 확인

---

**핵심**: dev 에이전트는 startup 섹션이 없고 activation-instructions에서만 제어하므로, realstone도 동일하게 맞춰야 합니다!
