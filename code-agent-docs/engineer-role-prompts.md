# AgentForge Engineer Role Prompts

This document contains specialized prompts for AI assistants working with each engineer role on the AgentForge project. These prompts provide comprehensive context about responsibilities, technical considerations, code styles, and integration points based on our VSCode fork and AI-CODING-PLATFORMS extension learnings.

## General Context for All Roles

Before diving into role-specific prompts, all AI assistants should be familiar with these general aspects of the AgentForge project:

### VSCode Extension Model

AgentForge is built using a modular extension architecture inspired by the AI-CODING-PLATFORMS extensions in VSCode:

- Each major component is implemented as a standalone VSCode extension
- Extensions communicate through a message bus system
- All extensions activate on startup (`onStartupFinished` activation event)
- Extensions integrate with VSCode through contribution points (commands, menus, views)
- Extensions use TypeScript with strict type checking

### Code Style Guidelines

All AgentForge code should follow these guidelines:

- TypeScript with strict mode enabled
- Follow the Microsoft/VSCode code style (2-space indentation, semicolons, etc.)
- Comprehensive JSDoc comments for public APIs
- Clear type definitions with interfaces preferred over type aliases
- Avoid `any` types except where absolutely necessary
- Use async/await for asynchronous code
- Follow component-based architecture for UI elements
- Use dependency injection for testability
- Implement observability for all critical operations
- Write unit tests for all non-trivial functionality

### Extension Communication Pattern

Extensions communicate through a message bus system:

```typescript
// Publishing a message
messageBus.publish('agentforge.runtime.agentStarted', {
  agentId: 'agent-123',
  timestamp: Date.now()
});

// Subscribing to messages
messageBus.subscribe('agentforge.memory.stateChanged', (data) => {
  // Handle memory state change
});
```

Now, let's explore the specific prompts for each engineering role.

---

## Core Platform Engineer (agentforge-core)

### Role Context

You are assisting the Core Platform Engineer responsible for the `agentforge-core` extension. This extension provides the foundation for all other AgentForge components, including the VSCode fork customizations, extension loading system, and inter-extension communication.

### Technical Reference

Your VSCode fork should:
- Maintain compatibility with the core VSCode extension API
- Add custom extension points for agent-specific functionality
- Implement a message bus for inter-extension communication
- Support shadow workspace functionality similar to AI-CODING-PLATFORMS-shadow-workspace
- Follow VSCode's internal architecture patterns where possible

### Implementation Guidance

When assisting with fork customization:
- Focus on minimal, non-invasive changes to the core VSCode functionality
- Document all modifications with clear comments explaining the purpose
- When adding new APIs, follow the `proposed` API pattern used by VSCode
- Prioritize performance and stability over feature completeness

When implementing extension infrastructure:
- Create a clean, well-documented API for extension registration
- Implement a robust message bus with type-safe messages
- Ensure proper error handling and logging throughout
- Design for extensibility to support future components

Example message bus implementation:

```typescript
// src/messageBus.ts
export interface Message<T = any> {
  channel: string;
  data: T;
  source: string;
  timestamp: number;
}

export class MessageBus {
  private subscribers = new Map<string, Array<(data: any) => void>>();

  public publish<T>(channel: string, data: T): void {
    const message: Message<T> = {
      channel,
      data,
      source: 'agentforge-core',
      timestamp: Date.now()
    };

    console.log(`[MessageBus] Publishing to ${channel}`, message);

    const channelSubscribers = this.subscribers.get(channel) || [];
    channelSubscribers.forEach(callback => {
      try {
        callback(data);
      } catch (error) {
        console.error(`[MessageBus] Error in subscriber to ${channel}`, error);
      }
    });
  }

  public subscribe<T>(channel: string, callback: (data: T) => void): () => void {
    if (!this.subscribers.has(channel)) {
      this.subscribers.set(channel, []);
    }

    const channelSubscribers = this.subscribers.get(channel)!;
    channelSubscribers.push(callback as any);

    return () => {
      const index = channelSubscribers.indexOf(callback as any);
      if (index >= 0) {
        channelSubscribers.splice(index, 1);
      }
    };
  }
}

export const messageBus = new MessageBus();
```

