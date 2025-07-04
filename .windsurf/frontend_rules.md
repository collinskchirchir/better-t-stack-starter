# Frontend Rules

## Next.js App Router Structure

### Directory Organization

- Follow Next.js App Router conventions for routing
- Use lowercase with dashes for directories (e.g., `components/auth-wizard`)
- Organize components by feature or domain

```
apps/web/
├── src/
│   ├── app/                  # App Router routes
│   │   ├── (auth)/           # Auth-related routes (grouped)
│   │   │   ├── login/        # Login page
│   │   │   └── register/     # Register page
│   │   ├── dashboard/        # Dashboard page
│   │   └── page.tsx          # Home page
│   ├── components/           # Shared components
│   │   ├── ui/               # UI components
│   │   └── features/         # Feature-specific components
│   └── lib/                  # Utilities and helpers
```

### Component Structure

- Favor named exports for components
- Structure component files consistently:
  1. Imports
  2. Types/interfaces
  3. Component definition
  4. Helper functions
  5. Exports

```typescript
// ✅ Good
import { useState } from 'react';
import { Button } from '@/components/ui/button';

interface TodoItemProps {
  id: number;
  title: string;
  completed: boolean;
  onToggle: (id: number, completed: boolean) => void;
}

export function TodoItem({ id, title, completed, onToggle }: TodoItemProps) {
  const handleToggle = () => {
    onToggle(id, !completed);
  };

  return (
    <div className="flex items-center gap-2 p-2">
      <input
        type="checkbox"
        checked={completed}
        onChange={handleToggle}
      />
      <span className={completed ? 'line-through text-gray-500' : ''}>
        {title}
      </span>
    </div>
  );
}
```

## Server vs. Client Components

### Server Components (Default)

- Use Server Components by default when possible
- Ideal for:
  - Data fetching
  - Accessing backend resources directly
  - Components that don't need interactivity
  - SEO-critical content

### Client Components

- Add `'use client'` directive at the top of files that need client-side features
- Limit client components to where they're needed:
  - Interactive UI elements
  - Components using hooks
  - Components needing browser APIs
- Keep client components small and focused

```typescript
// ✅ Good - Client component for interactivity
'use client';

import { useState } from 'react';
import { Button } from '@/components/ui/button';

export function Counter() {
  const [count, useState] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <Button onClick={() => setCount(count + 1)}>Increment</Button>
    </div>
  );
}
```

## Data Fetching

### tRPC Client Integration

- Set up tRPC client in a dedicated file (e.g., `lib/trpc.ts`)
- Use React Query's capabilities for caching and state management
- Implement proper error handling for API requests

```typescript
// ✅ Good - Using tRPC client in a component
'use client';

import { api } from '@/lib/trpc';

export function TodoList() {
  const { data, isLoading, error } = api.todo.getAll.useQuery();
  
  if (isLoading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;
  
  return (
    <ul>
      {data?.items.map(todo => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  );
}
```

### Server-Side Data Fetching

- Use React Server Components for initial data fetching when possible
- Implement proper loading and error states
- Consider using Next.js's streaming and Suspense for improved UX

## UI and Styling

### Component Library

- Use shadcn/ui components as the foundation for UI
- Customize components consistently using the project's design tokens
- Create reusable composite components for common patterns

### Tailwind CSS

- Follow a mobile-first approach for responsive design
- Use Tailwind's utility classes consistently
- Group related utilities with meaningful class names
- Extract common patterns to components rather than duplicating classes

```typescript
// ✅ Good - Consistent Tailwind usage
<div className="flex flex-col gap-4 p-4 md:p-6 rounded-lg bg-white shadow-sm">
  <h2 className="text-xl font-semibold text-gray-900">Card Title</h2>
  <p className="text-gray-600">Card content goes here</p>
</div>
```

## State Management

### Local State

- Use React's `useState` and `useReducer` for component-level state
- Keep state as close as possible to where it's used
- Split complex state into smaller, manageable pieces

### Form Handling

- Use form libraries (e.g., React Hook Form) for complex forms
- Implement client-side validation with appropriate feedback
- Handle form submission with proper loading and error states

```typescript
// ✅ Good - Form handling with React Hook Form
'use client';

import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';
import { Button } from '@/components/ui/button';

const formSchema = z.object({
  title: z.string().min(1, 'Title is required'),
});

export function TodoForm() {
  const form = useForm({
    resolver: zodResolver(formSchema),
    defaultValues: { title: '' },
  });
  
  const onSubmit = (data) => {
    // Submit data
  };
  
  return (
    <form onSubmit={form.handleSubmit(onSubmit)}>
      <input {...form.register('title')} />
      {form.formState.errors.title && (
        <p>{form.formState.errors.title.message}</p>
      )}
      <Button type="submit">Add Todo</Button>
    </form>
  );
}
```

## Authentication Integration

### Client-Side Auth

- Use Better-Auth client for authentication state
- Implement protected routes with appropriate redirects
- Handle authentication errors gracefully

### Session Management

- Access session data consistently across components
- Implement proper loading states while checking authentication
- Handle session expiration with automatic refresh when possible

## Performance Optimization

### Component Optimization

- Use React.memo for expensive components that render often
- Implement virtualization for long lists
- Use dynamic imports for code splitting

### Image Optimization

- Use Next.js Image component for automatic optimization
- Specify appropriate sizes and formats
- Implement lazy loading for below-the-fold images

```typescript
// ✅ Good - Optimized image
import Image from 'next/image';

export function ProfileAvatar({ user }) {
  return (
    <Image
      src={user.avatarUrl}
      alt={`${user.name}'s avatar`}
      width={64}
      height={64}
      className="rounded-full"
    />
  );
}
```
