# JavaScript/TypeScript Coding Standards

Standards and conventions for JavaScript and TypeScript projects in the ecosystem.

## Toolchain

- **Node.js Version:** 18+ LTS
- **Package Manager:** pnpm (preferred) or npm
- **Formatter:** Prettier
- **Linter:** ESLint with TypeScript plugin
- **Build Tool:** Vite or esbuild

## TypeScript Preference

All new JavaScript projects should use TypeScript. Benefits:
- Catch errors at compile time
- Better IDE support
- Self-documenting code
- Easier refactoring

## Project Structure

```
project/
├── package.json
├── tsconfig.json        # TypeScript configuration
├── .prettierrc          # Prettier configuration
├── .eslintrc.js         # ESLint configuration
├── src/
│   ├── index.ts         # Entry point
│   └── modules/
├── tests/
│   └── *.test.ts
├── dist/                # Build output (gitignored)
├── README.md
├── LICENSE-MIT
├── LICENSE-APACHE
└── .github/
    └── workflows/
        └── ci.yml
```

## Naming Conventions

| Item | Convention | Example |
|------|------------|---------|
| Files | kebab-case | `my-module.ts` |
| Classes | PascalCase | `MyClass` |
| Interfaces | PascalCase (no I prefix) | `UserConfig` |
| Functions | camelCase | `myFunction` |
| Constants | SCREAMING_SNAKE_CASE | `MAX_SIZE` |
| Variables | camelCase | `myVariable` |
| React Components | PascalCase | `MyComponent.tsx` |

## TypeScript Configuration

### Strict Mode Required

```json
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  }
}
```

## Type Definitions

### Prefer Interfaces for Objects

```typescript
// Good
interface UserConfig {
  name: string;
  email: string;
  settings?: Settings;
}

// Use type for unions/intersections
type Status = "pending" | "active" | "completed";
type ExtendedConfig = UserConfig & { extra: string };
```

### Avoid `any`

```typescript
// Bad
function process(data: any): any {
  return data.value;
}

// Good
function process<T extends { value: unknown }>(data: T): T["value"] {
  return data.value;
}
```

## Error Handling

### Custom Error Classes

```typescript
export class AppError extends Error {
  constructor(
    message: string,
    public code: string,
    public statusCode: number = 500
  ) {
    super(message);
    this.name = "AppError";
  }
}

export class ValidationError extends AppError {
  constructor(message: string) {
    super(message, "VALIDATION_ERROR", 400);
  }
}
```

### Result Type Pattern

```typescript
type Result<T, E = Error> =
  | { success: true; data: T }
  | { success: false; error: E };

function parseConfig(input: string): Result<Config> {
  try {
    const config = JSON.parse(input);
    return { success: true, data: config };
  } catch (e) {
    return { success: false, error: e as Error };
  }
}
```

## Documentation

### JSDoc for Public APIs

```typescript
/**
 * Fetches user data from the API.
 *
 * @param userId - The unique identifier of the user
 * @param options - Optional fetch configuration
 * @returns Promise resolving to user data
 * @throws {ApiError} When the API request fails
 *
 * @example
 * ```typescript
 * const user = await fetchUser("123");
 * console.log(user.name);
 * ```
 */
export async function fetchUser(
  userId: string,
  options?: FetchOptions
): Promise<User> {
  // ...
}
```

## Testing

### Use Vitest

```typescript
import { describe, it, expect, vi } from "vitest";

describe("MyFunction", () => {
  it("should process valid input", () => {
    const result = myFunction("input");
    expect(result).toBe("expected");
  });

  it("should throw on invalid input", () => {
    expect(() => myFunction("")).toThrow(ValidationError);
  });

  it("should call external service", async () => {
    const mockFetch = vi.fn().mockResolvedValue({ data: "test" });
    const result = await fetchData(mockFetch);
    expect(mockFetch).toHaveBeenCalledOnce();
  });
});
```

### Coverage Target

- Minimum: 80%
- Target: 90%+

## Dependencies

### package.json Standards

```json
{
  "name": "@ecosystem/package",
  "version": "0.1.0",
  "type": "module",
  "engines": {
    "node": ">=18"
  },
  "exports": {
    ".": {
      "import": "./dist/index.js",
      "types": "./dist/index.d.ts"
    }
  }
}
```

### Common Dependencies

| Purpose | Package |
|---------|---------|
| HTTP Client | `ky` or native fetch |
| Validation | `zod` |
| CLI | `commander` |
| Testing | `vitest` |
| Build | `tsup` or `vite` |

## CI/CD Requirements

```yaml
# .github/workflows/ci.yml
- pnpm lint
- pnpm typecheck
- pnpm test --coverage
- pnpm build
```

## Async Guidelines

### Always Use async/await

```typescript
// Good
async function fetchData(): Promise<Data> {
  const response = await fetch(url);
  return response.json();
}

// Avoid raw promises when possible
function fetchData(): Promise<Data> {
  return fetch(url).then((r) => r.json());
}
```

### Handle Concurrent Operations

```typescript
// Parallel execution
const [users, posts] = await Promise.all([
  fetchUsers(),
  fetchPosts(),
]);

// Sequential with error handling
const results = await Promise.allSettled([
  riskyOperation1(),
  riskyOperation2(),
]);
```
