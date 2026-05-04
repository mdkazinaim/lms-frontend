# EduCore — Frontend Architecture Plan
## Microfrontend Design Document

---

## 1. Architecture Overview

EduCore's frontend is built as a **Microfrontend (MFE) application** using a **Monorepo** structure. Each major product surface (Admin, Institute, Instructor, Student, Public) is an independently developed, independently deployable frontend application. A central **Shell App** acts as the host — it owns authentication, authorization, global routing, and the shared design system.

### Why Microfrontend?
- Each dashboard (Institute, Instructor, Student) is large enough to be its own product.
- Teams can develop and deploy each app independently without touching others.
- The platform can grow (new dashboards, new roles) without the codebase becoming a monolith.
- Shared code (UI, API client, auth) is maintained in one place and consumed by all apps.

---

## 2. Monorepo Structure

The project is organized as a **npm workspace monorepo**. All apps live under `apps/`, all shared libraries under `packages/`.

```
educore-frontend/
│
├── apps/
│   ├── shell/              # Host app — auth, routing, MFE loader
│   ├── public/             # Public pages — marketing, explore, profiles
│   ├── platform-admin/     # Platform admin dashboard
│   ├── institute/          # Institute dashboard
│   ├── instructor/         # Instructor/Teacher dashboard
│   └── student/            # Student dashboard
│
├── packages/
│   ├── ui/                 # Shared component library (design system)
│   ├── auth/               # Auth logic — hooks, context, token utilities
│   ├── api/                # API client — axios instance, service layer
│   ├── types/              # Shared TypeScript interfaces and enums
│   └── utils/              # Shared utility functions
│
├── npm-workspace.yaml
├── package.json            # Root scripts and devDependencies
└── turbo.json              # Turborepo build pipeline config
```

> **Tooling choice:** `npm workspaces` for package management + `Turborepo` for orchestrating builds, lint, and type-check across the monorepo efficiently.

---

## 3. Applications

### 3.1 Shell App (`apps/shell`)

**This is the host.** Every other app is a remote loaded by the shell. The shell is the only app the browser first loads.

**Responsibilities:**
- Serves as the Webpack/Vite Module Federation **host**.
- Handles the **authentication flow**: Login, Register, Forgot Password, Email Verification.
- Reads the authenticated user's role and **routes them to the correct remote MFE**.
- Renders global layout chrome (top navigation bar if any global nav exists).
- Provides the **shared auth context** singleton consumed by all remotes.
- Handles **global notifications** (toast, in-app notification center).
- Implements **lazy-loading** and **error boundaries** for each remote.
- Manages **route-level code splitting**: each MFE is only loaded when its route is activated.

**Routes owned by Shell:**
```
/                    → Public landing (loads public MFE)
/login               → Auth pages (shell-rendered)
/register            → Auth pages (shell-rendered) 
/explore             → Loads public MFE
/i/:slug             → Institute public landing page (public MFE)
/t/:slug             → Teacher public profile (public MFE)
/s/:slug             → Student portfolio (public MFE)
/admin/*             → Loads platform-admin MFE
/institute/*         → Loads institute MFE
/instructor/*        → Loads instructor MFE
/student/*           → Loads student MFE
```

---

### 3.2 Public App (`apps/public`)

Handles all **unauthenticated or post-auth public pages**. This is the discovery surface of the platform.

**Pages:**
- Platform marketing homepage
- Explore page (browse courses, teachers, institutes, resources)
- Institute landing page (`/i/:slug`) — dynamically rendered from CMS/API
- Teacher profile page (`/t/:slug`)
- Student portfolio page (`/s/:slug`) — respects per-section privacy settings
- Resource viewer (publicly shared resources)

**Notes:**
- This app is server-renderable (SSR/SSG friendly) for SEO — institute and teacher profile pages benefit significantly from SEO.
- Authentication state is read (if logged in, show different CTA), but the app is fully functional without it.

---

### 3.3 Platform Admin App (`apps/platform-admin`)

The internal admin panel for the **platform's own team**. Manages the entire platform.

**Pages/Features:**
- User management (all users: institutes, teachers, students)
- Content moderation queue (flagged resources, student posts, guidelines)
- Platform-wide content library (courses, tests, practice sets)
- News & announcements broadcast
- Certificate template management
- Analytics: platform-wide usage, enrollment trends, revenue
- Setting: platform subscription tiers, feature flags, email templates

**Access:** Platform admin accounts only. The shell redirects `role === 'platform_admin'` here.

---

### 3.4 Institute App (`apps/institute`)

The dashboard for **institute administrators**.

