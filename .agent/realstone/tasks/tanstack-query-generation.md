# tanstack-query-generation

<!-- Powered by BMAD™ Core -->

Automatically generates query and mutation hooks that conform to TanStack Query conventions by analyzing API specifications.

## Inputs

required:

- api_spec: "API specification code block (TypeScript format)"
- entity_name: "Entity name (e.g., outfits, users, products)"
- output_directory: "Output directory path (e.g., src/hooks/api)"

optional:

- file_naming_style: "File naming convention (kebab-case|camelCase, default: kebab-case)"

## Purpose

Following TanStack Query conventions (@tanstack-guide.md):

1. GET API → Generate Query Options
2. POST/PUT/DELETE API → Generate Mutation Hooks
3. Generate appropriate queryKey structure
4. Apply file separation rules

## Process

### Step 1: API Specification Analysis

1. **HTTP Method Extraction**

   - Find `@request {METHOD}:` pattern using regex
   - Supported methods: GET, POST, PUT, DELETE, PATCH

2. **Endpoint Path Parsing**

   - Identify path parameters (`{id}`, `{userId}`, etc.)
   - Extract query parameter types

3. **Function Name and Parameter Analysis**

   - Extract actions from function names (list, detail, create, update, delete)
   - Verify parameter types and required/optional status

4. **Return Type Analysis**

   - Extract response types from `@response` annotation
   - Parse TypeScript type definition to understand structure
   - Check pagination structure (cursor, hasMore, etc.)
   - Distinguish between single/multiple data
   - Analyze nested object properties and their types

5. **Pagination Detection**
   - Check if function name contains "List"
   - Check if parameters include cursor/offset
   - Check if response type includes pagination fields

### Step 2: User Input Collection - Mandatory Steps

**🚨 Important**: Must collect the following required inputs from user:

#### 2.1 Output Directory (All APIs)

```
📂 코드를 생성할 디렉토리 경로를 입력해주세요:
(예: src/hooks/api, hooks/api, etc.)

경로:
```

#### 2.2 Mock Data Selection (GET API only)

**GET API인 경우만** 다음 질문:

```
📝 Mock 데이터도 함께 생성하시겠습니까?

1. Yes - Generate Query + Mock data
2. No - Generate Query only

선택하세요 (1 또는 2):
```

**Implementation Rules**:

- ✅ **Required**: Must collect output directory for all APIs
- ✅ **Required**: Must wait for user response on mock data (GET API only)
- ✅ **Required**: Wait until receiving valid inputs
- ❌ **Forbidden**: Auto-generate mock data without user selection
- ❌ **Forbidden**: Use default directory without user input

**For POST/PUT/DELETE APIs**: Only collect output directory (Skip mock data question)

### Step 3: Code Generation Strategy

#### GET API Processing

1. **With Pagination** → Use `infiniteQueryOptions`
2. **Single Item Query** → Use `queryOptions`
3. **When Mock Data Selected** → Generate additional mock file

#### POST/PUT/DELETE API Processing

1. **Generate Mutation Hook**
2. **Include appropriate invalidation logic**
3. **Provide onSuccess, onError handler templates**

### Step 4: Variable Extraction and Template Data Preparation

#### Common Variables

```typescript
{
  entity_name: string; // Entity name (kebab-case)
  entity_pascal: string; // Entity name (PascalCase)
  entity_camel: string; // Entity name (camelCase)
  api_function_name: string; // API function name
  output_directory: string; // Output directory
  api_path: string; // API import path
  types_path: string; // Type import path
}
```

#### Query-specific Variables

```typescript
{
  is_infinite: boolean; // Whether infinite scroll
  query_params_type: string; // Query parameter type
  response_type: string; // Response type
  stale_time: number; // staleTime setting
  initial_page_param: string; // Initial page parameter
  next_cursor_field: string; // Next cursor field name
}
```

#### Mock Data-specific Variables (Enhanced)

