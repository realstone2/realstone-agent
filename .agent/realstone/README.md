# realStone(진짜돌) - TanStack Query Dev Agent 🎯💻

TanStack Query 전문 개발 에이전트 - API 스펙으로부터 고품질 TanStack Query 코드와 Mock 데이터를 자동 생성합니다.

## 특징

- 🚀 **TanStack Query 컨벤션 준수**: @tanstack-guide.md 패턴 100% 준수
- 📝 **Mock 데이터 통합**: GET API 시 실용적인 Mock 데이터 자동 생성
- ⚡ **타입 안전성**: TypeScript 타입 정보 완전 지원
- 🎯 **점진적 구성**: 1단계 TanStack Query 생성에 특화, 향후 확장 예정
- 💎 **고품질 코드**: ESLint/Prettier 규칙 준수, 재사용 가능한 구조

## 에이전트 정보

- **Agent Name**: realStone(진짜돌)
- **Agent ID**: realstone
- **Title**: Frontend Developer - TanStack Query Specialist
- **Icon**: 💻
- **Framework**: BMAD™ Core
- **Version**: 1.0

## 파일 구조

```text
.agent/
├── .core/agents/
│   └── realstone.md                # 핵심 에이전트 정의 파일
├── data/
│   ├── tanstack-preferences.md     # TanStack Query 설정 및 기본값
│   └── tanstack-guide.md          # TanStack Query 컨벤션 가이드
├── tasks/
│   └── tanstack-query-generation.md # 메인 코드 생성 태스크
├── templates/
│   ├── tanstack-query-tmpl.yaml    # Query 옵션 템플릿
│   ├── tanstack-mutation-tmpl.yaml # Mutation 훅 템플릿
│   └── mock-data-tmpl.yaml         # Mock 데이터 템플릿
└── README.md                       # 에이전트 사용 가이드
```

## 사용 방법

### 1. 에이전트 활성화

```bash
@realstone
```

### 2. 명령어 사용

#### TanStack Query 코드 생성

```bash
tanstack-query-gen {API 스펙}
```

**API 스펙 예시:**

```typescript
/**
 * @description 사용자의 아이디어 목록을 조회합니다.
 * @tags MyIdeas
 * @name MyIdeasList
 * @summary List My Ideas - 내 아이디어 목록 조회 ✅
 * @request GET:/my-ideas
 * @secure
 * @response `200` `DomainMyIdeasListResponse` 내 아이디어 목록 조회 반환값
 */
myIdeasList = (query: MyIdeasListParams, params: RequestParams = {}) =>
  this.http.request<DomainMyIdeasListResponse, any>({
    path: `/my-ideas`,
    method: "GET",
    query: query,
    secure: true,
    format: "json",
    ...params,
  });
```

#### 기타 명령어

- `explain`: 생성된 코드와 패턴 설명
- `help`: 명령어 목록 표시
- `exit`: 에이전트 모드 종료

### 3. 사용자 입력 수집

#### 출력 디렉토리 설정 (필수)

모든 API에 대해 코드 생성 경로를 지정해야 합니다:

```text
📂 코드를 생성할 디렉토리 경로를 입력해주세요:
(예: src/hooks/api, hooks/api, etc.)
```

#### Mock 데이터 생성 선택 (GET API만)

GET API인 경우 Mock 데이터 생성 여부를 선택할 수 있습니다:

```text
📝 Mock 데이터도 함께 생성하시겠습니까?

1. Yes - Generate Query + Mock data
2. No - Generate Query only

선택하세요 (1 또는 2):
```

### 4. 생성되는 파일

#### GET API (Query)

- `{entity-name}-queries.ts`: Query Options 파일
- `{entity-name}-mock.ts`: Mock 데이터 파일 (선택사항)

#### POST/PUT/DELETE API (Mutation)

- `use-{method}-{entity-name}-mutation.ts`: Mutation Hook 파일

### 5. 사용 예시

#### 생성된 Query 사용

```typescript
import { useSuspenseQuery } from "@tanstack/react-query";
import { myIdeasQueries } from "../hooks/api/my-ideas-queries";

function MyIdeasList() {
  const { data } = useSuspenseQuery(myIdeasQueries.list({ limit: 20 }));

  return (
    <div>
      {data.data.map((idea) => (
        <div key={idea.id}>{idea.title}</div>
      ))}
    </div>
  );
}
```

#### 생성된 Mutation 사용

```typescript
import { usePostMyIdeasMutation } from "../hooks/api/use-post-my-ideas-mutation";

function CreateIdea() {
  const mutation = usePostMyIdeasMutation();

  const handleCreate = () => {
    mutation.mutate({
      title: "새로운 아이디어",
      description: "멋진 아이디어입니다",
    });
  };

  return <button onClick={handleCreate}>아이디어 생성</button>;
}
```

## 지원하는 기능

### HTTP 메서드

- ✅ GET (Query Options + Mock 데이터)
- ✅ POST (Mutation Hook)
- ✅ PUT (Mutation Hook)
- ✅ DELETE (Mutation Hook)
- ✅ PATCH (Mutation Hook)

### 페이지네이션

- ✅ Cursor-based 페이지네이션 (`infiniteQueryOptions`)
- ✅ 단건 조회 (`queryOptions`)
- ✅ 적절한 `staleTime` 및 `gcTime` 자동 설정
- ✅ `getNextPageParam2` 헬퍼 함수 활용

### Mock 데이터 생성 (Enhanced Type-Safe)

