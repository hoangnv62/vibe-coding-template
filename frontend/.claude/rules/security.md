# Security Rules

## 🚨 CRITICAL — Never Violate These

- **Never** hardcode secrets, API keys, passwords, or tokens in source code
- **Never** commit `.env` files to version control
- **Never** log sensitive data (passwords, tokens, PII)
- **Never** use `eval()` or `Function()` with user input
- **Always** validate and sanitize all user inputs

## Environment Variables
```js
// ✅ Always use environment variables for secrets
const dbPassword = process.env.DB_PASSWORD;
const jwtSecret = process.env.JWT_SECRET;

// ❌ Never hardcode secrets
const dbPassword = 'mypassword123';
```

## Input Validation & XSS Prevention
```tsx
// ✅ Validate form inputs with Zod before submitting
import { z } from 'zod';

const loginSchema = z.object({
  email: z.string().email().max(255),
  password: z.string().min(8).max(128),
});

// ✅ Sanitize user-generated HTML content (e.g. rich text editors)
import DOMPurify from 'dompurify';
const cleanHtml = DOMPurify.sanitize(userInput);

// ❌ Never use dangerouslySetInnerHTML with unsanitized content
<div dangerouslySetInnerHTML={{ __html: userContent }} />       // ❌ XSS risk
<div dangerouslySetInnerHTML={{ __html: DOMPurify.sanitize(userContent) }} />  // ✅
```

## Authentication (Client-Side)
- Store access tokens in **memory** (not localStorage) to prevent XSS theft
- Store refresh tokens in **httpOnly cookies** (set by backend)
- Redirect to `/login` on 401 responses via axios interceptor

```ts
// ✅ Axios interceptor — handle 401 globally
api.interceptors.response.use(
  (res) => res,
  async (error) => {
    if (error.response?.status === 401) {
      authStore.getState().logout();
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);
```

## Authorization (Client-Side)
```tsx
// ✅ Protect routes with a guard component
export function AuthGuard({ children }: { children: ReactNode }) {
  const { isAuthenticated } = useAuthStore();
  if (!isAuthenticated) return <Navigate to="/login" replace />;
  return <>{children}</>;
}

// ✅ Hide UI elements based on role (visual only — enforce on backend too)
function AdminPanel() {
  const { user } = useAuthStore();
  if (user.role !== 'admin') return null;
  return <AdminControls />;
}
```

## Environment Variables
```ts
// ✅ Use NEXT_PUBLIC_ prefix for client-side env vars
const apiUrl = process.env.NEXT_PUBLIC_API_URL;

// ❌ Never expose secret keys in client-side code
const stripeSecret = process.env.STRIPE_SECRET_KEY; // stays server-side only
```

## Content Security Policy (CSP)
```ts
// next.config.ts — set CSP headers
const cspHeader = `
  default-src 'self';
  script-src 'self' 'nonce-{nonce}';
  style-src 'self' 'unsafe-inline';
  img-src 'self' data: https:;
  connect-src 'self' ${process.env.NEXT_PUBLIC_API_URL};
`;
```

## Dependency Security
```bash
# Regularly audit dependencies
npm audit
npm audit fix
```
