# CLAUDE.md

This file provides comprehensive guidance to Claude Code when working with Next.js 15 applications with React 19 and TypeScript, specifically tailored for Experity standards and best practices.

## Core Development Philosophy

### KISS (Keep It Simple, Stupid)
Simplicity should be a key goal in design. Choose straightforward solutions over complex ones whenever possible. Simple solutions are easier to understand, maintain, and debug.

### YAGNI (You Aren't Gonna Need It)
Avoid building functionality on speculation. Implement features only when they are needed, not when you anticipate they might be useful in the future.

### Design Principles
- **API First Design**: Prioritize API design before system design. A first draft of the API specification **MUST** be created before writing any code
- **Vertical Slice Architecture**: Organize by features, not layers
- **Component-First**: Build with reusable, composable components with single responsibility
- **Fail Fast**: Validate inputs early, throw errors immediately
- **Externalizable APIs**: All new APIs **MUST** be built as if they will be Public regardless of their end visibility

## 🤖 AI Assistant Guidelines

### Context Awareness
- When implementing features, always check existing patterns first
- Prefer composition over inheritance in all designs
- Use existing utilities before creating new ones
- Check for similar functionality in other domains/features
- **MUST** follow Experity template repositories when available

### Common Pitfalls to Avoid
- Creating duplicate functionality
- Overwriting existing tests
- Modifying core frameworks without explicit instruction
- Adding dependencies without checking existing alternatives
- Starting development without ARB approval for new APIs

### Workflow Patterns
- Preferably create tests BEFORE implementation (TDD)
- Use "think hard" for architecture decisions
- Break complex tasks into smaller, testable units
- Validate understanding before implementation
- **MUST** create API documentation in SwaggerHub before coding

### Search Command Requirements
**CRITICAL**: Always use `rg` (ripgrep) instead of traditional `grep` and `find` commands:

```bash
# ❌ Don't use grep
grep -r "pattern" .

# ✅ Use rg instead
rg "pattern"

# ❌ Don't use find with name
find . -name "*.tsx"

# ✅ Use rg with file filtering
rg --files | rg "\.tsx$"
# or
rg --files -g "*.tsx"
```

## 🚀 Next.js 15 & React 19 Key Features

### Experity PWA Requirements (MANDATORY)
All new UIs **MUST** be built and deployed as **Progressive Web Apps (PWAs)** with the following features:
- **Installable** to be available on the device's home screen or app launcher
- **Linkable**, so you can share it by sending a URL
- **Network independent**, so it works offline or with a poor network connection
- **Progressively enhanced**, it's still usable on a basic level on older browsers but fully functional on the latest ones
- **Re-engageable**, so it can send notifications whenever new content is available
- **Responsively designed**, it's usable on any device with a screen and a browser
- **Secure**, so the connections between the user, the app, and your server are secured

### PWA Capabilities (MUST IMPLEMENT)
- Service Workers
- Background Synchronization
- Background Fetch
- Periodic Background Synchronization
- Push API
- Notifications API

### Next.js 15 Core Features
- **Turbopack**: Fast bundler for development (stable)
- **App Router**: File-system based router with layouts and nested routing
- **Server Components**: React Server Components for performance
- **Server Actions**: Type-safe server functions
- **Parallel Routes**: Concurrent rendering of multiple pages
- **Intercepting Routes**: Modal-like experiences

### React 19 Features
- **React Compiler**: Eliminates need for `useMemo`, `useCallback`, and `React.memo`
- **Actions**: Handle async operations with built-in pending states
- **use() API**: Simplified data fetching and context consumption
- **Document Metadata**: Native support for SEO tags
- **Enhanced Suspense**: Better loading states and error boundaries

### TypeScript Integration (MANDATORY)
- **MUST use `ReactElement` instead of `JSX.Element`** for return types
- **MUST import types from 'react'** explicitly
- **NEVER use `JSX.Element` namespace** - use React types directly

```typescript
// ✅ CORRECT: Modern React 19 typing
import { ReactElement } from 'react';

function MyComponent(): ReactElement {
  return <div>Content</div>;
}

// ❌ FORBIDDEN: Legacy JSX namespace
function MyComponent(): JSX.Element {  // Cannot find namespace 'JSX'
  return <div>Content</div>;
}
```