```typescript
{
  // Type Analysis
  response_type_name: string;    // e.g., "DomainMyIdeasListResponse"
  type_definition: TypeStructure; // Parsed TypeScript type structure

  // Generated Mock Data
  mock_items_count: number;      // Mock item count (2-3 items)
  mock_objects: object[];        // Type-safe mock data objects
  mock_structure: object;        // Complete mock response structure

  // Pagination (if applicable)
  has_pagination: boolean;       // Whether pagination included
  pagination_fields: string[];   // e.g., ["nextCursor", "hasMore"]
  next_cursor_mock: string;      // Mock next cursor value
  has_more_mock: boolean;        // Mock hasMore value

  // Type Import Information
  required_imports: string[];    // Additional types needed for mock
  mock_function_signature: string; // Type-safe mock function signature
}
```

#### Mutation-specific Variables

```typescript
{
  method_lower: string; // HTTP method (lowercase)
  method_pascal: string; // HTTP method (PascalCase)
  request_type: string; // Request type
  mutation_params: string; // Mutation parameters
  api_call: string; // API call code
}
```

### Step 5: Mock Data Generation Logic (Type-Safe Approach)

#### 1. TypeScript Type Analysis

**New Approach**: Parse actual TypeScript response types from `@response` annotations:

```typescript
// Extract from API spec:
// @response `200` `DomainMyIdeasListResponse` 내 아이디어 목록 조회 반환값

// Steps:
1. Extract response type name: "DomainMyIdeasListResponse"
2. Look up type definition in data-contracts
3. Analyze type structure recursively
4. Generate mock data matching exact structure
```

#### 2. Type-based Mock Data Generation

**Priority Order for Mock Value Generation:**

1. **Actual Type Structure** (Highest Priority)

   - Parse TypeScript interface/type definition
   - Generate mock data matching exact property types
   - Preserve optional/required property distinction

2. **Property Name Heuristics** (Fallback)

   - `id` → UUID format or "prefix_1" format
   - `name`, `title` → Meaningful Korean names
   - `description` → Detailed Korean descriptions
   - `email` → "user@example.com" format
   - `*Url`, `*url` → "https://example.com/..." format
   - `*At` (dates) → ISO date strings

3. **Type-based Defaults** (Final Fallback)
   - `string` → "샘플 텍스트"
   - `number` → Random between 1-100
   - `boolean` → true
   - `Array<T>` → Generate 2-3 items of type T

#### 3. Enhanced Type Analysis Process

```typescript
interface MockGenerationContext {
  responseTypeName: string; // e.g., "DomainMyIdeasListResponse"
  typeDefinition: TypeStructure; // Parsed from data-contracts
  isPaginated: boolean; // Has pagination fields
  isArray: boolean; // Is array response
  nestedTypes: string[]; // Referenced types to resolve
}

interface TypeStructure {
  properties: Property[];
  required: string[];
  extends?: string;
}

interface Property {
  name: string;
  type: "string" | "number" | "boolean" | "object" | "array";
  arrayItemType?: string;
  objectType?: string;
  optional: boolean;
  description?: string;
}
```

#### 4. Mock Data Generation Strategy

```typescript
// Example: Generate mock for DomainMyIdeasListResponse
function generateMockFromType(typeName: string): any {
  // 1. Look up type definition
  const typeDefinition = getTypeDefinition(typeName);

  // 2. Generate mock object
  const mockObject = {};

  // 3. For each property, generate appropriate mock value
  typeDefinition.properties.forEach((prop) => {
    mockObject[prop.name] = generateMockValue(prop);
  });

  return mockObject;
}

function generateMockValue(property: Property): any {
  // Type-first approach
  switch (property.type) {
    case "array":
      return generateMockArray(property.arrayItemType);
    case "object":
      return generateMockFromType(property.objectType);
    case "string":
      return generateStringMock(property.name);
    case "number":
      return generateNumberMock(property.name);
    case "boolean":
      return generateBooleanMock(property.name);
  }
}
```

### Step 6: File Generation and Code Writing

#### Query File Generation

1. Load template: `tanstack-query-tmpl.yaml`
2. Variable substitution and code generation
3. **Template Compliance Validation** (Step 6.1)
4. Save file: `{entity-name}-queries.ts`

#### Mutation File Generation