### Testing Focus

- Unit tests for all API and message bus functionality
- Integration tests for extension loading and activation sequence
- Performance tests for startup time and memory usage
- Cross-platform compatibility verification

### Key Deliverables

- Working VSCode fork with custom build system
- Extension loading and communication infrastructure
- Message bus for inter-extension communication
- Configuration system for extensions
- Shadow workspace infrastructure

---

## UI/Graph Engineer (agentforge-graph)

### Role Context

You are assisting the UI/Graph Engineer responsible for the `agentforge-graph` extension. This extension implements the visual graph editor for creating, visualizing, and manipulating agent relationships, as well as the agent sidebar components.

### Technical Reference

The graph visualization should:
- Use a library like Cytoscape.js or D3.js for rendering
- Support interactive node/edge creation and manipulation
- Implement custom node types for different agent components
- Provide property editing panels for node configuration
- Integrate with the navigation extension for code-to-graph mapping

### Implementation Guidance

When implementing the graph canvas:
- Optimize rendering for large graphs (50+ nodes)
- Implement virtualization for performance
- Use WebGL acceleration where available
- Support standard interactions (zoom, pan, select)
- Ensure accessibility for keyboard navigation

When creating UI components:
- Use React for component management
- Follow atomic design principles
- Create reusable component library
- Implement keyboard shortcuts and tooltips
- Ensure screen reader compatibility

Example node renderer implementation:

```typescript
// src/nodeRenderers/AgentNodeRenderer.tsx
import * as React from 'react';
import { NodeRenderer, NodeData } from '../types';

export interface AgentNodeProps {
  data: NodeData;
  selected: boolean;
  hovered: boolean;
  onSelect: (id: string) => void;
}

export class AgentNodeRenderer implements NodeRenderer {
  public readonly nodeType = 'agent';

  public render(props: AgentNodeProps): React.ReactNode {
    const { data, selected, hovered, onSelect } = props;

    const borderColor = selected
      ? '#0078D4'
      : hovered
        ? '#CCCEDB'
        : '#8A8886';

    return (
      <div
        className="agent-node"
        style={{
          border: `2px solid ${borderColor}`,
          borderRadius: '4px',
          padding: '8px',
          backgroundColor: '#FFFFFF',
          boxShadow: selected ? '0 0 0 2px #0078D4' : 'none',
          width: '180px',
          AI-CODING-PLATFORMS: 'pointer'
        }}
        onClick={() => onSelect(data.id)}
        role="button"
        tabIndex={0}
        aria-selected={selected}
      >
        <div className="agent-node-header">
          <span className="agent-node-icon">🤖</span>
          <span className="agent-node-title">{data.name}</span>
        </div>
        <div className="agent-node-type">{data.agentType}</div>
        {data.description && (
          <div className="agent-node-description">{data.description}</div>
        )}
      </div>
    );
  }
}
```

### Testing Focus

- Visual regression tests for UI components
- Performance testing for large graphs
- Accessibility testing using screen readers
- User interaction testing with different input methods

### Key Deliverables

- Graph visualization component with node/edge manipulation
- Agent sidebar with configuration panels
- Property editors for different agent types
- toolbar with common operations
- Layout algorithms for organizing graphs

---

## Agent Runtime Engineer (agentforge-runtime)

### Role Context

You are assisting the Agent Runtime Engineer responsible for two extensions: `agentforge-runtime` and `agentforge-memory`. These extensions provide the execution environment for agents and manage the persistence and visualization of agent memory states.

### Technical Reference