**Pages/Features:**
- Institute Home Feed (after login)
- Teacher management (invite, remove, configure authority)
- Student management (profiles, progress, behavior)
- Batch management (create, manage, view analytics)
- Content management (courses, tests, quizzes, resources — approve/reject teacher submissions)
- Landing page builder (live preview editor for public institute page)
- News & announcements (institute-scoped or public)
- Analytics (enrollment trends, content performance, student activity)
- Notifications center
- Institute settings and branding

---

### 3.5 Instructor App (`apps/instructor`)

The dashboard for **individual teachers** and **affiliated teachers** (with a context switcher).

**Pages/Features:**
- Instructor overview
- My Students (with per-student profile, progress, behavior)
- Content creation & management (courses, tests, practice sets, quizzes)
- Batch management (create/manage batches, view progress)
- Live Quiz host panel
- Public profile page editor (`/t/:slug` builder)
- Analytics (content performance, student engagement)
- Earnings dashboard (individual teachers only)
- Institute Panel (affiliated teachers: context switcher to institute scope, pending approval queue)
- Notifications center

---

### 3.6 Student App (`apps/student`)

The dashboard for **students** (individual and enrolled).

**Pages/Features:**
- Home Feed (aggregated content from all enrolled providers + platform)
- My Providers sidebar (quick jump to any institute/teacher page)
- My Learning (courses in progress, completed)
- My Batches (batch schedule, batch-specific content and tests)
- My Tests (assigned, self-attempted, result history)
- Practice Module (sessions, saved sets, AI-generated)
- Following (students followed, their content)
- Saved content (bookmarks)
- Portfolio page editor (`/s/:slug` builder with per-section privacy)
- Notifications center
- Settings (profile, enrolled providers, notification preferences)

> **Note:** When a student upgrades to Individual Teacher, the Instructor App is loaded alongside Student App — the shell reads the dual role and provides a **role switcher** in the top bar.

---

## 4. Shared Packages

Packages are internal libraries consumed by all apps via pnpm workspace imports (e.g., `import { Button } from '@educore/ui'`).

---

### 4.1 `packages/ui` — Design System & Component Library

The single source of truth for all UI. Every app pulls components from here. No app re-implements its own button, card, or form input.

**Contents:**
- **Design Tokens** — Colors, typography scale, spacing, border radius, shadow presets — exported as CSS variables and JS constants.
- **Base Components** — `Button`, `Input`, `Textarea`, `Select`, `Checkbox`, `Radio`, `Toggle`, `Badge`, `Avatar`, `Spinner`.
- **Composite Components** — `Card`, `Modal`, `Drawer`, `Tooltip`, `Dropdown`, `Tabs`, `Accordion`, `Pagination`.
- **Layout Components** — `PageShell`, `Sidebar`, `SidebarItem`, `TopBar`, `ContentArea`.
- **Data Components** — `DataTable`, `StatCard`, `ProgressBar`, `EmptyState`, `SkeletonLoader`.
- **Form System** — `Form`, `FormField`, `FormLabel`, `FormError` — built on `react-hook-form`.
- **Rich Text** — `RichTextEditor` (for course content, guidelines, profile bios) and `RichTextRenderer`.
- **Notification** — `Toast`, `NotificationBell`, `NotificationItem`.

**Tech:** React 18, TypeScript, CSS Modules or Vanilla CSS, Storybook for component documentation.

---

### 4.2 `packages/auth` — Authentication & Authorization

Centralizes all auth logic. Consumed by the shell (which manages the flow) and by remotes (which read the auth state).

**Contents:**
- `AuthProvider` — React context provider that stores user session (id, role, email, name, avatar, enrolled providers).
- `useAuth()` hook — Reads auth context: `user`, `isAuthenticated`, `role`, `logout()`.
- `usePermission(permission)` hook — Returns boolean. Used to conditionally render UI elements.
- Token utilities — `getAccessToken()`, `refreshAccessToken()`, `clearTokens()`.
- Route guards — `<PrivateRoute>`, `<RoleRoute role="institute_admin">` components.
- Auth API calls — `login()`, `register()`, `logout()`, `refreshSession()`.

**Token Strategy:** JWT access token (short-lived, 15 min) + refresh token (HttpOnly cookie, 7 days). The shell silently refreshes access tokens. All remotes use the shared `getAccessToken()` function via Module Federation shared singleton.

---

### 4.3 `packages/api` — API Client & Service Layer

A pre-configured HTTP client and organized service modules for every API domain.