- ✅ **API 반환 타입 분석**: `@response` 주석에서 실제 TypeScript 타입 추출
- ✅ **타입 기반 정확한 생성**: 실제 API 응답 구조와 100% 일치하는 Mock 데이터
- ✅ **TypeScript 타입 주석**: Mock 데이터에 정확한 타입 정보 포함
- ✅ **네스티드 타입 지원**: 복잡한 중첩 객체 구조도 정확히 생성
- ✅ **한글 컨텍스트**: 의미에 맞는 한국어 Mock 데이터
- ✅ **페이지네이션 지원**: 실제 페이지네이션 구조 분석 및 Mock 생성
- ✅ **Type-safe Mock API**: 타입 안전한 Mock 함수 자동 생성

## 컨벤션

### 파일명 규칙

- Query: `{entity-name}-queries.ts`
- Mutation: `use-{method}-{entity-name}-mutation.ts`
- Mock: `{entity-name}-mock.ts`

### QueryKey 구조

```typescript
const {entity}Queries = {
  all: () => ['{entity}'],
  lists: () => [...{entity}Queries.all(), 'list'],
  details: () => [...{entity}Queries.all(), 'detail'],
  // 구체적인 쿼리들...
};
```

### Import 경로

```typescript
// API 클라이언트 (중앙 관리)
import { {entity}Api } from '@src/domains/app/swagger/acloset-api';

// 타입 정의
import type { {TypeName} } from '@src/domains/app/swagger/generated/data-contracts';

// TanStack Query
import { queryOptions, infiniteQueryOptions, useMutation, useQueryClient } from '@tanstack/react-query';

// 페이지네이션 헬퍼
import { getNextPageParam2 } from '@libs/react-query/utils';
```

## 파일 분리 원칙 (엄격 준수)

### 허용된 파일만 생성

```yaml
허용된 파일:
  GET_API:
    - "{entity-name}-queries.ts" # Query Options (필수)
    - "{entity-name}-mock.ts" # Mock 데이터 (사용자 선택 시만)

  POST_PUT_DELETE_API:
    - "use-{method}-{entity-name}-mutation.ts" # Mutation Hook (필수)

금지된 파일:
  - "use-{entity-name}-query.ts" # Hook 파일 (Query Options와 분리됨)
  - "{entity-name}-usage-example.tsx" # 사용 예시 컴포넌트
  - "{entity-name}-helpers.ts" # 헬퍼 함수 파일
  - "{entity-name}-types.ts" # 추가 타입 파일
  - "*.test.ts" # 테스트 파일
  - "*.stories.ts" # 스토리북 파일
```

### 핵심 규칙

- **Query 파일**: Query Options만 포함, Hook 함수 금지
- **Hook 파일**: 개별 Hook만 포함, Query Options 금지
- **Mixed 금지**: 하나의 파일에 여러 책임 포함 금지

## API 클라이언트 관리

### 1. acloset-api.ts에서 Import (우선순위)

```typescript
import { myIdeasApi } from "@src/domains/app/swagger/acloset-api";

// 사용 예시:
// - clothesApi, closetsApi, ideasApi, myIdeasApi 등
// - 각 도메인별 사전 생성된 인스턴스 사용
```

### 2. 인스턴스 매핑 예시

- `my-ideas` → `myIdeasApi`
- `clothes` → `clothesApi`
- `closets` → `closetsApi`
- `ideas` → `ideasApi`

## Enhanced Mock 데이터 생성 규칙 (Type-Safe)

### 타입 우선 생성 전략

#### 1순위: TypeScript 타입 분석 (최우선)

**프로세스:**

1. API 스펙의 `@response` 주석에서 응답 타입명 추출
2. data-contracts에서 TypeScript 타입 정의 파싱
3. 중첩 타입과 속성들을 재귀적으로 분석
4. 정확한 타입 구조에 맞는 Mock 데이터 생성

**타입 분석 규칙:**

- `string` → 속성명 컨텍스트에 기반해 생성
- `number` → 속성명과 제약조건에 기반해 생성
- `boolean` → 속성의 의미적 맥락에 기반해 생성
- `Array<T>` → T 타입의 2-3개 아이템 생성
- `object` → 중첩 객체 속성들을 재귀적으로 생성
- `union` → union의 첫 번째 타입 선택하여 Mock 생성
- `optional?` → 선택적 속성은 80% 확률로 포함

#### 2순위: 속성명 휴리스틱 (폴백)

**문자열 타입:**

- `id`: UUID 형식 또는 접두사\_숫자 형식
- `name`, `title`: 의미있는 한글명
- `description`: 상세한 한글 설명
- `email`: 실제 형식의 이메일
- `url`, `imageUrl`: 예시 URL
- `createdAt`, `updatedAt`: ISO 날짜 문자열

**숫자 타입:**

- `count`, `itemCount`: 1-100 사이 랜덤
- `price`: 1000단위 가격
- `age`: 20-60 사이

**불린 타입:**

- 의미에 따라 적절한 기본값 설정

#### 3순위: 타입 기반 기본값 (최종 폴백)

- `string` → "샘플 텍스트"
- `number` → 42
- `boolean` → true
- `Array<T>` → [] (타입을 모르는 경우 빈 배열)
- `object` → {} (타입을 모르는 경우 빈 객체)

### 타입 안전한 Mock 출력

**생성되는 Mock 구조:**

```typescript
// 타입 주석 포함
export const mockEntityList: ResponseTypeName = { ... };

// 타입 안전한 Mock 함수들
export const entityMockApi = {
  methodName: (params: ParamsType): Promise<ResponseType> => { ... }
};
```

## 향후 확장 계획

### 2단계 (예정)

- 컴포넌트 자동 생성
- 테스트 코드 생성
- 스토리북 스토리 생성

### 3단계 (예정)

- API 문서 자동 생성
- 에러 핸들링 패턴 생성
- 상태 관리 패턴 추가

---

**TanStack Query 코드 생성의 새로운 기준, realStone과 함께하세요!** 🚀

<!-- Powered by BMAD™ Core -->