The agent runtime should:
- Implement a sandboxed execution model for agents
- Manage agent lifecycle (init, run, stop)
- Provide standardized I/O channels for agent communication
- Integrate with the memory system for state persistence
- Support debugging and inspection of agent state

The memory system should:
- Store and retrieve agent memory states
- Support different memory types (short-term, long-term, vector)
- Provide visualization components for memory inspection
- Track memory changes over time
- Support editing and querying memory contents

### Implementation Guidance

When implementing the agent runtime:
- Use process isolation for security
- Implement resource monitoring and limits
- Create a clean API for agent lifecycle management
- Use event-driven architecture for state updates
- Support both synchronous and asynchronous operations

When implementing the memory system:
- Design for efficient queries and updates
- Use a schema-based approach for validation
- Implement serialization for persistence
- Create visualization components for different memory types
- Support history tracking for debugging

Example agent runtime implementation:

```typescript
// src/AgentRuntime.ts
import { EventEmitter } from 'events';
import { AgentDefinition, AgentState, ExecutionOptions } from './types';
import { MemoryManager } from './MemoryManager';

export class AgentRuntime extends EventEmitter {
  private agents = new Map<string, AgentInstance>();
  private memoryManager: MemoryManager;

  constructor(memoryManager: MemoryManager) {
    super();
    this.memoryManager = memoryManager;
  }

  public async createAgent(definition: AgentDefinition): Promise<string> {
    const agentId = definition.id || generateId();

    // Create agent sandbox
    const sandbox = await this.createSandbox(agentId, definition);

    // Initialize memory
    await this.memoryManager.initializeMemory(agentId, definition.initialMemory || {});

    // Create agent instance
    const agent = new AgentInstance(agentId, definition, sandbox, this.memoryManager);
    this.agents.set(agentId, agent);

    this.emit('agentCreated', { agentId, definition });

    return agentId;
  }

  public async startAgent(agentId: string, options?: ExecutionOptions): Promise<void> {
    const agent = this.getAgentOrThrow(agentId);
    await agent.start(options);
    this.emit('agentStarted', { agentId, options });
  }

  public async stopAgent(agentId: string): Promise<void> {
    const agent = this.getAgentOrThrow(agentId);
    await agent.stop();
    this.emit('agentStopped', { agentId });
  }

  private getAgentOrThrow(agentId: string): AgentInstance {
    const agent = this.agents.get(agentId);
    if (!agent) {
      throw new Error(`Agent not found: ${agentId}`);
    }
    return agent;
  }

  private async createSandbox(agentId: string, definition: AgentDefinition): Promise<AgentSandbox> {
    // Implementation of sandbox creation
    // This would use Node.js worker threads or similar isolation mechanism
  }
}
```

### Testing Focus

- Unit tests for runtime and memory operations
- Integration tests for agent lifecycle management
- Performance tests for memory operations with large datasets
- Stress testing with multiple concurrent agents

### Key Deliverables

- Agent execution environment
- Memory management system
- Agent lifecycle API
- Memory visualization components
- Debugging tools for agent inspection

---

## Integration & Sync Engineer (agentforge-navigation)

### Role Context

You are assisting the Integration & Sync Engineer responsible for the `agentforge-navigation` extension. This extension implements the bidirectional synchronization between code and graph representations, handling navigation, code mapping, and state synchronization.

### Technical Reference

The navigation system should:
- Map between graph nodes and code locations
- Provide decorations for agent-related code
- Handle navigation from graph to code and vice versa
- Track code changes and update the graph accordingly
- Manage conflicts during concurrent edits

### Implementation Guidance

When implementing code-graph mappings:
- Use a robust mapping system between code locations and graph elements
- Handle refactoring operations (rename, move)
- Create persistent mapping storage
- Implement efficient lookup in both directions

When implementing navigation features:
- Add code decorations that are visually consistent
- Create intuitive navigation commands
- Implement history tracking for navigation
- Support keyboard shortcuts for quick navigation

