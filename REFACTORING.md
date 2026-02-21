# RezaBook - Refactoring & Production Guide

## Summary of Changes

### 1. Architecture & Folder Structure

```
src/
├── app/                 # Next.js App Router pages
├── components/
│   ├── auth/           # Auth-specific components (AuthLayout)
│   └── ui/             # Reusable UI (Button, Input, Card, Alert, Select)
├── hooks/              # useAuth, useCompany
├── lib/                # Utils, validation, errors, Supabase config
├── services/           # Data access layer (auth, company, slug)
└── types/              # TypeScript interfaces
```

### 2. Clean Architecture

- **Service layer**: `services/` abstracts Supabase calls; pages use services instead of direct DB access
- **Validation**: `lib/validation.ts` provides input validation and sanitization
- **Error types**: `lib/errors.ts` defines `AppError`, `ValidationError`, etc.

### 3. Security Improvements

- Input validation on signup/login (email, phone, password, slug)
- Slug format validation and uniqueness check before company creation
- API routes validate and sanitize input (`/api/check-slug`, `/api/send-email`)
- `escapeHtml` utility for XSS prevention (use when rendering user content)

### 4. Removed Unused Dependencies

- `zustand`, `swr`, `axios`, `@supabase/auth-helpers-nextjs` removed from package.json

### 5. UI Components

- Shared `Button`, `Input`, `Select`, `Card`, `Alert` in `components/ui/`
- `AuthLayout` for login/signup consistency

### 6. Hook Integration

- Dashboard uses `useAuth` and `useCompany`
- `useAuth` returns `signOut` and `refreshSession`

---

## Production Deployment Checklist

### Environment Variables

```env
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=   # For admin operations
RESEND_API_KEY=              # For email (optional)
```

### Recommended Improvements

1. **Auth Middleware**: Current setup uses localStorage. For SSR/cookie-based auth:
   - Add `@supabase/ssr`
   - Use `createBrowserClient` and `createServerClient`
   - Implement session refresh in `middleware.ts`

2. **Database**: 
   - Add Supabase RLS (Row Level Security) policies
   - Create migrations for schema versioning
   - Use `supabaseAdmin` for privileged server-side operations

3. **Multi-tenant**: Current model: 1 user = 1 company (company.id = user.id). For true multi-tenant:
   - Separate `companies` and `users` tables
   - Add `company_members` join table with roles

4. **Caching**: Consider React Query or SWR for data fetching (reduce re-fetches)

5. **Monitoring**: Add error tracking (Sentry, etc.) in production

6. **Rate limiting**: Add to API routes for production

### Build & Deploy

```bash
npm install
npm run build
npm run start
```

### Vercel Deployment

- Set env vars in project settings
- Enable serverless functions
- Consider Edge middleware for auth (after @supabase/ssr migration)
