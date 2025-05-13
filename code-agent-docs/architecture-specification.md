# AgentForge Architecture Specification

## System Overview

AgentForge is a specialized development environment built on a VSCode fork, designed for creating and managing AI agents. This document outlines the system's architecture, components, interfaces, and data models.

## Extension-Based Architecture

Drawing inspiration from the AI-CODING-PLATFORMS extensions in VSCode, AgentForge adopts a modular extension-based architecture. Rather than implementing all functionality in a monolithic codebase, we separate concerns into distinct extensions with well-defined responsibilities:

1. **agentforge-core**: Base VSCode fork and extension infrastructure
2. **agentforge-graph**: Visual graph editor and node manipulation
3. **agentforge-runtime**: Agent execution environment and lifecycle management
4. **agentforge-memory**: Memory system, persistence, and visualization
5. **agentforge-navigation**: Code-graph bidirectional linking and synchronization

This approach enables parallel development by different engineers while maintaining clean interfaces between components.

## High-Level Architecture

```
+---------------------------------------------+
|                   AgentForge                |
+---------------------------------------------+
|                                             |
|  +-----------+      +-------------------+   |
|  |           |      |                   |   |
|  | VSCode    |<---->| Agent Extensions  |   |
|  | Core      |      |                   |   |
|  |           |      +-------------------+   |
|  +-----------+                              |
|       ^                                     |
|       |          +-------------------+      |
|       |          |                   |      |
|       +--------->| Graph Editor      |      |
|       |          |                   |      |
|       |          +-------------------+      |
|       |                                     |
|       |          +-------------------+      |
|       |          |                   |      |
|       +--------->| Agent Runtime     |      |
|       |          |                   |      |
|       |          +-------------------+      |
|       |                                     |
|       |          +-------------------+      |
|       |          |                   |      |
|       +--------->| Memory System     |      |
|       |          |                   |      |
|       |          +-------------------+      |
|       |                                     |
|       |          +-------------------+      |
|       |          |                   |      |
|       +--------->| LLM Integration   |      |
|                  |                   |      |
|                  +-------------------+      |
|                                             |
+---------------------------------------------+
```

## Inter-Extension Communication

Extensions communicate through a standardized message bus, allowing for:
- Event-based communication
- Service registration and discovery
- Consistent error handling
- Telemetry and logging

All extensions activate on startup (`onStartupFinished` activation event) to ensure immediate availability, with core services initialized in the proper sequence.

## Core Components

### 1. VSCode Fork Foundation (agentforge-core)

**Purpose:** Provide the base IDE environment with necessary hooks for agent integration.

**Key Modifications:**
- Custom extension points for agent UI components
- Specialized message passing system for agent events
- Integration hooks for graph visualization
- Custom sidebar panel management

**Technical Stack:**
- Electron
- TypeScript
- VSCode Extension API

**Responsibilities:**
- File editing and project management
- Extension loading and lifecycle management
- UI rendering and event handling
- Terminal and debug console integration

### 2. Graph Editor (agentforge-graph)

**Purpose:** Visualize and manipulate agent components, relationships, and workflows.

**Subcomponents:**
- Canvas Renderer
- Node Management System
- Edge Controller
- Property Panel
- Navigation Service

**Technical Stack:**
- Cytoscape.js / D3.js
- React for component UI
- Custom layout algorithms

**Responsibilities:**
- Render interactive graph of agents and workflows
- Handle node/edge creation, deletion, and modification
- Manage visual properties and layout
- Synchronize with code representation
- Support zoom, pan, and selection

### 3. Agent Runtime (agentforge-runtime)

**Purpose:** Execute and monitor agent behaviors within a controlled environment.

**Subcomponents:**
- Sandbox Manager
- Agent Lifecycle Controller
- I/O Routing System
- Execution Scheduler
- Debugging Interface

**Technical Stack:**
- Node.js sandboxed processes
- Message passing infrastructure
- Event emitter pattern

**Responsibilities:**
- Launch and terminate agents
- Route messages between agents
- Monitor resource usage
- Collect telemetry and logs
- Support debugging operations

### 4. Memory System (agentforge-memory)

**Purpose:** Store, retrieve, and visualize agent memory states.

**Subcomponents:**
- Memory Store
- Query Engine
- Visualization Components
- History Tracker
- Vector Storage (optional)

**Technical Stack:**
- IndexedDB
- Optional vector DB integration (e.g., Chroma)
- JSON document storage

**Responsibilities:**
- Persist agent memory between sessions
- Index memory for fast retrieval
- Track memory changes over time
- Visualize memory structure and content
- Support editing and overriding memory

### 5. LLM Integration

**Purpose:** Connect to language models for assistance and agent simulation.

**Subcomponents:**
- Provider Manager
- Request Optimizer
- Caching Layer
- Token Counter
- Prompt Template Engine

**Technical Stack:**
- Provider-specific APIs (OpenAI, Claude, etc.)
- Local model support (optional)
- Caching infrastructure

**Responsibilities:**
- Send requests to language models
- Manage API keys and quotas
- Optimize for cost and latency
- Handle fallbacks for offline use
- Apply prompt engineering patterns

### 6. Agent Shadow Workspace

**Purpose:** Provide a sandbox environment for agent operations.

**Subcomponents:**
- Hidden workspace manager
- Workspace state synchronization
- Change tracking and visualization
- Result presentation interface

**Technical Stack:**
- VSCode WebView API
- File system operations
- Diff visualization tools

**Responsibilities:**
- Create isolated copies of workspace files
- Allow agents to safely modify code
- Track changes made by agents
- Present changes to user for review
- Support accepting, rejecting, or modifying changes

## Data Models

### Agent Model