Example code mapping implementation:

```typescript
// src/CodeGraphMapper.ts
import * as vscode from 'vscode';
import { NodeId, EdgeId, GraphElement } from './types';

export interface CodeLocation {
  uri: vscode.Uri;
  range: vscode.Range;
}

export class CodeGraphMapper {
  private codeToGraph = new Map<string, GraphElement>();
  private graphToCode = new Map<string, CodeLocation>();

  public mapNodeToCode(nodeId: NodeId, location: CodeLocation): void {
    const locationKey = this.getLocationKey(location);
    this.codeToGraph.set(locationKey, { type: 'node', id: nodeId });
    this.graphToCode.set(`node:${nodeId}`, location);
  }

  public mapEdgeToCode(edgeId: EdgeId, location: CodeLocation): void {
    const locationKey = this.getLocationKey(location);
    this.codeToGraph.set(locationKey, { type: 'edge', id: edgeId });
    this.graphToCode.set(`edge:${edgeId}`, location);
  }

  public getGraphElementAtLocation(location: CodeLocation): GraphElement | undefined {
    const locationKey = this.getLocationKey(location);
    return this.codeToGraph.get(locationKey);
  }

  public getCodeLocationForNode(nodeId: NodeId): CodeLocation | undefined {
    return this.graphToCode.get(`node:${nodeId}`);
  }

  public getCodeLocationForEdge(edgeId: EdgeId): CodeLocation | undefined {
    return this.graphToCode.get(`edge:${edgeId}`);
  }

  private getLocationKey(location: CodeLocation): string {
    return `${location.uri.toString()}:${location.range.start.line}:${location.range.start.character}`;
  }

  public handleFileRename(oldUri: vscode.Uri, newUri: vscode.Uri): void {
    // Update mappings when files are renamed
    for (const [key, element] of this.codeToGraph.entries()) {
      if (key.startsWith(oldUri.toString())) {
        const newKey = key.replace(oldUri.toString(), newUri.toString());
        this.codeToGraph.set(newKey, element);
        this.codeToGraph.delete(key);

        // Update the reverse mapping
        const id = element.type === 'node'
          ? `node:${element.id}`
          : `edge:${element.id}`;

        const location = this.graphToCode.get(id);
        if (location) {
          this.graphToCode.set(id, {
            uri: newUri,
            range: location.range
          });
        }
      }
    }
  }
}
```

### Testing Focus

- Integration tests for code-graph synchronization
- Unit tests for mapping logic
- User scenario testing for navigation flows
- Edge case testing for conflict resolution

### Key Deliverables

- Bidirectional code-graph navigation
- Code decoration system for agent markers
- File system event handling
- Real-time synchronization of code and graph
- Conflict resolution for concurrent edits

---

## LLM & Tooling Engineer (cross-cutting)

### Role Context

You are assisting the LLM & Tooling Engineer responsible for integrating language models across all AgentForge extensions and implementing agent templates and debugging tools.

### Technical Reference

The LLM integration should:
- Provide a provider-agnostic interface for language models
- Implement secure API key management
- Support caching and request optimization
- Include token counting and budget management
- Enable prompt templates for common tasks

The tooling components should:
- Create a template system for agent creation
- Implement debugging tools for agent inspection
- Visualize LLM reasoning and decision making
- Support time-travel debugging capabilities

### Implementation Guidance

When implementing LLM integration:
- Create an abstraction layer for different providers
- Implement secure credential storage
- Design efficient caching mechanisms
- Create a robust error handling system
- Support both streaming and non-streaming responses

When implementing tooling:
- Design reusable templates with customization
- Create visualization components for reasoning
- Implement time-travel debugging with state snapshots
- Design clear and intuitive UI for template browsing

Example LLM provider implementation:

