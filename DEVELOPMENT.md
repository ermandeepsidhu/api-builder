# Developer Setup

APIForge is a web-based Node.js and TypeScript monorepo. The project includes a React frontend, plugin system powered by Node.js, and will eventually include a backend API for cloud features.

## Prerequisites

Make sure you have the following tools installed:

- [Node.js](https://nodejs.org/en/download/package-manager) (v18 or higher)
- [npm](https://www.npmjs.com/) (comes with Node.js)

Check the installations with the following commands:

```shell
node -v
npm -v
```

## Initial Setup

Install the NPM dependencies:

```shell
npm install
```

Run the `bootstrap` command to do some initial setup:

```shell
npm run bootstrap
```

## Run the App

After bootstrapping, start the app in development mode:

```shell
npm run dev
```

This will start the Vite dev server for the React frontend. The app will be available at `http://localhost:5173`

## Project Structure

```
apiforge/
├── packages/
│   ├── common-lib/           # Shared utilities
│   ├── plugin-runtime/       # Plugin execution engine (Node.js)
│   └── plugin-runtime-types/ # TypeScript types for plugins
├── plugins/                  # Built-in plugins
│   ├── auth-*/              # Authentication plugins
│   ├── importer-*/          # Import from other tools
│   ├── template-function-*/ # Dynamic template values
│   └── filter-*/            # Response filtering
├── src-web/                 # React frontend
│   ├── components/          # UI components
│   ├── hooks/              # React hooks
│   ├── lib/                # Utilities
│   └── main.tsx            # Entry point
└── scripts/                # Build and utility scripts
```

## Development Commands

### Building

```shell
npm run build                    # Build all workspace packages
npm run build-plugins            # Build plugins only
```

### Testing

```shell
npm test                         # Run tests in all workspaces
```

Individual plugins have their own test suites using vitest. To run tests for a specific plugin:

```shell
cd plugins/auth-bearer
npm test
```

### Linting

```shell
npm run lint                     # Lint all workspaces
```

Uses ESLint for TypeScript/JavaScript with the configuration in `eslint.config.cjs`.

## Plugin Development

Plugins extend APIForge's functionality. To create a new plugin:

1. Create a new directory in `plugins/`
2. Initialize with `npm init -y`
3. Add `@apiforge/api` as a dependency (from `plugin-runtime-types`)
4. Create an `index.ts` file with your plugin exports

Example plugin structure:

```typescript
// plugins/my-plugin/src/index.ts
import type { Plugin } from '@apiforge/api';

const plugin: Plugin = {
  name: 'my-plugin',
  version: '1.0.0',

  // Define your plugin hooks here
  async getAuthenticationConfig(args) {
    return {
      inputs: [
        { key: 'apiKey', label: 'API Key', type: 'text' },
      ],
    };
  },
};

export default plugin;
```

See existing plugins in `plugins/` for more examples.

## Technology Stack

- **Frontend**: React 19 + TypeScript + Vite
- **State Management**: Jotai + TanStack Query
- **Routing**: TanStack Router
- **Styling**: Tailwind CSS
- **Plugin Runtime**: Node.js with gRPC communication
- **Testing**: Vitest
- **Linting**: ESLint + Prettier

## Working with the Frontend

The frontend is a standard Vite + React application located in `src-web/`:

```shell
cd src-web
npm run dev      # Start dev server
npm run build    # Build for production
npm run lint     # Lint code
```

### Key Frontend Patterns

- **Hooks**: Most data fetching and mutations are wrapped in custom hooks (see `src-web/hooks/`)
- **State Management**: Jotai atoms for global state, TanStack Query for server state
- **Routing**: TanStack Router with file-based routing
- **Components**: Organized by feature in `src-web/components/`

## Common Issues

### Port Already in Use

If port 5173 is already in use:

```shell
# Kill the process using the port
lsof -ti:5173 | xargs kill -9

# Or specify a different port
npm run dev -- --port 3000
```

### Module Not Found Errors

If you see module resolution errors:

```shell
# Clean install
rm -rf node_modules package-lock.json
npm install
```

### Plugin Build Errors

If plugins fail to build:

```shell
# Rebuild all plugins
npm run build-plugins

# Or rebuild a specific plugin
cd plugins/auth-bearer
npm run build
```

## Future Backend Development

The project is being transitioned to include a backend API for cloud features. This will likely use:

- Next.js API routes, or
- Express.js, or
- Another Node.js framework

Database will be migrated from SQLite (desktop) to:

- PostgreSQL (recommended), or
- Turso (distributed SQLite), or
- Another cloud database

## Contributing

We welcome bug fix contributions! Please:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

**Note**: Feature contributions should be discussed in issues first.

## Additional Resources

- [Plugin Development Guide](./docs/PLUGINS.md) (coming soon)
- [Architecture Overview](./CLAUDE.md)
- [Rebranding Plan](./REBRANDING_PLAN.md)
