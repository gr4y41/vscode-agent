# AgentForge Development Guide

## Introduction

Welcome to the AgentForge development team! This guide provides comprehensive information on setting up your development environment, understanding the codebase structure, following our development workflow, and contributing high-quality code to the project.

AgentForge is an advanced development environment for AI agents, built on a customized fork of Visual Studio Code. Our mission is to provide developers with intuitive tools for creating, visualizing, debugging, and orchestrating agent-based systems.

## Getting Started

### System Requirements

- **Operating System**: Windows 10/11, macOS 10.15+, or Linux (Ubuntu 20.04+ recommended)
- **Memory**: 8GB RAM minimum, 16GB+ recommended
- **Storage**: 2GB free space minimum
- **Node.js**: v16.x or higher
- **Git**: 2.25.0 or higher
- **Visual Studio Code**: Latest stable version (for development)

### Development Environment Setup

#### 1. Clone the Repository

```bash
git clone https://github.com/agentforge/agentforge.git
cd agentforge
```

#### 2. Install Dependencies

```bash
# Install global dependencies
npm install -g yarn typescript vsce

# Install project dependencies
yarn install
```

#### 3. Setup Development Environment Variables

```bash
# Copy the example environment file
cp .env.example .env

# Edit the .env file with your configuration
# Required for LLM integrations and other services
```

#### 4. Build the Project

```bash
# Build the core components
yarn build

# Watch mode for development
yarn watch
```

#### 5. Run for Development

```bash
# Launch the development instance
yarn dev
```

### Repository Structure

```
agentforge/
├── .github/                # GitHub configuration
├── .vscode/                # VS Code settings
├── build/                  # Build scripts and configuration
├── extensions/             # AgentForge-specific extensions
│   ├── agent-graph/        # Graph visualization extension
│   ├── agent-runtime/      # Agent execution runtime
│   ├── memory-inspector/   # Memory inspection tools
│   └── llm-integration/    # LLM service integrations
├── src/                    # Core VSCode fork source code
│   ├── vs/                 # VS Code core modules
│   └── agent-core/         # AgentForge core modules
├── test/                   # Test suites
│   ├── unit/               # Unit tests
│   ├── integration/        # Integration tests
│   └── e2e/                # End-to-end tests
├── resources/              # Static resources
├── scripts/                # Utility scripts
├── docs/                   # Documentation
└── samples/                # Sample agent projects
```

## Development Workflow

### Branching Strategy

We follow a GitFlow-inspired branching strategy:

- `main`: Stable release branch
- `develop`: Main development branch
- `feature/<name>`: Feature branches
- `bugfix/<name>`: Bug fix branches
- `release/<version>`: Release preparation branches
- `hotfix/<name>`: Hotfix branches

### Development Cycle

1. **Issue Assignment**: Pick an issue from the issue tracker or create one for new work
2. **Branch Creation**: Create a new branch from `develop`
3. **Implementation**: Write code and tests
4. **Local Testing**: Ensure all tests pass
5. **Pull Request**: Create a PR to merge into `develop`
6. **Code Review**: Address feedback from reviewers
7. **Merge**: Once approved, the PR will be merged
8. **Release**: Periodically, `develop` will be merged into `main` for releases

### Commit Guidelines

Follow the Conventional Commits specification:

- `feat`: A new feature
- `fix`: A bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, etc.)
- `refactor`: Code changes that neither fix bugs nor add features
- `test`: Adding or modifying tests
- `chore`: Changes to build process or tools

Example:
```
feat(graph-editor): add zoom controls to canvas

Add zoom in/out buttons and mouse wheel support for the graph canvas.
Implements #123.
```

### Pull Request Process

1. Create a PR with a clear title following commit guidelines
2. Fill out the PR template with:
   - Issue reference
   - Description of changes
   - Testing performed
   - Screenshots/videos if applicable
3. Request reviews from appropriate team members
4. Address all comments and requested changes
5. Ensure CI checks pass
6. Once approved, maintainers will merge the PR

## Coding Standards

### General Guidelines

- Write clean, readable, and maintainable code
- Follow the Single Responsibility Principle
- Keep functions and methods small and focused
- Use meaningful variable and function names
- Comment complex logic, but prefer self-documenting code
- Add JSDoc comments for public APIs

### TypeScript Guidelines