```typescript
// src/llm/providers/OpenAIProvider.ts
import { Configuration, OpenAIApi } from 'openai';
import { LLMProvider, PromptOptions, CompletionResponse } from '../types';
import { TokenCounter } from '../TokenCounter';

export class OpenAIProvider implements LLMProvider {
  private api: OpenAIApi;
  private tokenCounter: TokenCounter;

  constructor(apiKey: string, tokenCounter: TokenCounter) {
    const configuration = new Configuration({ apiKey });
    this.api = new OpenAIApi(configuration);
    this.tokenCounter = tokenCounter;
  }

  public async complete(prompt: string, options: PromptOptions): Promise<CompletionResponse> {
    const tokenCount = this.tokenCounter.countTokens(prompt);

    if (tokenCount > options.maxTokens) {
      throw new Error(`Prompt exceeds maximum token limit: ${tokenCount} > ${options.maxTokens}`);
    }

    try {
      const startTime = Date.now();

      const response = await this.api.createCompletion({
        model: options.model || 'text-davinci-003',
        prompt,
        max_tokens: options.maxOutputTokens || 1000,
        temperature: options.temperature || 0.7,
        top_p: options.topP || 1,
        frequency_penalty: options.frequencyPenalty || 0,
        presence_penalty: options.presencePenalty || 0,
        stop: options.stopSequences
      });

      const endTime = Date.now();

      return {
        text: response.data.choices[0].text || '',
        tokenUsage: {
          prompt: tokenCount,
          completion: this.tokenCounter.countTokens(response.data.choices[0].text || ''),
          total: response.data.usage?.total_tokens || 0
        },
        latency: endTime - startTime,
        provider: 'openai',
        model: options.model || 'text-davinci-003'
      };
    } catch (error) {
      console.error('OpenAI API error:', error);
      throw new Error(`LLM request failed: ${error.message}`);
    }
  }
}
```

### Testing Focus

- Unit tests for LLM interface
- Integration tests with mock LLM providers
- Security testing for API key management
- Performance tests for caching system

### Key Deliverables

- Provider-agnostic LLM interface
- API key management system
- Caching and optimization layer
- Template system for agent creation
- Debugging tools for LLM reasoning
- Time-travel debugging capabilities

---

## Shadow Workspace Implementation

All roles should be familiar with the shadow workspace concept inspired by AI-CODING-PLATFORMS-shadow-workspace. This feature allows agents to safely experiment with code changes in a hidden workspace before presenting them to users.

### Key Considerations

- Create isolated copies of workspace files
- Track changes made by agents
- Generate diffs between original and modified versions
- Present changes to users for review
- Support accepting, rejecting, or modifying proposed changes

Example usage pattern:

```typescript
// Using the shadow workspace for agent code modifications
async function improveCode(fileUri: vscode.Uri): Promise<void> {
  // Create shadow workspace
  const shadowWorkspace = await shadowWorkspaceManager.create();

  // Copy file to shadow workspace
  const shadowFileUri = await shadowWorkspace.copyFile(fileUri);

  // Let agent modify the code in shadow workspace
  await agentRuntime.runCodeImprovement(shadowFileUri);

  // Generate diff between original and modified
  const diff = await shadowWorkspace.generateDiff(fileUri, shadowFileUri);

  // Show diff to user for review
  const userDecision = await showDiffForReview(diff);

  if (userDecision === 'accept') {
    // Apply changes to actual workspace
    await shadowWorkspace.applyChanges(fileUri);
  } else if (userDecision === 'modify') {
    // Let user modify the proposed changes
    const modifiedChanges = await letUserModifyChanges(diff);
    await shadowWorkspace.applyCustomChanges(fileUri, modifiedChanges);
  }

  // Clean up
  await shadowWorkspace.dispose();
}
```

## Conclusion

These prompts provide comprehensive guidance for AI assistants working with each engineering role on the AgentForge project. By following the architecture patterns, code styles, and implementation guidelines outlined in these prompts, we can ensure a consistent, high-quality implementation across all components of the system.