## 🏗️ Project Structure (Experity Standards)

### Template Repository Usage (MANDATORY)
**MUST** use the Experity NextJS template repository for all new projects:
- Repository: `https://github.com/Experity/nextjs-ui-template`
- This template pre-configures many Experity standards including:
  - PWA configuration
  - SwaggerHub integration
  - Experity linting rules
  - Security headers
  - Performance optimizations

### Vertical Slice Architecture
```
src/
├── app/                   # Next.js App Router
│   ├── (routes)/          # Route groups
│   ├── globals.css        # Global styles
│   ├── layout.tsx         # Root layout
│   └── page.tsx           # Home page
├── components/            # Shared UI components
│   ├── ui/                # Base components (shadcn/ui)
│   └── common/            # Application-specific shared components
├── features/              # Feature-based modules (RECOMMENDED)
│   └── [feature]/
│       ├── __tests__/     # Co-located tests
│       ├── components/    # Feature components
│       ├── hooks/         # Feature-specific hooks
│       ├── api/           # API integration
│       ├── schemas/       # Zod validation schemas
│       ├── types/         # TypeScript types
│       └── index.ts       # Public API
├── lib/                   # Core utilities and configurations
│   ├── utils.ts           # Utility functions
│   ├── env.ts             # Environment validation
│   └── constants.ts       # Application constants
├── hooks/                 # Shared custom hooks
├── styles/                # Styling files
└── types/                 # Shared TypeScript types
```

## 🎯 TypeScript Configuration (EXPERITY STRICT REQUIREMENTS)

### MUST Follow These Compiler Options
```json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["dom", "dom.iterable", "es6"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [{ "name": "next" }],
    "baseUrl": ".",
    "paths": { "@/*": ["./src/*"] }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx", ".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
```

### MANDATORY Type Requirements
- **NEVER use `any` type** - use `unknown` if type is truly unknown
- **MUST have explicit return types** for all functions and components
- **MUST use proper generic constraints** for reusable components
- **MUST use type inference from Zod schemas** using `z.infer<typeof schema>`
- **NEVER use `@ts-ignore`** or `@ts-expect-error` - fix the type issue properly

## 📦 Package Management & Dependencies (EXPERITY STANDARDS)

### Dependency Management (MANDATORY)
- **All dependencies MUST be pinned to specific versions** (no ^ or ~ prefixes)
- **Development dependencies MUST use -D flag**
- **Use `npm i --save-exact <package name>`** for new dependencies
- **Dependencies used only for development MUST be installed with the -D flag**

Example package.json dependency format:
```json
{
  "dependencies": {
    "next": "15.0.0",           // ✅ Pinned version
    "react": "19.0.0",          // ✅ Pinned version
    "typescript": "5.0.0"       // ✅ Pinned version
  },
  "devDependencies": {
    "@types/node": "20.0.0",    // ✅ Pinned version
    "eslint": "8.0.0"           // ✅ Pinned version
  }
}
```

### Essential Next.js 15 Dependencies
```json
{
  "dependencies": {
    "next": "15.0.0",
    "react": "19.0.0",
    "react-dom": "19.0.0",
    "typescript": "5.0.0"
  },
  "devDependencies": {
    "@types/node": "20.0.0",
    "@types/react": "18.0.0",
    "@types/react-dom": "18.0.0",
    "eslint": "8.0.0",
    "eslint-config-next": "15.0.0",
    "prettier": "3.0.0",
    "tailwindcss": "3.4.0",
    "postcss": "8.4.0",
    "autoprefixer": "10.4.0"
  }
}
```

### CI/CD Dependency Installation (MANDATORY)
When installing dependencies on build servers (CircleCI, Jenkins, etc.), scripts **MUST** use `npm ci` instead of `npm install` to ensure clean installs without updating dependencies.

## 🛡️ Data Validation with Zod (MANDATORY FOR ALL EXTERNAL DATA)