**Contents:**
- `apiClient` — Axios instance with base URL, auth header injection, and automatic token refresh interceptor.
- Service modules (one per domain):
  - `authService` — Login, register, refresh.
  - `userService` — Profile, settings.
  - `instituteService` — Institute CRUD, teacher management, student management.
  - `instructorService` — Teacher profile, content creation.
  - `studentService` — Enrollment, progress, test history.
  - `courseService` — Course CRUD, modules, lessons.
  - `batchService` — Batch CRUD, enrollment, schedule.
  - `testService` — Tests, questions, submissions, grading.
  - `practiceService` — Practice sets, sessions, AI generation.
  - `quizService` — Quiz CRUD, live sessions.
  - `resourceService` — Resource upload, browse, moderation.
  - `notificationService` — Fetch, mark read.
  - `searchService` — Platform-wide search.

**Server State:** All data fetching is handled with **TanStack Query** (React Query). The `api` package exports service functions; apps wrap them in `useQuery` / `useMutation`.

---

### 4.4 `packages/types` — Shared TypeScript Types

All interfaces, enums, and type aliases shared across apps.

**Examples:**
```ts
// User roles
enum UserRole {
  PLATFORM_ADMIN = 'platform_admin',
  INSTITUTE_ADMIN = 'institute_admin',
  INSTRUCTOR = 'instructor',
  STUDENT = 'student',
}

// Content visibility
enum Visibility {
  PUBLIC = 'public',
  MEMBERS_ONLY = 'members_only',
  PRIVATE = 'private',
}

// Batch status
enum BatchStatus {
  DRAFT = 'draft',
  OPEN = 'open',
  ACTIVE = 'active',
  COMPLETED = 'completed',
  ARCHIVED = 'archived',
}
```

---

### 4.5 `packages/utils` — Utility Functions

Pure, reusable utility functions with no React dependencies.

**Contents:**
- Date formatting (`formatDate`, `formatRelativeTime`, `formatDuration`).
- String helpers (`slugify`, `truncate`, `capitalize`).
- File helpers (`formatFileSize`, `getFileExtension`, `isVideoFile`).
- Validation helpers (`isValidEmail`, `isValidUrl`).
- Number helpers (`formatCurrency`, `formatPercentage`).

---

## 5. Module Federation Setup

The shell hosts all remotes. Each remote **exposes** its root component. The shell **imports** and renders them lazily.

```
Shell (HOST)
  ├── Consumes → public MFE        (exposes: PublicApp)
  ├── Consumes → platform-admin MFE (exposes: PlatformAdminApp)
  ├── Consumes → institute MFE     (exposes: InstituteApp)
  ├── Consumes → instructor MFE    (exposes: InstructorApp)
  └── Consumes → student MFE       (exposes: StudentApp)

Shared singletons (loaded once, shared across all):
  - react, react-dom
  - @educore/auth (AuthProvider, useAuth)
  - @educore/api (apiClient)
  - @educore/types
```

**Vite Plugin:** `@originjs/vite-plugin-federation` (supports Vite + Module Federation).

Each remote's `vite.config.ts`:
```ts
federation({
  name: 'student',
  filename: 'remoteEntry.js',
  exposes: {
    './App': './src/App.tsx',
  },
  shared: ['react', 'react-dom', '@educore/auth', '@educore/api'],
})
```

Shell's `vite.config.ts`:
```ts
federation({
  name: 'shell',
  remotes: {
    student:    'http://localhost:3004/remoteEntry.js',
    instructor: 'http://localhost:3003/remoteEntry.js',
    institute:  'http://localhost:3002/remoteEntry.js',
    platformAdmin: 'http://localhost:3001/remoteEntry.js',
    public:     'http://localhost:3005/remoteEntry.js',
  },
  shared: ['react', 'react-dom', '@educore/auth', '@educore/api'],
})
```

In production, remote URLs point to CDN paths (`https://cdn.educore.io/student/remoteEntry.js`).

---

## 6. Inter-App Communication

MFEs should be **loosely coupled**. Communication is kept minimal and explicit.

| Method | Use case |
|---|---|
| **Shared Auth Context** | Auth state readable by all remotes via Module Federation singleton |
| **URL / Query Params** | Navigating between MFEs with contextual data (e.g., `/institute/students?id=123`) |
| **Shared Event Bus** | Lightweight pub/sub for cross-MFE events (e.g., "notification received") — implemented as a singleton in `@educore/utils` |
| **TanStack Query Cache** | Server state is per-app. No shared cache between MFEs — each app manages its own server state |

**What NOT to do:** No direct function imports between remote apps. No shared Redux store between remotes. All cross-MFE data must go through the URL or the event bus.

---

## 7. Routing Strategy

- The **Shell** owns top-level routes and decides which remote to mount.
- Each **remote** manages its own internal routes using React Router v6, mounted at its base path.
- The shell passes the base path to the remote: the remote's router uses `basename` prop.

```
Shell route: /student/*  → mounts StudentApp
StudentApp internal routes (basename="/student"):
  /student/               → Home Feed
  /student/learning       → My Learning
  /student/batches        → My Batches
  /student/tests          → My Tests
  /student/practice       → Practice Module
  /student/explore        → Explore
  /student/profile        → My Portfolio Editor
  /student/settings       → Settings
```