- Use TypeScript for all new code
- Enable strict type checking
- Prefer interfaces over type aliases for object shapes
- Use enums for related constants
- Leverage generics for reusable components
- Avoid `any` types except where absolutely necessary
- Use optional chaining and nullish coalescing

### React Guidelines

- Use functional components with hooks
- Maintain a clear separation of state and UI
- Keep components small and focused
- Use React context for shared state
- Implement proper error boundaries
- Follow accessibility best practices

### CSS/Styling Guidelines

- Use CSS modules for component styles
- Follow BEM naming convention for classes
- Maintain a consistent color palette
- Ensure responsive design
- Support light and dark themes
- Consider high-contrast mode for accessibility

## Architecture Guidelines

### Component Design

- Follow the component-based architecture
- Design for extensibility and reusability
- Use dependency injection where appropriate
- Maintain clear boundaries between components
- Document component interfaces thoroughly

### State Management

- Use appropriate state management for the scope:
  - Component state: `useState`
  - Complex component state: `useReducer`
  - Application state: Custom context or state management library
- Keep state normalized to avoid inconsistencies
- Document state shape and transitions

### Performance Considerations

- Optimize rendering performance
- Implement virtualization for large data sets
- Use memoization for expensive calculations
- Lazy-load components and resources
- Profile and optimize critical paths

## Testing Guidelines

### Test Coverage Expectations

- Aim for 80%+ code coverage
- 100% coverage for critical paths
- All public APIs must have tests
- All bug fixes must include tests

### Unit Testing

- Use Jest for unit testing
- Mock external dependencies
- Focus on testing behavior, not implementation
- Keep tests small and focused
- Use descriptive test names

Example:
```typescript
describe('MemoryInspector', () => {
  describe('getValue', () => {
    it('should return the value at the specified path', () => {
      // Test implementation
    });

    it('should return undefined for non-existent paths', () => {
      // Test implementation
    });
  });
});
```

### Integration Testing

- Test component interactions
- Verify API integrations
- Test workflows across components
- Use realistic test data

### End-to-End Testing

- Use Playwright for E2E tests
- Test critical user workflows
- Verify UI interactions
- Test across supported platforms

## Documentation Guidelines

### Code Documentation

- Add JSDoc comments for all public APIs
- Document complex algorithms with explanations
- Include usage examples for components
- Keep documentation up-to-date with code changes

### User Documentation

- Update user docs for new features
- Include screenshots and examples
- Organize by user tasks
- Write in clear, simple language

### Architecture Documentation

- Maintain high-level architecture diagrams
- Document component interactions
- Update data flow diagrams
- Document design decisions and trade-offs

## Extensions Development

### Creating a New Extension

1. Use the extension template:
   ```bash
   yarn create-extension my-extension
   ```

2. Implement the extension interface:
   ```typescript
   import { Extension } from 'agent-core';

   export class MyExtension implements Extension {
     // Implementation
   }
   ```

3. Register the extension in the manifest:
   ```json
   {
     "name": "my-extension",
     "displayName": "My Extension",
     "description": "Description of my extension",
     "version": "0.1.0",
     "engines": {
       "agentforge": "^1.0.0"
     },
     "main": "./dist/extension.js",
     "activationEvents": [
       "onCommand:my-extension.activate"
     ]
   }
   ```

### Extension API Guidelines

- Use the provided extension points
- Follow the extension lifecycle
- Handle activation and deactivation properly
- Document extension capabilities
- Provide clear error messages

## Debugging AgentForge

### Debugging the Core

1. Launch the development instance with debugging:
   ```bash
   yarn debug
   ```

2. Attach VS Code debugger to the running instance
3. Set breakpoints in the source code
4. Use the Debug Console for evaluation

### Debugging Extensions

1. Set `"debug": true` in the extension manifest
2. Add breakpoints in the extension code
3. Launch with extension debugging:
   ```bash
   yarn debug-extensions
   ```
4. Check the Extension Development Host output

### Common Issues and Solutions

- **Extension not loading**: Check activation events and manifest
- **UI not updating**: Verify React component rendering cycle
- **Performance issues**: Use the Performance panel in DevTools
- **Memory leaks**: Watch for event listener cleanup

## Release Process

### Version Numbering

We follow Semantic Versioning (SemVer):
- Major (X.0.0): Incompatible API changes
- Minor (0.X.0): New features (backward compatible)
- Patch (0.0.X): Bug fixes and minor improvements

