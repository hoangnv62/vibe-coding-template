# Security Rules — Fullstack

## CRITICAL — Never Violate These (Both Layers)

- **Never** hardcode secrets, API keys, passwords, or tokens in source code
- **Never** commit `.env` files to version control
- **Never** log sensitive data (passwords, tokens, PII)
- **Never** use `eval()` or `Function()` with user input
- **Always** validate and sanitize all user inputs — on **both** frontend and backend

> Frontend validation = UX. Backend validation = security. You need both.

---

## Backend Security

### Input Validation
```ts
// Validate all incoming data with Zod (or equivalent)
const loginSchema = z.object({
  email: z.string().email().max(255),
  password: z.string().min(8).max(128),
});
```

### Authentication
- JWT with short expiry: 15 min access token + 7 day refresh token
- Hash passwords with bcrypt (rounds ≥ 12)
- Rate limit auth endpoints

```ts
const hashed = await bcrypt.hash(password, 12);
```

### Authorization
```ts
// Check permissions on every protected route
router.delete('/posts/:id', authenticate, authorize('admin'), asyncHandler(deletePost));

// Verify resource ownership
if (post.authorId !== req.user.id && req.user.role !== 'admin') {
  throw new AppError('Forbidden', 403);
}
```

### HTTP Security Headers
```ts
import helmet from 'helmet';
app.use(helmet());

app.use(cors({
  origin: process.env.ALLOWED_ORIGINS?.split(',') || [],
  credentials: true,
}));
```

### Rate Limiting
```ts
const apiLimiter = rateLimit({ windowMs: 15 * 60 * 1000, max: 100 });
const authLimiter = rateLimit({ windowMs: 15 * 60 * 1000, max: 5 });
app.use('/api/', apiLimiter);
app.use('/api/auth/', authLimiter);
```

### SQL Injection Prevention
- Always use ORM parameterized queries — never string-concatenate user input into SQL

---

## Frontend Security

### XSS Prevention
```tsx
// ✅ Sanitize user-generated HTML before rendering
import DOMPurify from 'dompurify';
<div dangerouslySetInnerHTML={{ __html: DOMPurify.sanitize(content) }} />

// ❌ Never render unsanitized content
<div dangerouslySetInnerHTML={{ __html: userContent }} />
```

### Token Storage
- **Access tokens** → store in memory (JS variable / Zustand store) — safest against XSS
- **Refresh tokens** → httpOnly cookie set by backend — immune to JS access
- **Never store tokens in localStorage** if the app handles sensitive data

### Auth Guard
```tsx
export function AuthGuard({ children }: { children: ReactNode }) {
  const { isAuthenticated } = useAuthStore();
  if (!isAuthenticated) return <Navigate to="/login" replace />;
  return <>{children}</>;
}
```

### Environment Variables
```ts
// ✅ Only NEXT_PUBLIC_ vars reach the browser
const apiUrl = process.env.NEXT_PUBLIC_API_URL;

// ❌ Never use secret keys in client-side code
const secret = process.env.STRIPE_SECRET_KEY; // stays server-side only
```

### Content Security Policy (CSP)
```ts
// next.config.ts
const cspHeader = `
  default-src 'self';
  script-src 'self';
  connect-src 'self' ${process.env.NEXT_PUBLIC_API_URL};
  img-src 'self' data: https:;
`;
```

---

## Fullstack Auth Flow

```
1. User submits credentials → Frontend
2. POST /auth/login → Backend validates, returns accessToken (body) + refreshToken (httpOnly cookie)
3. Frontend stores accessToken in memory
4. Each request: Authorization: Bearer <accessToken>
5. On 401: POST /auth/refresh → Backend reads httpOnly cookie, returns new accessToken
6. On logout: DELETE /auth/logout → Backend clears cookie; Frontend clears memory
```

---

## Dependency Security (Both Sides)

```bash
npm audit          # Check for vulnerabilities
npm audit fix      # Auto-fix safe upgrades
```

---

## Security Checklist (Pre-deploy)

**Backend:**
- [ ] All inputs validated with schema (Zod/Joi/Yup)
- [ ] Passwords hashed (bcrypt ≥ 12 rounds)
- [ ] JWT expiry enforced (15m access / 7d refresh)
- [ ] Auth on all protected routes
- [ ] Rate limiting on auth endpoints
- [ ] Helmet.js enabled
- [ ] CORS restricted to known origins
- [ ] No secrets in code or logs

**Frontend:**
- [ ] No sensitive data in localStorage
- [ ] User-generated HTML sanitized (DOMPurify)
- [ ] No secret keys in client-side env vars
- [ ] Auth guard on protected routes
- [ ] CSP headers configured
- [ ] `npm audit` passes with 0 high/critical
