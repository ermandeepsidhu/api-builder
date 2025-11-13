# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

APIForge is a web-based API development platform for designing, testing, and deploying APIs with AI assistance. It's being transitioned from a desktop application (Yaak) to a web-based platform with:
- **React frontend** (TypeScript + Vite) for the UI
- **Node.js plugin runtime** for extensibility
- **Future backend** for cloud features, authentication, and AI integration
- **Database** (will be migrated to PostgreSQL or cloud SQLite)

This is an **open-source project** forked from Yaak, adapted for web-based use with AI capabilities.

## Common Development Commands

### Initial Setup
```bash
npm install
npm run bootstrap
```

### Development
```bash
npm run dev
# Starts the Vite dev server for React frontend
# Available at http://localhost:5173
```

### Building
```bash
npm run build                    # Build all workspace packages
npm run build-plugins            # Build plugins only
npm run app-build                # Production Tauri build
```

### Testing
```bash
npm test                         # Run tests in all workspaces
```

Individual plugins have their own test suites (using vitest). Most tests are in the `plugins/*/tests/` directories.

### Linting
```bash
npm run lint                     # Lint all workspaces
```

Uses ESLint for TypeScript/JavaScript and follows the config in `eslint.config.cjs`.

### Database Migrations
```bash
npm run migration
```

Creates a new timestamped migration file in `src-tauri/yaak-models/migrations/`. Migrations are automatically applied when the app runs.

## Architecture

### High-Level Structure

**Monorepo Layout:**
- `src-web/` - React frontend (Vite + TanStack Router)
  - `components/` - React UI components
  - `hooks/` - React hooks for data fetching and state management
  - `lib/` - Utility functions and shared logic
- `plugins/` - Built-in plugins (auth, importers, template functions, filters)
- `packages/` - Shared packages
  - `common-lib/` - Shared utilities
  - `plugin-runtime/` - Node.js runtime for plugin execution
  - `plugin-runtime-types/` - TypeScript types for plugin development

**Note**: The `src-tauri/` directory contains the original Rust/Tauri backend from Yaak. This is being phased out in favor of a web-based backend.

### Key Architectural Concepts

**Frontend-Backend Communication (In Transition):**
The app is being migrated from Tauri's command system to standard web APIs. The original Tauri commands in `src-tauri/src/lib.rs` are being replaced with REST/GraphQL endpoints.

**Database Layer (In Transition):**
Originally used SQLite via Rust's `sqlx`. Will be migrated to:
- PostgreSQL for cloud deployment
- OR Turso (distributed SQLite) for edge deployment
- Web-based ORM (Prisma, Drizzle, or similar)

**Plugin System:**
- Plugins run in a Node.js sidecar process (`packages/plugin-runtime/`)
- Communication happens via gRPC between Rust and Node.js
- Plugin types: authentication, template functions, importers, filters, request actions
- Built-in plugins live in `plugins/` and follow the workspace structure
- Plugin events are handled in `src-tauri/src/plugin_events.rs`
- The `PluginManager` (`yaak-plugins` crate) coordinates plugin lifecycle

**State Management:**
- React uses Jotai for global state management
- TanStack Query for server state and data fetching
- TanStack Router for routing (see `src-web/routeTree.gen.ts`)
- Models are synced between Rust and React via Tauri events

**Template Rendering:**
The app supports templating with variables and functions. Templates are rendered via:
- `yaak-templates` crate in Rust
- Environment variables resolved through environment chains (workspace → folder → active environment)
- Plugin-provided template functions for dynamic values

**Request Execution Flow:**
1. User triggers request from UI
2. Frontend calls Tauri command (e.g., `cmd_send_http_request`)
3. Backend resolves template variables in environment chain
4. Authentication plugins are invoked if needed
5. Request is sent via `yaak-http`, `yaak-grpc`, or `yaak-ws`
6. Response is stored in database and streamed back to UI

**Workspace & Environment Model:**
- **Workspace**: Top-level container for requests, folders, environments
- **Folder**: Organizes requests hierarchically
- **Environment**: Key-value pairs for variables, can be workspace-level or folder-level
- **Request**: HTTP, gRPC, or WebSocket request definition
- **Response/Connection**: Stored results of request execution

### Important Rust Crates

- `yaak-models`: Database models, migrations, and query layer
- `yaak-plugins`: Plugin manager and gRPC communication with Node.js sidecar
- `yaak-templates`: Template tokenization and rendering
- `yaak-http`: HTTP request execution
- `yaak-grpc`: gRPC reflection and request execution
- `yaak-ws`: WebSocket connections
- `yaak-sse`: Server-sent events
- `yaak-sync`: Git-based workspace syncing
- `yaak-crypto`: Encryption for sensitive data
- `yaak-git`: Git operations for workspace mirroring

### Important Frontend Patterns

**Custom Hooks:**
Most data fetching and mutations are wrapped in hooks (e.g., `useActiveWorkspace`, `useSendHttpRequest`). These use TanStack Query under the hood.

**Model Store:**
The `@yaakapp-internal/models` package provides a Jotai-based store that syncs with the Rust backend via Tauri events. It's initialized in `src-web/main.tsx`.

**Dynamic Forms:**
The `DynamicForm` component renders forms from plugin-provided schemas, used extensively for authentication and template function configuration.

## Development Notes

- The project uses NPM workspaces for managing the monorepo
- Rust formatting uses `rustfmt` with config in `rustfmt.toml`
- TypeScript uses strict mode with `noUncheckedIndexedAccess: true`
- Tauri config files:
  - `src-tauri/tauri.conf.json` - Base config
  - `src-tauri/tauri.development.conf.json` - Dev overrides
  - `src-tauri/tauri.release.conf.json` - Release overrides
  - `src-tauri/tauri.linux.conf.json` - Linux-specific settings

## Plugin Development

Plugins extend Yaak's functionality. To create a plugin:
1. Use `@yaakapp/api` from `plugin-runtime-types` for TypeScript definitions
2. Define plugin metadata and export hook functions
3. Plugins can provide: auth methods, template functions, importers, filters, request actions
4. See existing plugins in `plugins/` for examples

## Database Schema

The database schema is defined through migrations. Key tables:
- `workspaces` - Top-level containers
- `folders` - Hierarchical request organization
- `http_requests`, `grpc_requests`, `websocket_requests` - Request definitions
- `http_responses`, `grpc_connections`, `websocket_connections` - Response/connection history
- `environments` - Variable collections
- `key_values` - Environment variable key-value pairs
- `plugins` - Installed plugins
- `settings` - Global application settings
- `workspace_meta` - Workspace-specific metadata (active IDs, etc.)
