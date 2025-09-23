# TanStack Query 설정 및 기본값

<!-- Powered by BMAD™ Core -->

## 기본 설정

### Query 옵션 기본값

- **staleTime**: 5분 (300,000ms) - 리스트 쿼리
- **staleTime**: 10분 (600,000ms) - 상세 쿼리
- **gcTime**: 10분 (600,000ms)
- **retry**: 3회
- **refetchOnWindowFocus**: false

### 페이지네이션 기본값

- **initialPageParam**: ""
- **cursor 필드명**: "cursor" 또는 "nextCursor"
- **hasMore 필드명**: "hasMore" 또는 "hasNext"

## 파일명 컨벤션

### Query 파일

- 형식: `{entity-name}-queries.ts`
- 예: `outfits-queries.ts`, `users-queries.ts`

### Mutation 파일

- 형식: `use-{method}-{entity-name}-mutation.ts`
- 예: `use-post-outfit-mutation.ts`, `use-put-user-mutation.ts`

### Mock 데이터 파일

- 형식: `{entity-name}-mock.ts`
- 예: `outfits-mock.ts`, `users-mock.ts`

## QueryKey 구조

```typescript
const {entity}_쿼리 = {
  all: () => ['{entity}'],
  lists: () => [...{entity}_쿼리.all(), 'list'],
  details: () => [...{entity}_쿼리.all(), 'detail'],
  // 구체적인 쿼리들...
};
```

## Import 경로 기본값

```typescript
// API 클라이언트
import { {entity}Api } from '../api/{entity}';

// 타입 정의
import { {TypeName} } from '../types/{entity}';

// TanStack Query
import { queryOptions, infiniteQueryOptions, useMutation, useQueryClient } from '@tanstack/react-query';
```

## Mock 데이터 생성 규칙

### 기본 Mock 값 매핑

#### 문자열 타입

- `id`: UUID 형식 또는 접두사\_숫자 형식
- `name`, `title`: 의미있는 한글명
- `description`: 상세한 한글 설명
- `email`: 실제 형식의 이메일
- `url`, `imageUrl`: 예시 URL
- `createdAt`, `updatedAt`: ISO 날짜 문자열

#### 숫자 타입

- `count`, `itemCount`: 1-100 사이 랜덤
- `price`: 1000단위 가격
- `age`: 20-60 사이

#### 불린 타입

- 의미에 따라 적절한 기본값 설정

### 배열 Mock 데이터

- 기본 2-3개 아이템 생성
- 다양성을 위해 각기 다른 값 설정

### 페이지네이션 Mock

- `nextCursor`: "cursor_page_2" 형식
- `hasMore`: true (테스트 편의성)
