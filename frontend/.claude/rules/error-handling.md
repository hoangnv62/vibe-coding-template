# Error Handling — Frontend (React / Next.js)

## Core Principles
- **Never show raw errors to users** — display friendly error UI with recovery actions
- **Handle at the right level** — global Error Boundary for crashes, local state for fetch errors
- **Always provide recovery** — retry button, back navigation, or fallback content
- **Log errors** — send to Sentry or similar before displaying fallback UI

---

## Error Boundaries (React)

Use Error Boundaries to catch render errors and prevent the entire app from crashing.

```tsx
// components/common/ErrorBoundary.tsx
'use client';

import { Component, type ReactNode } from 'react';

interface Props {
  children: ReactNode;
  fallback?: ReactNode;
}

interface State {
  hasError: boolean;
  error?: Error;
}

export class ErrorBoundary extends Component<Props, State> {
  state: State = { hasError: false };

  static getDerivedStateFromError(error: Error): State {
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, info: { componentStack: string }) {
    // Send to error tracking (Sentry, etc.)
    console.error('[ErrorBoundary]', error, info.componentStack);
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback ?? (
        <div role="alert" className="p-6 text-center">
          <p className="text-destructive font-medium">Something went wrong.</p>
          <button onClick={() => this.setState({ hasError: false })}>
            Try again
          </button>
        </div>
      );
    }
    return this.props.children;
  }
}
```

### Placement Strategy
```tsx
// app/layout.tsx — top-level catch-all
<ErrorBoundary fallback={<GlobalErrorPage />}>
  {children}
</ErrorBoundary>

// Wrap individual feature sections for isolated failures
<ErrorBoundary fallback={<WidgetError />}>
  <DashboardWidget />
</ErrorBoundary>
```

### Next.js Built-in Error Files
```
app/
├── error.tsx         ← Route-level error boundary (must be 'use client')
├── global-error.tsx  ← Root layout error (replaces <html>)
└── not-found.tsx     ← 404 page
```

```tsx
// app/error.tsx
'use client';

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  return (
    <div role="alert">
      <h2>Something went wrong</h2>
      <button onClick={reset}>Try again</button>
    </div>
  );
}
```

---

## Async / Data Fetching Errors

### TanStack Query (Client Components)
```tsx
const { data, isLoading, error, refetch } = useQuery({
  queryKey: ['user', id],
  queryFn: () => api.users.getById(id),
  retry: 2,
});

if (isLoading) return <UserSkeleton />;

// ✅ Always handle error state explicitly
if (error) {
  return (
    <ErrorState
      message="Failed to load user"
      onRetry={refetch}
    />
  );
}
```

### Server Components (Next.js)
```tsx
// Throw — caught by nearest error.tsx boundary
async function UserProfile({ id }: { id: string }) {
  const user = await api.users.getById(id); // throws on failure

  if (!user) notFound(); // renders not-found.tsx

  return <ProfileCard user={user} />;
}
```

---

## Reusable Error UI Components

```tsx
// components/common/ErrorState.tsx
interface ErrorStateProps {
  message?: string;
  onRetry?: () => void;
}

export function ErrorState({ message = 'Something went wrong', onRetry }: ErrorStateProps) {
  return (
    <div role="alert" className="flex flex-col items-center gap-3 py-10 text-center">
      <p className="text-muted-foreground">{message}</p>
      {onRetry && (
        <button onClick={onRetry} className="btn-secondary">
          Try again
        </button>
      )}
    </div>
  );
}
```

---

## Form Validation Errors

```tsx
// ✅ Use react-hook-form + Zod — errors are co-located with fields
const { register, formState: { errors } } = useForm<FormData>({
  resolver: zodResolver(schema),
});

<input
  id="email"
  aria-invalid={!!errors.email}
  aria-describedby={errors.email ? 'email-error' : undefined}
  {...register('email')}
/>
{errors.email && (
  <span id="email-error" role="alert" className="text-destructive text-sm">
    {errors.email.message}
  </span>
)}
```

---

## API Call Error Patterns

```tsx
// ✅ Centralized API error handling in axios interceptor
// src/api/interceptors/auth.interceptor.ts
api.interceptors.response.use(
  (res) => res,
  (error) => {
    if (error.response?.status === 401) {
      authStore.getState().logout();
      router.push('/login');
    }
    return Promise.reject(error);
  }
);
```

---

## Rules Summary

| Situation | Pattern |
|-----------|---------|
| Component render crash | `<ErrorBoundary>` |
| Route-level crash | `app/error.tsx` |
| Page not found | `app/not-found.tsx` + `notFound()` |
| Data fetch failure (client) | TanStack Query error state + `<ErrorState>` |
| Data fetch failure (server) | `throw` → caught by `error.tsx` |
| Form validation | react-hook-form + Zod + `role="alert"` |
| Auth errors (401/403) | Axios interceptor → redirect |

---

## Red Flags

Stop and reconsider if you're:
- Catching errors with empty `catch {}` blocks
- Displaying raw error messages from the server to users
- Not providing a retry or recovery action
- Using `console.log` instead of a proper logger/Sentry
- Missing `role="alert"` on dynamically injected error messages