---

## 8. Tech Stack Summary

| Layer | Technology | Reason |
|---|---|---|
| Monorepo | npm workspaces + Turborepo | Fast installs, efficient task caching |
| Build tool | Vite | Fast HMR, excellent DX, MFE plugin support |
| MFE | `@originjs/vite-plugin-federation` | Module Federation for Vite |
| Framework | React 18 + TypeScript | Component model, strict typing |
| Routing | React Router v6 | Declarative, nested routes, basename support |
| Server state | TanStack Query v5 | Caching, pagination, optimistic updates |
| Client state | Zustand | Lightweight, no boilerplate |
| Forms | React Hook Form + Zod | Performance-first forms with schema validation |
| Styling | Vanilla CSS + CSS Modules | Scoped styles, no class conflicts between MFEs |
| Component docs | Storybook | Design system documentation and visual testing |
| Testing | Vitest + React Testing Library | Unit and integration tests |
| E2E Testing | Playwright | Cross-MFE end-to-end flows |
| CI/CD | GitHub Actions | Per-app deploy pipelines |

---

## 9. Development Port Allocation

| App | Dev Port |
|---|---|
| shell | 3000 |
| platform-admin | 3001 |
| institute | 3002 |
| instructor | 3003 |
| student | 3004 |
| public | 3005 |

Each developer can run only the shell + the MFE they are working on, rather than starting all 6 apps.

---

## 10. Deployment Strategy

Each MFE is independently deployable. In production:

| App | Deployment Target |
|---|---|
| shell | Vercel / Netlify (or CDN-hosted) |
| public | Vercel (SSR via Next.js or Vite SSR) |
| platform-admin | Private CDN path (authenticated-only) |
| institute | CDN path |
| instructor | CDN path |
| student | CDN path |

The shell fetches `remoteEntry.js` from each MFE's CDN URL at runtime. Deploying only the `student` app means only `student`'s `remoteEntry.js` changes on the CDN — no other app is affected or re-deployed.

---

## 11. Folder Structure Per App

Each app follows a consistent internal structure:

```
apps/student/
├── src/
│   ├── pages/              # Route-level page components
│   │   ├── HomeFeed/
│   │   ├── MyLearning/
│   │   ├── MyBatches/
│   │   ├── MyTests/
│   │   ├── Practice/
│   │   ├── Portfolio/
│   │   └── Settings/
│   ├── components/         # App-local reusable components
│   ├── hooks/              # App-local custom hooks
│   ├── store/              # Zustand stores (client state)
│   ├── services/           # Thin wrappers around @educore/api
│   ├── utils/              # App-local utilities
│   ├── App.tsx             # Router + layout root
│   └── main.tsx            # Entry point (standalone dev mode)
├── vite.config.ts
├── tsconfig.json
└── package.json
```

> **Standalone dev mode:** Each app can run independently (visiting `localhost:3004` renders the student app directly). This lets developers work on a single MFE without running the shell — useful for feature development and UI testing in isolation.

---

## 12. Open Questions for Team Review

> These decisions should be confirmed before scaffolding begins.

**Q1 — SSR for Public Pages?**
Institute and teacher profile pages (`/i/:slug`, `/t/:slug`) are SEO-critical. The `public` MFE may need to be built with Next.js (App Router) instead of plain Vite, which is a different setup from the other MFEs. Alternatively, Vite SSR can be used but requires more manual configuration.

**Recommendation:** Build `apps/public` as a Next.js 14 App Router project. All other apps remain Vite.

---

**Q2 — Turborepo vs Nx?**
- **Turborepo** — Simpler, faster to set up, good for small-to-mid teams. Lower learning curve.
- **Nx** — More powerful (code generators, project graph, affected commands), better at scale. Steeper setup.

**Recommendation:** Start with Turborepo. Migrate to Nx if the team grows beyond 5 and build times become a bottleneck.

---

**Q3 — CSS Modules vs Tailwind?**
CSS Modules are scoped and have zero class-name conflicts between MFEs — important in a Module Federation setup. Tailwind's JIT can sometimes conflict or duplicate styles across MFEs if not configured correctly.

**Recommendation:** CSS Modules for MFE apps. Tailwind is acceptable only if a single shared Tailwind config is used across all apps from the `packages/ui` package.

---

**Q4 — Role Switcher UX (Student → Instructor)**
When a student upgrades to teacher, both dashboards are accessible. The shell needs a **role context switcher** in the top navigation bar. This is a shell-level concern, not an MFE concern.

---

*Document Version: 1.0 — April 2026*
*Status: Architecture Planning — Pending Team Review*
