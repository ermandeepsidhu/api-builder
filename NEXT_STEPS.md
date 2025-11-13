# APIForge - Next Steps

## ✅ Completed: Rebranding & Desktop Code Removal

Congratulations! The project has been successfully rebranded from **Yaak** to **APIForge** and all desktop-specific code has been removed. Here's what's been done:

### Achievements:

1. **Complete Rebranding**
   - All package names updated (`@yaak` → `@apiforge`)
   - Documentation rewritten (README, DEVELOPMENT, CLAUDE.md)
   - 31+ plugin packages updated
   - HTML title and meta tags updated

2. **Desktop Code Removed**
   - Entire `src-tauri/` directory deleted (~50% of codebase)
   - All Rust/Tauri dependencies removed
   - Desktop-specific scripts removed
   - Build process simplified

3. **Plugin System Preserved**
   - All 31+ plugins maintained
   - Plugin runtime intact
   - Plugin API updated to `@apiforge/api`

## 🚀 What to Do Next

### Step 1: Verify the Changes

```bash
# Clean install dependencies
rm -rf node_modules package-lock.json
npm install

# Try to run the frontend (may have errors due to missing backend)
npm run dev
```

Expected: Vite dev server starts, but app may have errors due to missing Tauri backend.

### Step 2: Choose Your Backend Strategy

You have several options:

#### Option A: Next.js (Recommended)
```bash
# Create a new Next.js app alongside src-web
npx create-next-app@latest api-builder-backend --typescript --tailwind --app --no-src-dir

# Or integrate Next.js directly with src-web
```

**Pros**: Full-stack in one repo, easy deployment, built-in API routes
**Cons**: Learning curve if new to Next.js

#### Option B: Separate Express Backend
```bash
# Create backend directory
mkdir backend
cd backend
npm init -y
npm install express cors pg # or your chosen DB
```

**Pros**: Clear separation, flexible architecture
**Cons**: More setup, separate deployment

#### Option C: Remix
```bash
npx create-remix@latest api-builder-backend
```

**Pros**: Excellent for data-heavy apps, progressive enhancement
**Cons**: Smaller ecosystem than Next.js

### Step 3: Set Up Database

Choose and set up your database:

#### PostgreSQL (Recommended for Production)
```bash
# Local development
brew install postgresql  # macOS
# or use Docker
docker run --name apiforge-db -e POSTGRES_PASSWORD=dev -p 5432:5432 -d postgres

# Cloud options
# - Supabase (includes auth)
# - Neon (serverless)
# - Railway
```

#### Turso (SQLite-compatible, Edge)
```bash
# Install Turso CLI
curl -sSfL https://get.tur.so/install.sh | bash

# Create database
turso db create apiforge-db
```

### Step 4: Implement Authentication

Choose an auth provider:

#### Clerk (Recommended for Startups)
```bash
npm install @clerk/nextjs  # if using Next.js
# Visit clerk.com to set up
```

#### Supabase Auth
```bash
npm install @supabase/supabase-js
# If using Supabase database, auth is included
```

#### NextAuth.js (Open Source)
```bash
npm install next-auth
```

### Step 5: Port Core Features

Priority order for porting from the removed Rust code:

1. **HTTP Request Execution** (Highest Priority)
   - Use `axios` or `node-fetch`
   - Port request building logic
   - Handle authentication plugins

2. **Environment Variables**
   - Template rendering
   - Variable resolution
   - Environment chains

3. **Database Models**
   - Workspaces
   - Folders
   - Requests
   - Responses
   - Environments

4. **WebSocket/SSE** (Lower Priority)
   - Can implement later
   - Use `ws` package for WebSocket
   - Use built-in Node.js EventSource for SSE

### Step 6: Integrate LLM

Once basic functionality works, add AI features:

```bash
# Install LLM SDK
npm install openai
# or
npm install @anthropic-ai/sdk
```

Create LLM service:

```typescript
// backend/services/llm.ts
import OpenAI from 'openai';

export class LLMService {
  private client: OpenAI;

  constructor() {
    this.client = new OpenAI({
      apiKey: process.env.OPENAI_API_KEY,
    });
  }

  async generateAPISpec(prompt: string) {
    const completion = await this.client.chat.completions.create({
      model: 'gpt-4',
      messages: [{
        role: 'system',
        content: 'You are an API design expert. Generate OpenAPI specs from natural language descriptions.',
      }, {
        role: 'user',
        content: prompt,
      }],
    });

    return completion.choices[0].message.content;
  }
}
```

## 📚 Reference Original Code

Although the Rust code has been removed, you can reference it in the original Yaak repository:

- **Request Execution**: `yaak/src-tauri/yaak-http/src/lib.rs`
- **Template Rendering**: `yaak/src-tauri/yaak-templates/`
- **Database Models**: `yaak/src-tauri/yaak-models/src/models.rs`
- **Plugin Communication**: `yaak/src-tauri/yaak-plugins/`

Link: https://github.com/mountain-loop/yaak

## 🎯 Recommended Timeline

### Week 1-2: Backend Setup
- [ ] Choose backend framework
- [ ] Set up database
- [ ] Implement authentication
- [ ] Create basic API structure

### Week 3-4: Core Features
- [ ] Port HTTP request execution
- [ ] Implement workspace/folder management
- [ ] Add environment variables
- [ ] Connect frontend to backend

### Week 5-6: LLM Integration
- [ ] Integrate LLM service
- [ ] Build chat interface
- [ ] Implement API spec generation
- [ ] Add code generation

### Week 7-8: Polish & Deploy
- [ ] Testing
- [ ] Documentation
- [ ] Deployment setup
- [ ] Beta launch prep

## 🛠️ Tools You'll Need

```bash
# Development tools
npm install -g nodemon      # Auto-restart backend
npm install -g tsx          # Run TypeScript directly

# Database tools
npm install -g prisma       # ORM (if using Prisma)
# or
npm install drizzle-orm     # Alternative ORM

# Testing
npm install -D vitest @testing-library/react
```

## 📝 Important Files to Review

1. **[MIGRATION_SUMMARY.md](./MIGRATION_SUMMARY.md)** - Detailed changes made
2. **[REBRANDING_PLAN.md](./REBRANDING_PLAN.md)** - Complete transformation strategy
3. **[README.md](./README.md)** - Updated project overview
4. **[DEVELOPMENT.md](./DEVELOPMENT.md)** - Development workflow
5. **[CLAUDE.md](./CLAUDE.md)** - Architecture for AI assistance

## ⚠️ Known Issues

1. **Frontend may not start**: Missing backend will cause errors. This is expected.
2. **Plugin CLI (`apiforgecli`)**: Renamed but not yet created. Use existing `yaakcli` or fork it.
3. **Database migrations**: Original migrations removed, need to recreate schema.

## 🤝 Getting Help

If you need assistance:

1. Review the original Yaak codebase for reference
2. Check `packages/plugin-runtime/` for plugin system docs
3. Open issues on your fork for tracking
4. Consider using Claude Code to help implement features

## 🎉 You're Ready!

The foundation is set. APIForge is rebranded, slimmed down, and ready for its transformation into a web-based, AI-powered API builder. 

Start with Step 1 above and work your way through. Good luck! 🚀

---

**Questions?** Review the documentation files or examine the preserved plugin system for implementation patterns.
