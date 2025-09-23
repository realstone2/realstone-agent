# realStone(진짜돌) Dev Agent 🎯💻

TanStack Query 전문 개발 에이전트 - API 스펙으로부터 고품질 TanStack Query 코드와 Mock 데이터를 자동 생성합니다.

## 특징

- 🚀 **TanStack Query 컨벤션 준수**: @tanstack-guide.md 패턴 100% 준수
- 📝 **Mock 데이터 통합**: GET API 시 실용적인 Mock 데이터 자동 생성
- ⚡ **타입 안전성**: TypeScript 타입 정보 완전 지원
- 🎯 **점진적 구성**: 1단계 TanStack Query 생성에 특화, 향후 확장 예정
- 💎 **고품질 코드**: ESLint/Prettier 규칙 준수, 재사용 가능한 구조

## 파일 구조

```
agent/
├── .cursor/rules/bmad/
│   └── dev.mdc                     # IDE 규칙 파일
├── .bmad-core/agents/
│   └── dev.md                      # 에이전트 정의 파일
├── bmad/web-bundles/agents/
│   └── dev.txt                     # 웹 번들 (모든 의존성 포함)
├── data/
│   └── tanstack-preferences.md     # TanStack Query 설정 및 기본값
├── tasks/
│   └── tanstack-query-generation.md # 메인 코드 생성 태스크
├── templates/
│   ├── tanstack-query-tmpl.yaml    # Query 옵션 템플릿
│   ├── tanstack-mutation-tmpl.yaml # Mutation 훅 템플릿
│   └── mock-data-tmpl.yaml         # Mock 데이터 템플릿
└── README.md                       # 이 파일
```

## 사용 방법

### 1. 에이전트 활성화

```
@dev
```

### 2. 명령어 사용

#### TanStack Query 코드 생성

```
tanstack-query-gen {API 스펙}
```

**API 스펙 예시:**

```typescript
/**
 * @description 사용자의 코디 북마크 컬렉션 목록을 조회합니다.
 * @tags Outfit
 * @name OutfitsBookmarkCollectionList
 * @summary ListOutfitBookmarkCollections - 코디 북마크 컬렉션 목록 조회 ✅
 * @request GET:/outfits-bookmark-collection
 * @secure
 * @response `200` `DomainListOutfitBookmarkCollectionsResponse` 코디 북마크 컬렉션 목록 조회 반환값
 */
outfitsBookmarkCollectionList = (
  query: OutfitsBookmarkCollectionListParams,
  params: RequestParams = {}
) =>
  this.http.request<DomainListOutfitBookmarkCollectionsResponse, any>({
    path: `/outfits-bookmark-collection`,
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

### 3. 생성되는 파일

#### GET API (Query)

- `{entity-name}-queries.ts`: Query Options 파일
- `{entity-name}-mock.ts`: Mock 데이터 파일 (선택사항)

#### POST/PUT/DELETE API (Mutation)

- `use-{method}-{entity-name}-mutation.ts`: Mutation Hook 파일

### 4. 사용 예시

#### 생성된 Query 사용

```typescript
import { useSuspenseQuery } from "@tanstack/react-query";
import { outfitsQueries } from "../hooks/api/outfits-queries";

function OutfitsList() {
  const { data } = useSuspenseQuery(
    outfitsQueries.bookmarkCollectionList({ limit: 20 })
  );

  return (
    <div>
      {data.data.map((collection) => (
        <div key={collection.id}>{collection.name}</div>
      ))}
    </div>
  );
}
```

#### 생성된 Mutation 사용

```typescript
import { usePostOutfitMutation } from "../hooks/api/use-post-outfit-mutation";

function CreateOutfit() {
  const mutation = usePostOutfitMutation();

  const handleCreate = () => {
    mutation.mutate({
      name: "새로운 코디",
      description: "멋진 코디입니다",
    });
  };

  return <button onClick={handleCreate}>코디 생성</button>;
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

### Mock 데이터 생성

- ✅ 타입 기반 실제적인 데이터 생성
- ✅ 한글 문맥에 맞는 Mock 데이터
- ✅ 페이지네이션 지원 Mock 구조
- ✅ Type-safe Mock API 함수

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
// API 클라이언트
import { {entity}Api } from '../api/{entity}';

// 타입 정의
import { {TypeName} } from '../types/{entity}';

// TanStack Query
import { queryOptions, infiniteQueryOptions, useMutation, useQueryClient } from '@tanstack/react-query';
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

## 개발자 정보

- **Agent Name**: realStone(진짜돌)
- **Agent ID**: dev
- **Specialization**: TanStack Query Code Generation
- **Framework**: BMAD™ Core
- **Version**: 1.0 (1단계)

---

**TanStack Query 코드 생성의 새로운 기준, realStone과 함께하세요!** 🚀
