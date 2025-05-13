# AgentForge UI Design Specification

## Overview

This document specifies the user interface design for AgentForge, detailing the visual layout, interaction patterns, and component relationships. The UI design aims to balance the familiarity of VSCode with the specialized needs of agent development, focusing on clarity, discoverability, and productivity.

## Design Principles

1. **Spatial Organization** - Organize the UI to reflect the mental model of agent development
2. **Visual Hierarchy** - Emphasize important elements and de-emphasize secondary ones
3. **Contextual Relevance** - Show tools and information relevant to the current task
4. **Affordance** - Make interactive elements clearly distinguishable
5. **Consistency** - Maintain visual and behavioral patterns throughout
6. **Progressive Disclosure** - Reveal complexity gradually as needed

## UI Layout Overview

```
+-----------------------------------------------------------+
| Menu Bar                                                  |
+-----------------------------------------------------------+
| Toolbar                                                   |
+-----------------------------------------------------------+
|             |                               |             |
|             |                               |             |
|             |                               |             |
|             |                               |             |
|  Explorer   |                               |   Agent     |
|  Sidebar    |         Graph Editor          |   Sidebar   |
|             |             or                |             |
|             |         Code Editor           |             |
|             |                               |             |
|             |                               |             |
|             |                               |             |
+-------------+-------------------------------+-------------+
|                                                           |
|                   Terminal / Console                      |
|                                                           |
+-----------------------------------------------------------+
|                       Status Bar                          |
+-----------------------------------------------------------+
```

## Component Specifications

### 1. Explorer Sidebar

**Purpose:** Provide file navigation and project structure.

**Components:**
- File tree with special icons for agent files
- Project structure navigator
- Agent-aware search
- Template gallery

**Behaviors:**
- Files can be dragged to graph
- Agent files have special context menu
- Filter options for agent-related files
- Template preview on hover

**Visual Design:**
- Standard VSCode tree UI with custom iconography
- Color coding for agent-related files
- Section dividers for different views
- Collapsible panels for organization

### 2. Agent Sidebar

**Purpose:** Provide agent-specific tools and configuration.

**Components:**
- Agent Inspector
- Memory Explorer
- Template Browser
- Agent Configuration Panel
- Debug Controls

**Behaviors:**
- Displays details of selected agent
- Allows direct editing of agent properties
- Shows current state and memory
- Provides runtime controls
- Supports template instantiation

**Visual Design:**
- Tabbed interface for different tools
- Property grid for configuration
- Status indicators with color coding
- Expandable sections for complex properties
- Action buttons with clear iconography

### 3. Graph Editor

**Purpose:** Visualize and edit agent relationships and workflows.

**Components:**
- Canvas with zoom/pan controls
- Node palette
- Mini-map navigator
- Property inspector
- Context toolbar

**Behaviors:**
- Nodes can be dragged, selected, and edited
- Edges can be created by dragging between nodes
- Zoom and pan with mouse or keyboard
- Selection highlights related elements
- Double-click navigates to code

**Visual Design:**
- Clean, minimal canvas with grid backdrop
- High-contrast nodes with distinctive shapes per type
- Directional edges with clear indicators
- Selection highlighting with prominent borders
- Smart layout algorithms for auto-arrangement

### 4. Code Editor

**Purpose:** Edit agent code with enhanced awareness of agent concepts.

**Components:**
- Standard VSCode editor
- Agent-aware code decorations
- Enhanced tooltips
- Navigation breadcrumbs
- Agent-specific code snippets

**Behaviors:**
- Syntax highlighting for agent constructs
- Click decorations to navigate to graph
- Hover for agent context and documentation
- Intelligent code completion for agent patterns
- Inline validation for agent-specific logic

**Visual Design:**
- Subtle background shading for agent-related code
- Margin indicators for agent code sections
- Custom color scheme for agent keywords
- Icon decorations for navigable elements
- Inline documentation with agent context

### 5. Terminal/Console

**Purpose:** Display agent output and provide debugging information.