```typescript
interface Agent {
  id: string;
  name: string;
  description: string;
  type: AgentType;
  sourceFile: string;
  memoryIds: string[];
  capabilities: Capability[];
  configuration: Record<string, any>;
  metadata: Record<string, any>;
}

enum AgentType {
  Autonomous,
  Assistant,
  Tool,
  Orchestrator,
  Custom
}

interface Capability {
  id: string;
  name: string;
  description: string;
  parameters: Parameter[];
  returnType: string;
  sourceLocation: SourceLocation;
}
```

### Memory Model

```typescript
interface Memory {
  id: string;
  agentId: string;
  type: MemoryType;
  data: any;
  createdAt: number;
  updatedAt: number;
  ttl?: number;
  metadata: Record<string, any>;
}

enum MemoryType {
  ShortTerm,
  LongTerm,
  Episodic,
  Semantic,
  Procedural,
  Vector
}
```

### Workflow Model

```typescript
interface Workflow {
  id: string;
  name: string;
  description: string;
  nodes: WorkflowNode[];
  edges: WorkflowEdge[];
  metadata: Record<string, any>;
}

interface WorkflowNode {
  id: string;
  type: NodeType;
  position: Position;
  data: any;
  agentId?: string;
  memoryId?: string;
  actionId?: string;
}

interface WorkflowEdge {
  id: string;
  source: string;
  target: string;
  label?: string;
  condition?: string;
}
```

### Source Location Model

```typescript
interface SourceLocation {
  file: string;
  startLine: number;
  endLine: number;
  startColumn: number;
  endColumn: number;
}
```

## Component Interactions

### Agent Creation Flow

1. User creates new agent via UI or template
2. Graph Editor adds agent node to canvas
3. Agent Extensions generate code scaffold in source file
4. Memory System initializes empty memory store for agent
5. Bi-directional binding established between code and graph

### Agent Execution Flow

1. User triggers agent execution via UI
2. Agent Runtime creates sandbox environment
3. Agent code loaded and initialized
4. Runtime routes I/O and monitors execution
5. Memory System captures and persists state changes
6. UI updates with execution status and outputs

### LLM Assistance Flow

1. User requests assistance in code or graph
2. Context collector gathers relevant information
3. Prompt template applied to context
4. Request sent to LLM provider
5. Response processed and applied to code or graph
6. User reviews and accepts/modifies changes

### Shadow Workspace Flow

1. User initiates agent-based code modification
2. System creates shadow copy of relevant files
3. Agent performs modifications in shadow workspace
4. System generates diff between original and modified files
5. User reviews changes with diff visualization
6. User accepts, rejects, or modifies proposed changes
7. Accepted changes are applied to the actual workspace

## Extension Integration

Each extension integrates with VSCode through well-defined contribution points:

### Commands and Menus

```json
"contributes": {
  "commands": [
    {
      "command": "agentforge.graph.createNode",
      "title": "Create Agent Node",
      "category": "AgentForge"
    }
  ],
  "menus": {
    "explorer/context": [
      {
        "command": "agentforge.graph.createNodeFromFile",
        "when": "resourceLangId == javascript || resourceLangId == typescript",
        "group": "agentforge"
      }
    ]
  }
}
```

### Configuration

```json
"contributes": {
  "configuration": {
    "title": "AgentForge",
    "properties": {
      "agentforge.runtime.enableShadowWorkspace": {
        "type": "boolean",
        "default": true,
        "description": "Enable sandbox environment for agent operations"
      }
    }
  }
}
```

### Views and Panels

```json
"contributes": {
  "views": {
    "agentforge": [
      {
        "id": "agentforgeAgents",
        "name": "Agents"
      },
      {
        "id": "agentforgeMemory",
        "name": "Memory"
      }
    ]
  },
  "viewsContainers": {
    "activitybar": [
      {
        "id": "agentforge",
        "title": "AgentForge",
        "icon": "resources/agentforge.svg"
      }
    ]
  }
}
```

## Storage and Persistence

### Local Storage

- Agent definitions stored as code files in project
- Graph layouts saved in `.agentforge` directory
- User preferences in VSCode settings
- Agent memory in IndexedDB

### Optional Remote Storage

- Template library on cloud service
- Shared agent configurations
- Vector memory in managed service
- Analytics and telemetry data

## Security Considerations

### Agent Sandboxing

- Execution in isolated processes
- Resource limits on CPU, memory, and network
- Permission system for sensitive operations

### LLM Data Privacy

- Local API key management
- Option to use local models only
- Data minimization in LLM requests
- Clear policies on data retention

## Extension Points

### Plugin API

- Custom agent types
- Specialized node renderers
- Memory storage adapters
- New LLM providers
- Custom agent templates

### Integration API

- External tools and services
- Continuous integration systems
- Deployment targets
- Analytics platforms

## Performance Considerations

### Graph Rendering

- Virtualization for large graphs
- Lazy loading of node details
- Optimized layout algorithms
- WebGL acceleration when available

### Agent Execution

- Parallel execution where possible
- Resource pooling for LLM requests
- Incremental memory updates
- Background processing for heavy operations

## Technical Debt Mitigation

- Regular sync with upstream VSCode
- Comprehensive test suite
- Clean separation of concerns
- Well-documented interfaces
- Feature flags for experimental features

## Implementation Notes

- Use TypeScript for all components
- Prefer composition over inheritance
- Use dependency injection for testability
- Follow reactive programming patterns where appropriate
- Document all public APIs
- Implement telemetry for performance monitoring

## Open Questions

- Integration strategy with specific agent frameworks (LangChain, AutoGen, etc.)
- Handling of very large agent systems (100+ agents)
- Cross-platform performance consistency
- Network requirements for offline use
- Merge strategy for multi-user edits