### MUST Follow These Validation Rules
- **MUST validate ALL external data**: API responses, form inputs, URL params, environment variables
- **MUST use branded types**: For all IDs and domain-specific values
- **MUST fail fast**: Validate at system boundaries, throw errors immediately
- **MUST use type inference**: Always derive TypeScript types from Zod schemas
- **NEVER trust external data** without validation

### Schema Example (MANDATORY PATTERNS)
```typescript
import { z } from 'zod';

// MUST use branded types for ALL IDs
const UserIdSchema = z.string().uuid().brand<'UserId'>();
type UserId = z.infer<typeof UserIdSchema>;

// Environment validation (REQUIRED)
export const envSchema = z.object({
  NODE_ENV: z.enum(['development', 'test', 'production']),
  NEXT_PUBLIC_APP_URL: z.string().url(),
  DATABASE_URL: z.string().min(1),
  NEXTAUTH_SECRET: z.string().min(1),
  NEXTAUTH_URL: z.string().url(),
});

export const env = envSchema.parse(process.env);

// API response validation
export const apiResponseSchema = <T extends z.ZodTypeAny>(dataSchema: T) =>
  z.object({
    success: z.boolean(),
    data: dataSchema,
    error: z.string().optional(),
    timestamp: z.string().datetime(),
  });
```

## 🧪 Testing Strategy (EXPERITY REQUIREMENTS)

### MUST Meet These Testing Standards
- **MINIMUM 80% code coverage** - NO EXCEPTIONS
- **MUST co-locate tests** with components in `__tests__` folders
- **MUST use React Testing Library** for all component tests (Jest framework)
- **MUST use Cypress** for end-to-end testing
- **MUST test user behavior** not implementation details
- **MUST mock external dependencies** appropriately

### Jest Configuration (MANDATORY)
Coverage threshold MUST be configured:
```javascript
coverageThreshold: {
  global: {
    functions: 80,
    lines: 80,
    statements: 80,
  },
}
```

### Test Commands (STANDARD)
```json
{
  "scripts": {
    "test": "jest",
    "test:e2e:chrome": "cypress run --browser chrome",
    "test:e2e:edge": "cypress run --browser edge"
  }
}
```

### Mocking Requirements (MANDATORY)
- **All external references MUST be mocked** in tests
- This includes database interactions and third-party APIs
- Extract external interactions into classes using dependency injection

## 🎨 Component Guidelines (EXPERITY REQUIREMENTS)

### Skeleton Loaders (MANDATORY FOR MOBILE)
**MUST** implement skeleton loaders for better user experience:

**When to use:**
- High-traffic pages where resources take time to load
- Components with lots of information (Lists, Cards)
- When there's more than 1 element loading simultaneously
- Replace spinners for better UX

**When NOT to use:**
- Long-running processes (data imports)
- Fast processes under 300ms

### MANDATORY Component Documentation
```typescript
/**
 * Button component with multiple variants and sizes.
 *
 * Provides a reusable button with consistent styling and behavior
 * across the application. Supports keyboard navigation and screen readers.
 *
 * @component
 * @example
 * ```tsx
 * <Button
 *   variant="primary"
 *   size="medium"
 *   onClick={handleSubmit}
 * >
 *   Submit Form
 * </Button>
 * ```
 */
interface ButtonProps {
  /** Visual style variant of the button */
  variant: 'primary' | 'secondary';

  /** Size of the button @default 'medium' */
  size?: 'small' | 'medium' | 'large';

  /** Click handler for the button */
  onClick: (event: React.MouseEvent<HTMLButtonElement>) => void;

  /** Content to be rendered inside the button */
  children: React.ReactNode;

  /** Whether the button is disabled @default false */
  disabled?: boolean;
}
```

## 🔐 Security Requirements (EXPERITY MANDATORY)

### JWT Security (MANDATORY)
- **Practice PK MUST be encoded** in JWT issued by the API
- **All requests MUST be checked** to ensure incoming request's practice PK matches JWT
- **403 MUST be returned** if requested practice doesn't match JWT practice

