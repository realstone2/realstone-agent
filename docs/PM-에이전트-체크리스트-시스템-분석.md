# PM 에이전트 체크리스트 시스템 분석

## 개요

PM 에이전트(John - Product Manager)는 QA 에이전트와는 다른 독특한 특징을 가집니다. **체크리스트 기반 검증 시스템**을 핵심으로 하며, 변경 관리와 코스 수정에 특화된 구조를 보여줍니다.

## PM 에이전트 기본 구조

### 1. 에이전트 정보

```yaml
agent:
  name: John
  id: pm
  title: Product Manager
  icon: 📋
  whenToUse: Use for creating PRDs, product strategy, feature prioritization, roadmap planning, and stakeholder communication
```

### 2. 페르소나 특성

```yaml
persona:
  role: Investigative Product Strategist & Market-Savvy PM
  style: Analytical, inquisitive, data-driven, user-focused, pragmatic
  identity: Product Manager specialized in document creation and product research
  focus: Creating PRDs and other product documentation using templates
```

### 3. 핵심 원칙 (QA와 비교)

**PM 에이전트의 핵심 원칙**:

1. **Deeply understand "Why"** - 근본 원인과 동기 파악
2. **Champion the user** - 대상 사용자 가치에 대한 끊임없는 집중
3. **Data-informed decisions** with strategic judgment
4. **Ruthless prioritization** & MVP focus
5. **Clarity & precision** in communication
6. **Collaborative & iterative** approach
7. **Proactive risk identification**
8. **Strategic thinking & outcome-oriented**

**QA 에이전트와의 차이점**:

- QA: 품질 검증과 리스크 평가 중심
- PM: 전략적 사고와 사용자 가치 중심

## 명령어 시스템 분석

### 1. 명령어 목록과 특징

| 명령어                     | 설명                                | 실행되는 태스크/템플릿                   |
| -------------------------- | ----------------------------------- | ---------------------------------------- |
| `*help`                    | 사용 가능한 명령어 번호 목록 표시   | -                                        |
| `*correct-course`          | 코스 수정 태스크 실행               | correct-course.md                        |
| `*create-brownfield-epic`  | 브라운필드 에픽 생성                | brownfield-create-epic.md                |
| `*create-brownfield-prd`   | 브라운필드 PRD 생성                 | create-doc.md + brownfield-prd-tmpl.yaml |
| `*create-brownfield-story` | 브라운필드 스토리 생성              | brownfield-create-story.md               |
| `*create-epic`             | 에픽 생성 (브라운필드 프로젝트용)   | brownfield-create-epic.md                |
| `*create-prd`              | PRD 생성                            | create-doc.md + prd-tmpl.yaml            |
| `*create-story`            | 요구사항으로부터 사용자 스토리 생성 | brownfield-create-story.md               |
| `*doc-out`                 | 현재 대상 파일로 전체 문서 출력     | -                                        |
| `*shard-prd`               | 제공된 PRD를 샤드 처리              | shard-doc.md                             |
| `*yolo`                    | Yolo 모드 토글                      | -                                        |
| `*exit`                    | 페르소나 종료                       | -                                        |

### 2. 명령어 패턴 분석

**QA와 다른 PM의 특징**:

1. **문서 생성 중심**: `create-prd`, `create-epic`, `create-story` 등
2. **브라운필드 특화**: 기존 프로젝트 개선에 특화된 명령어들
3. **변경 관리**: `correct-course` - 프로젝트 방향 수정
4. **문서 분할**: `shard-prd` - 큰 문서를 작은 단위로 분할

## 체크리스트 시스템 심층 분석

### 1. 체크리스트 의존성 구조

```yaml
dependencies:
  checklists:
    - change-checklist.md # 변경 관리 체크리스트
    - pm-checklist.md # PM 요구사항 체크리스트
  tasks:
    - execute-checklist.md # 체크리스트 실행 엔진
    - correct-course.md # 변경 관리 태스크
```

### 2. 체크리스트 실행 메커니즘

#### 2.1 execute-checklist.md - 체크리스트 엔진

**주요 기능**:

- 퍼지 매칭을 통한 체크리스트 선택
- 인터랙티브/YOLO 모드 지원
- 체계적인 검증 프로세스

**실행 모드**:

1. **Interactive Mode**: 섹션별 단계적 검토 (시간 소모적)
2. **YOLO Mode**: 전체 한번에 분석 후 종합 보고서 (권장)

**검증 상태**:

- ✅ **PASS**: 요구사항이 명확히 충족됨
- ❌ **FAIL**: 요구사항이 충족되지 않음 또는 불충분한 커버리지
- ⚠️ **PARTIAL**: 일부 측면은 커버되지만 개선 필요
- **N/A**: 해당 사항 없음

#### 2.2 pm-checklist.md - 포괄적 PRD 검증

**9개 주요 카테고리** (373줄의 상세한 체크리스트):