### Release Checklist

1. Create a release branch from `develop`
2. Update version numbers
3. Generate changelog
4. Run full test suite
5. Create release build
6. Verify packaging
7. Create GitHub release
8. Merge to `main`
9. Publish to distribution channels

### Hotfix Process

1. Create hotfix branch from `main`
2. Implement fix with tests
3. Update patch version
4. Create PR to both `main` and `develop`
5. Release after approval

## Contributing to the Project

### Finding Issues to Work On

- Check the [Issues](https://github.com/agentforge/agentforge/issues) page
- Look for issues tagged `good-first-issue` or `help-wanted`
- Join discussions in the community forum

### Submitting Feature Requests

- Use the feature request template
- Provide clear use cases
- Explain the value proposition
- Consider implementation complexity

### Reporting Bugs

- Use the bug report template
- Provide steps to reproduce
- Include expected and actual behavior
- Attach screenshots or videos if applicable
- Include environment information

### Community Communication

- Join our [Discord server](https://discord.gg/agentforge)
- Follow the [AgentForge Twitter](https://twitter.com/agentforge)
- Participate in weekly developer meetings
- Review open pull requests

## Performance Optimization

### Profiling Tools

- VS Code built-in CPU profiler
- Chrome DevTools for renderer process
- Memory Heap Snapshots
- Extension Host profiling

### Common Performance Issues

- Excessive rendering
- Large state objects
- Unoptimized graph rendering
- Memory leaks from event listeners
- Synchronous operations blocking the UI thread

### Optimization Techniques

- Use React.memo for pure components
- Implement virtualization for lists and graphs
- Optimize LLM API calls with caching
- Use web workers for CPU-intensive tasks
- Lazy load components and resources

## Accessibility Guidelines

- Support keyboard navigation for all features
- Ensure proper contrast ratios
- Add aria attributes for screen readers
- Test with screen readers
- Support high contrast mode
- Allow disabling animations

## Security Guidelines

### Secure Coding Practices

- Validate all inputs
- Sanitize data before rendering
- Use safe APIs for file system operations
- Implement proper error handling
- Follow the principle of least privilege

### API Key Management

- Never commit API keys to the repository
- Use environment variables for local development
- Implement secure storage for user credentials
- Support encrypted configuration

### Dependency Management

- Regularly update dependencies
- Use dependency scanning tools
- Review security advisories
- Minimize use of third-party libraries

## Continuous Integration

### CI Pipeline

- Automated builds on pull requests
- Unit and integration tests
- Linting and code style checks
- Security scans
- Performance benchmarks

### Quality Gates

- All tests must pass
- Code coverage must not decrease
- No new linting errors
- No security vulnerabilities
- Performance regression tests pass

## Additional Resources

- [VSCode Extension API Documentation](https://code.visualstudio.com/api)
- [TypeScript Documentation](https://www.typescriptlang.org/docs/)
- [React Documentation](https://reactjs.org/docs/getting-started.html)
- [Jest Testing Guide](https://jestjs.io/docs/getting-started)
- [Electron Documentation](https://www.electronjs.org/docs)

## Troubleshooting

### Build Issues

- **Error: Cannot find module**: Ensure dependencies are installed with `yarn install`
- **TypeScript errors**: Check TypeScript version compatibility
- **Out of memory**: Increase Node.js memory limit with `NODE_OPTIONS=--max_old_space_size=4096`

### Runtime Issues

- **Extension not activating**: Check activation events in `package.json`
- **UI not rendering**: Check for React rendering errors in console
- **Unexpected behavior**: Enable verbose logging with `AGENTFORGE_LOG=verbose`

## FAQ

**Q: How do I debug the VSCode core?**
A: Use the `yarn debug-core` command and attach the debugger to the main process.

**Q: How can I test my extension without rebuilding everything?**
A: Use `yarn watch` in one terminal and `yarn test-extension my-extension` in another.

**Q: How do I add a new dependency?**
A: Use `yarn add <package>` for runtime dependencies or `yarn add -D <package>` for development dependencies.

**Q: How can I contribute documentation?**
A: Documentation is in the `docs/` folder. Submit changes via PRs following the same process as code changes.

## Contact

- **GitHub Issues**: For bugs and feature requests
- **Discord**: For community discussions
- **Email**: dev@agentforge.ai for private communications

Thank you for contributing to AgentForge! Together, we're building the future of agent development.
