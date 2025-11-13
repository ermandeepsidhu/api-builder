# APIForge Rebranding & Web Transformation Plan

## Executive Summary

This document outlines the strategy to transform Yaak (a desktop API client) into **APIForge** - a web-based API builder with LLM-powered natural language capabilities for building, testing, deploying, and iterating on APIs.

## Brand Name Recommendation: APIForge

**Rationale:**
- **Enterprise-friendly**: Strong, professional connotation
- **Descriptive**: Clearly communicates API building/creation
- **Memorable**: Short, punchy, easy to spell
- **Domain availability**: Likely available (apiforge.io, apiforge.dev, getapiforge.com)
- **Tagline options**:
  - "Build APIs at the speed of thought"
  - "AI-powered API development platform"
  - "From concept to deployment in minutes"

## Key Transformation Tasks

### 1. Remove Desktop/Tauri Dependencies

**Files/Directories to Remove:**
```
src-tauri/                           # Entire Rust/Tauri backend
.github/workflows/*-tauri-*.yml      # Desktop build workflows
scripts/vendor-*.cjs                 # Desktop vendoring scripts
```

**Dependencies to Remove from package.json:**
- `@tauri-apps/cli`
- `@tauri-apps/api`
- All `@tauri-apps/plugin-*` packages
- Desktop-specific Rust crates

**Scripts to Remove/Modify:**
- `app-build`, `app-dev` (Tauri-specific)
- `bootstrap:vendor-*` (Desktop vendoring)
- `icons:*` (Desktop icon generation)

### 2. Branding Changes

**Files Requiring Updates:**

| File | Changes Needed |
|------|----------------|
| `package.json` | Name, description, repository URL |
| `README.md` | Complete rewrite for APIForge |
| `DEVELOPMENT.md` | Update for web-only development |
| `CLAUDE.md` | Update architecture section |
| `src-web/index.html` | Title, meta tags |
| `src-web/package.json` | Name scoping (@apiforge) |
| All `packages/*/package.json` | Rename scopes from @yaakapp to @apiforge |
| All plugin `package.json` files | Update metadata |
| `plugins/themes-yaak` | Rename to `plugins/themes-apiforge` |
| Icon/logo files | Replace with APIForge branding |

**Text Replacements Needed:**
- `yaak` → `apiforge`
- `Yaak` → `APIForge`
- `YAAK` → `APIFORGE`
- `@yaakapp` → `@apiforge`
- `yaak-app` → `apiforge-app`
- Update all copyright notices
- Update license holder names

### 3. Convert to Web-Only Architecture

**Current Stack to Keep:**
- React frontend (src-web/)
- SQLite database (migrate to server-side)
- Plugin system (adapt for web)
- Environment/workspace concepts
- Request execution logic

**New Web Backend Architecture:**

```
┌─────────────────────────────────────────────────────┐
│                  APIForge Web App                    │
├─────────────────────────────────────────────────────┤
│                                                       │
│  Frontend (React + Vite)                             │
│  ├── UI Components (reuse from src-web)              │
│  ├── State Management (Jotai + TanStack Query)       │
│  └── LLM Chat Interface (NEW)                        │
│                                                       │
├─────────────────────────────────────────────────────┤
│                                                       │
│  Backend (Node.js/Express or Next.js API Routes)     │
│  ├── Database (PostgreSQL or SQLite)                 │
│  ├── Authentication (Auth0/Supabase/Clerk)           │
│  ├── API Execution Engine (from Rust logic)          │
│  │   ├── HTTP Client                                 │
│  │   ├── gRPC Client                                 │
│  │   ├── WebSocket Handler                           │
│  │   └── SSE Handler                                 │
│  ├── Template Engine (port from Rust)                │
│  ├── Plugin System (Node.js - already exists!)       │
│  └── LLM Integration Layer (NEW)                     │
│      ├── OpenAI/Anthropic/Custom LLM                 │
│      ├── API Schema Generator                        │
│      ├── Test Case Generator                         │
│      └── Code Generator (OpenAPI, Postman, etc.)     │
│                                                       │
├─────────────────────────────────────────────────────┤
│                                                       │
│  Deployment Engine (NEW)                             │
│  ├── Cloud Provider Integrations                     │
│  │   ├── AWS Lambda                                  │
│  │   ├── Vercel Functions                            │
│  │   ├── Cloudflare Workers                          │
│  │   └── Railway/Render                              │
│  └── CI/CD Pipeline Generation                       │
│                                                       │
└─────────────────────────────────────────────────────┘
```

### 4. Technology Stack Recommendations

**Backend Framework Options:**

1. **Next.js 15 (Recommended)**
   - ✅ Full-stack in one repo
   - ✅ API routes + React frontend
   - ✅ Built-in deployment (Vercel)
   - ✅ Edge runtime support
   - ✅ TypeScript native

2. **Remix**
   - ✅ Excellent for data-heavy apps
   - ✅ Progressive enhancement
   - ✅ TypeScript native

3. **Express + Vite**
   - ✅ Full control
   - ❌ More setup required

**Database Options:**

1. **PostgreSQL (Recommended)**
   - ✅ Multi-tenant support
   - ✅ JSON support for flexible schemas
   - ✅ Hosted options (Supabase, Neon, Railway)

2. **SQLite (via Turso)**
   - ✅ Can reuse existing migrations
   - ✅ Edge deployment
   - ✅ Local-first capability

**Authentication:**
- Clerk (recommended for startups)
- Supabase Auth (if using Supabase)
- Auth0 (enterprise)
- NextAuth.js (open source)

### 5. LLM Integration Architecture

**Core LLM Features:**

