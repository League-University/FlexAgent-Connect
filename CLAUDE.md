# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Common Commands

### Development
- `pnpm install` - Install dependencies
- `pnpm dev` - Start development server with live reloading
- `pnpm dev:firefox` - Start development server for Firefox
- `pnpm build` - Build for production (Chrome)
- `pnpm build:firefox` - Build for Firefox
- `pnpm zip` - Create distributable zip package

### Code Quality
- `pnpm lint` - Run ESLint with auto-fix
- `pnpm lint:fix` - Run ESLint with aggressive fixes
- `pnpm prettier` - Format code with Prettier
- `pnpm type-check` - Run TypeScript type checking

### Testing & Validation
- `pnpm e2e` - Run end-to-end tests
- `pnpm e2e:firefox` - Run Firefox-specific e2e tests

### Utility Commands
- `pnpm clean` - Clean all build artifacts and node_modules
- `pnpm clean:bundle` - Clean only build outputs
- `pnpm update-version` - Update version across all packages
- `pnpm set-global-env` - Set global environment variables
- `pnpm module-manager` - Start module manager tool

## Architecture Overview

FlexAgent Connect is a browser extension that connects AI platforms to FlexAgent servers through agent-centric identity management. Unlike traditional tool-centric approaches, FlexAgent Connect focuses on managing OpenID-based agent identities that correspond to specialized Linux user accounts on FlexAgent servers. The codebase uses a modern, modular architecture with TypeScript and React.

### Key Architectural Components

#### 1. Plugin System (`pages/content/src/plugins/`)
- **Plugin Registry**: Centralized management of website adapters
- **Site-Specific Adapters**: Tailored integrations for different AI platforms
- **Event-Driven Architecture**: Real-time communication between components
- **Capabilities**: Text insertion, form submission, URL navigation, DOM manipulation

#### 2. Core Services (`pages/content/src/core/`)
- **Main Initializer**: Complete application initialization sequence
- **Circuit Breaker**: Prevents cascading failures with automatic recovery
- **Context Bridge**: Cross-context communication for Chrome extension
- **Error Handler**: Centralized error handling and reporting
- **Performance Monitor**: Tracks performance metrics and memory usage

#### 3. State Management (`pages/content/src/stores/`)
- **Zustand Stores**: Lightweight state management for different domains
- **Adapter Store**: Plugin and adapter state
- **Connection Store**: MCP server connection management
- **Tool Store**: MCP tool execution state
- **UI Store**: User interface state

#### 4. Event System (`pages/content/src/events/`)
- **Event Bus**: Type-safe event communication
- **Event Handlers**: Centralized event processing
- **Event Types**: Comprehensive TypeScript definitions

#### 5. Website Adapters (`pages/content/src/components/websites/`)
- **ChatGPT**: OpenAI ChatGPT integration
- **Gemini**: Google Gemini integration
- **Perplexity**: Perplexity AI integration
- **Grok**: xAI Grok integration
- **AI Studio**: Google AI Studio integration
- **OpenRouter**: OpenRouter Chat integration
- **DeepSeek**: DeepSeek Chat integration

### Package Structure

This is a monorepo using pnpm workspaces with Turbo for build orchestration:

- `chrome-extension/` - Chrome extension manifest and background scripts
- `pages/content/` - Main content script application
- `pages/content/src/render_prescript/` - Function result rendering system
- `packages/` - Shared packages and utilities

### Technologies Used

- **TypeScript**: Type safety and modern JavaScript features
- **React 19**: Modern UI framework with concurrent features
- **Zustand**: Lightweight state management
- **Vite**: Fast build tool and development server
- **ESLint**: Code linting with TypeScript support
- **Prettier**: Code formatting
- **Turbo**: Monorepo build orchestration
- **Firebase**: Remote configuration and analytics

### Development Workflow

1. **Plugin Development**: Create site-specific adapters extending `BaseAdapterPlugin`
2. **Event Integration**: Use the event bus for cross-component communication
3. **State Management**: Use Zustand stores for persistent state
4. **Error Handling**: Leverage circuit breaker and error handler systems
5. **Testing**: Use the built-in debug utilities and console tools

### Chrome Extension Structure

The extension follows Chrome's Manifest V3 architecture:
- **Background Script**: Service worker for extension lifecycle
- **Content Script**: Main application injected into web pages
- **Popup**: Extension popup interface
- **Options**: Extension settings page

### FlexAgent Server Integration

The extension connects to FlexAgent servers through:
- **Agent Identity Management**: OpenID-based identity selection and management
- **SSH Authentication**: Secure connection using OpenID credentials
- **Universal Bash Tool**: All operations use a single bash tool interface
- **I/O Forwarding**: Seamless forwarding of LLM output and server responses

### Build System

Uses Turbo for efficient monorepo builds:
- Parallel execution across packages
- Dependency-aware build order
- Incremental builds with caching
- Environment-specific configurations

## Development Notes

- Always run `pnpm type-check` before committing to ensure type safety
- Use the plugin system's event bus for cross-component communication
- Leverage the circuit breaker pattern for error-prone operations
- Use the debug utilities in development: `window._appDebug`
- Follow the existing adapter patterns when adding new platform support
- Ensure proper cleanup in plugin lifecycle methods
- Focus on agent identity management rather than individual tool management
- All server communication should use the universal bash tool interface

## Environment Variables

The project uses several environment variables prefixed with `CEB_*` and `CLI_CEB_*`:
- `CLI_CEB_DEV` - Development mode flag
- `CLI_CEB_FIREFOX` - Firefox build flag
- Other environment variables are set via `bash-scripts/set_global_env.sh`