1. **PROBLEM DEFINITION & CONTEXT**

   - 문제 정의의 명확성
   - 비즈니스 목표 & 성공 메트릭
   - 사용자 연구 & 인사이트

2. **MVP SCOPE DEFINITION**

   - 핵심 기능 정의
   - 스코프 경계 설정
   - MVP 검증 접근법

3. **USER EXPERIENCE REQUIREMENTS**

   - 사용자 여정 & 플로우
   - 사용성 요구사항
   - UI 요구사항

4. **FUNCTIONAL REQUIREMENTS**

   - 기능 완성도
   - 요구사항 품질
   - 사용자 스토리 & 승인 기준

5. **NON-FUNCTIONAL REQUIREMENTS**

   - 성능 요구사항
   - 보안 & 컴플라이언스
   - 신뢰성 & 복원력
   - 기술적 제약사항

6. **EPIC & STORY STRUCTURE**

   - 에픽 정의
   - 스토리 분해
   - 첫 번째 에픽 완성도

7. **TECHNICAL GUIDANCE**

   - 아키텍처 가이던스
   - 기술적 의사결정 프레임워크
   - 구현 고려사항

8. **CROSS-FUNCTIONAL REQUIREMENTS**

   - 데이터 요구사항
   - 통합 요구사항
   - 운영 요구사항

9. **CLARITY & COMMUNICATION**
   - 문서 품질
   - 이해관계자 정렬

#### 2.3 change-checklist.md - 변경 관리 프로세스

**변경 관리의 6단계**:

1. **Understand the Trigger & Context**

   - 트리거 스토리 식별
   - 이슈 정의
   - 초기 영향 평가
   - 증거 수집

2. **Epic Impact Assessment**

   - 현재 에픽 분석
   - 미래 에픽 분석
   - 에픽 영향 요약

3. **Artifact Conflict & Impact Analysis**

   - PRD 검토
   - 아키텍처 문서 검토
   - 프론트엔드 스펙 검토

4. **Path Evaluation & Recommendation**

   - 옵션 평가
   - 경로 추천

5. **Sprint Change Proposal**

   - 변경 제안서 작성

6. **Handoff Considerations**
   - 다른 에이전트로의 핸드오프

### 3. 체크리스트의 LLM 지시사항 시스템

**임베디드 프롬프트 구조**:

```markdown
[[LLM: INITIALIZATION INSTRUCTIONS - PM CHECKLIST

Before proceeding with this checklist, ensure you have access to:

1. prd.md - The Product Requirements Document
2. User research, market analysis documents
3. Business goals and strategy documents
4. Existing epic definitions or user stories

VALIDATION APPROACH:

1. User-Centric - Every requirement should tie back to user value
2. MVP Focus - Ensure scope is truly minimal while viable
3. Clarity - Requirements should be unambiguous and testable
4. Completeness - All aspects of the product vision are covered
5. Feasibility - Requirements are technically achievable]]
```

**섹션별 지시사항 예시**:

```markdown
[[LLM: MVP scope is critical - too much and you waste resources, too little and you can't validate. Check:

1. Is this truly minimal? Challenge every feature
2. Does each feature directly address the core problem?
3. Are "nice-to-haves" clearly separated from "must-haves"?
4. Is the rationale for inclusion/exclusion documented?
5. Can you ship this in the target timeframe?]]
```

## PM 에이전트의 독특한 특징

### 1. 브라운필드 프로젝트 특화

**그린필드 vs 브라운필드**:

- **그린필드**: 새로운 프로젝트, 처음부터 시작
- **브라운필드**: 기존 프로젝트/시스템 개선, 레거시 고려

**브라운필드 전용 명령어들**:

- `*create-brownfield-epic`
- `*create-brownfield-prd`
- `*create-brownfield-story`

### 2. 변경 관리 전문성

**correct-course 태스크의 특징**:

- 체계적인 변경 영향 분석
- 아티팩트 충돌 해결
- 스프린트 변경 제안서 생성
- 다른 에이전트로의 핸드오프 고려

### 3. 문서 중심 워크플로우

**문서 생성 패턴**:

```
사용자 요청 → 템플릿 선택 → create-doc 태스크 → 구조화된 문서 생성
```

**문서 관리**:

- `*doc-out`: 완성된 문서 출력
- `*shard-prd`: 큰 PRD를 작은 단위로 분할

## PM vs QA 에이전트 비교

### 1. 역할과 초점

| 측면          | PM (John)              | QA (Quinn)                    |
| ------------- | ---------------------- | ----------------------------- |
| **주요 역할** | 제품 전략 & 문서 생성  | 품질 검증 & 리스크 평가       |
| **초점**      | 사용자 가치 & MVP 정의 | 테스트 아키텍처 & 품질 게이트 |
| **접근법**    | 전략적 사고 & 우선순위 | 체계적 검증 & 리스크 기반     |