```typescript
// LLM Integration Points

1. Natural Language → API Spec
   "Create a REST API for user management with CRUD operations"
   → Generates OpenAPI spec
   → Creates requests in workspace
   → Generates test cases

2. Code Generation
   - Generate API implementations (Node.js, Python, Go)
   - Generate client SDKs
   - Generate database schemas
   - Generate CI/CD configs

3. Test Generation
   - Generate test scenarios from API spec
   - Create edge case tests
   - Generate test data

4. Documentation Generation
   - Auto-generate API docs
   - Create integration guides
   - Generate examples

5. Debugging Assistant
   - Analyze failed requests
   - Suggest fixes for errors
   - Recommend improvements

6. Deployment Assistant
   - Suggest deployment targets
   - Generate deployment configs
   - Create infrastructure as code
```

**Implementation Pattern:**

```typescript
// Example: LLM Service Layer

interface LLMService {
  generateAPISpec(prompt: string): Promise<OpenAPISpec>;
  generateTests(apiSpec: OpenAPISpec): Promise<TestSuite>;
  generateImplementation(
    apiSpec: OpenAPISpec,
    language: 'nodejs' | 'python' | 'go'
  ): Promise<CodeFiles>;
  analyzeError(
    request: HttpRequest,
    response: HttpResponse
  ): Promise<ErrorAnalysis>;
  suggestDeployment(apiSpec: OpenAPISpec): Promise<DeploymentPlan>;
}

// Backend API Routes
// POST /api/llm/generate-spec
// POST /api/llm/generate-tests
// POST /api/llm/generate-implementation
// POST /api/llm/analyze-error
// POST /api/llm/suggest-deployment
```

### 6. Migration Strategy

**Phase 1: Fork & Rebrand (Week 1)**
- ✅ Fork repository
- Create APIForge organization/repo
- Update all branding
- Replace icons/logos
- Update documentation

**Phase 2: Extract Frontend (Week 2)**
- Create new Next.js project
- Move React components from src-web/
- Adapt state management
- Remove Tauri-specific code

**Phase 3: Build Backend (Weeks 3-4)**
- Port database schema to PostgreSQL/SQLite
- Implement API routes
- Port request execution logic from Rust
- Adapt plugin system for web

**Phase 4: LLM Integration (Weeks 5-6)**
- Implement LLM service layer
- Create chat interface
- Build spec generation
- Add code generation

**Phase 5: Deployment Engine (Weeks 7-8)**
- Cloud provider integrations
- Deployment pipeline generation
- Testing and iteration

**Phase 6: Beta Launch (Week 9+)**
- User testing
- Documentation
- Marketing site
- Launch!

### 7. Features to Remove (Not Web-Compatible)

- Desktop window management
- Native file system access (use cloud storage)
- OS keychain integration (use encrypted vault)
- Auto-updater
- Desktop notifications (use web push)
- Deep linking (use web URLs)
- Mac window controls
- System fonts (use web fonts)

### 8. New Features to Add

**LLM-Powered Features:**
- Natural language API design
- Intelligent test generation
- Code generation for multiple languages
- Deployment recommendations
- Error analysis and debugging
- Documentation generation

**Collaboration Features:**
- Team workspaces
- Real-time collaboration
- Comment threads
- Version history
- Role-based access control

**Cloud Features:**
- Cloud storage for workspaces
- Automated backups
- Cross-device sync
- Scheduled requests/monitoring
- Webhook support

**Enterprise Features:**
- SSO integration
- Audit logs
- Compliance reports
- API usage analytics
- Cost tracking

### 9. Monetization Strategy

**Tiers:**

1. **Free Tier**
   - Up to 3 workspaces
   - 100 LLM requests/month
   - Community plugins
   - Basic support

2. **Pro ($29/month)**
   - Unlimited workspaces
   - 1,000 LLM requests/month
   - Premium plugins
   - Priority support
   - Team collaboration (up to 5 users)

3. **Team ($99/month)**
   - Everything in Pro
   - Unlimited LLM requests
   - SSO integration
   - Custom plugins
   - Dedicated support
   - Unlimited team members

4. **Enterprise (Custom)**
   - Self-hosted option
   - Custom LLM integration
   - SLA guarantees
   - Training and onboarding
   - Custom development

### 10. Quick Start Commands

**After transformation, the project will use:**

```bash
# Development
npm run dev              # Start Next.js dev server
npm run db:migrate       # Run database migrations
npm run db:seed          # Seed test data

# Building
npm run build            # Build for production
npm run start            # Start production server

# Testing
npm test                 # Run tests
npm run test:e2e         # Run E2E tests

# Deployment
npm run deploy           # Deploy to Vercel/Railway/etc
```

### 11. Critical Files to Update First

**Priority 1 (Brand Identity):**
1. `package.json` - Name and metadata
2. `README.md` - Project description
3. `src-web/index.html` - Title and meta
4. All logos/icons
5. License file

**Priority 2 (Architecture):**
1. Create new backend structure
2. Database migration strategy
3. Port plugin system
4. Authentication setup

**Priority 3 (LLM Integration):**
1. LLM service abstraction
2. Chat UI component
3. API route handlers
4. Code generation logic

## Next Steps

1. **Choose a brand name** (APIForge recommended)
2. **Decide on tech stack** (Next.js + PostgreSQL recommended)
3. **Set up new repository** with clean structure
4. **Begin Phase 1** migration
5. **Prototype LLM integration** early to validate concept

## Questions to Answer

1. Will you self-host or use a platform? (Vercel, Railway, etc.)
2. Which LLM provider? (OpenAI, Anthropic Claude, open source)
3. Target market? (Startups, enterprises, both?)
4. Freemium or paid-only?
5. Open source the new version or keep proprietary?