**Components:**
- Output console
- Debug console
- Agent communication view
- Log explorer
- Command input

**Behaviors:**
- Filters for different output types
- Search functionality
- Auto-scroll with option to pause
- Copy and export capabilities
- Direct command input to agents

**Visual Design:**
- Color-coded output by source or type
- Expandable entries for detailed inspection
- Timestamp and metadata display
- Severity indicators for important messages
- Clear visual separation between agent outputs

### 6. Toolbar

**Purpose:** Provide quick access to common actions.

**Components:**
- New agent button
- Run/debug controls
- Layout options
- View switchers
- Tool selectors

**Behaviors:**
- Icon buttons with tooltips
- Contextual enabling/disabling
- Grouped by functional area
- Customizable arrangement

**Visual Design:**
- High-contrast icons for primary actions
- Grouped with subtle separators
- Consistent sizing and spacing
- State indicators (active/inactive)
- Dropdown menus for related options

### 7. Status Bar

**Purpose:** Display system status and contextual information.

**Components:**
- Agent count and status
- Memory usage
- LLM connection status
- Current editing context
- Notification indicators

**Behaviors:**
- Click for more detailed information
- Hover for quick insights
- Color coding for status indication
- Progressive disclosure of complex state

**Visual Design:**
- Compact, information-dense layout
- Icon + text for clarity
- Color coding for status (green/yellow/red)
- Separation between different information types
- Subtle animations for state changes

## Interaction Patterns

### Graph Editing

1. **Node Creation:**
   - Drag from palette to canvas
   - Double-click canvas with type selected
   - Right-click menu on canvas
   - Keyboard shortcut (Ctrl+N)

2. **Edge Creation:**
   - Click and drag from node port to another node
   - Select two nodes and use connect command
   - Right-click menu between nodes

3. **Selection:**
   - Click for single select
   - Shift+click for multi-select
   - Marquee drag for area select
   - Alt+click to select connected nodes

4. **Navigation:**
   - Pan: Space+drag or middle-mouse drag
   - Zoom: Scroll wheel or Ctrl+/- keys
   - Fit to view: F key or button
   - Center on selection: C key or button

### Code-Graph Navigation

1. **Code to Graph:**
   - Click on agent decoration in margin
   - Use "Go to Graph" command in context menu
   - Ctrl+click on agent identifier
   - Alt+G keyboard shortcut

2. **Graph to Code:**
   - Double-click on node
   - Right-click node and select "View Code"
   - Select node and press Enter
   - Use "Open Source" button in property panel

### Agent Execution

1. **Starting Agents:**
   - Click play button on agent node
   - Use Run menu in toolbar
   - Right-click agent in sidebar
   - F5 keyboard shortcut

2. **Debugging:**
   - Set breakpoints in graph or code
   - Use step controls in debug toolbar
   - Hover over nodes to see current state
   - Inspect variables in debug sidebar

### Memory Inspection

1. **Viewing Memory:**
   - Select agent and open memory tab
   - Click memory node in graph
   - Use "View Memory" from context menu
   - Use keyboard shortcut (Alt+M)

2. **Editing Memory:**
   - Double-click memory value
   - Use property editor in sidebar
   - Right-click and select "Edit Value"
   - Use structured editor for complex types

## Color System

### Primary Colors

- **Primary Blue:** #0078D7 (Buttons, active items, primary actions)
- **Secondary Blue:** #005A9E (Hover states, secondary elements)
- **Accent Green:** #107C10 (Success states, completion indicators)
- **Accent Amber:** #FF8C00 (Warnings, attention items)
- **Accent Red:** #E81123 (Errors, critical states)

### Neutral Colors

- **Background:** #1E1E1E (Main UI background)
- **Secondary Background:** #252526 (Panels, sidebars)
- **Border:** #3F3F3F (Separators, outlines)
- **Text Primary:** #FFFFFF (Primary text)
- **Text Secondary:** #BBBBBB (Secondary text, descriptions)

### Semantic Colors

