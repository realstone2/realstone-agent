# TanStack Query Settings and Defaults

<!-- Powered by BMAD™ Core -->

## Basic Configuration

### Query Option Defaults

- **staleTime**: 5 minutes (300,000ms) - List queries
- **staleTime**: 10 minutes (600,000ms) - Detail queries
- **gcTime**: 10 minutes (600,000ms)
- **retry**: 3 times
- **refetchOnWindowFocus**: false

### Pagination Defaults

- **initialPageParam**: ""
- **cursor field name**: "cursor" or "nextCursor"
- **hasMore field name**: "hasMore" or "hasNext"
- **getNextPageParam**: Use `getNextPageParam2(lastPage.pagination)` helper function

## File Naming Conventions

### Query Files

- Format: `{entity-name}-queries.ts`
- Examples: `outfits-queries.ts`, `users-queries.ts`

### Mutation Files

- Format: `use-{method}-{entity-name}-mutation.ts`
- Examples: `use-post-outfit-mutation.ts`, `use-put-user-mutation.ts`

### Mock Data Files

- Format: `{entity-name}-mock.ts`
- Examples: `outfits-mock.ts`, `users-mock.ts`

## QueryKey Structure

```typescript
const {entity}Queries = {
  all: () => ['{entity}'],
  lists: () => [...{entity}Queries.all(), 'list'],
  details: () => [...{entity}Queries.all(), 'detail'],
  // Specific queries...
};
```

## Default Import Paths

```typescript
// API client
import { {entity} } from '@src/domains/app/swagger/acloset-api';


// Type definitions
import type { {TypeName} } from '@src/domains/app/swagger/generated/data-contracts';

// TanStack Query
import { queryOptions, infiniteQueryOptions, useMutation, useQueryClient } from '@tanstack/react-query';

// Pagination helper
import { getNextPageParam2 } from '@libs/react-query/utils';
```

## API Client Instance Management Rules

### 1. Default import from acloset-api.ts (Priority)

```typescript
import { myIdeasApi } from "@src/domains/app/swagger/acloset-api";

// Usage examples:
// - clothesApi, closetsApi, ideasApi, myIdeasApi, etc.
// - Use pre-generated instances for each domain
```

### 2. Create if not in acloset-api.ts

**Add instance directly to acloset-api.ts file:**

```typescript
// Add to @src/domains/app/swagger/acloset-api.ts
import { {EntityClass} } from './generated/{EntityClass}';

/**
 * {Entity} domain API service.
 */
export const {entity}Api = new {EntityClass}(httpClient);
```

**Then use standard import:**

```typescript
import { {entity}Api } from '@src/domains/app/swagger/acloset-api';
```

### API Instance Mapping Examples

- `my-ideas` → `myIdeasApi`
- `clothes` → `clothesApi`
- `closets` → `closetsApi`
- `ideas` → `ideasApi`

## Enhanced Mock Data Generation Rules (Type-Safe)

### Type-First Mock Generation Strategy

#### Priority 1: TypeScript Type Analysis (Highest Priority)

**Process:**

1. Extract response type name from `@response` annotation in API spec
2. Parse TypeScript type definition from data-contracts
3. Recursively analyze nested types and properties
4. Generate mock data matching exact type structure

**Type Analysis Rules:**

- `string` → Generate based on property name context
- `number` → Generate based on property name and constraints
- `boolean` → Generate based on property semantic meaning
- `Array<T>` → Generate 2-3 items of type T
- `object` → Recursively generate nested object properties
- `union` → Select first type from union for mock data
- `optional?` → Include optional properties with 80% probability

#### Priority 2: Property Name Heuristics (Fallback)

**String Types:**

- `id`: UUID format or prefix_number format
- `name`, `title`: Meaningful Korean names
- `description`: Detailed Korean descriptions
- `email`: Valid email format
- `url`, `imageUrl`: Example URLs
- `createdAt`, `updatedAt`: ISO date strings

**Number Types:**

- `count`, `itemCount`: Random between 1-100
- `price`: Prices in thousands units
- `age`: Between 20-60

**Boolean Types:**

- Set appropriate default values based on meaning

#### Priority 3: Type-based Defaults (Final Fallback)

- `string` → "샘플 텍스트"
- `number` → 42
- `boolean` → true
- `Array<T>` → [] (empty array if type unknown)
- `object` → {} (empty object if type unknown)

### Type-Safe Mock Output

**Generated Mock Structure:**

```typescript
// Type annotation included
export const mockEntityList: ResponseTypeName = { ... };

// Type-safe mock functions
export const entityMockApi = {
  methodName: (params: ParamsType): Promise<ResponseType> => { ... }
};
```

## Mock Data Application Rules

### Using Type-Safe Mock Data in Query Options

When mock data generation is selected:

```typescript
// Real API call (commented out)
// queryFn: async ({ pageParam }) => {
//   return {entity}Api.{functionName}({
//     ...query,
//     cursor: pageParam,
//   });
// },

// Type-safe mock data usage (development)
queryFn: async ({ pageParam }) => {
  // TODO: Remove mock data and connect real API
  // Using type-safe mock data: {ResponseTypeName}
  return Promise.resolve(mock{EntityPascal}List);
},
```

**Key Improvements:**

- Mock data includes exact TypeScript type annotation
- Generated data structure matches API response type
- Type safety maintained throughout development
- Easy transition from mock to real API
