# Backend Rules

## tRPC + Hono Integration

### Router Organization

- Organize routers by domain/feature (e.g., `todo.ts`, `user.ts`)
- Export all routers through the main `index.ts` router
- Keep procedure definitions clean and focused on a single responsibility

### Procedure Types

- Use `publicProcedure` for endpoints that don't require authentication
- Use `protectedProcedure` for endpoints that require a valid session
- Document the purpose and requirements of each procedure with comments

```typescript
// ✅ Good
export const todoRouter = router({
  // Public endpoint - no auth required
  getPublicTodos: publicProcedure.query(async () => {
    return await db.select().from(todo).where(eq(todo.isPublic, true));
  }),
  
  // Protected endpoint - requires authentication
  create: protectedProcedure
    .input(z.object({ title: z.string() }))
    .mutation(async ({ input, ctx }) => {
      return await db.insert(todo).values({
        title: input.title,
        userId: ctx.session.user.id,
      });
    }),
});
```

### Input Validation

- Always use Zod schemas for input validation
- Define reusable schemas for common data structures
- Provide meaningful error messages in schema validations

```typescript
// ✅ Good - Reusable schema
const todoInputSchema = z.object({
  title: z.string().min(1, "Title cannot be empty"),
  completed: z.boolean().optional().default(false),
});

// Using the schema
.input(todoInputSchema)
```

### Error Handling

- Use consistent error handling patterns across all tRPC routes
- Use tRPC's error system with appropriate error codes
- Include useful error messages and context for debugging

```typescript
// ✅ Good
if (!todo) {
  throw new TRPCError({
    code: 'NOT_FOUND',
    message: `Todo with id ${input.id} not found`,
  });
}
```

## Authentication with Better-Auth

### Configuration

- Keep authentication configuration in a dedicated file (`auth.ts`)
- Store sensitive information in environment variables
- Document authentication providers and their configuration

### Session Handling

- Always verify session validity before accessing user data
- Use the session context consistently across protected procedures
- Don't expose sensitive user information in responses

```typescript
// ✅ Good
export const protectedProcedure = t.procedure.use(({ ctx, next }) => {
  if (!ctx.session) {
    throw new TRPCError({
      code: "UNAUTHORIZED",
      message: "Authentication required",
    });
  }
  return next({
    ctx: {
      ...ctx,
      session: ctx.session,
    },
  });
});
```

## Drizzle ORM Patterns

### Schema Organization

- Define schemas in dedicated files under the `db/schema` directory
- Use consistent naming conventions for tables and columns
- Document relationships between tables with comments

```typescript
// ✅ Good
export const user = pgTable('user', {
  id: serial('id').primaryKey(),
  email: text('email').notNull().unique(),
  name: text('name'),
  // Relations documented in comments
  // Has many todos
});

export const todo = pgTable('todo', {
  id: serial('id').primaryKey(),
  title: text('title').notNull(),
  completed: boolean('completed').default(false),
  // Foreign key relationship
  userId: integer('user_id').references(() => user.id),
});
```

### Query Patterns

- Use typed queries with Drizzle's query builder
- Prefer parameterized queries to prevent SQL injection
- Use transactions for operations that modify multiple tables

```typescript
// ✅ Good - Parameterized query
const result = await db
  .select()
  .from(todo)
  .where(eq(todo.id, todoId));

// ✅ Good - Transaction
await db.transaction(async (tx) => {
  const newTodo = await tx.insert(todo).values({
    title: input.title,
    userId: ctx.session.user.id,
  }).returning();
  
  await tx.insert(todoActivity).values({
    action: 'create',
    todoId: newTodo[0].id,
    userId: ctx.session.user.id,
  });
});
```

## API Design

### Naming Conventions

- Use consistent naming for tRPC procedures:
  - `getAll`, `getById`, `create`, `update`, `delete`, `toggle`
- Use camelCase for procedure names
- Use descriptive names that indicate the action and resource

### Response Formats

- Return consistent data structures from similar procedures
- Include appropriate metadata in responses when needed
- Filter sensitive data before returning responses

```typescript
// ✅ Good - Consistent response format
export const todoRouter = router({
  getAll: protectedProcedure.query(async ({ ctx }) => {
    const items = await db
      .select()
      .from(todo)
      .where(eq(todo.userId, ctx.session.user.id));
    
    return {
      items,
      count: items.length,
    };
  }),
});
```

## Performance

- Use appropriate indexes on database tables
- Implement pagination for endpoints that return large collections
- Use query optimization techniques for complex database queries
- Consider caching strategies for frequently accessed data
