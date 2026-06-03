# Testing Standards

## Testing Pyramid
```
         [E2E Tests]         ← Few, slow, catch integration issues
       [Integration Tests]   ← Some, test component interaction
     [Unit Tests]            ← Many, fast, test isolated logic
```

## Requirements
- Unit test coverage: **minimum 80%**
- All new features must have tests
- All bug fixes must have a regression test
- Tests run in CI before any merge

## Test File Organization
```
tests/
├── unit/
│   ├── lib/utils.test.ts
│   └── hooks/useDebounce.test.ts
├── integration/
│   └── features/tasks/TaskList.test.tsx
└── e2e/
    ├── auth-flow.spec.ts
    └── task-management.spec.ts
```

## Unit Test Example — Custom Hook (Vitest + Testing Library)
```tsx
// tests/unit/hooks/useTasks.test.ts
import { renderHook, waitFor } from '@testing-library/react';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { useTasks } from '@/features/tasks/hooks/useTasks';
import { vi } from 'vitest';

vi.mock('@/api/endpoints/tasks.api', () => ({
  getTasks: vi.fn().mockResolvedValue([{ id: '1', title: 'Buy milk', done: false }]),
}));

const wrapper = ({ children }) => (
  <QueryClientProvider client={new QueryClient()}>
    {children}
  </QueryClientProvider>
);

describe('useTasks', () => {
  it('should return tasks on success', async () => {
    const { result } = renderHook(() => useTasks(), { wrapper });

    await waitFor(() => expect(result.current.isSuccess).toBe(true));

    expect(result.current.data).toHaveLength(1);
    expect(result.current.data[0].title).toBe('Buy milk');
  });
});
```

## Integration Test Example — Component (Vitest + Testing Library)
```tsx
// tests/integration/features/tasks/TaskList.test.tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { TaskList } from '@/features/tasks/components/TaskList';

describe('TaskList', () => {
  const mockTasks = [
    { id: '1', title: 'Buy milk', done: false },
    { id: '2', title: 'Walk dog', done: true },
  ];

  it('should render all tasks', () => {
    render(<TaskList tasks={mockTasks} onToggle={vi.fn()} />);

    expect(screen.getByText('Buy milk')).toBeInTheDocument();
    expect(screen.getByText('Walk dog')).toBeInTheDocument();
  });

  it('should call onToggle when task is clicked', () => {
    const onToggle = vi.fn();
    render(<TaskList tasks={mockTasks} onToggle={onToggle} />);

    fireEvent.click(screen.getByText('Buy milk'));

    expect(onToggle).toHaveBeenCalledWith('1');
  });

  it('should show empty state when no tasks', () => {
    render(<TaskList tasks={[]} onToggle={vi.fn()} />);

    expect(screen.getByText(/no tasks/i)).toBeInTheDocument();
  });
});
```

## E2E Test Example (Playwright)
```ts
// tests/e2e/auth-flow.spec.ts
import { test, expect } from '@playwright/test';

test('user can log in and see dashboard', async ({ page }) => {
  await page.goto('/login');

  await page.fill('[name="email"]', 'user@example.com');
  await page.fill('[name="password"]', 'password123');
  await page.click('button[type="submit"]');

  await expect(page).toHaveURL('/dashboard');
  await expect(page.getByRole('heading', { name: /dashboard/i })).toBeVisible();
});
```

## Test Commands
```bash
npm test                    # Run all Vitest tests
npm run test:unit           # Unit tests only
npm run test:coverage       # Generate coverage report
npm run test:watch          # Watch mode for development
npx playwright test         # Run E2E tests
npx playwright test --ui    # Playwright UI mode
```

## Naming Conventions
- Test files: `[filename].test.tsx` or `[filename].spec.ts` (E2E)
- `describe` blocks: Match the component or hook being tested
- `it` blocks: `should [expected behavior] when [condition]`
