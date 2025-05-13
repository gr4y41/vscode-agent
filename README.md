# AgentForge

A specialized IDE for AI agent development based on VSCode with a modular extension architecture.

<p align="center">
  <img alt="AgentForge Logo" src="docs/images/agentforge-logo.png" width="256">
</p>

## Overview

AgentForge is a specialized IDE built on a VSCode fork that enables developers to create, visualize, debug, and deploy AI agents through a graph-based interface. It bridges the gap between code and visual agent development, providing a bidirectional synchronization system that keeps both representations in sync.

### Key Components

- **Core Platform**: VSCode fork with message bus and extension loading system
- **Graph Interface**: Visual editor for creating and managing agents
- **Agent Runtime**: Execution environment for agents with memory management
- **Navigation & Sync**: Bidirectional synchronization between code and graph
- **LLM & Tools**: AI integration and agent debugging capabilities

## Architecture

AgentForge uses a modular extension-based architecture:

- Each major component is implemented as a standalone VSCode extension
- Extensions communicate through a message bus
- All extensions activate on startup
- The system follows a strict separation of concerns

## Getting Started

### Prerequisites

- Node.js 16+
- Git
- Yarn

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/agentforge.git
cd agentforge

# Install dependencies
yarn install

# Build extensions
yarn build

# Start AgentForge
yarn start
```

## Development

AgentForge is organized around specific engineering roles, each responsible for different aspects of the system:

### Engineering Roles

- **Core Platform Engineer**: Responsible for VSCode fork, message bus, extension infrastructure
- **UI/Graph Engineer**: Responsible for graph visualization, node rendering, UI components
- **Agent Runtime Engineer**: Responsible for agent execution environment, memory systems
- **Integration & Sync Engineer**: Responsible for code-graph mappings, navigation features
- **LLM & Tooling Engineer**: Responsible for language model integration, debugging tools

### Development Workflows

For detailed guides on common development workflows, see the [development documentation](docs/development.md), which covers:

- Creating new extensions
- Implementing agent types
- Developing UI components
- Establishing cross-extension communication
- Testing and debugging

### Contributing

We welcome contributions to AgentForge! To contribute:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

All contributions should follow our [code style](docs/code-style.md) and include appropriate tests.

## Documentation

- [Architecture Overview](docs/architecture.md)
- [Project Structure](docs/project-structure.md)
- [Engineer Role Guides](docs/engineer-roles.md)
- [Shadow Workspace Concept](docs/shadow-workspace.md)
- [Error Handling Guidelines](docs/error-handling.md)
- [Testing Strategies](docs/testing-strategies.md)
- [Documentation Standards](docs/documentation-standards.md)
- [Collaboration Patterns](docs/collaboration-patterns.md)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE.txt) file for details.

## Acknowledgments

- Built on [Visual Studio Code](https://github.com/microsoft/vscode)
- Inspired by Cursor extensions architecture
- Special thanks to all contributors
