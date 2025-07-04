# TypeScript Standards

## General Principles

- Use TypeScript for all code in the project
- Aim for strict type safety across the entire codebase
- Prefer explicit types over implicit `any` or type inference when the intent isn't clear

## Type Definitions

- **Prefer interfaces over types** for object definitions (better for extension)
- **Use type for unions, intersections, and utility types**
- **Avoid enums**; use string literal union types or const objects instead
- Define shared types in dedicated files (e.g., `types.ts`)

```typescript
// ✅ Good
interface User {
  id: string;
  name: string;
  email: string;
}

// ✅ Good
type UserRole = 'admin' | 'user' | 'guest';

// ❌ Avoid
enum UserRole {
  Admin,
  User,
  Guest
}

// ✅ Better alternative to enum
const UserRoles = {
  ADMIN: 'admin',
  USER: 'user',
  GUEST: 'guest'
} as const;
type UserRole = typeof UserRoles[keyof typeof UserRoles];
```

## Function Definitions

- Use the `function` keyword for named functions
- Use arrow functions for callbacks and anonymous functions
- Always define parameter and return types for public functions
- Use function overloads for complex type scenarios

```typescript
// ✅ Good
function getUserById(id: string): Promise<User | null> {
  // implementation
}

// ✅ Good
const formatUser = (user: User): string => `${user.name} (${user.email})`;
```

## Naming Conventions

- Use descriptive variable names with auxiliary verbs:
  - `isLoading`, `hasError`, `shouldRefetch`
- Use PascalCase for:
  - Types, interfaces, classes, components
- Use camelCase for:
  - Variables, functions, methods, instances
- Use UPPER_SNAKE_CASE for constants

## Code Organization

- Structure files with a consistent pattern:
  1. Imports
  2. Type definitions
  3. Constants
  4. Main exported function/component
  5. Helper functions
  6. Exports

## Error Handling

- Use typed error handling with custom error classes or discriminated unions
- Avoid throwing generic errors; provide context and type information
- Use try/catch blocks with appropriate error typing

```typescript
// ✅ Good
type ApiError = {
  code: string;
  message: string;
  status: number;
};

try {
  const result = await apiCall();
  return result;
} catch (error) {
  if (isApiError(error)) {
    // Handle API error
  } else {
    // Handle other errors
  }
}
```

## Functional Programming

- Use functional and declarative programming patterns
- Avoid classes when functions can accomplish the same goal
- Prefer immutability; use spread operators and map/filter/reduce instead of mutating arrays/objects
- Use optional chaining (`?.`) and nullish coalescing (`??`) operators

```typescript
// ✅ Good
const updatedUsers = users.map(user => 
  user.id === targetId ? { ...user, active: true } : user
);

// ❌ Avoid
users.forEach(user => {
  if (user.id === targetId) {
    user.active = true;
  }
});
```

## Asynchronous Code

- Use async/await over raw promises for better readability
- Always handle promise rejections
- Use Promise.all for parallel async operations

```typescript
// ✅ Good
async function fetchUserData(userId: string) {
  try {
    const [user, posts] = await Promise.all([
      fetchUser(userId),
      fetchUserPosts(userId)
    ]);
    return { user, posts };
  } catch (error) {
    handleError(error);
    return null;
  }
}
```