1. Load template: `tanstack-mutation-tmpl.yaml`
2. Variable substitution and code generation
3. **Template Compliance Validation** (Step 6.1)
4. Save file: `use-{method}-{entity-name}-mutation.ts`

#### Mock Data File Generation (Only when user selected)

1. Load template: `mock-data-tmpl.yaml`
2. Generate mock data objects
3. **Template Compliance Validation** (Step 6.1)
4. Save file: `{entity-name}-mock.ts`

### Step 6.1: Template Compliance Validation (Required)

**🚨 Important**: Must perform the following validation after generating each file:

#### 1. Import Validation

```typescript
// ✅ Only allowed imports
import { queryOptions, infiniteQueryOptions } from "@tanstack/react-query";
import { useMutation, useQueryClient } from "@tanstack/react-query";
import { myIdeasApi } from "@src/domains/app/swagger/acloset-api"; // Always from acloset-api

// ❌ Forbidden import detection
import { createQueryKeys } from "@lukemorales/query-key-factory"; // External library
import { someHelper } from "./helpers"; // Undefined helper
import { MyIdeas } from "@src/domains/app/swagger/generated/MyIdeas"; // Direct class import forbidden
```

#### 2. File Structure Validation

```typescript
// ✅ Correct structure (tanstack-query-tmpl.yaml compliant)
const {entity}Queries = {
  all: () => ['{entity}'],
  lists: () => [...{entity}Queries.all(), 'list'],
  // Only Query Options included
};

// ❌ Incorrect structure
export const useEntityQuery = () => { ... };  // Hook functions forbidden
```

#### 3. File Separation Validation

- **Query Files**: Only Query Options included, Hook functions forbidden
- **Hook Files**: Only individual Hooks included, Query Options forbidden
- **Mixed Forbidden**: Multiple responsibilities in one file forbidden

#### 4. Error Handling for Validation Failures

```
❌ **Template Compliance Validation Failed**

Issue: {specific_violation}

Examples:
- Undefined library '{library_name}' usage detected
- Hook functions included in Query Options file
- File structure differs from template

Solution: Use only patterns defined in tanstack-query-tmpl.yaml.
```

### Step 7: Result Summary and Guidance

#### File Separation Rules (Mandatory Compliance)

**🚨 Important**: Must **strictly** follow these file separation rules:

```
📁 Generated File Structure:
├── {entity-name}-queries.ts           # Query Options only (query keys + queryOptions)
├── use-{method}-{entity-name}-mutation.ts  # Individual Mutation Hook
└── {entity-name}-mock.ts              # Mock data (optional)

❌ Prohibitions:
- Mixing Query Options + Hook functions in one file
- Combining multiple Hooks in one file
- Creating usage example component files
- Creating helper function files
```

#### Success Message Template

````
✅ **TanStack Query Code Generation Complete!**

📁 **Generated Files:**
- `{output_directory}/{entity-name}-queries.ts` - Query Options (query keys + queryOptions only)
{mock_file_message}
{mutation_file_message}

🔧 **File Separation Compliance:**
- ✅ Complete separation of Query Options and Hook functions
- ✅ 100% template pattern compliance
- ✅ Single responsibility principle applied

📖 **Usage:**
```typescript
// 1. Import from Query Options file
import { {entity_camel}Queries } from './{entity-name}-queries';

// 2. Use in component
const { data } = useSuspenseQuery({entity_camel}Queries.{method_name}(params));

// 3. Use Mutation (if applicable)
import { use{Method}{Entity}Mutation } from './use-{method}-{entity-name}-mutation';
const mutation = use{Method}{Entity}Mutation();
````

🚀 **Next Steps:**

1. Copy generated files to your project
2. Verify and adjust import paths
3. Verify API client connections

❌ **Warnings:**

- Adding Hook functions to Query Options files is forbidden
- Creating additional files outside templates is forbidden
- Importing external libraries is forbidden

## Output Restriction Rules (Required)

### Generate Only Allowed Output Files

**🚨 Important**: Only the following files are allowed to be generated:

```yaml
Allowed Files:
  GET_API:
    - "{entity-name}-queries.ts" # Query Options (required)
    - "{entity-name}-mock.ts" # Mock data (only when user selected)

  POST_PUT_DELETE_API:
    - "use-{method}-{entity-name}-mutation.ts" # Mutation Hook (required)