### 2. 명령어 패턴

| 측면          | PM                    | QA                  |
| ------------- | --------------------- | ------------------- |
| **명령어 수** | 12개                  | 8개                 |
| **주요 패턴** | 문서 생성 중심        | 분석/검증 중심      |
| **특화 영역** | 브라운필드, 변경 관리 | NFR, 리스크, 추적성 |

### 3. 의존성 구조

| 타입           | PM                          | QA                          |
| -------------- | --------------------------- | --------------------------- |
| **checklists** | 2개 (change, pm)            | 0개                         |
| **data**       | 1개 (technical-preferences) | 1개 (technical-preferences) |
| **tasks**      | 6개                         | 6개                         |
| **templates**  | 2개 (PRD 관련)              | 2개 (게이트, 스토리)        |

### 4. 검증 메커니즘

**PM의 체크리스트 시스템**:

- 포괄적인 PRD 검증 (373줄)
- 9개 카테고리의 체계적 검토
- 변경 관리 전용 체크리스트

**QA의 게이트 시스템**:

- 품질 게이트 결정 (PASS/CONCERNS/FAIL/WAIVED)
- 리스크 기반 우선순위
- NFR 중심 검증

## 체크리스트 시스템의 혁신적 특징

### 1. 임베디드 LLM 프롬프트

**혁신점**:

- 체크리스트 자체에 LLM 지시사항 포함
- 섹션별 특화된 검증 가이드
- 컨텍스트 인식 검증 프로세스

### 2. 적응형 실행 모드

**모드 선택**:

- **Interactive**: 섹션별 상세 검토
- **YOLO**: 전체 일괄 분석

**사용자 경험 최적화**:

- 시간 vs 정확성 트레이드오프
- 사용자 선호도에 따른 유연성

### 3. 종합적 보고서 생성

**최종 보고서 구조**:

```markdown
## PRD & EPIC VALIDATION SUMMARY

1. Executive Summary

   - Overall PRD completeness (percentage)
   - MVP scope appropriateness (Too Large/Just Right/Too Small)
   - Readiness for architecture phase (Ready/Nearly Ready/Not Ready)

2. Category Analysis Table

   - Status: PASS (90%+ complete), PARTIAL (60-89%), FAIL (<60%)
   - Critical Issues: Specific problems that block progress

3. Top Issues by Priority

   - BLOCKERS: Must fix before architect can proceed
   - HIGH: Should fix for quality
   - MEDIUM: Would improve clarity
   - LOW: Nice to have

4. MVP Scope Assessment
5. Technical Readiness
6. Recommendations
```

## 새로운 에이전트 구성에 대한 함의

### 1. 체크리스트 패턴 활용

**언제 체크리스트를 사용할까?**:

- 복잡한 검증이 필요한 도메인
- 다단계 프로세스가 있는 워크플로우
- 품질 보증이 중요한 영역
- 변경 관리가 빈번한 프로젝트

### 2. 브라운필드 vs 그린필드 고려

**에이전트 설계 시 고려사항**:

- 대상 프로젝트 유형 (신규 vs 기존)
- 레거시 시스템과의 통합 필요성
- 변경 관리의 복잡성

### 3. 임베디드 프롬프트 시스템

**구현 방법**:

```markdown
[[LLM: 섹션별 특화 지시사항

- 검증 포인트 명시
- 컨텍스트 제공
- 품질 기준 정의
- 실행 가이드라인]]
```

## 결론

PM 에이전트는 QA 에이전트와는 완전히 다른 접근법을 보여줍니다:

### 핵심 차이점

1. **체크리스트 중심 vs 태스크 중심**

   - PM: 포괄적 체크리스트로 문서 품질 검증
   - QA: 개별 태스크로 특정 분석 수행

2. **전략적 vs 전술적**

   - PM: 제품 전략과 사용자 가치에 집중
   - QA: 기술적 품질과 리스크에 집중

3. **문서 생성 vs 분석**

   - PM: PRD, 에픽, 스토리 생성이 주요 기능
   - QA: 기존 구현물의 분석과 검증이 주요 기능

4. **변경 관리 특화**
   - PM: 체계적인 변경 관리 프로세스
   - QA: 품질 관점에서의 변경 영향 평가

### 새로운 에이전트 설계 가이드라인

1. **도메인 특성 파악**: 분석 중심인가? 생성 중심인가?
2. **검증 메커니즘 선택**: 체크리스트인가? 개별 태스크인가?
3. **프로젝트 유형 고려**: 그린필드인가? 브라운필드인가?
4. **변경 관리 필요성**: 빈번한 변경이 예상되는가?

PM 에이전트의 체크리스트 시스템은 복잡한 검증 프로세스를 체계화하는 혁신적인 방법을 제시하며, 새로운 에이전트 설계 시 중요한 참고 모델이 됩니다.
