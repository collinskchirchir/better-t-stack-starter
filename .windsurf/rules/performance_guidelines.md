# Performance Guidelines

## General Principles

- Performance should be considered from the beginning, not as an afterthought
- Measure before optimizing - use appropriate tools to identify bottlenecks
- Focus on optimizations that provide the most user-facing benefits
- Balance performance with code readability and maintainability

## Backend Performance

### Database Optimization

- Use appropriate indexes for frequently queried columns
- Write efficient queries that retrieve only needed data
- Use pagination for endpoints that return large collections
- Consider database connection pooling for better resource utilization

```typescript
// ✅ Good - Paginated query
const { page = 1, limit = 10 } = input;
const offset = (page - 1) * limit;

const items = await db
  .select()
  .from(todo)
  .where(eq(todo.userId, ctx.session.user.id))
  .limit(limit)
  .offset(offset);

const total = await db
  .select({ count: sql`count(*)` })
  .from(todo)
  .where(eq(todo.userId, ctx.session.user.id));

return {
  items,
  pagination: {
    page,
    limit,
    total: Number(total[0].count),
    totalPages: Math.ceil(Number(total[0].count) / limit),
  },
};
```

### API Optimization

- Use appropriate HTTP caching headers
- Implement rate limiting for public endpoints
- Consider batching related requests to reduce network overhead
- Optimize payload size by returning only necessary data

### Server Resources

- Monitor memory usage and implement limits where appropriate
- Use efficient data structures and algorithms
- Consider implementing timeouts for long-running operations
- Use connection pooling for external services

## Frontend Performance

### Core Web Vitals

- Optimize for key metrics:
  - Largest Contentful Paint (LCP): < 2.5s
  - First Input Delay (FID): < 100ms
  - Cumulative Layout Shift (CLS): < 0.1
- Use tools like Lighthouse to measure and monitor these metrics

### Rendering Optimization

- Use appropriate rendering strategies:
  - Static Generation for content that doesn't change often
  - Server-Side Rendering for dynamic but SEO-important pages
  - Client-Side Rendering only when necessary for interactivity
- Implement code splitting to reduce initial bundle size
- Use dynamic imports for non-critical components and routes

```typescript
// ✅ Good - Dynamic import for modal component
import dynamic from 'next/dynamic';

const Modal = dynamic(() => import('@/components/Modal'), {
  loading: () => <p>Loading...</p>,
});
```

### Asset Optimization

- Optimize images:
  - Use WebP or AVIF formats when supported
  - Implement responsive images with appropriate sizes
  - Use lazy loading for below-the-fold images
- Minimize CSS and JavaScript:
  - Remove unused code
  - Use production builds with minification
  - Consider code splitting for large libraries

### React-Specific Optimizations

- Use React.memo for components that render often with the same props
- Implement useMemo and useCallback for expensive calculations and callbacks
- Use virtualization for long lists (react-window or react-virtualized)
- Avoid unnecessary re-renders by managing state efficiently

```typescript
// ✅ Good - Memoized component
import { memo } from 'react';

interface TodoItemProps {
  id: number;
  title: string;
  completed: boolean;
  onToggle: (id: number) => void;
}

export const TodoItem = memo(function TodoItem({ 
  id, title, completed, onToggle 
}: TodoItemProps) {
  return (
    <div>
      <input 
        type="checkbox" 
        checked={completed} 
        onChange={() => onToggle(id)} 
      />
      <span>{title}</span>
    </div>
  );
});
```

## Network Optimization

### Data Fetching

- Implement data prefetching for likely user journeys
- Use appropriate caching strategies:
  - SWR or React Query's caching capabilities
  - Service worker caching for offline support
- Optimize API response sizes:
  - Use compression (gzip/brotli)
  - Return only necessary data
  - Consider GraphQL for flexible data requirements

### Connection Handling

- Implement connection pooling for database and external services
- Use keep-alive connections when appropriate
- Consider implementing retry logic with exponential backoff for failed requests

## Monitoring and Continuous Improvement

- Implement performance monitoring in production
- Set up alerts for performance regressions
- Regularly review performance metrics and identify areas for improvement
- Test performance on various devices and network conditions

## Environment-Specific Optimizations

### Development

- Use fast refresh and hot module replacement
- Implement efficient build processes
- Use performance-focused linting rules

### Production

- Implement proper caching strategies
- Use a CDN for static assets
- Enable compression at the server level
- Consider edge caching for API responses
