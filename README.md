# APIForge

<p align="center">
  <img width="200px" src="./assets/logo.png" alt="APIForge Logo">
</p>

<h1 align="center">
  ⚡ APIForge - AI-Powered API Builder ⚡
</h1>

<p align="center">
    A web-based platform for designing, testing, and deploying APIs using natural language and AI assistance.
</p>

<p align="center">
  <strong>Built for developers, startups, and enterprises who want to move fast.</strong>
</p>

---

## 🚀 What is APIForge?

APIForge is a modern web-based API development platform that combines the power of traditional API clients with AI-driven automation. Design, test, and deploy APIs using natural language, with intelligent code generation, automated testing, and deployment assistance.

### Key Features

#### 🤖 AI-Powered Development
- **Natural Language API Design**: Describe your API in plain English, get OpenAPI specs
- **Intelligent Code Generation**: Generate API implementations in Node.js, Python, Go, and more
- **Automated Test Creation**: AI generates comprehensive test suites from your API specs
- **Smart Error Analysis**: Get instant debugging suggestions and fixes
- **Documentation Generator**: Auto-generate beautiful API documentation

#### 🌐 Universal API Support
- **REST APIs**: Full HTTP client with advanced features
- **GraphQL**: Query builder and introspection
- **gRPC**: Built-in reflection and streaming support
- **WebSocket**: Real-time connection testing
- **Server-Sent Events (SSE)**: Event stream monitoring

#### 🔐 Enterprise-Ready Security
- **OAuth 2.0, JWT, Basic Auth**: Built-in authentication methods
- **Custom Auth Plugins**: Extend with your own authentication
- **Encrypted Secrets**: Secure storage for sensitive values
- **Environment Variables**: Separate dev, staging, and production configs

#### 🧩 Extensible Plugin System
- **Template Functions**: Insert dynamic values (UUIDs, timestamps, etc.)
- **Authentication Plugins**: Custom auth methods
- **Importers**: Import from Postman, Insomnia, OpenAPI, Curl
- **Filters**: JSONPath and XPath for response inspection
- **Custom Plugins**: Build your own extensions

#### ☁️ Cloud-Native & Collaborative
- **Team Workspaces**: Collaborate with your team in real-time
- **Version Control**: Git-based workspace syncing
- **Cloud Deployment**: One-click deployment to AWS, Vercel, Railway, and more
- **Scheduled Requests**: Monitoring and automated testing
- **Webhook Support**: Integrate with your CI/CD pipeline

---

## 🎯 Use Cases

### For Startups
- Rapidly prototype and iterate on API designs
- Generate production-ready code from specifications
- Automated testing reduces QA time
- Deploy to cloud providers with minimal configuration

### For Enterprises
- Standardize API development across teams
- Enforce security and compliance policies
- Audit trails and usage analytics
- SSO and role-based access control

### For Individual Developers
- Learn API design best practices
- Quickly test and debug APIs
- Build side projects faster
- Create professional documentation

---

## 🏗️ Architecture

APIForge is built on modern web technologies:

- **Frontend**: React + TypeScript + Vite
- **State Management**: Jotai + TanStack Query
- **Routing**: TanStack Router
- **Plugin System**: Node.js runtime with hot-reloading
- **Database**: SQLite/PostgreSQL (flexible)
- **AI Integration**: OpenAI, Anthropic Claude, or custom LLMs

### Plugin Architecture

The plugin system (inherited from the original Yaak codebase) allows for extensive customization:

```typescript
// Example: Custom Authentication Plugin
export default {
  name: 'custom-auth',
  version: '1.0.0',

  async getAuthenticationConfig(args) {
    return {
      inputs: [
        { key: 'apiKey', label: 'API Key', type: 'text' },
      ],
    };
  },

  async authenticateRequest(args) {
    const { apiKey } = args.inputs;
    return {
      headers: [
        { name: 'X-API-Key', value: apiKey },
      ],
    };
  },
};
```

---

## 🚀 Getting Started

### Development

```bash
# Clone the repository
git clone https://github.com/yourusername/apiforge.git
cd apiforge

# Install dependencies
npm install

# Start development server
npm run dev
```

The app will be available at `http://localhost:5173`

### Building

```bash
# Build all packages and plugins
npm run build

# Run tests
npm test

# Lint code
npm run lint
```

---

## 🔌 Plugin Development

APIForge supports custom plugins for authentication, template functions, importers, and more.

```bash
# Create a new plugin
cd plugins
mkdir my-custom-plugin
cd my-custom-plugin

# Initialize plugin
npm init -y
```

See the [Plugin Development Guide](./docs/PLUGINS.md) for detailed instructions.

---

## 🗺️ Roadmap

### Phase 1: Web Conversion (In Progress)
- [x] Remove desktop dependencies
- [x] Rebrand to APIForge
- [ ] Create web-based backend API
- [ ] Migrate database layer
- [ ] User authentication

### Phase 2: AI Integration
- [ ] LLM service abstraction layer
- [ ] Natural language API spec generation
- [ ] Intelligent test generation
- [ ] Code generation for multiple languages
- [ ] Error analysis and suggestions

### Phase 3: Cloud & Deployment
- [ ] Cloud provider integrations (AWS, Vercel, Railway)
- [ ] CI/CD pipeline generation
- [ ] Automated deployment workflows
- [ ] Monitoring and alerting

### Phase 4: Collaboration
- [ ] Real-time team workspaces
- [ ] Comment threads and reviews
- [ ] Version history and rollback
- [ ] Role-based access control

---

## 📦 Project Structure

```
apiforge/
├── packages/
│   ├── common-lib/           # Shared utilities
│   ├── plugin-runtime/       # Plugin execution engine
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

---

## 🤝 Contributing

APIForge is open source! We welcome contributions for bug fixes.

**Note**: This project accepts bug fix contributions only. Feature contributions should be discussed in issues first.

### Development Setup

See [DEVELOPMENT.md](./DEVELOPMENT.md) for detailed setup instructions.

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](./LICENSE) file for details.

**Note**: This is a fork of [Yaak](https://github.com/mountain-loop/yaak), adapted for web-based use with AI capabilities.

---

## 🙏 Acknowledgments

- Built on the foundation of [Yaak](https://yaak.app) by Gregory Schier
- Inspired by Postman, Insomnia, and Bruno
- Thanks to the open-source community

---

## 📞 Contact & Support

- **Documentation**: Coming soon
- **Issues**: [GitHub Issues](https://github.com/yourusername/apiforge/issues)
- **Discussions**: [GitHub Discussions](https://github.com/yourusername/apiforge/discussions)

---

<p align="center">
  <strong>⚡ Build APIs at the speed of thought ⚡</strong>
</p>