- **Agent Node:** #2B88D8 (Agent representation)
- **Memory Node:** #8954A8 (Memory representation)
- **Workflow Node:** #68217A (Workflow representation)
- **Action Node:** #107C10 (Action representation)
- **Template Node:** #FF8C00 (Template representation)

## Typography

- **Primary Font:** 'Segoe UI', -apple-system, sans-serif
- **Code Font:** 'Cascadia Code', 'Consolas', monospace
- **Base Size:** 13px
- **Scale Ratio:** 1.2
- **Weights:** Regular (400), Semi-Bold (600), Bold (700)

## Icons

### System Icons

- Standard VSCode iconography for common actions
- Custom icons for agent-specific concepts:
  - Agent: Brain-like icon
  - Memory: Database-like icon
  - Workflow: Flowchart-like icon
  - Action: Gear/cog-like icon
  - Template: Blueprint-like icon

### Status Icons

- Running: Green circle with play symbol
- Paused: Amber circle with pause bars
- Stopped: Gray circle with stop square
- Error: Red circle with exclamation
- Connecting: Blue circle with loading animation

## Responsive Behavior

- **Panel Resizing:** All dividers between panels are draggable
- **Sidebar Collapse:** Sidebars can collapse to icons-only mode
- **Graph Scaling:** Graph automatically scales to available space
- **Terminal Height:** Terminal panel height is adjustable
- **Layout Presets:** Predefined layouts for different workflows:
  - Code-focused
  - Graph-focused
  - Debugging
  - Memory inspection

## Accessibility Considerations

- **Keyboard Navigation:** All functions accessible without mouse
- **Screen Reader Support:** ARIA attributes for all components
- **Contrast Ratios:** AA compliance for all text elements
- **Focus Indicators:** Clear visual focus states
- **Color Independence:** Information not conveyed by color alone
- **Scalable UI:** Support for OS text scaling
- **Reduced Motion:** Option to minimize animations

## Special UI States

### Agent States

- **Creating:** Animation showing node materializing
- **Starting:** Pulsing animation during initialization
- **Running:** Subtle continuous animation
- **Paused:** Static with pause indicator
- **Error:** Highlighted with error styling
- **Stopping:** Fade-out animation

### Memory States

- **Reading:** Brief highlight when accessed
- **Writing:** Animation showing update
- **Searching:** Ripple effect through nodes
- **Dirty:** Modified but not saved indicator
- **Conflicted:** Warning indicator for conflicts

## UI Workflows

### Creating a New Agent

1. Click "New Agent" in toolbar or sidebar
2. Select agent type from template gallery
3. Configure basic properties in dialog
4. Agent node appears in graph
5. Code scaffold generated in editor
6. Property panel opens for detailed configuration

### Debugging an Agent System

1. Set breakpoints in graph or code
2. Start system in debug mode (F5)
3. System runs until breakpoint hit
4. Debug sidebar shows current state
5. Step controls advance execution
6. Variables panel shows memory state
7. Continue or stop execution

### Creating a Complex Workflow

1. Drag agent nodes from palette to canvas
2. Connect nodes with appropriate edges
3. Configure edge conditions in property panel
4. Add memory nodes and connect to agents
5. Set initial properties for all nodes
6. Validate workflow with "Check" button
7. Save and run workflow

## Future UI Considerations

### Collaboration Features

- User presence indicators
- Concurrent editing visualization
- Change attribution
- Comment and annotation system

### Mobile Adaptations

- Responsive layout for tablet use
- Touch-optimized interactions
- Reduced feature set for monitoring

### AR/VR Potential

- 3D visualization of complex agent systems
- Spatial arrangement of agent components
- Immersive debugging environment

## Implementation Guidelines

### Component Architecture

- React-based UI components
- CSS modules for styling
- State management via context or Redux
- WebView integration with VSCode

### Performance Considerations

- Virtualization for large graphs
- Lazy loading of property panels
- Throttled updates for real-time data
- Canvas rendering optimization

### Testing Strategy

- Component unit tests
- Visual regression testing
- Interaction user testing
- Accessibility audits
