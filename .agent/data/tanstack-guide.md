# 범용 React Query API Hook 작성 가이드

이 가이드는 "API 리스트(엔드포인트/메서드/파라미터 등)를 주면 아래 방식으로 쿼리/뮤테이션 훅 및 쿼리키/옵션을 구성한다"는 원칙을 설명합니다. (도메인/엔티티/프로젝트 무관하게 적용 가능)

---

## 1. 쿼리키(queryKey) 패턴

- 모든 쿼리(조회)는 `쿼리키` 객체에서 관리 (예: queries.ts)
- 쿼리(조회)만 쿼리키로 관리, mutation은 쿼리키에 포함하지 않음
- 예시:

```ts
const 엔티티Queries = {
  all: () => ["todos"],
  lists: () => [...엔티티Queries.all(), "list"],
  list: (filters: string) =>
    queryOptions({
      queryKey: [...엔티티Queries.lists(), filters],
      queryFn: () => fetchTodos(filters),
    }),
  details: () => [...엔티티Queries.all(), "detail"],
  detail: (id: number) =>
    queryOptions({
      queryKey: [...엔티티Queries.details(), id],
      queryFn: () => fetchTodo(id),
      staleTime: 5000,
    }),
};
```

## 2. Query 작성법 (파일 분리)

- get API에 대한 쿼리 옵션은 queries.ts에서만 정의하고 custom hook을 생성하지 않음
- **페이지네이션이 있는 리스트 API는 infiniteQueryOptions를, 일반 단건 조회(get)는 queryOptions를 사용해야 함**
- 예시:

```ts
const 엔티티Queries = {
  all: () => ["todos"],
  lists: () => [...엔티티Queries.all(), "list"],
  // queries.ts
  // 페이지네이션 리스트
  list: (query) =>
    infiniteQueryOptions({
      queryKey: [...엔티티Queries키.lists(), query] as const,
      queryFn: async ({ pageParam }) => {
        return entityApi.entityList({ ...query, cursor: pageParam });
      },
      initialPageParam: "",
      getNextPageParam: (lastPage) => lastPage.nextCursor,
    }),
  details: () => [...엔티티Queries.all(), "detail"],
  detail: (id: number) =>
    queryOptions({
      queryKey: [...엔티티Queries.details(), id],
      queryFn: () => fetchTodo(id),
      staleTime: 5000,
    }),
};
```

## 3. Mutation Hook 작성법 (파일 분리)

- useMutation 사용, 성공 시 invalidate 등 후처리 필수
- useMutation에 제네릭 타입 명시하지 않음
- 각 mutation 훅은 별도 파일로 분리 (예: use-post-엔티티-mutation.ts 등)
- 예시:
- mutate request로 path, body를 전달 받도록함
- hook에서 props로 data를 받지 않음

  ```ts
  // use-post-엔티티-mutation.ts
  import { useMutation, useQueryClient } from "@tanstack/react-query";
  import { entityApi } from "...";
  import { 엔티티Queries키 } from "../queries";
  import { EntityCreateRequest } from "...";

  export const usePostEntityMutation = () => {
    const queryClient = useQueryClient();
    return useMutation({
      mutationFn: (body: EntityCreateRequest) => entityApi.entityCreate(body),
      onSuccess: () => {
        queryClient.invalidateQueries({ queryKey: 엔티티Queries키.all });
      },
    });
  };
  ```

## 4. 네이밍 규칙

- 뮤테이션: `usePost...Mutation`, `usePatch...Mutation`, `useDelete...Mutation` 등 HTTP 메서드에 맞게 작성
- 각 훅은 1파일 1훅 원칙으로 분리

## 5. 기타

- 파라미터/반환 타입은 API 자동 생성 타입(혹은 명확한 타입) 사용
- 쿼리키, 쿼리 옵션, 훅 모두 일관성 있게 작성