Forbidden Files:
  - "use-{entity-name}-query.ts" # Hook files (separated from Query Options)
  - "{entity-name}-usage-example.tsx" # Usage example components
  - "{entity-name}-helpers.ts" # Helper function files
  - "{entity-name}-types.ts" # Additional type files
  - "*.test.ts" # Test files
  - "*.stories.ts" # Storybook files
  - Any other files not defined in templates
```

### File Generation Validation Steps

Perform the following validation before generating each file:

```typescript
// 1. File name validation
function validateFileName(fileName: string, apiMethod: string): boolean {
  const allowedPatterns = {
    GET: [/^.+-queries\.ts$/, /^.+-mock\.ts$/],
    POST: [/^use-post-.+-mutation\.ts$/],
    PUT: [/^use-put-.+-mutation\.ts$/],
    DELETE: [/^use-delete-.+-mutation\.ts$/],
    PATCH: [/^use-patch-.+-mutation\.ts$/],
  };

  return allowedPatterns[apiMethod]?.some((pattern) => pattern.test(fileName));
}

// 2. Content validation
function validateFileContent(content: string, fileType: string): boolean {
  const rules = {
    queries: {
      required: ["Queries", "queryOptions", "infiniteQueryOptions"],
      forbidden: ["useMutation", "export const use", "React.FC"],
    },
    mutation: {
      required: ["useMutation", "useQueryClient"],
      forbidden: ["Queries = {", "queryOptions", "React.FC"],
    },
    mock: {
      required: ["mock", "export const"],
      forbidden: ["useMutation", "Queries = {", "React.FC"],
    },
  };

  // Validation logic implementation
}
```

## Error Handling

### Unsupported API Specifications

- When HTTP method cannot be found
- When required type information is insufficient
- When function signature is invalid

### File Generation Violation Errors

```
❌ **File Generation Rule Violation**

Issue: Attempted to generate unauthorized file - "{fileName}"

Allowed Files:
- GET API: {entity-name}-queries.ts, {entity-name}-mock.ts (optional)
- POST/PUT/DELETE: use-{method}-{entity-name}-mutation.ts

Forbidden Files:
- Usage example components (*.tsx)
- Helper function files (*-helpers.ts)
- Test files (*.test.ts)
- Other files outside templates

Solution: Generate only files defined in templates.
```

### API Specification Analysis Failure Errors

````
❌ **API Specification Analysis Failed**

Issue: {error_description}

Solution:
1. Verify API specification follows this format:
   ```typescript
   /**
    * @request {METHOD}:{path}
    */
   functionName = (params) => this.http.request<ResponseType>({...});
````

2. Supported HTTP methods: GET, POST, PUT, DELETE, PATCH
3. Verify function names and parameter types are clearly defined

다시 시도해주세요.

```

### Missing API Instance Errors

```

❌ **API Instance Not Found**

Issue: The API instance '{entityApi}' is not available in acloset-api.ts

Solution:

1. Add the missing instance to @src/domains/app/swagger/acloset-api.ts:

   ```typescript
   import { {EntityClass} } from './generated/{EntityClass}';

   /**
    * {Entity} 도메인 API 서비스.
    */
   export const {entity}Api = new {EntityClass}(httpClient);
   ```

2. Then use the standard import:
   ```typescript
   import { {entity}Api } from '@src/domains/app/swagger/acloset-api';
   ```

Note: All API instances must be centrally managed in acloset-api.ts

```

## Quality Assurance

### Generated Code Validation Items
1. **Syntax Check**: No TypeScript compilation errors
2. **Convention Compliance**: Follow @tanstack-guide.md patterns
3. **Type Safety**: All type information accurately mapped
4. **Import Paths**: Relative path accuracy
5. **QueryKey Structure**: Hierarchical structure and invalidation strategy

### Code Quality Standards
- ESLint/Prettier rule compliance
- Naming convention consistency
- Comments and documentation
- Reusable structure
```