```typescript
// Example JWT creation with practice PK
const token = jwt.sign(
  { id: loginInfo.UserProfilePK, practice: loginInfo.PracticePK },
  process.env.JWT_SECRET as jwt.Secret,
  { expiresIn: "24h" }
);

// Example practice verification
try {
  const { practice } = jwt.verify(
    token,
    process.env.JWT_SECRET as jwt.Secret
  ) as AccessToken;

  if (request.params["practiceId"].toLowerCase() === practice.toLowerCase())
    return Promise.resolve({});
} catch (error) {
  return Promise.reject({});
}
```

### Input Validation (MUST IMPLEMENT ALL)
- **MUST sanitize ALL user inputs** with Zod before processing
- **MUST validate file uploads**: type, size, and content
- **MUST prevent XSS** with proper escaping
- **MUST implement CSRF protection** for forms
- **NEVER use dangerouslySetInnerHTML** without sanitization

## 🚀 Performance Guidelines (EXPERITY STANDARDS)

### API Performance Requirements (MANDATORY)
- **APIs MUST return results in under 250 milliseconds**
- **API chaining SHOULD be avoided** due to 250ms limit
- **If execution exceeds 250ms, call MUST be ASYNC** with callback webhook or event-driven implementation

### Collection API Requirements (MANDATORY)
- **All collection APIs MUST accept limit parameter** (default: 30, max: 100)
- **MUST implement pagination** with page parameter for results > 100
- **MUST be safe by default** - return count only unless `?includeData=true`

Example API response structure:
```json
{
  "data": [],
  "total_count": 5,
  "page": 1,
  "limit": 30
}
```

### Next.js 15 Optimizations
- **Use Server Components** by default for data fetching
- **Client Components** only when necessary (interactivity)
- **Dynamic imports** for large client components
- **Image optimization** with next/image
- **Font optimization** with next/font

## 💅 Code Style & Quality (EXPERITY STANDARDS)

### Linting Requirements (MANDATORY)
- **Linting MUST be checked automatically** on each push
- **SHOULD be run locally** before pushing to GitHub
- **eslint** is the required tool

### Formatting Requirements (MANDATORY)
- **Formatting MUST be checked automatically** on each push
- **prettier** is the required tool
- **SHOULD be run locally** before pushing

### ESLint Configuration (MANDATORY)
```javascript
{
  "rules": {
    "@typescript-eslint/no-explicit-any": "error",
    "@typescript-eslint/explicit-function-return-type": "error",
    "no-console": ["error", { "allow": ["warn", "error"] }],
    "complexity": "error",
    "max-depth": "error",
    "no-duplicate-imports": "error"
  }
}
```

## 🔄 Git Workflow (EXPERITY STANDARDS)

### Commit Message Format (MANDATORY)
**MUST** follow Conventional Commits format with Jira card number:

```
<type>[optional scope]: <short description>

<optional body>

Jira: <Jira card ID>
```

Valid types: `feat`, `fix`, `build`, `chore`, `ci`, `docs`, `style`, `refactor`, `perf`, `test`

### Commit Linting (MANDATORY)
- **All repositories MUST have automated commit format check** on every Pull Request
- **All commits MUST contain valid Jira card number**
- **Breaking changes MUST add ! before colon**: `feat(vitals)!: changes temp data type`

### Versioning (MANDATORY)
- **ALL APIs MUST use semantic versioning 2.0** (X.Y.Z format)
- **APIs MUST be automatically versioned** via linting process with git commit messages
- **Creating new MAJOR version requires ARB approval** (carries extra costs)
- **Previous MAJOR version MUST be supported minimum 12 months** (24 months for public APIs)

## 📦 Time and Date Handling (EXPERITY STANDARDS)

### UTC Requirements (MANDATORY)
- **All times transferred or saved MUST be in UTC**
- **All Dates should be kept as-is**
- **Convert to user's TimeZone only when displaying**

### Attribute Naming (MANDATORY)
New attributes **should** explicitly state UTC in the attribute name:
- ✅ `createdAtUtc`
- ✅ `checkinTimeUtc`
- ❌ `appointmentTime`

### When to Convert
| Data Type | Store in UTC? | Best Practice |
|-----------|---------------|---------------|
| DATE (birthdate) | ❌ No | Store as is, no UTC conversion |
| TIMESTAMP (events) | ✅ Yes | Store in UTC, convert on display |
| DATETIME | ✅ Yes | Store in UTC, convert on display |
| User's Time Zone | ❌ No | Store full time zone name (America/New_York) for DST handling |

## 📋 Development Commands (EXPERITY STANDARDS)

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint --max-warnings 0",
    "lint:fix": "next lint --fix",
    "test": "jest",
    "test:coverage": "jest --coverage",
    "test:e2e:chrome": "cypress run --browser chrome",
    "test:e2e:edge": "cypress run --browser edge",
    "prettier:check": "prettier --check \"src/**/*.{ts,tsx,js,jsx,json,css,md}\"",
    "prettier:format": "prettier --write \"src/**/*.{ts,tsx,js,jsx,json,css,md}\"",
    "commitlint": "commitlint --from HEAD~1 --to HEAD --verbose",
    "validate": "npm run lint && npm run test:coverage && npm run prettier:check"
  }
}
```

## 📚 Documentation Requirements (EXPERITY MANDATORY)

### SwaggerHub Integration (MANDATORY)
- **MUST use SwaggerHub** for API design and documentation
- **API specification MUST be created** before writing any code
- **Work with team architect** to design API before coding
- **Use OAS (Open API Specification) version 3.1**
- Link: https://app.swaggerhub.com/organizations/Experity

### README Requirements (MANDATORY)
Every repo **MUST** have README.md with sections on:
- What does this repo do? (purpose)
- Development environment setup (including Node.js version)
- How to run development server locally
- Pre-commit steps (linting, tests, audit)
- Production build instructions
- Required/Optional environment variables
- Critical maintenance information

### Postman Requirements (MANDATORY)
- **Individual API repositories MUST contain Postman collection** (.postman_collection file)
- **Collection MUST be maintained** as API evolves
- **Authentication tokens/passwords MUST NOT be saved** in Postman file
- **Use dynamic variables** for tokens and URLs

## ⚠️ CRITICAL GUIDELINES (EXPERITY REQUIREMENTS)

1. **MUST use Experity NextJS template** for all new projects
2. **MUST get ARB approval** before starting new API development
3. **MUST implement as PWA** with all required capabilities
4. **MUST pin all dependencies** to specific versions
5. **MUST achieve 80% test coverage** - NO EXCEPTIONS
6. **MUST use SwaggerHub** for API documentation before coding
7. **MUST follow Conventional Commits** with Jira card numbers
8. **MUST implement skeleton loaders** for mobile UX
9. **MUST validate practice PK** in all JWT requests
10. **MUST return API results** in under 250ms
11. **MUST store times in UTC** with proper attribute naming
12. **MUST use semantic versioning** with automated commit linting

## 📋 Pre-commit Checklist (EXPERITY MANDATORY)

- [ ] **ARB approval obtained** for new APIs
- [ ] **SwaggerHub documentation created** before coding
- [ ] **Experity template used** for project initialization
- [ ] **PWA capabilities implemented** (Service Workers, etc.)
- [ ] **Dependencies pinned** to specific versions
- [ ] **TypeScript compiles** with ZERO errors
- [ ] **Tests written and passing** (80%+ coverage)
- [ ] **ESLint passes** with ZERO warnings (`--max-warnings 0`)
- [ ] **Prettier formatting** applied
- [ ] **Skeleton loaders implemented** for loading states
- [ ] **JWT practice validation** implemented
- [ ] **API performance** under 250ms
- [ ] **UTC time handling** implemented correctly
- [ ] **Conventional commit format** with Jira card
- [ ] **Postman collection** updated

### FORBIDDEN Practices (EXPERITY)
- **NEVER start development** without ARB approval for new APIs
- **NEVER use ^ or ~ prefixes** in dependency versions
- **NEVER use `npm install`** on build servers (use `npm ci`)
- **NEVER skip PWA requirements** for new UIs
- **NEVER exceed 250ms** API response time without async implementation
- **NEVER store authentication tokens** in Postman collections
- **NEVER use non-UTC times** for timestamps
- **NEVER create MAJOR API version** without ARB approval
- **NEVER commit** without Jira card number
- **NEVER exceed complexity limits** set by ESLint
