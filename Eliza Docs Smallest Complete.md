Eliza Docs 1 Intro Quickstart Overview Runtime Projects


# Introduction to Eliza


## What is Eliza?

Eliza is a powerful multi-agent simulation framework designed to create, deploy, and manage autonomous AI agents. Built with TypeScript, it provides a flexible and extensible platform for developing intelligent agents that can interact across multiple platforms while maintaining consistent personalities and knowledge.


## Key Features

- **Platform Integration**: Clients for Discord, X (Twitter), Telegram, and many others
- **Flexible Model Support**: Deepseek, Ollama, Grok, OpenAI, Anthropic, Gemini, LLama, etc.
- **Character System**: Create diverse agents using [character files](https://github.com/elizaOS/characterfile)
- **Multi-Agent Architecture**: Manage multiple unique AI personalities simultaneously
- **Memory Management**: Easily ingest and interact with documents using RAG
- **Media Processing**: PDF, URLs, Audio transcription, Video processing, Image analysis, Conversation summarization
- **Technical Foundation**:
  - 100% TypeScript implementation
  - Modular architecture
  - Highly extensible action and plugin system
  - Custom client support
  - Comprehensive API

## Use Cases

Eliza can be used to create:

- **AI Assistants**: Customer support agents, Community moderators, Personal assistants
- **Social Media Personas**: Automated content creators, Brand representatives, Influencers
- **Knowledge Workers**: Research assistants, Content analysts, Document processors
- **Interactive Characters**: Role-playing characters, Educational tutors, Entertainment bots

---

## Getting Started


### Prerequisites

- [Node.js 23+](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
- Git for version control
- For Windows Users: [WSL 2](https://learn.microsoft.com/en-us/windows/wsl/install-manual) is required

### Multiple Paths to Start

Eliza offers different paths depending on your goals:

#### 1. Simple Start - Run Quickly

```bash
# Install globally (optional but recommended)
npm install -g @elizaos/cli

# Start with default settings
npx elizaos start
```

Visit https://localhost:3000 to interact with your agent through a web interface.

#### 2. Create Your Own Project

```bash
# Create a new project through interactive setup
npx elizaos create

# Navigate to your project directory
cd my-project-name

# Start your project
npx elizaos start
```

Add plugins to your project:

```bash
# List available plugins
npx elizaos project list-plugins

# Add a plugin
npx elizaos project add-plugin @elizaos/plugin-discord
```

#### 3. Develop a Custom Plugin

```bash
# Create a plugin project
npx elizaos create --type plugin

# Follow the interactive prompts
```

Develop and test your plugin:

```bash
# Test your plugin
npx elizaos start

# Publish your plugin when ready
npx elizaos plugin publish
```

#### 4. Contribute to ElizaOS Core

```bash
# Clone the repository
git clone git@github.com:elizaOS/eliza.git
cd eliza

# Install dependencies and build
bun install
bun run build

# Start ElizaOS
bun start
```

Visit https://localhost:3000 to interact with your agent through a web interface.


---

## Community and Support

Eliza is backed by an active community of developers and users.

- [**Open Source**](https://github.com/elizaos/eliza): Contribute to the project on GitHub


# Quickstart Guide

---

## 1. Simple Start - Get Running Quickly

The fastest way to get started with ElizaOS is using the CLI:

```bash
# Install globally (optional but recommended)
npm install -g @elizaos/cli

# Or use directly with npx
npx elizaos start
```

This will:

1. Start ElizaOS with default settings
2. Load the default character
3. Make the agent accessible via terminal and REST API

### Chat with your agent:

Visit https://localhost:3000 to interact with your agent through a web interface.

## 2. Creating a Project

If you want to create a custom ElizaOS project with your own characters and configurations:

```bash
# Create a new project with the interactive wizard
npx elizaos create

# Or specify project type directly
npx elizaos create --type project
```

Follow the interactive prompts to configure your project. Once created:

```bash
# Navigate to your project directory
cd my-project-name

# Start your project
npx elizaos start
```

### Add plugins to your project:

```bash
# List available plugins
npx elizaos project list-plugins

# Add a plugin
npx elizaos project add-plugin @elizaos-plugins/client-discord
```

## 3. Creating a Plugin

Want to extend ElizaOS with custom functionality?

```bash
# Create a new plugin project
npx elizaos create --type plugin

# Follow the interactive prompts
```

Develop your plugin following the structure in your generated project:

```bash
# Test your plugin
npx elizaos start

# Publish your plugin when ready
npx elizaos plugin publish
```

### Publishing options:

```bash
# Test publish without making changes
npx elizaos plugin publish --test

# Publish to npm
npx elizaos plugin publish --npm

# Specify platform compatibility
npx elizaos plugin publish --platform node
```

## 4. Contributing to ElizaOS

If you want to add features or fix bugs in the ElizaOS core:

```bash
# Clone the repository
git clone git@github.com:elizaOS/eliza.git
cd eliza

# Switch to development branch
git checkout develop

# Install dependencies
bun install

# Build the project
bun build

# Start ElizaOS
bun start
```

Visit https://localhost:3000 to interact with your agent through a web interface.

### Automated setup:

```bash
git clone git@github.com:elizaOS/eliza.git
cd eliza

# Run the start script with verbose logging
./scripts/start.sh -v
```

---

## Troubleshooting

### Common Issues

#### Node Version

- Use Node.js 23.3.0+ (`node -v` to check)
- Try using NVM: `nvm use 23`

#### Installation Problems

```bash
# Clean and reinstall
bun clean
bun install --no-frozen-lockfile
bun build
```

#### Plugin Issues

```bash
# Rebuild problematic packages
bun rebuild better-sqlite3
```

#### Docker Issues

```bash
# Clean up Docker environment
docker rmi -f $(docker images -aq)
docker builder prune -a -f
```
---

# ElizaOS Documentation

Welcome to ElizaOS - a comprehensive framework for building AI agents with persistent personalities across multiple platforms. ElizaOS provides the architecture, tools, and systems needed to create sophisticated agents that maintain consistent behavior, learn from interactions, and seamlessly integrate with a variety of services.

## System Architecture

ElizaOS uses a modular architecture that separates concerns while providing a cohesive framework for AI agent development:

```mermaid
graph TB
    %% Main Components with vertical orientation
    User((User)):::user

    %% First Level - Services
    PlatformServices[Services]:::services

    %% Second Level - Runtime
    AgentRuntime[Agent Runtime]:::core

    %% Core Processing Components - Side by side
    subgraph "Core Processing"
        direction LR
        Providers[Providers]:::int
        Actions[Actions]:::int
        Evaluators[Evaluators]:::int
    end

    %% Knowledge and DB - Side by side
    subgraph "Knowledge & Storage"
        direction LR
        Knowledge[Knowledge]:::int
        DB[(Database)]:::db
    end

    %% Organization Components - Vertical layout
    subgraph "Organization"
        direction TB
        Worlds[Worlds]:::struct
        Rooms[Rooms]:::struct
        Entities[Entities]:::struct
    end

    %% Development Components - Side by side
    subgraph "Development & Integration"
        direction LR
        Plugins[Plugins]:::dev
        Projects[Projects]:::dev
        Tasks[Tasks]:::dev
    end

    %% Main Flow - Vertical emphasis
    User <-->|Interaction| PlatformServices
    PlatformServices -->|Process| AgentRuntime

    %% Runtime connections - Simplified
    AgentRuntime ---|Context| Providers
    AgentRuntime ---|Behavior| Actions
    AgentRuntime ---|Analysis| Evaluators

    %% Data connections
    AgentRuntime <-->|Storage| DB
    Knowledge -->|Informs| Providers

    %% Structure connections - Clean vertical hierarchy
    AgentRuntime -->|Manages| Worlds
    Worlds -->|Contains| Rooms
    Rooms -->|Has| Entities

    %% Development connections
    Projects -->|Configure| AgentRuntime
    Plugins -->|Extend| AgentRuntime
    Tasks -->|Scheduled by| AgentRuntime

    %% Clickable nodes with links to docs
    click AgentRuntime "/docs/core/agents" "Learn about Agent Runtime"
    click PlatformServices "/docs/core/services" "Learn about Services"
    click DB "/docs/core/database" "Learn about Database Systems"
    click Actions "/docs/core/actions" "Learn about Actions"
    click Providers "/docs/core/providers" "Learn about Providers"
    click Evaluators "/docs/core/evaluators" "Learn about Evaluators"
    click Knowledge "/docs/core/knowledge" "Learn about Knowledge System"
    click Worlds "/docs/core/worlds" "Learn about Worlds"
    click Rooms "/docs/core/rooms" "Learn about Rooms"
    click Entities "/docs/core/entities" "Learn about Entities"
    click Plugins "/docs/core/plugins" "Learn about Plugins"
    click Projects "/docs/core/project" "Learn about Projects"
    click Tasks "/docs/core/tasks" "Learn about Tasks"

    %% Styling
    classDef core fill:#3498db,stroke:#2c3e50,stroke-width:1px,color:#fff,font-weight:bold
    classDef services fill:#9b59b6,stroke:#2c3e50,stroke-width:1px,color:#fff,font-weight:bold
    classDef db fill:#27ae60,stroke:#2c3e50,stroke-width:1px,color:#fff,font-weight:bold
    classDef int fill:#e74c3c,stroke:#2c3e50,stroke-width:1px,color:#fff,font-weight:bold
    classDef struct fill:#f39c12,stroke:#2c3e50,stroke-width:1px,color:#fff,font-weight:bold
    classDef dev fill:#1abc9c,stroke:#2c3e50,stroke-width:1px,color:#fff,font-weight:bold
    classDef user fill:#ecf0f1,stroke:#2c3e50,stroke-width:2px,color:#2c3e50,font-weight:bold,border-radius:50%
```

### How ElizaOS Works

When a user message is received:

1. **Service Reception**: Platform service (Discord, Telegram, etc.) receives the message
2. **Runtime Processing**: Agent runtime coordinates the response generation
3. **Context Building**: Providers supply relevant context (time, recent messages, knowledge)
4. **Action Selection**: The agent evaluates and selects appropriate actions
5. **Response Generation**: The chosen action generates a response
6. **Learning & Reflection**: Evaluators analyze the conversation for insights and learning
7. **Memory Storage**: New information is stored in the database
8. **Response Delivery**: The response is sent back through the service

This creates a continuous cycle of interaction, reflection, and improvement that allows agents to maintain consistent personalities while adapting to new information.

## Core Components

Agent Runtime
Services
Database

 Agent Runtime
          The central system that orchestrates agent behavior, processes messages, manages state, and coordinates all other components.
          

Services
          Platform-specific integrations that enable agents to communicate across Discord, Twitter, Telegram, and other channels.
    

Database
          Persistent storage for memories, entity data, relationships, and configuration using vector search capabilities.


## Intelligence & Behavior

Actions
Executable capabilities that define how agents respond to messages and interact with external systems.
       
    
Providers
Data sources that supply contextual information to inform agent decision-making in real-time.
    

Evaluators
          Analytical systems that process conversations to extract insights, learn facts, and improve future responses.
 
Knowledge
          RAG system for document processing, semantic search, and context-aware memory retrieval.
      
## Structure & Organization

Worlds
          Collection spaces that organize entities and rooms into coherent environments (like a Discord server).
       
   
          Rooms
          Conversation spaces where entities interact through messages (channels, DMs, threads).
        
    
Entities
          Representation of users, agents, and other participants using a flexible entity-component architecture.

## Development & Integration

Plugins
          Modular extensions that add new capabilities, integrations, and behaviors to agents.
    
    
Projects
          Organizational structure for defining and deploying one or more agents with their configuration.
        
Tasks
          System for managing deferred, scheduled, and repeating operations across conversations.
        
---

## Key Concepts

### Action-Provider-Evaluator Cycle

The core of the ElizaOS system operates as a continuous cycle:

1. **Providers** gather context before response generation
2. **Actions** determine what the agent can do and are executed to generate responses
3. **Evaluators** analyze conversations after responses to extract insights
4. These insights become part of the agent's memory
5. Future **Providers** access this memory to inform new responses

This creates a virtuous cycle where agents continuously learn and improve from interactions.

### Entity-Component Architecture

ElizaOS uses an entity-component architecture for flexible data modeling:

- **Entities** are base objects with unique IDs (users, agents, etc.)
- **Components** are pieces of data attached to entities (profiles, settings, etc.)
- This approach allows for dynamic composition without complex inheritance hierarchies

### Memory System

The memory system in ElizaOS provides:

- **Vector-based semantic search** for finding relevant memories
- **Multi-level memory types** (messages, facts, knowledge)
- **Temporal awareness** through timestamped memories
- **Cross-platform continuity** while maintaining appropriate context boundaries

## Getting Started

If you're new to ElizaOS, we recommend this learning path:

1. Start with this overview to understand the system architecture
2. Explore the Agent Runtime to understand the core system
3. Learn about Projects to set up your development environment
4. Learn how Actions and Providers work together.
5. Explore Services to connect with external platforms
6. Dive into Plugins to extend functionality

## FAQ

### What's the difference between Actions, Evaluators, and Providers?

**Actions** define what an agent can do and are executed during response generation. **Evaluators** analyze conversations after they happen to extract insights and improve future responses. **Providers** supply contextual information before the agent decides how to respond.

### How does ElizaOS handle cross-platform conversation context?

ElizaOS maintains separate conversation contexts for different platforms by default, but shares entity relationships and learned facts across platforms. This ensures agents maintain a consistent understanding of users while respecting platform-specific conversation boundaries.

### How does the memory system work?

Memory is organized into different types (messages, facts, knowledge) and stored with vector embeddings for semantic search. This allows agents to retrieve relevant memories based on context rather than just recency, creating more natural conversations.

### What's the relationship between Worlds, Rooms, and Entities?

Worlds are container spaces (like a Discord server) that can have multiple Rooms (channels, DMs). Entities (users, agents) participate in Rooms within Worlds. This hierarchical structure mirrors real-world platforms while providing a consistent abstraction.

### How extensible is ElizaOS?

ElizaOS is highly extensible through its plugin system. You can create custom actions, providers, evaluators, services, and more to extend functionality. The architecture is designed to be modular and composable at every level.


# Agent Runtime

The `AgentRuntime` is the core runtime environment for Eliza agents. It handles message processing, state management, plugin integration, and interaction with external services. You can think of it as the brains that provide the high-level orchestration layer for Eliza agents.

```mermaid
sequenceDiagram
    actor User
    participant Platform as Platform
    participant Runtime as Runtime
    participant State as State
    participant P as Providers
    participant A as Actions
    participant M as Models
    participant E as Evaluators
    participant DB as Database

    User->>Platform: Message
    Platform->>Runtime: Forward

    %% Context building (condensed)
    Runtime->>State: Get context
    State->>P: Gather data
    Note over P: Character, Knowledge,<br>Messages, Time, etc.
    P-->>State: Context data
    State-->>Runtime: Assembled context

    %% Action flow (condensed)
    Runtime->>A: Execute action
    A->>M: Generate content
    M-->>A: Generated text
    A-->>Runtime: Result

    %% Evaluation (condensed)
    Runtime->>E: Analyze
    E->>DB: Store insights
    E-->>Runtime: Evaluation

    %% Delivery
    Runtime->>Platform: Response
    Platform->>User: Deliver

    %% Background (simplified)
    par Background
        Runtime->>Runtime: Tasks & Events
    end
```

The runtime follows this general flow:

1. **Initial Reception**: The user sends a message which is received by the Platform Services
2. **Context Building**:

   - The Runtime Core requests context from the State Composition system
   - State gathers data from various Providers (Character, Knowledge, Recent Messages, etc.)
   - The complete context is returned to the Runtime

3. **Action Processing**:

   - The Runtime determines applicable actions and selects the optimal one
   - The selected action may request content generation from Models
   - The action result is returned to the Runtime

4. **Learning & Persistence**:

   - The conversation is analyzed by Evaluators for insights and facts
   - Knowledge updates are sent to the Memory System
   - All relevant data is persisted to the Database

5. **Response Delivery**:
   - The final response is sent back to the user through Platform Services

---

## Overview

The [AgentRuntime](/api/classes/AgentRuntime) class is the primary implementation of the [IAgentRuntime](/api/interfaces/IAgentRuntime) interface, which manages the agent's core functions, including:

| Component             | Description                                                                                                                                                                             | API Reference                                                    | Related Files                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Services**          | Supports multiple communication platforms and specialized functionalities for seamless interaction.                                                                                     | [Services API](/api/interfaces/IAgentRuntime/#services)          | [`service.ts`](https://github.com/elizaOS/eliza/tree/develop/packages/core/src/service.ts), [`Discord`](https://github.com/elizaos-plugins/plugin-discord), [`Telegram`](https://github.com/elizaos-plugins/plugin-telegram), [`Twitter`](https://github.com/elizaos-plugins/plugin-twitter), [`Farcaster`](https://github.com/elizaos-plugins/plugin-farcaster), [`Lens`](https://github.com/elizaos-plugins/plugin-lens), [`Slack`](https://github.com/elizaos-plugins/plugin-slack), [`Auto`](https://github.com/elizaos-plugins/plugin-auto), [`GitHub`](https://github.com/elizaos-plugins/plugin-github) |
| **State**             | Maintains context for coherent cross-platform interactions, updates dynamically. Also tracks goals, knowledge, and recent interactions                                                  | [State API](/api/interfaces/State)                               | [`state.ts`](https://github.com/elizaos/runtime/state.ts)                                                                                                                                                                                                       |
| **Plugins**           | Dynamic extensions of agent functionalities using custom actions, evaluators, providers, and adapters                                                                                   | [Plugins API](/api/type-aliases/Plugin/)                         | [`plugins.ts`](https://github.com/elizaos/runtime/plugins.ts), [actions](../actions), [evaluators](../evaluators), [providers](../providers)                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| **Services**          | Connects with external services for `IMAGE_DESCRIPTION`, `TRANSCRIPTION`, `TEXT_GENERATION`, `SPEECH_GENERATION`, `VIDEO`, `PDF`, `BROWSER`, `WEB_SEARCH`, `EMAIL_AUTOMATION`, and more | [Services API](/api/interfaces/IAgentRuntime/#services)          | [`services.ts`](https://github.com/elizaos/runtime/services.ts)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| **Memory Systems**    | Creates, retrieves, and embeds memories and manages conversation history.                                                                                                               | [Memory API](/api/interfaces/IMemoryManager)                     | [`memory.ts`](https://github.com/elizaos/runtime/memory.ts)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| **Database Adapters** | Persistent storage and retrieval for memories and knowledge                                                                                                                             | [databaseAdapter](api/interfaces/IAgentRuntime/#databaseAdapter) | [`MongoDB`](https://github.com/elizaos-plugins/adapter-mongodb), [`PostgreSQL`](https://github.com/elizaos-plugins/adapter-postgres), [`SQLite`](https://github.com/elizaos-plugins/adapter-sqlite), [`Supabase`](https://github.com/elizaos-plugins/adapter-supabase), [`PGLite`](https://github.com/elizaos-plugins/adapter-pglite), [`Qdrant`](https://github.com/elizaos-plugins/adapter-qdrant), [`SQL.js`](https://github.com/elizaos-plugins/adapter-sqljs)                                                                                                                                             |
| **Cache Management**  | Provides flexible storage and retrieval via various caching methods.                                                                                                                    | [Cache API](/api/interfaces/ICacheManager)                       | [`cache.ts`](https://github.com/elizaos/runtime/cache.ts)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

## Advanced: IAgentRuntime Interface

```typescript
interface IAgentRuntime {
    // Core identification
    agentId: UUID;
    
    // Configuration
    character: Character;                          // Personality and behavior settings
    
    // Components
    plugins: Plugin[];                             // Additional capabilities
    services: Map<ServiceTypeName, Service>;       // Platform connections and functionality
    providers: Provider[];                         // Real-time data sources
    actions: Action[];                             // Available behaviors
    evaluators: Evaluator[];                       // Analysis & learning
    routes: Route[];                               // API endpoints
    
    // Memory Management
    getMemories(...): Promise<Memory[]>;           // Retrieve conversation history
    createMemory(...): Promise<UUID>;              // Store new memories
    searchMemories(...): Promise<Memory[]>;        // Semantic search
    
    // State Composition
    composeState(...): Promise<State>;             // Gather data from providers
    
    // Plugin Management
    registerPlugin(...): Promise<void>;            // Register plugins
    
    // Service Management
    getService<T>(...): T | null;                  // Access services
    registerService(...): Promise<void>;           // Register services
    
    // Model Integration
    useModel<T, R>(...): Promise<R>;               // Use AI models
    
    // Additional Utilities
    getSetting(...): any;                          // Access settings
    setSetting(...): void;                         // Configure settings
    getCache<T>(...): Promise<T | undefined>;      // Access cached data
    setCache<T>(...): Promise<boolean>;            // Store cached data
}
```

Source: [/api/interfaces/IAgentRuntime/](/api/interfaces/IAgentRuntime/)


---

### **Key Methods**

- **`initialize()`**: Sets up the agent's runtime environment, including services, plugins, and knowledge processing.
- **`processActions()`**: Executes actions based on message content and state.
- **`evaluate()`**: Assesses messages and state using registered evaluators.
- **`composeState()`**: Constructs the agent's state object for response generation.
- **`registerService()`**: Adds a service to the runtime.
- **`getService()`**: Retrieves a registered service by type.
- **`useModel()`**: Utilizes AI models with typesafe parameters and results.
- **`ensureRoomExists()` / `ensureConnection()`**: Ensures the existence of communication channels and connections.

## Service System

Services provide specialized functionality with standardized interfaces that can be accessed cross-platform:

```typescript
// Speech Generation
const speechService = runtime.getService<ISpeechService>('speech_generation');
const audioStream = await speechService.process(text);

// PDF Processing
const pdfService = runtime.getService<IPdfService>('pdf');
const textContent = await pdfService.convertPdfToText(pdfBuffer);

// Discord Integration
const discordService = runtime.getService<IDiscordService>('discord');
await discordService.sendMessage(channelId, content);
```

---

## State Management

The runtime maintains comprehensive state through the State interface:

```typescript
interface State {
  // Core state data
  values: {
    [key: string]: any;
  };
  data: {
    [key: string]: any;
  };
  text: string;
}

// State composition example
async function manageState() {
  // Initial state composition with all regular providers
  const state = await runtime.composeState(message);

  // State with specific providers only
  const filteredState = await runtime.composeState(message, ['timeProvider', 'recentMessages']);

  // Include private or dynamic providers
  const enhancedState = await runtime.composeState(message, null, [
    'weatherProvider',
    'portfolioProvider',
  ]);
}
```

---

## Plugin System

Plugins extend agent functionality through a modular interface. The runtime supports various types of plugins including services, adapters, actions, and more:

```typescript
interface Plugin {
  name: string;
  description: string;
  init?: (config: Record<string, string>, runtime: IAgentRuntime) => Promise<void>;

  // Components
  services?: (typeof Service)[]; // Communication platforms and external integrations
  actions?: Action[]; // Custom behaviors
  providers?: Provider[]; // Data providers
  evaluators?: Evaluator[]; // Response assessment
  adapters?: Adapter[]; // Database/cache adapters
  routes?: Route[]; // API endpoints
  tests?: TestSuite[]; // Testing utilities
}
```

Plugins can be configured through [characterfile](./characterfile) settings:

```json
{
  "name": "MyAgent",
  "plugins": ["@elizaos/plugin-solana", "@elizaos/plugin-twitter"],
  "settings": {
    "twitter": {
      "shouldRespondToMentions": true
    },
    "solana": {
      "enableAutoTrading": false
    }
  }
}
```

For detailed information about plugin development and usage, see the [ElizaOS Registry](https://github.com/elizaos-plugins/registry).

---

## Running Multiple Agents

To run multiple agents:

```bash
bun start --characters="characters/agent1.json,characters/agent2.json"
```

Or use environment variables:

```
REMOTE_CHARACTER_URLS=https://example.com/characters.json
```

---

## FAQ

### What's the difference between an agent and a character?

A character defines personality and knowledge, while an agent provides the runtime environment and capabilities to bring that character to life.

### How do I choose the right database adapter?

Choose based on your needs:

- MongoDB: For scalable, document-based storage
- PostgreSQL: For relational data with complex queries
- SQLite: For simple, file-based storage
- Qdrant: For vector search capabilities

### How do I implement custom plugins?

Create a plugin that follows the plugin interface and register it with the runtime. See the plugin documentation for detailed examples.

### Do agents share memory across platforms?

By default, agents maintain separate memory contexts for different platforms to avoid mixing conversations. Use the memory management system and database adapters to persist and retrieve state information.

### How do I handle multiple authentication methods?

Use the character configuration to specify different authentication methods for different services. The runtime will handle the appropriate authentication flow.

### How do I manage environment variables?

Use a combination of:

- `.env` files for local development
- Character-specific settings for per-agent configuration
- Environment variables for production deployment

### Can agents communicate with each other?

Yes, through the message system and shared memory spaces when configured appropriately.

# 📝 ElizaOS Projects

Projects are the main organizational structure in ElizaOS, containing all the necessary components to create and deploy AI agents. A project can include one or more agents, each with their own character definition, plugins, and configurations.

## Project Structure

A typical ElizaOS project structure:

```
my-eliza-project/
├── src/
│   └── index.ts        # Main entry point
├── knowledge/          # Knowledge base files
├── package.json        # Dependencies and scripts
└── tsconfig.json       # TypeScript configuration
```

## Creating a New Project

You can create a new ElizaOS project using:

```bash
# Using npm
npm create eliza@beta

# Or using npx
npx @elizaos/cli@beta create
```

The CLI will guide you through the setup process, including:

- Project name
- Database selection (pglite, postgres, etc.)
- Initial configuration

## Project Configuration

The main project file (`src/index.ts`) exports a default project object:

```typescript
import type { Character, IAgentRuntime, Project, ProjectAgent } from '@elizaos/core';
import customPlugin from './plugin';

// Define the character
export const character: Character = {
  name: 'Agent Name',
  plugins: ['@elizaos/plugin-discord', '@elizaos/plugin-direct'],
  // Other character properties
};

// Create a ProjectAgent that includes the character
export const projectAgent: ProjectAgent = {
  character,
  init: async (runtime: IAgentRuntime) => {
    // Initialize agent-specific functionality
    console.log('Initializing agent:', character.name);
  },
  plugins: [customPlugin],
  tests: [], // Optional tests for your agent
};

// Export the full project with all agents
const project: Project = {
  agents: [projectAgent],
};

export default project;
```

## Character Configuration

Each agent in your project requires a character definition that controls its personality, knowledge, and behavior.

### Required Character Fields

```typescript
{
  name: "agent_name", // Character's display name
  plugins: ["@elizaos/plugin-discord"], // Example plugins
  settings: {
    // Configuration settings
    secrets: {}, // API keys and sensitive data
    voice: {}, // Voice configuration
  },
  bio: [], // Character background as a string or array of statements
  style: {
    // Interaction style guide
    all: [], // General style rules
    chat: [], // Chat-specific style
    post: [] // Post-specific style
  }
}
```

### Plugins

Plugins provide your agent with capabilities and integrations:

- `@elizaos/plugin-discord`: Discord integration
- `@elizaos/plugin-telegram`: Telegram integration
- `@elizaos/plugin-twitter`: Twitter/X integration
- `@elizaos/plugin-slack`: Slack integration
- `@elizaos/plugin-direct`: Direct chat interface
- `@elizaos/plugin-simsai`: SimsAI platform integration

View all available plugins: https://github.com/elizaos-plugins/registry

### Settings Configuration

The `settings` object supports various configurations:

```typescript
{
  "settings": {
    "ragKnowledge": false, // Enable RAG knowledge mode
    "voice": {
      "model": "string", // Voice synthesis model
      "url": "string" // Optional voice API URL
    },
    "secrets": {
      // API keys (use env vars in production)
      "API_KEY": "string"
    },
  }
}
```

### Bio & Style

Define your agent's personality and communication style:

```typescript
{
  "bio": ["Expert in blockchain development", "Specializes in DeFi protocols"],
  "style": {
    "all": [
      // Applied to all interactions
      "Keep responses clear",
      "Maintain professional tone"
    ],
    "chat": [
      // Chat-specific style
      "Engage with curiosity",
      "Provide explanations"
    ],
    "post": [
      // Social post style
      "Keep posts informative",
      "Focus on key points"
    ]
  }
}
```

**Style Tips**

- Be specific about tone and mannerisms
- Include platform-specific guidance
- Define clear boundaries and limitations

### Optional Character Fields

```typescript
{
  "username": "handle", // Character's username/handle
  "system": "System prompt text", // Custom system prompt
  "lore": [], // Additional background/history
  "knowledge": [
    // Knowledge base entries
    "Direct string knowledge",
    { "path": "file/path.md", "shared": false },
    { "directory": "knowledge/path", "shared": false }
  ],
  "messageExamples": [], // Example conversations
  "postExamples": [], // Example social posts
  "topics": [], // Areas of expertise
  "adjectives": [] // Character traits
}
```

## Knowledge Management

ElizaOS supports two knowledge modes:

### Classic Mode (Default)

- Direct string knowledge added to character's context
- No chunking or semantic search
- Enabled by default (`settings.ragKnowledge: false`)
- Only processes string knowledge entries
- Simpler but less sophisticated

### RAG Mode

- Advanced knowledge processing with semantic search
- Chunks content and uses embeddings
- Must be explicitly enabled (`settings.ragKnowledge: true`)
- Supports three knowledge types:
  1. Direct string knowledge
  2. Single file references: `{ "path": "path/to/file.md", "shared": false }`
  3. Directory references: `{ "directory": "knowledge/dir", "shared": false }`
- Supported file types: .md, .txt, .pdf
- Optional `shared` flag for knowledge reuse across characters

### Knowledge Path Configuration

- Knowledge files are relative to the project's `knowledge` directory
- Paths should not contain `../` (sanitized for security)
- Both shared and private knowledge supported
- Files automatically reloaded if content changes

## Example Project

Here's a complete example of a project configuration:

```typescript
import type { Character, IAgentRuntime, Project, ProjectAgent } from '@elizaos/core';

export const character: Character = {
  name: 'Tech Helper',
  plugins: ['@elizaos/plugin-discord', '@elizaos/plugin-direct'],
  settings: {
    ragKnowledge: true,
    voice: {
      model: 'en_US-male-medium',
    },
    discord: {
      shouldRespondOnlyToMentions: false,
      allowedChannelIds: ['123456789012345678'],
    },
  },
  bio: ['Friendly technical assistant', 'Specializes in explaining complex topics simply'],
  lore: ['Pioneer in open-source AI development', 'Advocate for AI accessibility'],
  messageExamples: [
    [
      {
        name: 'user1',
        content: { text: 'Can you explain how AI models work?' },
      },
      {
        name: 'TechAI',
        content: {
          text: 'Think of AI models like pattern recognition systems.',
        },
      },
    ],
  ],
  topics: ['artificial intelligence', 'machine learning', 'technology education'],
  knowledge: [
    {
      directory: 'tech_guides',
      shared: true,
    },
  ],
  style: {
    all: ['Clear', 'Patient', 'Educational'],
    chat: ['Interactive', 'Supportive'],
    post: ['Concise', 'Informative'],
  },
};

export const projectAgent: ProjectAgent = {
  character,
  init: async (runtime: IAgentRuntime) => {
    console.log('Initializing Tech Helper agent');
  },
  plugins: [], // Project-specific plugins
};

const project: Project = {
  agents: [projectAgent],
};

export default project;
```

## Character File Export

While projects are the primary structure in ElizaOS, you can still export standalone character files for compatibility with other systems or sharing character definitions:

```typescript
import fs from 'fs';
import { character } from './src/index';

// Export character to JSON file
fs.writeFileSync('character.json', JSON.stringify(character, null, 2));
```

## Managing Multiple Agents

A project can contain multiple agents, each with its own character and plugins:

```typescript
const project: Project = {
  agents: [
    {
      character: technicalSupportCharacter,
      init: async (runtime) => {
        /* init code */
      },
      plugins: [customSupportPlugin],
    },
    {
      character: communityManagerCharacter,
      init: async (runtime) => {
        /* init code */
      },
      plugins: [communityPlugin],
    },
  ],
};
```

Each agent operates independently but can share the same database and resources.

## Running Your Project

After configuring your project, you can run it using:

```bash
npx @elizaos/cli start
```

This will start your agents according to your project configuration.

Eliza Docs 2 Action Evaluators Providers


#  Actions

Actions define how agents respond to and interact with messages. They enable agents to perform tasks beyond simple message responses by integrating with external systems and modifying behavior.

## Overview

Actions are core components that define an agent's capabilities and how it can respond to conversations. Each action represents a distinct operation that an agent can perform, ranging from simple replies to complex interactions with external systems.

1. Structure:

An Action consists of:

- `name`: Unique identifier
- `similes`: Alternative names/triggers
- `description`: Purpose and usage explanation
- `validate`: Function to check if action is appropriate
- `handler`: Core implementation logic
- `examples`: Sample usage patterns
- `suppressInitialMessage`: Optional flag to suppress initial response

2. Agent Decision Flow:

When a message is received:

- The agent evaluates all available actions using their validation functions
- Valid actions are provided to the LLM via the `actionsProvider`
- The LLM decides which action(s) to execute
- Each action's handler generates a response including a "thought" component (agent's internal reasoning)
- The response is processed and sent back to the conversation

3. Integration:

Actions work in concert with:

- **Providers** - Supply context before the agent decides what action to take
- **Evaluators** - Process conversations after actions to extract insights and update memory
- **Services** - Enable actions to interact with external systems

---

## Implementation

The core Action interface includes the following components:

```typescript
interface Action {
  name: string; // Unique identifier
  similes: string[]; // Alternative names/triggers
  description: string; // Purpose and usage explanation
  validate: (runtime: IAgentRuntime, message: Memory, state?: State) => Promise<boolean>;
  handler: (
    runtime: IAgentRuntime,
    message: Memory,
    state?: State,
    options?: any,
    callback?: HandlerCallback
  ) => Promise<boolean>;
  examples: ActionExample[][];
  suppressInitialMessage?: boolean; // Optional flag
}

// Handler callback for generating responses
type HandlerCallback = (content: Content) => Promise<void>;

// Response content structure
interface Content {
  text: string;
  thought?: string; // Internal reasoning (not shown to users)
  actions?: string[]; // List of action names being performed
  action?: string; // Legacy single action name
  attachments?: Attachment[]; // Optional media attachments
}
```

### Basic Action Template

Here's a simplified template for creating a custom action:

```typescript
const customAction: Action = {
  name: 'CUSTOM_ACTION',
  similes: ['ALTERNATE_NAME', 'OTHER_TRIGGER'],
  description: 'Detailed description of when and how to use this action',

  validate: async (runtime: IAgentRuntime, message: Memory, state?: State) => {
    // Logic to determine if this action applies to the current message
    // Should be efficient and quick to check
    return true; // Return true if action is valid for this message
  },

  handler: async (
    runtime: IAgentRuntime,
    message: Memory,
    state?: State,
    options?: any,
    callback?: HandlerCallback
  ) => {
    // Implementation logic - what the action actually does

    // Generate a response with thought and text components
    const responseContent = {
      thought: 'Internal reasoning about what to do (not shown to users)',
      text: 'The actual message to send to the conversation',
      actions: ['CUSTOM_ACTION'], // List of actions being performed
    };

    // Send the response using the callback
    if (callback) {
      await callback(responseContent);
    }

    return true; // Return true if action executed successfully
  },

  examples: [
    [
      {
        name: '{{name1}}',
        content: { text: 'Trigger message' },
      },
      {
        name: '{{name2}}',
        content: {
          text: 'Response',
          thought: 'Internal reasoning',
          actions: ['CUSTOM_ACTION'],
        },
      },
    ],
  ],
};
```

### Character File Example

Actions can be referenced in character files to define how an agent should respond to specific types of messages:

```json
"messageExamples": [
    [
        {
            "user": "{{user1}}",
            "content": {
                "text": "Can you help transfer some SOL?"
            }
        },
        {
            "user": "SBF",
            "content": {
                "text": "yeah yeah for sure, sending SOL is pretty straightforward. just need the recipient and amount. everything else is basically fine, trust me.",
                "actions": ["SEND_SOL"]
            }
        }
    ]
]
```

### The Reply Action

The most fundamental action is the `REPLY` action, which allows agents to respond to messages with text. It serves as the default action when no specialized behavior is needed:

```typescript
const replyAction: Action = {
  name: 'REPLY',
  similes: ['GREET', 'REPLY_TO_MESSAGE', 'SEND_REPLY', 'RESPOND', 'RESPONSE'],
  description: 'Replies to the current conversation with the text from the generated message.',

  validate: async (_runtime: IAgentRuntime) => true, // Always valid

  handler: async (
    runtime: IAgentRuntime,
    message: Memory,
    state: State,
    _options: any,
    callback: HandlerCallback
  ) => {
    // Compose state with necessary providers
    state = await runtime.composeState(message, [
      ...(message.content.providers ?? []),
      'RECENT_MESSAGES',
    ]);

    // Generate response using LLM
    const response = await runtime.useModel(ModelType.TEXT_SMALL, {
      prompt: composePromptFromState({
        state,
        template: replyTemplate,
      }),
    });

    // Parse and format response
    const responseContentObj = parseJSONObjectFromText(response);
    const responseContent = {
      thought: responseContentObj.thought,
      text: responseContentObj.message || '',
      actions: ['REPLY'],
    };

    // Send response via callback
    await callback(responseContent);
    return true;
  },

  examples: [
    /* Examples omitted for brevity */
  ],
};
```

---

## Actions Provider Integration

The actions provider is responsible for making valid actions available to the agent's reasoning process. When a message is received:

1. The provider validates all available actions against the current message
2. It formats the valid actions for inclusion in the agent context
3. This formatted information is used by the agent to decide which action(s) to take

```typescript
const actionsProvider: Provider = {
  name: 'ACTIONS',
  description: 'Possible response actions',
  position: -1, // High priority provider
  get: async (runtime: IAgentRuntime, message: Memory, state: State) => {
    // Validate all actions for this message
    const actionPromises = runtime.actions.map(async (action: Action) => {
      const result = await action.validate(runtime, message, state);
      return result ? action : null;
    });

    const resolvedActions = await Promise.all(actionPromises);
    const actionsData = resolvedActions.filter(Boolean);

    // Format action information for the agent
    const values = {
      actionNames: `Possible response actions: ${formatActionNames(actionsData)}`,
      actions: formatActions(actionsData),
      actionExamples: composeActionExamples(actionsData, 10),
    };

    // Return data, values, and text representation
    return {
      data: { actionsData },
      values,
      text: [values.actionNames, values.actionExamples, values.actions]
        .filter(Boolean)
        .join('\n\n'),
    };
  },
};
```

## Example Implementations

ElizaOS includes a wide variety of predefined actions across various plugins in the ecosystem. Here are some key categories:

### Communication Actions

- **REPLY**: Standard text response
- **CONTINUE**: Extend the conversation
- **IGNORE**: End the conversation or ignore irrelevant messages

### Blockchain and Token Actions

- **SEND_TOKEN**: Transfer cryptocurrency
- **CREATE_TOKEN**: Create a new token on a blockchain
- **READ_CONTRACT/WRITE_CONTRACT**: Interact with smart contracts

### Media and Content Generation

- **GENERATE_IMAGE**: Create images from text descriptions
- **SEND_GIF**: Share animated content
- **GENERATE_3D**: Create 3D content

### AI and Agent Management

- **LAUNCH_AGENT**: Create and start a new agent
- **START_SESSION**: Begin an interactive session
- **GENERATE_MEME**: Create humorous content

### Example Image Generation Action

Here's a more detailed example of an image generation action:

```typescript
const generateImageAction: Action = {
  name: 'GENERATE_IMAGE',
  similes: ['CREATE_IMAGE', 'MAKE_IMAGE', 'DRAW'],
  description: "Generates an image based on the user's description",
  suppressInitialMessage: true, // Don't send initial text response

  validate: async (runtime: IAgentRuntime, message: Memory) => {
    const text = message.content.text.toLowerCase();
    return (
      text.includes('generate') ||
      text.includes('create') ||
      text.includes('draw') ||
      text.includes('make an image')
    );
  },

  handler: async (
    runtime: IAgentRuntime,
    message: Memory,
    state?: State,
    _options?: any,
    callback?: HandlerCallback
  ) => {
    try {
      // Get appropriate service
      const imageService = runtime.getService(ServiceType.IMAGE_GENERATION);

      // Generate the response with thought component
      const responseContent = {
        thought:
          "This request is asking for image generation. I'll use the image service to create a visual based on the user's description.",
        text: "I'm generating that image for you now...",
        actions: ['GENERATE_IMAGE'],
      };

      // Send initial response if callback provided
      if (callback) {
        await callback(responseContent);
      }

      // Generate image
      const imageUrl = await imageService.generateImage(message.content.text);

      // Create follow-up message with the generated image
      await runtime.createMemory(
        {
          id: generateId(),
          content: {
            text: "Here's the image I generated:",
            attachments: [
              {
                type: 'image',
                url: imageUrl,
              },
            ],
          },
          agentId: runtime.agentId,
          roomId: message.roomId,
        },
        'messages'
      );

      return true;
    } catch (error) {
      console.error('Image generation failed:', error);

      // Send error response if callback provided
      if (callback) {
        await callback({
          thought: 'The image generation failed due to an error.',
          text: "I'm sorry, I wasn't able to generate that image. There was a technical problem.",
          actions: ['REPLY'],
        });
      }

      return false;
    }
  },

  examples: [
    /* Examples omitted for brevity */
  ],
};
```

## Action-Evaluator-Provider Cycle

Actions are part of a larger cycle in ElizaOS agents:

1. **Providers** fetch relevant context for decision-making
2. **Actions** execute the agent's chosen response
3. **Evaluators** process the conversation to extract insights
4. These insights are stored in memory
5. Future **Providers** can access these insights
6. This informs future **Actions**

For example:

- The FACTS provider retrieves relevant facts about users
- The agent uses this context to decide on an appropriate action
- After the action, the reflection evaluator extracts new facts and relationships
- These are stored in memory and available for future interactions
- This creates a virtuous cycle of continuous learning and improvement

---
#  Evaluators

Evaluators are cognitive components in the ElizaOS framework that enable agents to process conversations, extract knowledge, and build understanding - similar to how humans form memories after interactions. They provide a structured way for agents to introspect, learn from interactions, and evolve over time.

## Understanding Evaluators

Evaluators are specialized functions that work with the [`AgentRuntime`](/api/classes/AgentRuntime) to analyze conversations after a response has been generated. Unlike actions that create responses, evaluators perform background cognitive tasks that enable numerous advanced capabilities:

- **Knowledge Building**: Automatically extract and store facts from conversations
- **Relationship Tracking**: Identify connections between entities
- **Conversation Quality**: Perform self-reflection on interaction quality
- **Goal Tracking**: Determine if conversation objectives are being met
- **Tone Analysis**: Evaluate emotional content and adjust future responses
- **User Profiling**: Build understanding of user preferences and needs over time
- **Performance Metrics**: Gather data on agent effectiveness and learn from interactions

### Core Structure

```typescript
interface Evaluator {
  name: string; // Unique identifier
  similes?: string[]; // Alternative names/triggers
  description: string; // Purpose explanation
  examples: EvaluationExample[]; // Sample usage patterns
  handler: Handler; // Implementation logic
  validate: Validator; // Execution criteria check
  alwaysRun?: boolean; // Run regardless of validation
}
```

### Evaluator Execution Flow

The agent runtime executes evaluators as part of its cognitive cycle:

1. Agent processes a message and generates a response
2. Runtime calls `evaluate()` after response generation
3. Each evaluator's `validate()` method determines if it should run
4. For each valid evaluator, the `handler()` function is executed
5. Results are stored in memory and inform future responses

---

## Fact Evaluator: Memory Formation System

The Fact Evaluator serves as the agent's "episodic memory formation" system - similar to how humans process conversations and form memories. Just as you might reflect after a conversation "Oh, I learned something new about Sarah today", the Fact Evaluator systematically processes conversations to build up the agent's understanding of the world and the people in it.

### How It Works

#### 1. Triggering (The "When to Reflect" System)

```typescript
validate: async (runtime: IAgentRuntime, message: Memory): Promise<boolean> => {
  const messageCount = await runtime.messageManager.countMemories(message.roomId);
  const reflectionCount = Math.ceil(runtime.getConversationLength() / 2);
  return messageCount % reflectionCount === 0;
};
```

Just like humans don't consciously analyze every single word in real-time, the Fact Evaluator runs periodically rather than after every message. It triggers a "reflection" phase every few messages to process what's been learned.

#### 2. Fact Extraction (The "What Did I Learn?" System)

The evaluator uses a template-based approach to extract three types of information:

- **Facts**: Unchanging truths about the world or people
  - "Bob lives in New York"
  - "Sarah has a degree in Computer Science"
- **Status**: Temporary or changeable states
  - "Bob is currently working on a new project"
  - "Sarah is visiting Paris this week"
- **Opinions**: Subjective views, feelings, or non-factual statements
  - "Bob thinks the project will be successful"
  - "Sarah loves French cuisine"

#### 3. Memory Deduplication (The "Is This New?" System)

```typescript
const filteredFacts = facts.filter((fact) => {
  return (
    !fact.already_known &&
    fact.type === 'fact' &&
    !fact.in_bio &&
    fact.claim &&
    fact.claim.trim() !== ''
  );
});
```

Just as humans don't need to consciously re-learn things they already know, the Fact Evaluator:

- Checks if information is already known
- Verifies if it's in the agent's existing knowledge (bio)
- Filters out duplicate or corrupted facts

#### 4. Memory Storage (The "Remember This" System)

```typescript
const factMemory = await factsManager.addEmbeddingToMemory({
  userId: agentId!,
  agentId,
  content: { text: fact },
  roomId,
  createdAt: Date.now(),
});
```

Facts are stored with embeddings to enable:

- Semantic search of related facts
- Context-aware recall
- Temporal tracking (when the fact was learned)

### Example Processing

Given this conversation:

```
User: "I just moved to Seattle last month!"
Agent: "How are you finding the weather there?"
User: "It's rainy, but I love my new job at the tech startup"
```

The Fact Evaluator might extract:

```json
[
  {
    "claim": "User moved to Seattle last month",
    "type": "fact",
    "in_bio": false,
    "already_known": false
  },
  {
    "claim": "User works at a tech startup",
    "type": "fact",
    "in_bio": false,
    "already_known": false
  },
  {
    "claim": "User enjoys their new job",
    "type": "opinion",
    "in_bio": false,
    "already_known": false
  }
]
```

### Key Design Considerations

1. **Episodic vs Semantic Memory**

   - Facts build up the agent's semantic memory (general knowledge)
   - The raw conversation remains in episodic memory (specific experiences)

2. **Temporal Awareness**

   - Facts are timestamped to track when they were learned
   - Status facts can be updated as they change

3. **Confidence and Verification**

   - Multiple mentions of a fact increase confidence
   - Contradictory facts can be flagged for verification

4. **Privacy and Relevance**
   - Only stores relevant, conversation-appropriate facts
   - Respects explicit and implicit privacy boundaries

---

## Reflection Evaluator: Self-Awareness System

The reflection evaluator extends beyond fact extraction to enable agents to develop a form of "self-awareness" about their conversational performance. It allows agents to:

1. Generate self-reflective thoughts about the conversation quality
2. Extract factual information from conversations (similar to the Fact Evaluator)
3. Identify and track relationships between entities

### How Reflections Work

When triggered, the reflection evaluator:

1. Analyzes recent conversations and existing knowledge
2. Generates structured reflection output with:
   - Self-reflective thoughts about conversation quality
   - New facts extracted from conversation
   - Identified relationships between entities
3. Stores this information in the agent's memory for future reference

### Example Reflection Output

```json
{
  "thought": "I'm engaging appropriately with John, maintaining a welcoming and professional tone. My questions are helping learn more about him as a new community member.",
  "facts": [
    {
      "claim": "John is new to the community",
      "type": "fact",
      "in_bio": false,
      "already_known": false
    },
    {
      "claim": "John found the community through a friend interested in AI",
      "type": "fact",
      "in_bio": false,
      "already_known": false
    }
  ],
  "relationships": [
    {
      "sourceEntityId": "sarah-agent",
      "targetEntityId": "user-123",
      "tags": ["group_interaction"]
    },
    {
      "sourceEntityId": "user-123",
      "targetEntityId": "sarah-agent",
      "tags": ["group_interaction"]
    }
  ]
}
```

### Implementation Details

The reflection evaluator uses a defined schema to ensure consistent output:

```typescript
const reflectionSchema = z.object({
  facts: z.array(
    z.object({
      claim: z.string(),
      type: z.string(),
      in_bio: z.boolean(),
      already_known: z.boolean(),
    })
  ),
  relationships: z.array(relationshipSchema),
});

const relationshipSchema = z.object({
  sourceEntityId: z.string(),
  targetEntityId: z.string(),
  tags: z.array(z.string()),
  metadata: z
    .object({
      interactions: z.number(),
    })
    .optional(),
});
```

### Validation Logic

The reflection evaluator includes validation logic that determines when reflection should occur:

```typescript
validate: async (runtime: IAgentRuntime, message: Memory): Promise<boolean> => {
  const lastMessageId = await runtime.getCache<string>(
    `${message.roomId}-reflection-last-processed`
  );
  const messages = await runtime.getMemories({
    tableName: 'messages',
    roomId: message.roomId,
    count: runtime.getConversationLength(),
  });

  if (lastMessageId) {
    const lastMessageIndex = messages.findIndex((msg) => msg.id === lastMessageId);
    if (lastMessageIndex !== -1) {
      messages.splice(0, lastMessageIndex + 1);
    }
  }

  const reflectionInterval = Math.ceil(runtime.getConversationLength() / 4);

  return messages.length > reflectionInterval;
};
```

This ensures reflections occur at appropriate intervals, typically after a set number of messages have been exchanged.

## Common Memory Formation Patterns

1. **Progressive Learning**

   ```typescript
   // First conversation
   "I live in Seattle" -> Stores as fact

   // Later conversation
   "I live in the Ballard neighborhood" -> Updates/enhances existing fact
   ```

2. **Fact Chaining**

   ```typescript
   // Original facts
   'Works at tech startup';
   'Startup is in Seattle';

   // Inference potential
   'Works in Seattle tech industry';
   ```

3. **Temporal Tracking**

   ```typescript
   // Status tracking
   t0: 'Looking for a job'(status);
   t1: 'Got a new job'(fact);
   t2: 'Been at job for 3 months'(status);
   ```

4. **Relationship Building**

   ```typescript
   // Initial relationship
   {
     "sourceEntityId": "user-123",
     "targetEntityId": "sarah-agent",
     "tags": ["new_interaction"]
   }

   // Evolving relationship
   {
     "sourceEntityId": "user-123",
     "targetEntityId": "sarah-agent",
     "tags": ["frequent_interaction", "positive_sentiment"],
     "metadata": { "interactions": 15 }
   }
   ```

## Integration with Other Systems

Evaluators work alongside other components:

- **Goal Evaluator**: Facts and reflections may influence goal progress
- **Trust Evaluator**: Fact consistency affects trust scoring
- **Memory Manager**: Facts enhance context for future conversations
- **Providers**: Facts inform response generation

---

## Creating Custom Evaluators

You can create your own evaluators by implementing the `Evaluator` interface:

```typescript
const customEvaluator: Evaluator = {
  name: 'CUSTOM_EVALUATOR',
  similes: ['ANALYZE', 'ASSESS'],
  description: 'Performs custom analysis on conversations',

  validate: async (runtime: IAgentRuntime, message: Memory): Promise<boolean> => {
    // Your validation logic here
    return true;
  },

  handler: async (runtime: IAgentRuntime, message: Memory, state?: State) => {
    // Your evaluation logic here

    // Example of storing evaluation results
    await runtime.addEmbeddingToMemory({
      entityId: runtime.agentId,
      content: { text: 'Evaluation result' },
      roomId: message.roomId,
      createdAt: Date.now(),
    });

    return { result: 'evaluation complete' };
  },

  examples: [
    {
      prompt: `Example context`,
      messages: [
        { name: 'User', content: { text: 'Example message' } },
        { name: 'Agent', content: { text: 'Example response' } },
      ],
      outcome: `{ "result": "example outcome" }`,
    },
  ],
};
```

### Registering Custom Evaluators

Custom evaluators can be registered with the agent runtime:

```typescript
// In your plugin's initialization
export default {
  name: 'custom-evaluator-plugin',
  description: 'Adds custom evaluation capabilities',

  init: async (config: any, runtime: IAgentRuntime) => {
    // Register your custom evaluator
    runtime.registerEvaluator(customEvaluator);
  },

  // Include the evaluator in the plugin exports
  evaluators: [customEvaluator],
};
```

## Best Practices for Memory Formation

1. **Validate Facts**

   - Cross-reference with existing knowledge
   - Consider source reliability
   - Track fact confidence levels

2. **Manage Memory Growth**

   - Prioritize important facts
   - Consolidate related facts
   - Archive outdated status facts

3. **Handle Contradictions**

   - Flag conflicting facts
   - Maintain fact history
   - Update based on newest information

4. **Respect Privacy**

   - Filter sensitive information
   - Consider contextual appropriateness
   - Follow data retention policies

5. **Balance Reflection Frequency**
   - Too frequent: Computational overhead
   - Too infrequent: Missing important information
   - Adapt based on conversation complexity and pace

---

# 🔌 Providers

[Providers](/packages/core/src/providers.ts) are the sources of information for the agent. They provide data or state while acting as the agent's "senses", injecting real-time information into the agent's context. They serve as the eyes, ears, and other sensory inputs that allow the agent to perceive and interact with its environment, like a bridge between the agent and various external systems such as market data, wallet information, sentiment analysis, and temporal context. Anything that the agent knows is either coming from like the built-in context or from a provider. For more info, see the [providers API page](/api/interfaces/provider).

Here's an example of how providers work within ElizaOS:

- A news provider could fetch and format news.
- A computer terminal provider in a game could feed the agent information when the player is near a terminal.
- A wallet provider can provide the agent with the current assets in a wallet.
- A time provider injects the current date and time into the context.

---

## Overview

A provider's primary purpose is to supply dynamic contextual information that integrates with the agent's runtime. They format information for conversation templates and maintain consistent data access. For example:

- **Function:** Providers run during or before an action is executed.
- **Purpose:** They allow for fetching information from other APIs or services to provide different context or ways for an action to be performed.
- **Example:** Before a "Mars rover action" is executed, a provider could fetch information from another API. This fetched information can then be used to enrich the context of the Mars rover action.

The provider interface is defined in [types.ts](/packages/core/src/types.ts):

```typescript
interface Provider {
  /** Provider name */
  name: string;

  /** Description of the provider */
  description?: string;

  /** Whether the provider is dynamic */
  dynamic?: boolean;

  /** Position of the provider in the provider list, positive or negative */
  position?: number;

  /**
   * Whether the provider is private
   *
   * Private providers are not displayed in the regular provider list, they have to be called explicitly
   */
  private?: boolean;

  /** Data retrieval function */
  get: (runtime: IAgentRuntime, message: Memory, state: State) => Promise<ProviderResult>;
}
```

The `get` function takes:

- `runtime`: The agent instance calling the provider
- `message`: The last message received
- `state`: Current conversation state

It returns a `ProviderResult` object that contains:

```typescript
interface ProviderResult {
  values?: {
    [key: string]: any;
  };
  data?: {
    [key: string]: any;
  };
  text?: string;
}
```

- `values`: Key-value pairs to be merged into the agent's state values
- `data`: Additional structured data that can be used by the agent but not directly included in the context
- `text`: String that gets injected into the agent's context

---

## Provider Types and Properties

Providers come with several properties that control how and when they are used:

### Dynamic Providers

Dynamic providers are not automatically included in the context. They must be explicitly requested either in the filter list or include list when composing state.

```typescript
const dynamicProvider: Provider = {
  name: 'dynamicExample',
  description: 'A dynamic provider example',
  dynamic: true,
  get: async (runtime, message, state) => {
    // ...implementation
    return {
      text: 'Dynamic information fetched on demand',
      values: {
        /* key-value pairs */
      },
    };
  },
};
```

### Private Providers

Private providers are not included in the regular provider list and must be explicitly included in the include list when composing state.

```typescript
const privateProvider: Provider = {
  name: 'privateExample',
  description: 'A private provider example',
  private: true,
  get: async (runtime, message, state) => {
    // ...implementation
    return {
      text: 'Private information only available when explicitly requested',
      values: {
        /* key-value pairs */
      },
    };
  },
};
```

### Provider Positioning

The `position` property determines the order in which providers are processed. Lower numbers are processed first.

```typescript
const earlyProvider: Provider = {
  name: 'earlyExample',
  description: 'Runs early in the provider chain',
  position: -100,
  get: async (runtime, message, state) => {
    // ...implementation
    return {
      text: 'Early information',
      values: {
        /* key-value pairs */
      },
    };
  },
};

const lateProvider: Provider = {
  name: 'lateExample',
  description: 'Runs late in the provider chain',
  position: 100,
  get: async (runtime, message, state) => {
    // ...implementation
    return {
      text: 'Late information that might depend on earlier providers',
      values: {
        /* key-value pairs */
      },
    };
  },
};
```

---

## State Composition with Providers

The runtime composes state by gathering data from enabled providers. When calling `composeState`, you can control which providers are used:

```typescript
// Get state with all non-private, non-dynamic providers
const state = await runtime.composeState(message);

// Get state with specific providers only
const filteredState = await runtime.composeState(
  message,
  ['timeProvider', 'factsProvider'], // Only include these providers
  null
);

// Include private or dynamic providers
const enhancedState = await runtime.composeState(
  message,
  null,
  ['privateExample', 'dynamicExample'] // Include these private/dynamic providers
);
```

The system caches provider results to optimize performance. When a provider is called multiple times with the same message, the cached result is used unless you explicitly request a new evaluation.

---

## Examples

ElizaOS providers typically fall into these categories, with examples from the ecosystem:

### System & Integration

- **Time Provider**: Injects current date/time for temporal awareness
- **Giphy Provider**: Provides GIF responses using Giphy API
- **GitBook Provider**: Supplies documentation context from GitBook
- **Topics Provider**: Caches and serves Allora Network topic information

### Blockchain & DeFi

- **Wallet Provider**: Portfolio data from Zerion, balances and prices
- **DePIN Provider**: Network metrics via DePINScan API
- **Chain Providers**: Data from Abstract, Fuel, ICP, EVM networks
- **Market Provider**: Token data from DexScreener, Birdeye APIs

### Knowledge & Data

- **DKG Provider**: OriginTrail decentralized knowledge integration
- **News Provider**: Current events via NewsAPI
- **Trust Provider**: Calculates and injects trust scores

Visit the [ElizaOS Plugin Registry](https://github.com/elizaos-plugins/registry) for a complete list of available plugins and providers.

### Time Provider Example

```typescript
const timeProvider: Provider = {
  name: 'time',
  description: 'Provides the current date and time',
  position: -10, // Run early to ensure time is available for other providers
  get: async (_runtime: IAgentRuntime, _message: Memory) => {
    const currentDate = new Date();
    const options = {
      timeZone: 'UTC',
      dateStyle: 'full' as const,
      timeStyle: 'long' as const,
    };
    const humanReadable = new Intl.DateTimeFormat('en-US', options).format(currentDate);

    return {
      text: `The current date and time is ${humanReadable}. Please use this as your reference for any time-based operations or responses.`,
      values: {
        currentDate: currentDate.toISOString(),
        humanReadableDate: humanReadable,
      },
    };
  },
};
```

### Dynamic Provider Example

```typescript
const weatherProvider: Provider = {
  name: 'weather',
  description: 'Provides weather information for a location',
  dynamic: true, // Only used when explicitly requested
  get: async (runtime: IAgentRuntime, message: Memory, state: State) => {
    // Extract location from state if available
    const location = state?.values?.location || 'San Francisco';

    try {
      // Fetch weather data from an API
      const weatherData = await fetchWeatherData(location);

      return {
        text: `The current weather in ${location} is ${weatherData.description} with a temperature of ${weatherData.temperature}°C.`,
        values: {
          weather: {
            location,
            temperature: weatherData.temperature,
            description: weatherData.description,
            humidity: weatherData.humidity,
          },
        },
        data: {
          // Additional detailed data that doesn't go into the context
          weatherDetails: weatherData,
        },
      };
    } catch (error) {
      // Handle errors gracefully
      return {
        text: `I couldn't retrieve weather information for ${location} at this time.`,
        values: {
          weather: { error: true },
        },
      };
    }
  },
};
```

---

## Best Practices

### 1. Optimize for Efficiency

- Return both structured data (`values`) and formatted text (`text`)
- Use caching for expensive operations
- Include a clear provider name and description

```typescript
const efficientProvider: Provider = {
  name: 'efficientExample',
  description: 'Efficiently provides cached data',
  get: async (runtime, message) => {
    // Check for cached data
    const cacheKey = `data:${message.roomId}`;
    const cachedData = await runtime.getCache(cacheKey);

    if (cachedData) {
      return cachedData;
    }

    // Fetch fresh data if not cached
    const result = {
      text: 'Freshly generated information',
      values: {
        /* key-value pairs */
      },
      data: {
        /* structured data */
      },
    };

    // Cache the result with appropriate TTL
    await runtime.setCache(cacheKey, result, { expires: 30 * 60 * 1000 }); // 30 minutes

    return result;
  },
};
```

### 2. Handle Errors Gracefully

Always handle errors without throwing exceptions that would interrupt the agent's processing:

```typescript
try {
  // Risky operation
} catch (error) {
  return {
    text: "I couldn't retrieve that information right now.",
    values: { error: true },
  };
}
```

### 3. Use Position for Optimal Order

Position providers according to their dependencies:

- Negative positions: Fundamental information providers (time, location)
- Zero (default): Standard information providers
- Positive positions: Providers that depend on other information

### 4. Structure Return Values Consistently

Maintain a consistent structure in your provider's return values to make data easier to use across the system.

Eliza Docs 3 - Entities Services

---

# Entities

Entities in ElizaOS represent users, agents, or any participant that can interact within the system. They form the basis of the entity-component architecture, allowing for flexible data modeling and relationships across the platform.


## Entity Structure

An entity in ElizaOS has the following properties:

```typescript
interface Entity {
  /** Unique identifier, optional on creation */
  id?: UUID;

  /** Names of the entity */
  names: string[];

  /** Optional additional metadata */
  metadata?: { [key: string]: any };

  /** Agent ID this account is related to, for agents should be themselves */
  agentId: UUID;

  /** Optional array of components */
  components?: Component[];
}
```

| Property     | Description                                              |
| ------------ | -------------------------------------------------------- |
| `id`         | Unique identifier for the entity (optional on creation)  |
| `names`      | Array of names the entity is known by                    |
| `metadata`   | Additional information about the entity                  |
| `agentId`    | ID of the agent related to this entity                   |
| `components` | Array of modular data components attached to this entity |

## Components

Components are modular pieces of data attached to entities with the following structure:

```typescript
interface Component {
  id: UUID;
  entityId: UUID;
  agentId: UUID;
  roomId: UUID;
  worldId: UUID;
  sourceEntityId: UUID;
  type: string;
  data: {
    [key: string]: any;
  };
}
```

| Property         | Description                                       |
| ---------------- | ------------------------------------------------- |
| `id`             | Unique identifier for the component               |
| `entityId`       | ID of the entity this component belongs to        |
| `agentId`        | ID of the agent managing this component           |
| `roomId`         | ID of the room this component is associated with  |
| `worldId`        | ID of the world this component is associated with |
| `sourceEntityId` | ID of the entity that created this component      |
| `type`           | Type of component (e.g., "profile", "settings")   |
| `data`           | Additional data specific to this component type   |

## Entity Creation and Management

### Creating an Entity

```typescript
const entityId = await runtime.createEntity({
  names: ['John Doe', 'JohnD'],
  agentId: runtime.agentId,
  metadata: {
    discord: {
      username: 'john_doe',
      name: 'John Doe',
    },
  },
});
```

### Retrieving an Entity

```typescript
// Get an entity by ID
const entity = await runtime.getEntityById(entityId);

// Get all entities in a room
const entitiesInRoom = await runtime.getEntitiesForRoom(roomId, true); // true to include components
```

### Updating an Entity

```typescript
await runtime.updateEntity({
  id: entityId,
  names: [...entity.names, 'Johnny'],
  metadata: {
    ...entity.metadata,
    customProperty: 'value',
  },
});
```

## Component Management

Components allow for flexible data modeling by attaching different types of data to entities.

### Creating a Component

```typescript
await runtime.createComponent({
  id: componentId,
  entityId: entityId,
  agentId: runtime.agentId,
  roomId: roomId,
  worldId: worldId,
  sourceEntityId: creatorEntityId,
  type: 'profile',
  data: {
    bio: 'Software developer interested in AI',
    location: 'San Francisco',
    website: 'https://example.com',
  },
});
```

### Retrieving Components

```typescript
// Get a specific component type
const profileComponent = await runtime.getComponent(
  entityId,
  'profile',
  worldId, // optional filter by world
  sourceEntityId // optional filter by source
);

// Get all components for an entity
const allComponents = await runtime.getComponents(entityId, worldId, sourceEntityId);
```

### Updating Components

```typescript
await runtime.updateComponent({
  id: profileComponent.id,
  data: {
    ...profileComponent.data,
    bio: 'Updated bio information',
  },
});
```

### Deleting Components

```typescript
await runtime.deleteComponent(componentId);
```

## Entity Relationships

Entities can have relationships with other entities, stored in the database:

```typescript
// Create a relationship between entities
await runtime.createRelationship({
  sourceEntityId: entityId1,
  targetEntityId: entityId2,
  tags: ['friend', 'collaborator'],
  metadata: {
    interactions: 5,
    lastInteraction: Date.now(),
  },
});

// Get relationships for an entity
const relationships = await runtime.getRelationships({
  entityId: entityId1,
  tags: ['friend'], // optional filter by tags
});

// Get a specific relationship
const relationship = await runtime.getRelationship({
  sourceEntityId: entityId1,
  targetEntityId: entityId2,
});

// Update a relationship
await runtime.updateRelationship({
  ...relationship,
  metadata: {
    ...relationship.metadata,
    interactions: relationship.metadata.interactions + 1,
    lastInteraction: Date.now(),
  },
});
```

## Entity Resolution

ElizaOS includes a system for resolving entity references from messages and context. This is particularly useful for determining which entity is being referenced in a conversation.

```typescript
// Find an entity by name or reference
const entity = await findEntityByName(runtime, message, state);
```

The entity resolution system considers:

1. Exact matches by ID or username
2. Contextual matches from recent conversations
3. Relationship strength between entities
4. Role-based permissions in worlds

## Entity Details

To get formatted information about entities in a room:

```typescript
// Get detailed information about entities in a room
const entityDetails = await getEntityDetails({
  runtime,
  roomId,
});

// Format entities into a string representation
const formattedEntities = formatEntities({ entities: entitiesInRoom });
```

## Relationship with Rooms and Worlds

Entities participate in rooms and, by extension, in worlds:

```typescript
// Add an entity as a participant in a room
await runtime.addParticipant(entityId, roomId);

// Get all rooms where an entity is a participant
const entityRooms = await runtime.getRoomsForParticipant(entityId);

// Get all participants in a room
const participants = await runtime.getParticipantsForRoom(roomId);
```

When an entity is a participant in a room that belongs to a world, the entity has an implicit relationship with that world.

## Creating Unique Entity IDs

For situations where you need to create deterministic, unique IDs for entity-agent pairs:

```typescript
const uniqueId = createUniqueUuid(runtime, baseUserId);
```

This ensures that each user-agent interaction has a consistent, unique identifier.

## Best Practices

1. **Use meaningful names**: Provide descriptive names in the `names` array to make entity identification easier
2. **Structure metadata carefully**: Organize metadata by source (e.g., `discord`, `telegram`) for clarity
3. **Component segregation**: Use components to separate different aspects of entity data rather than storing everything in metadata
4. **Permission checking**: Always verify permissions before accessing components created by other entities
5. **Relationship maintenance**: Update relationship metadata regularly to reflect recent interactions
6. **Entity resolution**: Use the entity resolution system to correctly identify entities in conversations
7. **Deterministic IDs**: Use `createUniqueUuid` for consistent entity identification across sessions

---

# Services

Services are core components in Eliza that enable AI agents to interact with external platforms and services. Each service provides a specialized interface for communication while maintaining consistent agent behavior across different platforms.

---

## Supported Services

| Service                                                                            | Type          | Key Features                                                                           | Use Cases                                                            |
| ---------------------------------------------------------------------------------- | ------------- | -------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| [Discord](https://github.com/elizaos-plugins/plugin-discord)                       | Communication | • Voice channels • Server management • Moderation tools • Channel management           | • Community management • Gaming servers • Event coordination         |
| [Twitter](https://github.com/elizaos-plugins/plugin-twitter)                       | Social Media  | • Post scheduling • Timeline monitoring • Engagement analytics • Content automation    | • Brand management • Content creation • Social engagement            |
| [Telegram](https://github.com/elizaos-plugins/plugin-telegram)                     | Messaging     | • Bot API • Group chat • Media handling • Command system                               | • Customer support • Community engagement • Broadcast messaging      |
| [Direct](https://github.com/elizaOS/eliza/tree/develop/packages/plugin-direct/src) | API           | • REST endpoints • Web integration • Custom applications • Real-time communication     | • Backend integration • Web apps • Custom interfaces                 |
| [GitHub](https://github.com/elizaos-plugins/plugin-github)                         | Development   | • Repository management • Issue tracking • Pull requests • Code review                 | • Development workflow • Project management • Team collaboration     |
| [Slack](https://github.com/elizaos-plugins/plugin-slack)                           | Enterprise    | • Channel management • Conversation analysis • Workspace tools • Integration hooks     | • Team collaboration • Process automation • Internal tools           |
| [Lens](https://github.com/elizaos-plugins/plugin-lens)                             | Web3          | • Decentralized networking • Content publishing • Memory management • Web3 integration | • Web3 social networking • Content distribution • Decentralized apps |
| [Farcaster](https://github.com/elizaos-plugins/plugin-farcaster)                   | Web3          | • Decentralized social • Content publishing • Community engagement                     | • Web3 communities • Content creation • Social networking            |
| [Auto](https://github.com/elizaos-plugins/plugin-auto)                             | Automation    | • Workload management • Task scheduling • Process automation                           | • Background jobs • Automated tasks • System maintenance             |

**\*Additional services**:

- Instagram: Social media content and engagement
- XMTP: Web3 messaging and communications
- Alexa: Voice interface and smart device control
- Home Assistant: Home automation OS
- Devai.me: AI first social service
- Simsai: Jeeter / Social media platform for AI

---

## System Overview

Services serve as bridges between Eliza agents and various platforms, providing core capabilities:

1. **Message Processing**

   - Platform-specific message formatting and delivery
   - Media handling and attachments via [`Memory`](/api/interfaces/Memory) objects
   - Reply threading and context management
   - Support for different content types

2. **State & Memory Management**

   - Each service maintains independent state to prevent cross-platform contamination
   - Integrates with runtime memory managers for different types of content:
   - Messages processed by one service don't automatically appear in other services' contexts
   - [`State`](/api/interfaces/State) persists across agent restarts through the database adapter

3. **Platform Integration**
   - Authentication and API compliance
   - Event processing and webhooks
   - Rate limiting and cache management
   - Platform-specific feature support

## Service Configuration

Services are configured through the [`Character`](/api/type-aliases/Character) configuration's `settings` property:

```typescript
export type Character = {
  // ... other properties ...
  settings?: {
    discord?: {
      shouldIgnoreBotMessages?: boolean;
      shouldIgnoreDirectMessages?: boolean;
      shouldRespondOnlyToMentions?: boolean;
      messageSimilarityThreshold?: number;
      isPartOfTeam?: boolean;
      teamAgentIds?: string[];
      teamLeaderId?: string;
      teamMemberInterestKeywords?: string[];
      allowedChannelIds?: string[];
      autoPost?: {
        enabled?: boolean;
        monitorTime?: number;
        inactivityThreshold?: number;
        mainChannelId?: string;
        announcementChannelIds?: string[];
        minTimeBetweenPosts?: number;
      };
    };
    telegram?: {
      shouldIgnoreBotMessages?: boolean;
      shouldIgnoreDirectMessages?: boolean;
      shouldRespondOnlyToMentions?: boolean;
      shouldOnlyJoinInAllowedGroups?: boolean;
      allowedGroupIds?: string[];
      messageSimilarityThreshold?: number;
      // ... other telegram-specific settings
    };
    slack?: {
      shouldIgnoreBotMessages?: boolean;
      shouldIgnoreDirectMessages?: boolean;
    };
    // ... other service configs
  };
};
```

## Service Implementation

Each service manages its own:

- Platform-specific message formatting and delivery
- Event processing and webhooks
- Authentication and API integration
- Message queueing and rate limiting
- Media handling and attachments
- State management and persistence

Example of a basic service implementation:

```typescript
import { Service, IAgentRuntime } from '@elizaos/core';

export class CustomService extends Service {
  static serviceType = 'custom';
  capabilityDescription = 'The agent is able to interact with the custom platform';

  constructor(protected runtime: IAgentRuntime) {
    super();
    // Initialize platform connection
    // Set up event handlers
    // Configure message processing
  }

  static async start(runtime: IAgentRuntime): Promise<CustomService> {
    const service = new CustomService(runtime);
    // Additional initialization if needed
    return service;
  }

  async stop(): Promise<void> {
    // Cleanup resources
    // Close connections
  }
}
```

### Runtime Integration

Services interact with the agent runtime through the [`IAgentRuntime`](api/interfaces/IAgentRuntime/) interface, which provides:

- Memory managers for different types of data storage
- Service access for capabilities like transcription or image generation
- State management and composition
- Message processing and action handling

### Memory System Integration

Services use the runtime's memory managers to persist conversation data (source: [`memory.ts`](/api/interfaces/Memory)).

- `messageManager` Chat messages
- `documentsManager` File attachments
- `descriptionManager` Media descriptions

<details>
<summary>See example</summary>
```typescript
// Store a new message
await runtime.messageManager.createMemory({
    id: messageId,
    content: { text: message.content },
    userId: userId,
    roomId: roomId,
    agentId: runtime.agentId
});

// Retrieve recent messages
const recentMessages = await runtime.messageManager.getMemories({
roomId: roomId,
count: 10
});

```
</details>


---

## FAQ

### What can services actually do?

Services handle platform-specific communication (like Discord messages or Twitter posts), manage memories and state, and execute actions like processing media or handling commands. Each service adapts these capabilities to its platform while maintaining consistent agent behavior.

### Can multiple services be used simultaneously?
Yes, Eliza supports running multiple services concurrently while maintaining consistent agent behavior across platforms.

### How are service-specific features handled?
Each service implements platform-specific features through its capabilities system, while maintaining a consistent interface for the agent.

### How do services handle rate limits?
Services implement platform-specific rate limiting with backoff strategies and queue management.

### How is service state managed?
Services maintain their own connection state while integrating with the agent's runtime database adapter and memory / state management system.

### How do services handle messages?

Services translate platform messages into Eliza's internal format, process any attachments (images, audio, etc.), maintain conversation context, and manage response queuing and rate limits.

### How are messages processed across services?
Each service processes messages independently in its platform-specific format, while maintaining conversation context through the shared memory system. V2 improves upon this architecture.

### How is state managed between services?
Each service maintains separate state to prevent cross-contamination, but can access shared agent state through the runtime.


### How do services integrate with platforms?

Each service implements platform-specific authentication, API compliance, webhook handling, and follows the platform's rules for rate limiting and content formatting.

### How do services manage memory?

Services use Eliza's memory system to track conversations, user relationships, and state, enabling context-aware responses and persistent interactions across sessions.
```

Eliza Docs 4 - Database System Knowledge Management

---

#  Database System

The ElizaOS database system provides persistent storage capabilities for agents. It handles memory storage, entity relationships, knowledge management, and more through a flexible adapter-based architecture.

## Overview

```mermaid
graph TB
    %% Main Components
    Runtime([Agent Runtime])
    DbAdapter([Database Adapter])
    DbConnection[("Database (PGLite/PostgreSQL)")]

    %% Data Models in compact form
    DataModels["Data Models: Entities, Components, Memories, Relationships, Rooms, Worlds, Tasks Cache"]

    %% Vector Search
    VectorStore[(Vector Store)]

    %% Memories Knowledge
    MemoriesKnowledge[(Memories / Knowledge)]

    %% Connection flow
    Runtime -->|Uses| DbAdapter
    DbAdapter -->|Connects to| DbConnection
    DbConnection -->|Stores & Retrieves| DataModels

    %% Connect Vector Store
    DbConnection -->|Utilizes| VectorStore
    VectorStore -->|Enables Search on| MemoriesKnowledge

    %% Styling
    classDef default fill:#f0f4f8,stroke:#2c3e50,stroke-width:1px;
    classDef runtime fill:#3498db,stroke:#2c3e50,stroke-width:1px,color:#fff;
    classDef adapter fill:#9b59b6,stroke:#2c3e50,stroke-width:1px,color:#fff;
    classDef db fill:#27ae60,stroke:#2c3e50,stroke-width:1px,color:#fff;
    classDef datamodels fill:#52be80,stroke:#2c3e50,stroke-width:1px,color:#fff;
    classDef memories fill:#2c5e1a,stroke:#2c3333,stroke-width:1px,color:#fff;

    class Runtime runtime;
    class DbAdapter adapter;
    class DbConnection,VectorStore db;
    class DataModels datamodels;
    class MemoriesKnowledge memories;
```

ElizaOS uses a unified database architecture based on Drizzle ORM with adapters that implement the [`IDatabaseAdapter`](/api/interfaces/IDatabaseAdapter) interface. The current release includes support for:

| Adapter        | Best For                    | Key Features                                                      |
| -------------- | --------------------------- | ----------------------------------------------------------------- |
| **PGLite**     | Local development & testing | Lightweight PostgreSQL implementation running in Node.js process  |
| **PostgreSQL** | Production deployments      | Full PostgreSQL with vector search, scaling, and high reliability |

Additional database adapters will be supported in future releases as ElizaOS continues to evolve.

## Core Functionality

All database adapters extend the `BaseDrizzleAdapter` abstract class, which provides a comprehensive set of methods for managing all aspects of agent data:

### Entity System

| Method                 | Description                           |
| ---------------------- | ------------------------------------- |
| `createEntity()`       | Create a new entity                   |
| `getEntityById()`      | Retrieve an entity by ID              |
| `getEntitiesForRoom()` | Get all entities in a room            |
| `updateEntity()`       | Update entity attributes              |
| `getComponent()`       | Get a specific component of an entity |
| `getComponents()`      | Get all components for an entity      |
| `createComponent()`    | Add a component to an entity          |
| `updateComponent()`    | Update a component                    |
| `deleteComponent()`    | Remove a component                    |

### Memory Management

| Method                        | Description                          |
| ----------------------------- | ------------------------------------ |
| `createMemory()`              | Store a new memory with metadata     |
| `getMemoryById()`             | Retrieve a specific memory           |
| `getMemories()`               | Get memories matching criteria       |
| `getMemoriesByIds()`          | Get multiple memories by IDs         |
| `getMemoriesByRoomIds()`      | Get memories from multiple rooms     |
| `searchMemories()`            | Search memories by vector similarity |
| `searchMemoriesByEmbedding()` | Search using raw embedding vector    |
| `deleteMemory()`              | Remove a specific memory             |
| `deleteAllMemories()`         | Remove all memories in a room        |
| `countMemories()`             | Count memories matching criteria     |

### Room & Participant Management

| Method                       | Description                     |
| ---------------------------- | ------------------------------- |
| `createRoom()`               | Create a new conversation room  |
| `getRoom()`                  | Get room by ID                  |
| `getRooms()`                 | Get all rooms in a world        |
| `updateRoom()`               | Update room attributes          |
| `deleteRoom()`               | Remove a room                   |
| `addParticipant()`           | Add entity to room              |
| `removeParticipant()`        | Remove entity from room         |
| `getParticipantsForEntity()` | Get all rooms an entity is in   |
| `getParticipantsForRoom()`   | List entities in a room         |
| `getParticipantUserState()`  | Get entity's state in a room    |
| `setParticipantUserState()`  | Update entity's state in a room |

### Relationship Management

| Method                 | Description                            |
| ---------------------- | -------------------------------------- |
| `createRelationship()` | Create a relationship between entities |
| `updateRelationship()` | Update relationship attributes         |
| `getRelationship()`    | Get a specific relationship            |
| `getRelationships()`   | Get all relationships for an entity    |

### Caching System

| Method          | Description            |
| --------------- | ---------------------- |
| `getCache()`    | Retrieve cached data   |
| `setCache()`    | Store data in cache    |
| `deleteCache()` | Remove data from cache |

### World & Task Management

| Method             | Description                 |
| ------------------ | --------------------------- |
| `createWorld()`    | Create a new world          |
| `getWorld()`       | Get world by ID             |
| `getAllWorlds()`   | List all worlds             |
| `updateWorld()`    | Update world attributes     |
| `removeWorld()`    | Delete a world              |
| `createTask()`     | Create a new task           |
| `getTasks()`       | Get tasks matching criteria |
| `getTasksByName()` | Find tasks by name          |
| `getTask()`        | Get task by ID              |
| `updateTask()`     | Update task attributes      |
| `deleteTask()`     | Remove a task               |

### Agent Management

| Method          | Description               |
| --------------- | ------------------------- |
| `createAgent()` | Create a new agent record |
| `getAgent()`    | Get agent by ID           |
| `getAgents()`   | List all agents           |
| `updateAgent()` | Update agent attributes   |
| `deleteAgent()` | Remove an agent           |
| `countAgents()` | Count total agents        |

### Embedding & Search

| Method                        | Description                    |
| ----------------------------- | ------------------------------ |
| `ensureEmbeddingDimension()`  | Configure embedding dimensions |
| `getCachedEmbeddings()`       | Retrieve cached embeddings     |
| `searchMemories()`            | Vector search for memories     |
| `searchMemoriesByEmbedding()` | Advanced vector search         |

## Architecture

ElizaOS uses a singleton pattern for database connections to ensure efficient resource usage:

```
┌─────────────────────────────────────┐
│           AgentRuntime              │
└───────────────┬─────────────────────┘
                │
                ▼
┌─────────────────────────────────────┐
│        IDatabaseAdapter             │
└───────────────┬─────────────────────┘
                │
                ▼
┌─────────────────────────────────────┐
│       BaseDrizzleAdapter            │
└───────────────┬─────────────────────┘
                │
        ┌───────┴───────┐
        ▼               ▼
┌───────────────┐ ┌─────────────────┐
│ PGLiteAdapter │ │ PostgresAdapter │
└───────┬───────┘ └────────┬────────┘
        │                  │
        ▼                  ▼
┌───────────────┐ ┌─────────────────┐
│PGLiteManager  │ │PostgresManager  │
│  (Singleton)  │ │  (Singleton)    │
└───────────────┘ └─────────────────┘
```

Each adapter is associated with a singleton connection manager that ensures only one database connection is maintained per process, regardless of how many agents are running.

## Implementation

### Initialization

The database adapter is initialized through the SQL plugin:

```typescript
// Plugin registration in project configuration
const project = {
  plugins: ['@elizaos/plugin-sql'],
  // ...
};
```

The SQL plugin automatically selects and initializes the appropriate database adapter based on environment settings:

```typescript
function createDatabaseAdapter(
  config: {
    dataDir?: string;
    postgresUrl?: string;
  },
  agentId: UUID
): IDatabaseAdapter {
  if (config.postgresUrl) {
    return new PgDatabaseAdapter(agentId, postgresConnectionManager);
  }

  // Default to PGLite
  return new PgliteDatabaseAdapter(agentId, pgLiteClientManager);
}
```

### Configuration

Configure the database adapter using environment variables or settings:

```typescript
// For PostgreSQL
process.env.POSTGRES_URL = 'postgresql://username:password@localhost:5432/elizaos';

// For PGLite (default)
process.env.PGLITE_DATA_DIR = './elizadb'; // Optional, defaults to './pglite'
```

### Retry Logic & Error Handling

The database system includes built-in retry logic with exponential backoff and jitter:

```typescript
protected async withRetry<T>(operation: () => Promise<T>): Promise<T> {
  let attempt = 0;
  let lastError: Error | null = null;

  while (attempt < this.maxRetries) {
    try {
      return await operation();
    } catch (error) {
      lastError = error as Error;
      const isRetryable = this.isRetryableError(error);

      if (!isRetryable) {
        break;
      }

      // Calculate delay with exponential backoff and jitter
      const delay = Math.min(
        this.baseDelay * Math.pow(2, attempt) + Math.random() * this.jitterMax,
        this.maxDelay
      );

      await new Promise(resolve => setTimeout(resolve, delay));
      attempt++;
    }
  }

  throw lastError;
}
```

## Example Usage

Here are examples of common database operations:

### Store a Memory

```typescript
await runtime.createMemory(
  {
    entityId: message.entityId,
    agentId: runtime.agentId,
    content: { text: 'Important information to remember' },
    roomId: message.roomId,
    embedding: await runtime.useModel(ModelType.TEXT_EMBEDDING, {
      text: 'Important information to remember',
    }),
  },
  'facts'
);
```

### Search for Memories

```typescript
const embedding = await runtime.useModel(ModelType.TEXT_EMBEDDING, {
  text: 'What did we discuss about databases?',
});

const relevantMemories = await runtime.searchMemories({
  tableName: 'messages',
  embedding,
  roomId: message.roomId,
  count: 5,
});
```

### Manage Entity Relationships

```typescript
// Create a relationship between entities
await runtime.createRelationship({
  sourceEntityId: userEntityId,
  targetEntityId: agentEntityId,
  tags: ['friend', 'frequent_interaction'],
  metadata: {
    interactions: 42,
    trust_level: 'high',
  },
});

// Retrieve relationships
const relationships = await runtime.getRelationships({
  entityId: userEntityId,
  tags: ['friend'],
});
```

## Database Schema

The schema is managed by Drizzle ORM and includes the following key tables:

### Core Tables

- **entities**: The fundamental objects in the system (users, agents, etc.)
- **components**: Modular data attached to entities (profiles, settings, etc.)
- **memories**: Conversation history and other remembered information
- **relationships**: Connections between entities
- **rooms**: Conversation channels
- **participants**: Entity participation in rooms
- **worlds**: Container for multiple rooms
- **tasks**: Scheduled or queued operations
- **cache**: Temporary key-value storage
- **agents**: Agent configuration and state

### Entity-Component System

ElizaOS uses an entity-component architecture where:

- Entities are the base objects (users, agents, etc.)
- Components are pieces of data attached to entities
- This allows for flexible data modeling and extension

For example, a user entity might have profile, preferences, and authentication components.

## Vector Search

Both adapters support vector-based semantic search with some differences:

- **PostgreSQL**: Uses pgvector extension for optimized vector operations
- **PGLite**: Implements vector search in JavaScript with an efficient algorithm

The embedding dimension is configurable based on the model used:

```typescript
await adapter.ensureEmbeddingDimension(1536); // For OpenAI embeddings
```

## FAQ

### How do I choose between PGLite and PostgreSQL?

- Use **PGLite** for:

  - Local development and testing
  - Single-user deployments
  - Situations where installing PostgreSQL is impractical

- Use **PostgreSQL** for:
  - Production deployments
  - Multi-user systems
  - High-volume data
  - When you need advanced scaling features

### How do I configure the database connection?

For PostgreSQL, set the `POSTGRES_URL` environment variable:

```
POSTGRES_URL=postgresql://username:password@localhost:5432/elizaos
```

For PGLite, set the data directory (optional):

```
PGLITE_DATA_DIR=./my-data
```

### How can I inspect the database contents?

For PostgreSQL, use standard PostgreSQL tools like pgAdmin or psql.

For PGLite, the data is stored in the specified data directory as files. You can use tools like DB Browser for SQLite to inspect the SQLite files that PGLite generates.

### How do I migrate between different database adapters?

Currently, there's no built-in migration tool between adapters. For production systems, it's recommended to start with PostgreSQL if you anticipate needing its features.

### What about vector embedding dimension mismatches?

The system automatically handles embedding dimensions based on the model used. If you change embedding models, make sure to:

1. Set the correct dimension with `ensureEmbeddingDimension()`
2. Be aware that mixing different dimensions in the same database can cause issues

### How does the entity-component system work?

The entity-component system (ECS) provides a flexible way to model data:

- **Entities** are base objects with unique IDs
- **Components** are pieces of data attached to entities
- This allows for dynamic composition of objects without complex inheritance

For example, a user entity might have profile, preferences, and authentication components.

### How can I improve database performance?

- For **PostgreSQL**:

  - Ensure the pgvector extension is properly installed
  - Index frequently queried fields
  - Use connection pooling
  - Consider partitioning for large datasets

- For **PGLite**:
  - Keep database size reasonable (under 1GB)
  - Regularly clean up old memories
  - Limit the number of concurrent operations


# Knowledge Management

## Overview

The Knowledge Management system in ElizaOS is a powerful Retrieval-Augmented Generation (RAG) feature that enables agents to process, store, and retrieve information from various sources. This allows agents to provide contextually relevant responses by leveraging stored knowledge during conversations.

## Adding Knowledge to Agents

ElizaOS provides multiple ways to add knowledge to your agents, both during initialization and at runtime.

### Adding Knowledge During Runtime Creation

#### 1. Via Character Definition

The simplest approach is to define knowledge directly in your character configuration:

```typescript
const character: Character = {
  name: 'My Agent',
  // Other character properties...
  knowledge: [
    // Direct string knowledge
    'Important fact: ElizaOS supports multiple knowledge formats',

    // File references
    { path: 'knowledge/documentation.md', shared: false },

    // Directory references
    { directory: 'knowledge/guides', shared: true },
  ],
};
```

The knowledge array supports three formats:

- String literals for direct knowledge
- File objects pointing to specific files
- Directory objects for entire folders of content

#### 2. Programmatically Before Runtime Initialization

You can dynamically load knowledge before creating your runtime:

```typescript
// Load knowledge from files or other sources
const knowledge = [];

// Example: Recursively load documentation files
function loadDocumentation(directoryPath) {
  const files = getFilesRecursively(directoryPath, ['.md']);
  return files.map((filePath) => {
    const relativePath = path.relative(basePath, filePath);
    const content = fs.readFileSync(filePath, 'utf-8');
    return `Path: ${relativePath}\n\n${content}`;
  });
}

// Load documentation
const docKnowledge = loadDocumentation('./docs');
knowledge.push(...docKnowledge);

// Then include in your character definition
const character: Character = {
  // Other character properties...
  knowledge: knowledge,
};
```

### Adding Knowledge After Runtime Creation

#### 1. Using the `addKnowledge` Method

Add knowledge programmatically after the runtime is initialized:

```typescript
// Import needed utilities
import { createUniqueUuid } from '@elizaos/core';

// Create a knowledge item
const knowledgeItem = {
  id: createUniqueUuid(runtime, 'unique-knowledge-identifier'),
  content: {
    text: 'Important information the agent should know...',
  },
};

// Add to runtime with default chunking settings
await runtime.addKnowledge(knowledgeItem);

// Or with custom chunking settings
await runtime.addKnowledge(knowledgeItem, {
  targetTokens: 1500, // Target chunk size (default: 3000)
  overlap: 100, // Overlap between chunks (default: 200)
  modelContextSize: 8192, // Context size of your model (default: 4096)
});
```

#### 2. Processing Files at Runtime

You can dynamically process files at runtime:

```typescript
// For PDF files, use the PDF service
const pdfService = runtime.getService<IPdfService>('pdf');
if (pdfService) {
  const pdfBuffer = fs.readFileSync('./knowledge/document.pdf');
  const textContent = await pdfService.convertPdfToText(pdfBuffer);

  const knowledgeItem = {
    id: createUniqueUuid(runtime, 'document.pdf'),
    content: { text: textContent },
  };

  await runtime.addKnowledge(knowledgeItem);
}
```

## Directory Structure

ElizaOS expects knowledge files to be organized in the following structure:

```
knowledge/          # Root knowledge directory
├── shared/         # Shared knowledge accessible to all agents
└── {agent-name}/   # Agent-specific knowledge directories
```

## Supported File Types

- PDF files (`.pdf`)
- Markdown files (`.md`)
- Text files (`.txt`)

## Knowledge Modes

ElizaOS supports two knowledge modes:

### Classic Mode (Default)

- Direct string knowledge added to character's context
- No chunking or semantic search
- Enabled by default (`settings.ragKnowledge: false`)
- Only processes string knowledge entries
- Simpler but less sophisticated

### RAG Mode

- Advanced knowledge processing with semantic search
- Chunks content and uses embeddings
- Must be explicitly enabled (`settings.ragKnowledge: true`)
- Supports three knowledge types:
  1. Direct string knowledge
  2. Single file references: `{ "path": "path/to/file.md", "shared": false }`
  3. Directory references: `{ "directory": "knowledge/dir", "shared": false }`
- Supported file types: .md, .txt, .pdf
- Optional `shared` flag for knowledge reuse across characters

To enable RAG mode, add this to your character settings:

```typescript
const character: Character = {
  // Other character properties...
  settings: {
    ragKnowledge: true,
  },
};
```

## How Knowledge Processing Works

### Document Processing Flow

The RAG system processes documents through several stages:

1. **Directory Processing**

   - The system scans configured directories in `knowledge/`
   - Files are processed based on their shared/private status and file type

2. **File Processing Pipeline**

   - **Preprocessing**: Reading, cleaning, and normalizing text
   - **Document-level Processing**: Generating embeddings for the entire document
   - **Chunk Processing**: Splitting content into manageable chunks and generating embeddings for each

3. **Retrieval Process**
   - When a user message is received, its embedding is generated
   - This embedding is compared to stored knowledge embeddings
   - The most semantically similar chunks are retrieved
   - Retrieved knowledge is incorporated into the agent's context

This multi-level approach enables:

- Broad document-level semantic search
- Fine-grained chunk-level retrieval for specific information
- Efficient parallel processing of large documents
- Maintenance of document context through metadata linking

### Knowledge Processing Flow Diagram

```mermaid
graph TB
    subgraph Directory_Processing
        A[Read Files from Directory] --> B[File Content]
    end

    subgraph Preprocessing
        B --> C[Clean & Normalize Text]
    end

    subgraph Document_Processing
        C --> D[Generate Document Embedding]
        D --> E[Store Full Document]
        E --> |Metadata| F[File Path]
        E --> |Metadata| G[File Type]
        E --> |Metadata| H[Shared Status]
    end

    subgraph Chunk_Processing
        C --> I[Split into Chunks]
        I --> |512 tokens| J[Chunk 1]
        I --> |20 token overlap| K[...]
        I --> L[Chunk N]

        subgraph Parallel_Processing
            J --> M1[Generate Embedding]
            K --> M2[Generate Embedding]
            L --> M3[Generate Embedding]
        end

        subgraph Chunk_Storage
            M1 --> N1[Store Chunk]
            M2 --> N2[Store Chunk]
            M3 --> N3[Store Chunk]

            N1 --> |Metadata| O[Original Doc Reference]
            N1 --> |Metadata| P[Chunk Index]
            N2 --> |Metadata| O
            N2 --> |Metadata| P
            N3 --> |Metadata| O
            N3 --> |Metadata| P
        end
    end

    style Directory_Processing fill:#f9f,stroke:#333,stroke-width:2px
    style Preprocessing fill:#bbf,stroke:#333,stroke-width:2px
    style Document_Processing fill:#bfb,stroke:#333,stroke-width:2px
    style Chunk_Processing fill:#fbf,stroke:#333,stroke-width:2px
    style Parallel_Processing fill:#fbb,stroke:#333,stroke-width:2px
    style Chunk_Storage fill:#bff,stroke:#333,stroke-width:2px
```

### Processing Parameters

- **Chunk Size**: 512 tokens (default, configurable when adding knowledge)
- **Chunk Overlap**: 20 tokens (default, configurable)
- **Processing Batch Size**: 10 chunks processed concurrently
- **Default Similarity Threshold**: 0.85 for retrieval
- **Default Match Count**: 5 results returned

## Best Practices for Knowledge Management

### Content Organization

1. **Document Structure**

   - Use clear section headings and hierarchical organization
   - Break large documents into logical smaller files
   - Include metadata and context in markdown files
   - Structure information from general to specific

2. **File Management**

   - Use descriptive filenames that reflect content
   - Group related files in subdirectories
   - Keep paths short and meaningful
   - Avoid special characters in filenames

3. **Knowledge Optimization**
   - Keep individual documents focused on specific topics
   - For very detailed information, use smaller chunks (200-300 tokens) by setting `targetTokens`
   - Balance the total number of knowledge items for performance
   - Prefer markdown (.md) files for best processing results

### Processing Large Knowledge Bases

When adding many knowledge items at once, consider implementing a semaphore pattern:

```typescript
import { Semaphore } from '@elizaos/core';

// Create semaphore to limit concurrent processing
const semaphore = new Semaphore(10);

// Process items with controlled concurrency
await Promise.all(
  items.map(async (item) => {
    await semaphore.acquire();
    try {
      await runtime.addKnowledge(item);
    } finally {
      semaphore.release();
    }
  })
);
```

### Knowledge ID Management

When adding knowledge programmatically, use consistent ID generation:

```typescript
import { createUniqueUuid } from '@elizaos/core';
const knowledgeId = createUniqueUuid(runtime, 'my-content');
```

This ensures deterministic IDs that remain stable across sessions.

## Troubleshooting

### Common Issues and Solutions

1. **Knowledge Not Being Retrieved**:

   - Verify the file is in a supported format (PDF, MD, TXT)
   - Check if embeddings were properly generated
   - Ensure similarity threshold isn't too high (default: 0.85)
   - Test retrieval with more specific queries
   - Verify RAG mode is enabled if using file/directory references

2. **Poor Quality Retrievals**:

   - Break down large documents into smaller, focused files
   - Ensure document content is clear and well-structured
   - Review the chunking size and overlap settings
   - Check if the query contains too many common words

3. **Performance Issues**:

   - Monitor the total number of knowledge items
   - Consider reducing the match count for faster retrieval
   - Check embedding processing time for large documents
   - Use shared knowledge efficiently across agents

4. **File Processing Errors**:
   - Verify file permissions
   - Check if paths are correctly structured
   - Ensure PDF files are readable and not password-protected
   - Validate that text encoding is UTF-8

## Technical Implementation Details

### Knowledge ID Relationships

The RAG system uses a hierarchical ID structure to maintain relationships:

```mermaid
classDiagram
    class Document {
        +UUID id
        +String filePath
        +String fileType
        +Boolean isShared
        +Float32Array embedding
        +String content
    }

    class Fragment {
        +UUID id
        +UUID originalId
        +Number chunkIndex
        +String content
        +Float32Array embedding
        +String originalPath
    }

    Document "1" --> "*" Fragment : generates
```

#### ID Generation and Linking

Documents IDs are generated using `createUniqueUuid(runtime, path, isShared)`, making them deterministic. Fragment IDs follow the format `${documentId}-chunk-${index}` to maintain the relationship to their source document.

## API Reference

### Key Methods

#### `runtime.addKnowledge(item: KnowledgeItem, options?): Promise<void>`

Adds new knowledge to the agent.

- Parameters:
  - `item`: A knowledge item containing:
    - `id`: UUID
    - `content`: Object with `text` property
  - `options`: Optional processing configuration:
    - `targetTokens`: Number (default: 3000)
    - `overlap`: Number (default: 200)
    - `modelContextSize`: Number (default: 4096)

#### `runtime.getKnowledge(message: Memory): Promise<KnowledgeItem[]>`

Retrieves knowledge based on a message's content.

- Parameters:
  - `message`: Memory object containing user message
- Returns: Array of matching KnowledgeItem objects

### Knowledge Item Definition

```typescript
interface KnowledgeItem {
  id: UUID;
  content: {
    text: string;
    // Optional additional metadata
    [key: string]: any;
  };
}
```

## Security Considerations

1. **Access Control**:

   - Use the `shared` flag appropriately to control document access
   - Keep sensitive information in agent-specific directories
   - Regularly audit knowledge access patterns

2. **Data Privacy**:
   - Do not store sensitive personal information in knowledge files
   - Review documents for potentially sensitive content before adding
   - Implement appropriate backup and recovery procedures

## Future Considerations

1. **Scalability**:

   - Monitor knowledge base size and performance
   - Plan for regular maintenance and cleanup
   - Consider implementing document versioning

2. **Integration**:
   - Document integration points with other systems
   - Plan for potential future file format support
   - Consider implementing knowledge base analytics

## Support and Resources

- Review the implementation in `packages/core/src/ragknowledge.ts`
- Check the issue tracker for known issues and solutions
- Contribute improvements and bug fixes through pull requests

Eliza Docs 5 Tasks Rooms Worlds

# Tasks

Tasks in ElizaOS provide a powerful way to manage deferred, scheduled, and interactive operations. The Task system allows agents to queue work for later execution, repeat actions at defined intervals, await user input, and implement complex workflows across multiple interactions.

## Task Structure

A task in ElizaOS has the following properties:

```typescript
interface Task {
  id?: UUID; // Unique identifier (auto-generated if not provided)
  name: string; // Name of the task (must match a registered task worker)
  updatedAt?: number; // Timestamp when the task was last updated
  metadata?: {
    // Optional additional configuration
    updateInterval?: number; // For repeating tasks: milliseconds between executions
    options?: {
      // For choice tasks: options for user selection
      name: string;
      description: string;
    }[];
    [key: string]: unknown; // Additional custom metadata
  };
  description: string; // Human-readable description of the task
  roomId?: UUID; // Optional room association (for room-specific tasks)
  worldId?: UUID; // Optional world association (for world-specific tasks)
  tags: string[]; // Tags for categorizing and filtering tasks
}
```

## Task Workers

Task workers define the actual logic that executes when a task runs. Each task worker is registered with the runtime and is identified by name.

```typescript
interface TaskWorker {
  name: string; // Matches the name in the Task
  execute: (
    runtime: IAgentRuntime,
    options: { [key: string]: unknown }, // Options passed during execution
    task: Task // The task being executed
  ) => Promise<void>;
  validate?: (
    // Optional validation before execution
    runtime: IAgentRuntime,
    message: Memory,
    state: State
  ) => Promise<boolean>;
}
```

## Creating and Managing Tasks

### Registering a Task Worker

Before creating tasks, you must register a worker to handle the execution:

```typescript
runtime.registerTaskWorker({
  name: 'SEND_REMINDER',
  validate: async (runtime, message, state) => {
    // Optional validation logic
    return true;
  },
  execute: async (runtime, options, task) => {
    // Task execution logic
    const { roomId } = task;
    const { reminder, userId } = options;

    await runtime.createMemory(
      {
        entityId: runtime.agentId,
        roomId,
        content: {
          text: `Reminder for <@${userId}>: ${reminder}`,
        },
      },
      'messages'
    );

    // Delete the task after it's completed
    await runtime.deleteTask(task.id);
  },
});
```

### Creating a One-time Task

Create a task that will execute once:

```typescript
await runtime.createTask({
  name: 'SEND_REMINDER',
  description: 'Send a reminder message to the user',
  roomId: currentRoomId,
  tags: ['reminder', 'one-time'],
  metadata: {
    userId: message.entityId,
    reminder: 'Submit your weekly report',
    scheduledFor: Date.now() + 86400000, // 24 hours from now
  },
});
```

### Creating a Recurring Task

Create a task that repeats at regular intervals:

```typescript
await runtime.createTask({
  name: 'DAILY_REPORT',
  description: 'Generate and post the daily report',
  roomId: announcementChannelId,
  worldId: serverWorldId,
  tags: ['report', 'repeat', 'daily'],
  metadata: {
    updateInterval: 86400000, // 24 hours in milliseconds
    updatedAt: Date.now(), // When the task was last updated/executed
  },
});
```

### Creating a Task Awaiting User Choice

Create a task that presents options and waits for user input:

```typescript
await runtime.createTask({
  name: 'CONFIRM_ACTION',
  description: 'Confirm the requested action',
  roomId: message.roomId,
  tags: ['confirmation', 'AWAITING_CHOICE'],
  metadata: {
    options: [
      { name: 'confirm', description: 'Proceed with the action' },
      { name: 'cancel', description: 'Cancel the action' },
    ],
    action: 'DELETE_FILES',
    files: ['document1.txt', 'document2.txt'],
  },
});
```

### Managing Tasks

Retrieve, update, and delete tasks as needed:

```typescript
// Get tasks by specific criteria
const reminderTasks = await runtime.getTasks({
  roomId: currentRoomId,
  tags: ['reminder'],
});

// Get tasks by name
const reportTasks = await runtime.getTasksByName('DAILY_REPORT');

// Get a specific task
const task = await runtime.getTask(taskId);

// Update a task
await runtime.updateTask(taskId, {
  description: 'Updated description',
  metadata: {
    ...task.metadata,
    priority: 'high',
  },
});

// Delete a task
await runtime.deleteTask(taskId);
```

## Task Processing

Tasks are processed based on their configuration:

### One-time Tasks

Tasks without an `updateInterval` are executed once when triggered by your code. You are responsible for scheduling their execution by checking for pending tasks in appropriate contexts.

### Recurring Tasks

Tasks with an `updateInterval` are automatically considered for re-execution when:

1. The current time exceeds `updatedAt + updateInterval`
2. Your code explicitly checks for pending recurring tasks

To process recurring tasks, implement logic like this:

```typescript
// In an initialization function or periodic check
async function processRecurringTasks() {
  const now = Date.now();
  const recurringTasks = await runtime.getTasks({
    tags: ['repeat'],
  });

  for (const task of recurringTasks) {
    if (!task.metadata?.updateInterval) continue;

    const lastUpdate = task.metadata.updatedAt || 0;
    const interval = task.metadata.updateInterval;

    if (now >= lastUpdate + interval) {
      const worker = runtime.getTaskWorker(task.name);
      if (worker) {
        try {
          await worker.execute(runtime, {}, task);

          // Update the task's last update time
          await runtime.updateTask(task.id, {
            metadata: {
              ...task.metadata,
              updatedAt: now,
            },
          });
        } catch (error) {
          logger.error(`Error executing task ${task.name}: ${error}`);
        }
      }
    }
  }
}
```

### Tasks Awaiting User Input

Tasks tagged with `AWAITING_CHOICE` are presented to users and wait for their input. These tasks use:

1. The `choice` provider to display available options to users
2. The `CHOOSE_OPTION` action to process user selections

## Common Task Patterns

### Deferred Follow-ups

Create a task to follow up with a user later:

```typescript
runtime.registerTaskWorker({
  name: 'FOLLOW_UP',
  execute: async (runtime, options, task) => {
    const { roomId } = task;
    const { userId, topic } = task.metadata;

    await runtime.createMemory(
      {
        entityId: runtime.agentId,
        roomId,
        content: {
          text: `Hi <@${userId}>, I'm following up about ${topic}. Do you have any updates?`,
        },
      },
      'messages'
    );

    await runtime.deleteTask(task.id);
  },
});

// Create a follow-up task for 2 days later
await runtime.createTask({
  name: 'FOLLOW_UP',
  description: 'Follow up with user about project status',
  roomId: message.roomId,
  tags: ['follow-up', 'one-time'],
  metadata: {
    userId: message.entityId,
    topic: 'the project timeline',
    scheduledFor: Date.now() + 2 * 86400000, // 2 days
  },
});
```

### Multi-step Workflows

Implement complex workflows that span multiple interactions:

```typescript
// First step: Gather requirements
runtime.registerTaskWorker({
  name: 'GATHER_REQUIREMENTS',
  execute: async (runtime, options, task) => {
    // Ask user for requirements and create a new task for the next step
    await runtime.createTask({
      name: 'CONFIRM_REQUIREMENTS',
      description: 'Confirm gathered requirements',
      roomId: task.roomId,
      tags: ['workflow', 'AWAITING_CHOICE'],
      metadata: {
        previousStep: 'GATHER_REQUIREMENTS',
        requirements: options.requirements,
        options: [
          { name: 'confirm', description: 'Confirm requirements are correct' },
          { name: 'revise', description: 'Need to revise requirements' },
        ],
      },
    });

    await runtime.deleteTask(task.id);
  },
});

// Second step: Confirm requirements
runtime.registerTaskWorker({
  name: 'CONFIRM_REQUIREMENTS',
  execute: async (runtime, options, task) => {
    if (options.option === 'confirm') {
      // Move to the next step
      await runtime.createTask({
        name: 'GENERATE_SOLUTION',
        description: 'Generate solution based on requirements',
        roomId: task.roomId,
        tags: ['workflow'],
        metadata: {
          previousStep: 'CONFIRM_REQUIREMENTS',
          requirements: task.metadata.requirements,
        },
      });
    } else {
      // Go back to requirements gathering
      await runtime.createTask({
        name: 'GATHER_REQUIREMENTS',
        description: 'Revise requirements',
        roomId: task.roomId,
        tags: ['workflow'],
        metadata: {
          previousStep: 'CONFIRM_REQUIREMENTS',
          previousRequirements: task.metadata.requirements,
        },
      });
    }

    await runtime.deleteTask(task.id);
  },
});
```

### Scheduled Reports

Create tasks that generate and post reports on a schedule:

```typescript
runtime.registerTaskWorker({
  name: 'GENERATE_WEEKLY_REPORT',
  execute: async (runtime, options, task) => {
    const { roomId } = task;

    // Generate report content
    const reportData = await generateWeeklyReport(runtime);

    // Post the report
    await runtime.createMemory(
      {
        entityId: runtime.agentId,
        roomId,
        content: {
          text: `# Weekly Report\n\n${reportData}`,
        },
      },
      'messages'
    );

    // The task stays active for next week (updateInterval handles timing)
  },
});

// Create a weekly report task
await runtime.createTask({
  name: 'GENERATE_WEEKLY_REPORT',
  description: 'Generate and post weekly activity report',
  roomId: reportChannelId,
  worldId: serverWorldId,
  tags: ['report', 'repeat', 'weekly'],
  metadata: {
    updateInterval: 7 * 86400000, // 7 days
    updatedAt: Date.now(),
    format: 'markdown',
  },
});
```

## Task Events and Monitoring

ElizaOS doesn't currently provide built-in events for task lifecycle, so implement your own monitoring if needed:

```typescript
// Custom monitoring for task execution
async function executeTaskWithMonitoring(runtime, taskWorker, task) {
  try {
    // Create a start log
    await runtime.log({
      body: { taskId: task.id, action: 'start' },
      entityId: runtime.agentId,
      roomId: task.roomId,
      type: 'TASK_EXECUTION',
    });

    // Execute the task
    await taskWorker.execute(runtime, {}, task);

    // Create a completion log
    await runtime.log({
      body: { taskId: task.id, action: 'complete', success: true },
      entityId: runtime.agentId,
      roomId: task.roomId,
      type: 'TASK_EXECUTION',
    });
  } catch (error) {
    // Create an error log
    await runtime.log({
      body: { taskId: task.id, action: 'error', error: error.message },
      entityId: runtime.agentId,
      roomId: task.roomId,
      type: 'TASK_EXECUTION',
    });
  }
}
```

## Best Practices

1. **Use descriptive names and descriptions**: Make tasks easily identifiable with clear names and descriptions

2. **Clean up completed tasks**: Delete one-time tasks after execution to prevent database bloat

3. **Add error handling**: Implement robust error handling in task workers to prevent failures from breaking workflows

4. **Use appropriate tags**: Tag tasks effectively for easy retrieval and categorization

5. **Validate carefully**: Use the `validate` function to ensure tasks only execute in appropriate contexts

6. **Keep tasks atomic**: Design tasks to perform specific, well-defined operations rather than complex actions

7. **Provide clear choices**: When creating choice tasks, make option names and descriptions clear and unambiguous

8. **Manage task lifecycles**: Have a clear strategy for when tasks are created, updated, and deleted

9. **Set reasonable intervals**: For recurring tasks, choose appropriate update intervals that balance timeliness and resource usage

10. **Handle concurrent execution**: Ensure task execution is idempotent to handle potential concurrent executions

# Rooms

Rooms in ElizaOS represent individual interaction spaces within a world. A room can be a conversation, a channel, a thread, or any other defined space where entities can exchange messages and interact. Rooms are typically contained within a world, though they can also exist independently.

## Room Structure

A room in ElizaOS has the following properties:

```typescript
type Room = {
  id: UUID;
  name?: string;
  agentId?: UUID;
  source: string;
  type: ChannelType;
  channelId?: string;
  serverId?: string;
  worldId?: UUID;
  metadata?: Record<string, unknown>;
};
```

| Property    | Description                                                      |
| ----------- | ---------------------------------------------------------------- |
| `id`        | Unique identifier for the room                                   |
| `name`      | Optional display name for the room                               |
| `agentId`   | Optional ID of the agent associated with this room               |
| `source`    | The platform or origin of the room (e.g., 'discord', 'telegram') |
| `type`      | Type of room (DM, GROUP, THREAD, etc.)                           |
| `channelId` | External system channel identifier                               |
| `serverId`  | External system server identifier                                |
| `worldId`   | Optional ID of the parent world                                  |
| `metadata`  | Additional room configuration data                               |

## Room Types

ElizaOS supports several room types, defined in the `ChannelType` enum:

| Type          | Description                               |
| ------------- | ----------------------------------------- |
| `SELF`        | Messages to self                          |
| `DM`          | Direct messages between two participants  |
| `GROUP`       | Group messages with multiple participants |
| `VOICE_DM`    | Voice direct messages                     |
| `VOICE_GROUP` | Voice channels with multiple participants |
| `FEED`        | Social media feed                         |
| `THREAD`      | Threaded conversation                     |
| `WORLD`       | World channel                             |
| `FORUM`       | Forum discussion                          |
| `API`         | Legacy type - Use DM or GROUP instead     |

## Room Creation and Management

### Creating a Room

You can create a new room using the AgentRuntime:

```typescript
const roomId = await runtime.createRoom({
  name: 'general-chat',
  source: 'discord',
  type: ChannelType.GROUP,
  channelId: 'external-channel-id',
  serverId: 'external-server-id',
  worldId: parentWorldId,
});
```

### Ensuring a Room Exists

To create a room if it doesn't already exist:

```typescript
await runtime.ensureRoomExists({
  id: roomId,
  name: 'general-chat',
  source: 'discord',
  type: ChannelType.GROUP,
  channelId: 'external-channel-id',
  serverId: 'external-server-id',
  worldId: parentWorldId,
});
```

### Retrieving Room Information

```typescript
// Get a specific room
const room = await runtime.getRoom(roomId);

// Get all rooms in a world
const worldRooms = await runtime.getRooms(worldId);
```

### Updating Room Properties

```typescript
await runtime.updateRoom({
  id: roomId,
  name: 'renamed-channel',
  metadata: {
    ...room.metadata,
    customProperty: 'value',
  },
});
```

### Deleting a Room

```typescript
await runtime.deleteRoom(roomId);
```

## Participants in Rooms

Rooms can have multiple participants (entities) that can exchange messages.

### Managing Room Participants

```typescript
// Add a participant to a room
await runtime.addParticipant(entityId, roomId);

// Remove a participant from a room
await runtime.removeParticipant(entityId, roomId);

// Get all participants in a room
const participants = await runtime.getParticipantsForRoom(roomId);

// Get all rooms where an entity is a participant
const entityRooms = await runtime.getRoomsForParticipant(entityId);
```

### Participant States

Participants can have different states in a room:

```typescript
// Get a participant's state in a room
const state = await runtime.getParticipantUserState(roomId, entityId);
// Returns: 'FOLLOWED', 'MUTED', or null

// Set a participant's state in a room
await runtime.setParticipantUserState(roomId, entityId, 'FOLLOWED');
```

The participant states are:

| State      | Description                                                                               |
| ---------- | ----------------------------------------------------------------------------------------- |
| `FOLLOWED` | The agent actively follows the conversation and responds without being directly mentioned |
| `MUTED`    | The agent ignores messages in this room                                                   |
| `null`     | Default state - the agent responds only when directly mentioned                           |

## Following and Unfollowing Rooms

ElizaOS allows agents to "follow" rooms to actively participate in conversations without being explicitly mentioned. This functionality is managed through the `FOLLOW_ROOM` and `UNFOLLOW_ROOM` actions.

```typescript
// Follow a room (typically triggered by an action)
await runtime.setParticipantUserState(roomId, runtime.agentId, 'FOLLOWED');

// Unfollow a room
await runtime.setParticipantUserState(roomId, runtime.agentId, null);
```

## Memory and Messages in Rooms

Rooms store messages as memories in the database:

```typescript
// Create a new message in a room
const messageId = await runtime.createMemory(
  {
    entityId: senderEntityId,
    agentId: runtime.agentId,
    roomId: roomId,
    content: {
      text: 'Hello, world!',
      source: 'discord',
    },
    metadata: {
      type: 'message',
    },
  },
  'messages'
);

// Retrieve recent messages from a room
const messages = await runtime.getMemories({
  roomId: roomId,
  count: 10,
  unique: true,
});
```

## Events Related to Rooms

ElizaOS emits events related to room activities:

| Event              | Description                                  |
| ------------------ | -------------------------------------------- |
| `ROOM_JOINED`      | Emitted when an entity joins a room          |
| `ROOM_LEFT`        | Emitted when an entity leaves a room         |
| `MESSAGE_RECEIVED` | Emitted when a message is received in a room |
| `MESSAGE_SENT`     | Emitted when a message is sent to a room     |

### Handling Room Events

```typescript
// Register event handlers in your plugin
const myPlugin: Plugin = {
  name: 'my-room-plugin',
  description: 'Handles room events',

  events: {
    [EventTypes.ROOM_JOINED]: [
      async (payload) => {
        const { runtime, entityId, roomId } = payload;
        console.log(`Entity ${entityId} joined room ${roomId}`);
      },
    ],

    [EventTypes.MESSAGE_RECEIVED]: [
      async (payload: MessagePayload) => {
        const { runtime, message } = payload;
        console.log(`Message received in room ${message.roomId}`);
      },
    ],
  },
};
```

## Room Connection with External Systems

When integrating with external platforms, rooms are typically mapped to channels, conversations, or other interaction spaces:

```typescript
// Ensure the connection exists for a room from an external system
await runtime.ensureConnection({
  entityId: userEntityId,
  roomId: roomId,
  userName: 'username',
  name: 'display-name',
  source: 'discord',
  channelId: 'external-channel-id',
  serverId: 'external-server-id',
  type: ChannelType.GROUP,
  worldId: parentWorldId,
});
```

## Best Practices

1. **Use appropriate room types**: Select the most appropriate room type for each interaction context
2. **Follow relationship order**: Create worlds before creating rooms, as rooms often have a parent world
3. **Use ensureRoomExists**: Use this method to avoid duplicate rooms when syncing with external systems
4. **Clean up rooms**: Delete rooms when they're no longer needed to prevent database bloat
5. **Room metadata**: Use metadata for room-specific configuration that doesn't fit into the standard properties
6. **Follow state management**: Implement clear rules for when agents should follow or unfollow rooms
7. **Handle participants carefully**: Ensure that participant management aligns with external platform behavior

---

# Worlds

Worlds in ElizaOS are collections of entities (users, agents) and rooms (conversations, channels) that form a cohesive environment for interactions. Think of a world as a virtual space, like a Discord server, Slack workspace, or 3D MMO environment, where entities can communicate across multiple channels or areas.


## World Structure

A world in ElizaOS has the following properties:

```typescript
type World = {
  id: UUID;
  name?: string;
  agentId: UUID;
  serverId: string;
  metadata?: {
    ownership?: {
      ownerId: string;
    };
    roles?: {
      [entityId: UUID]: Role;
    };
    [key: string]: unknown;
  };
};
```

| Property   | Description                                          |
| ---------- | ---------------------------------------------------- |
| `id`       | Unique identifier for the world                      |
| `name`     | Optional display name                                |
| `agentId`  | ID of the agent managing this world                  |
| `serverId` | External system identifier (e.g., Discord server ID) |
| `metadata` | Additional world configuration data                  |

The metadata can store custom information, including ownership details and role assignments for entities within the world.

## World Creation and Management

### Creating a World

You can create a new world using the AgentRuntime:

```typescript
const worldId = await runtime.createWorld({
  name: 'My Project Space',
  agentId: runtime.agentId,
  serverId: 'external-system-id',
  metadata: {
    ownership: {
      ownerId: ownerEntityId,
    },
  },
});
```

For many integrations, worlds are automatically created during connection setup with external platforms like Discord or Slack.

### Ensuring a World Exists

If you're not sure if a world exists, you can use `ensureWorldExists()`:

```typescript
await runtime.ensureWorldExists({
  id: worldId,
  name: 'My Project Space',
  agentId: runtime.agentId,
  serverId: 'external-system-id',
});
```

### Retrieving World Information

```typescript
// Get a specific world
const world = await runtime.getWorld(worldId);

// Get all worlds
const allWorlds = await runtime.getAllWorlds();
```

### Updating World Properties

```typescript
await runtime.updateWorld({
  id: worldId,
  name: 'Updated Name',
  metadata: {
    ...world.metadata,
    customProperty: 'value',
  },
});
```

## World Roles System

Worlds support a role-based permission system with the following roles:

| Role    | Description                                           |
| ------- | ----------------------------------------------------- |
| `OWNER` | Full control over the world, can assign any roles     |
| `ADMIN` | Administrative capabilities, can manage most settings |
| `NONE`  | Standard participant with no special permissions      |

### Managing Roles

Roles are stored in the world's metadata and can be updated:

```typescript
// Get existing world
const world = await runtime.getWorld(worldId);

// Ensure roles object exists
if (!world.metadata) world.metadata = {};
if (!world.metadata.roles) world.metadata.roles = {};

// Assign a role to an entity
world.metadata.roles[entityId] = Role.ADMIN;

// Save the world
await runtime.updateWorld(world);
```

For programmatic role management, you can use role-related utilities:

```typescript
import { canModifyRole, findWorldForOwner } from '@elizaos/core';

// Check if user can modify roles
if (canModifyRole(userRole, targetRole, newRole)) {
  // Allow role change
}

// Find world where user is owner
const userWorld = await findWorldForOwner(runtime, entityId);
```

## World Settings

Worlds support configurable settings that can be stored and retrieved:

```typescript
// Get settings for a world
const worldSettings = await getWorldSettings(runtime, serverId);

// Update world settings
worldSettings.MY_SETTING = {
  name: 'My Setting',
  description: 'Description for users',
  value: 'setting-value',
  required: false,
};

// Save settings
await updateWorldSettings(runtime, serverId, worldSettings);
```

## World Events

ElizaOS emits events related to world activities:

| Event             | Description                                    |
| ----------------- | ---------------------------------------------- |
| `WORLD_JOINED`    | Emitted when an agent joins a world            |
| `WORLD_CONNECTED` | Emitted when a world is successfully connected |
| `WORLD_LEFT`      | Emitted when an agent leaves a world           |

### Handling World Events

```typescript
// Register event handlers in your plugin
const myPlugin: Plugin = {
  name: 'my-world-plugin',
  description: 'Handles world events',

  events: {
    [EventTypes.WORLD_JOINED]: [
      async (payload: WorldPayload) => {
        const { world, runtime } = payload;
        console.log(`Joined world: ${world.name}`);
      },
    ],

    [EventTypes.WORLD_LEFT]: [
      async (payload: WorldPayload) => {
        const { world, runtime } = payload;
        console.log(`Left world: ${world.name}`);
      },
    ],
  },
};
```

## Relationship with Rooms

A world contains multiple rooms that entities can interact in. Each room points back to its parent world via the `worldId` property.

```typescript
// Get all rooms in a world
const worldRooms = await runtime.getRooms(worldId);
```

See the [Rooms](./rooms.md) documentation for more details on managing rooms within worlds.

## Best Practices

1. **Always check permissions**: Before performing administrative actions, verify the user has appropriate roles
2. **Handle world metadata carefully**: The metadata object can contain critical configuration, so modify it with care
3. **World-room syncing**: When syncing with external platforms, keep world and room structures in alignment
4. **Event-driven architecture**: Use events to respond to world changes rather than polling for updates
5. **Default settings**: Provide sensible defaults for world settings to make configuration easier


# ElizaOS CLI

The ElizaOS Command Line Interface (CLI) provides a comprehensive set of tools to create, manage, and interact with ElizaOS projects and agents.

## Installation

Install the ElizaOS CLI globally using npm:

```bash
npm install -g @elizaos/cli
```

Or use it directly with npx:

```bash
npx @elizaos/cli [command]
```

## Available Commands

| Command                    | Description                                          |
| -------------------------- | ---------------------------------------------------- |
| [`create`](./create.md)    | Create new projects, plugins, or agents              |
| [`start`](./start.md)      | Start an ElizaOS project or agent                    |
| [`dev`](./create.md)       | Run a project in development mode with hot reloading |
| [`agent`](./agent.md)      | Manage agent configurations and state                |
| [`plugin`](./plugins.md)   | Manage plugins in your project                       |
| [`project`](./projects.md) | Manage project configuration and settings            |
| [`env`](./env.md)          | Configure environment variables and API keys         |
| [`publish`](./publish.md)  | Publish packages to npm registry                     |
| [`update`](./update.md)    | Update ElizaOS components                            |
| [`test`](./test.md)        | Run tests for your project                           |

## Quick Start

### Creating a new project

```bash
# Create a new project using the interactive wizard
npx @elizaos/cli create

# Or specify a name directly
npx @elizaos/cli create my-agent-project
```

### Starting a project

```bash
# Navigate to your project directory
cd my-agent-project

# Start the project
npx @elizaos/cli start
```

### Development mode

```bash
# Run in development mode with hot reloading
npx @elizaos/cli dev
```

## Global Options

These options apply to most commands:

| Option            | Description                   |
| ----------------- | ----------------------------- |
| `--help`, `-h`    | Display help information      |
| `--version`, `-v` | Display version information   |
| `--debug`         | Enable debug logging          |
| `--quiet`         | Suppress non-essential output |
| `--json`          | Output results in JSON format |

## Working with Projects

ElizaOS organizes work into projects, which can contain one or more agents along with their configurations, knowledge files, and dependencies. The CLI provides commands to manage the entire lifecycle of a project:

1. **Create** a new project with `create`
2. **Configure** settings with `env`
3. **Develop** using `dev` for hot reloading
4. **Test** functionality with `test`
5. **Start** in production with `start`
6. **Share** by publishing with `publish`

## Working with Plugins

Plugins extend the functionality of your agents. Manage them with the `plugin` command:

```bash
# Add a plugin to your project
npx @elizaos/cli plugin add @elizaos/plugin-discord

# Remove a plugin
npx @elizaos/cli plugin remove @elizaos/plugin-discord

# List installed plugins
npx @elizaos/cli plugin list
```

## Environment Configuration

Configure your API keys and environment variables with the `env` command:

```bash
# Set OpenAI API key
npx @elizaos/cli env set OPENAI_API_KEY your-api-key

# List all environment variables
npx @elizaos/cli env list
```
---

# Agent Command

The `agent` command allows you to manage, configure, and interact with ElizaOS agents. Use this command to list, get information, start, stop, and update your agents.

## Usage

```bash
npx @elizaos/cli agent <action> [options]
```

## Actions

| Action         | Description                                      |
| -------------- | ------------------------------------------------ |
| `list`, `ls`   | List all agents in the project                   |
| `get`, `g`     | Show detailed information about a specific agent |
| `start`, `s`   | Start an agent                                   |
| `stop`, `st`   | Stop an agent                                    |
| `remove`, `rm` | Remove an agent                                  |
| `set`          | Update agent configuration                       |

## Options

The available options vary by action. Here are some common options:

| Option                | Description                                                   |
| --------------------- | ------------------------------------------------------------- |
| `-n, --name <name>`   | Agent id, name, or index number from list                     |
| `-j, --json`          | Output in JSON format                                         |
| `-p, --path <path>`   | Local path to character JSON file (for start)                 |
| `-r, --remote <url>`  | Remote URL to character JSON file (for start)                 |
| `-c, --config <json>` | Configuration as JSON string (for set)                        |
| `-f, --file <path>`   | Path to configuration file (for set) or output file (for get) |
| `-o, --output <file>` | Output to file (for get)                                      |

## Managing Agents

### List Agents

List all agents in the current project:

```bash
npx @elizaos/cli agent list

# With JSON output
npx @elizaos/cli agent list --json
```

The output includes:

- Agent name
- Agent ID
- Status

### Get Agent Information

Get detailed information about a specific agent:

```bash
# Get agent by name, ID, or index
npx @elizaos/cli agent get --name customer-support

# Save agent configuration to JSON file
npx @elizaos/cli agent get --name customer-support --json

# Specify output file
npx @elizaos/cli agent get --name customer-support --json --output ./my-agent.json
```

### Start an Agent

Start an agent using various methods:

```bash
# Start by name
npx @elizaos/cli agent start --name customer-support

# Start from local JSON file
npx @elizaos/cli agent start --path ./agents/my-agent.json

# Start from remote URL
npx @elizaos/cli agent start --remote https://example.com/agents/my-agent.json

# Start with inline JSON
npx @elizaos/cli agent start --json '{"name":"My Agent","description":"A custom agent"}'
```

### Stop an Agent

Stop a running agent:

```bash
npx @elizaos/cli agent stop --name customer-support
```

### Remove an Agent

Remove an agent from the system:

```bash
npx @elizaos/cli agent remove --name customer-support
```

### Update Agent Configuration

Modify an existing agent's configuration:

```bash
# Update using JSON string
npx @elizaos/cli agent set --name customer-support --config '{"name":"Customer Care Bot"}'

# Update using JSON file
npx @elizaos/cli agent set --name customer-support --file ./updated-config.json
```

## Agent Configuration

ElizaOS agents are configured through a combination of:

- Agent definition file
- Knowledge files
- Runtime configuration options

A typical agent definition looks like:

```typescript
{
  "id": "customer-support",
  "name": "Customer Support Bot",
  "description": "Helps customers with common questions and issues",
  "character": {
    "persona": "You are a friendly and knowledgeable customer support agent.",
    "goals": ["Resolve customer issues efficiently", "Provide accurate information"],
    "constraints": [
      "Never share private customer information",
      "Escalate complex issues to human agents"
    ]
  },
  "llm": {
    "provider": "openai",
    "model": "gpt-4",
    "temperature": 0.7
  },
  "knowledge": [
    "./knowledge/shared/company-info.md",
    "./knowledge/customer-support/faq.md",
    "./knowledge/customer-support/policies.md"
  ],
  "services": {
    "discord": { "enabled": true, "channels": ["support"] },
    "web": { "enabled": true }
  }
}
```

## Examples

### Basic Agent Management

```bash
# List all available agents
npx @elizaos/cli agent list

# Get information about a specific agent
npx @elizaos/cli agent get --name support-bot

# Stop a running agent
npx @elizaos/cli agent stop --name support-bot

# Remove an agent
npx @elizaos/cli agent remove --name old-agent
```

### Starting Agents

```bash
# Start an agent by name
npx @elizaos/cli agent start --name customer-support

# Start an agent from a local file
npx @elizaos/cli agent start --path ./agents/sales-bot.json
```

### Updating Configuration

```bash
# Update an agent's name and description
npx @elizaos/cli agent set --name tech-support --config '{"name":"Technical Support","description":"Technical help desk support"}'

# Update configuration from a file
npx @elizaos/cli agent set --name tech-support --file ./updated-config.json
```

## Troubleshooting

### Agent not found

If you get an "Agent not found" error:

```bash
# Check available agents
npx @elizaos/cli agent list

# Try using the agent ID directly
npx @elizaos/cli agent get --name agent_123456
```

### Configuration errors

If you encounter errors when updating configuration:

```bash
# Validate your JSON syntax
# Use a proper JSON validator

# Check the structure against the expected schema
# Refer to the agent configuration example
```

### Connection issues

If you can't connect to the agent runtime:

```bash
# Check if the runtime is running
npx @elizaos/cli start

# By default, the CLI connects to http://localhost:3000
# If using a different address, set the AGENT_RUNTIME_URL environment variable
AGENT_RUNTIME_URL=http://my-server:3000 npx @elizaos/cli agent list
```

## Related Commands

- [`create`](./create.md): Create a new project with agents
- [`start`](./start.md): Start your project with agents
- [`dev`](./dev.md): Run your project in development mode
- [`env`](./env.md): Configure environment variables

# Create Command

The `create` command is used to scaffold new ElizaOS projects or plugins. It guides you through an interactive setup process to generate the appropriate files and configurations.

## Usage

```bash
npx @elizaos/cli create [options]
```

## Options

| Option         | Description                                                            |
| -------------- | ---------------------------------------------------------------------- |
| `--dir`, `-d`  | Installation directory (defaults to project name in current directory) |
| `--yes`, `-y`  | Skip confirmation prompts                                              |
| `--type`, `-t` | Type to create: `project` or `plugin`                                  |

## Project Types

### Project

A standard ElizaOS project with agent configuration, knowledge setup, and essential components.

```bash
npx @elizaos/cli create --type project
```

This creates a complete project structure:

```
my-agent-project/
├── knowledge/          # Knowledge files for RAG
├── src/                # Source code directory
├── package.json
└── other configuration files
```

### Plugin

A plugin that extends ElizaOS functionality with custom actions, services, providers, or other extensions.

```bash
npx @elizaos/cli create --type plugin
```

This creates a plugin structure:

```
my-plugin/
├── src/                # Plugin source code
├── package.json
└── other configuration files
```

## Interactive Process

When run without all options specified, the command launches an interactive wizard:

1. **Project Type**: If not specified, select between project or plugin
2. **Project Name**: Enter a name for your project or plugin
3. **Database Selection**: For projects, choose your database (PGLite or Postgres)
4. **Database Configuration**: For Postgres, you'll be prompted for your database URL

## Examples

### Creating a basic project

```bash
npx @elizaos/cli create
# Then follow the interactive prompts
```

### Creating a plugin

```bash
npx @elizaos/cli create --type plugin
# Then follow the interactive prompts
```

### Specifying a directory

```bash
npx @elizaos/cli create --dir ./my-projects/new-agent
```

### Skipping confirmation prompts

```bash
npx @elizaos/cli create --yes
```

## After Creation

Once your project is created:

1. Navigate to the project directory:

   ```bash
   cd my-project-name
   ```

2. Start your project:

   ```bash
   npx @elizaos/cli start
   ```

   Or start in development mode:

   ```bash
   npx @elizaos/cli dev
   ```

3. Visit `http://localhost:3000` to view your project in the browser

For plugins, you can:

1. Start development:

   ```bash
   npx @elizaos/cli start
   ```

2. Test your plugin:

   ```bash
   npx @elizaos/cli test
   ```

3. Publish your plugin:

   ```bash
   npx @elizaos/cli plugins publish
   ```

## Related Commands

- [`start`](./start.md): Start your created project
- [`dev`](./dev.md): Run your project in development mode
- [`env`](./env.md): Configure environment variables
- [`plugin`](./plugins.md): Manage plugins in your project

# Dev Command

The `dev` command runs your ElizaOS project or plugin in development mode with auto-rebuild and restart on file changes. This is the recommended way to develop and test your implementations locally.

## Usage

```bash
npx @elizaos/cli dev [options]
```

## Options

| Option         | Description                                                          |
| -------------- | -------------------------------------------------------------------- |
| `--port`, `-p` | Port to listen on                                                    |
| `--configure`  | Reconfigure services and AI models (skips using saved configuration) |
| `--character`  | Path or URL to character file to use instead of default              |
| `--build`      | Build the project before starting                                    |

## Development Features

When you run `dev`, ElizaOS provides several developer-friendly features:

1. **Auto Rebuilding**: Automatically rebuilds your project when source files change
2. **Auto Restarting**: Restarts the server after rebuilds to apply changes
3. **File Watching**: Monitors your source files for changes
4. **TypeScript Support**: Compiles TypeScript files during rebuilds

## What Happens During Dev Mode

When you run the `dev` command, ElizaOS:

1. Detects whether you're in a project or plugin directory
2. Builds the project initially
3. Starts the server using the `start` command with your options
4. Sets up file watching for .ts, .js, .tsx, and .jsx files
5. Rebuilds and restarts when files change

## Examples

### Basic Development Mode

Start your project in development mode:

```bash
# Navigate to your project
cd my-agent-project

# Start development mode
npx @elizaos/cli dev
```

### Custom Port

Run the development server on a specific port:

```bash
npx @elizaos/cli dev --port 8080
```

### Using a Custom Character

Use a specific character file:

```bash
npx @elizaos/cli dev --character ./characters/custom-assistant.json
```

### Force Configuration

Skip using saved configuration and reconfigure services:

```bash
npx @elizaos/cli dev --configure
```

## Development Process

A typical development workflow with ElizaOS:

1. **Edit code**: Modify TypeScript/JavaScript files in your project
2. **Automatic rebuild**: The dev server detects changes and rebuilds
3. **Automatic restart**: The server restarts with your changes
4. **Test**: Interact with your updated implementation
5. **Repeat**: Continue the development cycle

## File Watching

The dev command watches for changes in your project's source files, specifically:

- TypeScript files (`.ts`, `.tsx`)
- JavaScript files (`.js`, `.jsx`)

The file watcher ignores:

- `node_modules/` directory
- `dist/` directory
- `.git/` directory

## Logs and Debugging

The dev mode provides information about the file watching and rebuild process:

```
[info] Running in project mode
[info] Building project...
[success] Build successful
[info] Starting server...
[info] Setting up file watching for directory: /path/to/your/project
[success] File watching initialized in: /path/to/your/project/src
[info] File event: change - src/index.ts
[info] Triggering rebuild for file change: src/index.ts
[info] Rebuilding project after file change...
[success] Rebuild successful, restarting server...
```

## Project Type Detection

The dev command automatically detects whether you're working with:

1. **Project**: A complete ElizaOS project with agents
2. **Plugin**: An ElizaOS plugin that provides extensions

It determines this by checking:

- The package.json metadata
- Export patterns in src/index.ts
- Project structure

## Exiting Dev Mode

To stop the development server:

- Press `Ctrl+C` in the terminal
- The server and file watcher will gracefully shut down

## Troubleshooting

### Build failures

If your project fails to build:

```
[error] Initial build failed: Error message
[info] Continuing with dev mode anyway...
```

The server will still start, but you'll need to fix the build errors for proper functionality.

### File watching issues

If file changes aren't being detected:

1. Check if your files are in the watched directories
2. Ensure you're modifying the right types of files (.ts, .js, .tsx, .jsx)
3. Check for error messages in the console

## Related Commands

- [`start`](./start.md): Run your project in production mode
- [`build`](./projects.md): Build your project manually
- [`project`](./projects.md): Manage project settings


# Environment Command

The `env` command helps you manage environment variables and API keys for your ElizaOS projects. It provides a secure and convenient way to set, view, and manage sensitive configuration.

## Usage

```bash
npx @elizaos/cli env [command] [options]
```

## Commands

| Command           | Description                                               |
| ----------------- | --------------------------------------------------------- |
| `list`            | List all environment variables                            |
| `edit-global`     | Edit global environment variables                         |
| `edit-local`      | Edit local environment variables                          |
| `reset`           | Reset all environment variables and wipe the cache folder |
| `set-path <path>` | Set a custom path for the global environment file         |
| `interactive`     | Start interactive environment variable manager            |

If no command is provided, a help message will be shown with available commands.

## Global vs Local Environment Variables

ElizaOS maintains two levels of environment variables:

1. **Global variables** - Stored in `~/.eliza/.env` by default or in a custom location if set
2. **Local variables** - Stored in `.env` in your current project directory

Global variables are applied to all projects, while local variables are specific to the current project.

## Interactive Mode

The interactive mode provides a user-friendly way to manage environment variables:

```bash
npx @elizaos/cli env interactive
```

This opens a menu with options to:

- List environment variables
- Edit global environment variables
- Edit local environment variables
- Set custom environment path
- Reset environment variables

## Managing Environment Variables

### Listing Variables

View all configured environment variables:

```bash
npx @elizaos/cli env list
```

This will display both global and local variables (if available).

### Editing Global Variables

Edit the global environment variables interactively:

```bash
npx @elizaos/cli env edit-global
```

This provides an interactive interface to:

- View existing global variables
- Add new variables
- Edit existing variables
- Delete variables

### Editing Local Variables

Edit the local environment variables in the current project:

```bash
npx @elizaos/cli env edit-local
```

If no local `.env` file exists, you will be prompted to create one.

### Setting Custom Environment Path

Set a custom location for the global environment file:

```bash
npx @elizaos/cli env set-path /path/to/custom/location
```

If the specified path is a directory, the command will use `/path/to/custom/location/.env`.

### Resetting Environment Variables

Reset all environment variables and clear the cache:

```bash
npx @elizaos/cli env reset
```

This will:

1. Remove the global `.env` file
2. Clear any custom environment path setting
3. Wipe the cache folder
4. Optionally reset the database folder (you'll be prompted)

## Examples

### Viewing Environment Variables

```bash
# List all variables
npx @elizaos/cli env list
```

Output example:

```
Global environment variables (.eliza/.env):
  OPENAI_API_KEY: sk-1234...5678
  MODEL_PROVIDER: openai

Local environment variables (.env):
  PORT: 8080
  LOG_LEVEL: debug
```

### Setting Custom Environment Path

```bash
# Set a custom path for global environment variables
npx @elizaos/cli env set-path ~/projects/eliza-config/.env
```

### Interactive Editing

```bash
# Start interactive mode
npx @elizaos/cli env interactive

# Edit only global variables
npx @elizaos/cli env edit-global

# Edit only local variables
npx @elizaos/cli env edit-local
```

## Key Variables

ElizaOS commonly uses these environment variables:

| Variable             | Description                                  |
| -------------------- | -------------------------------------------- |
| `OPENAI_API_KEY`     | OpenAI API key for model access              |
| `ANTHROPIC_API_KEY`  | Anthropic API key for Claude models          |
| `TELEGRAM_BOT_TOKEN` | Token for Telegram bot integration           |
| `DISCORD_BOT_TOKEN`  | Token for Discord bot integration            |
| `POSTGRES_URL`       | PostgreSQL database connection string        |
| `MODEL_PROVIDER`     | Default model provider to use                |
| `LOG_LEVEL`          | Logging verbosity (debug, info, warn, error) |
| `PORT`               | HTTP API port number                         |

## Security Best Practices

1. **Never commit .env files** to version control
2. **Use separate environments** for development, testing, and production
3. **Set up global variables** for commonly used API keys
4. **Regularly rotate API keys** for security

## Related Commands

- [`start`](./start.md): Start your project with the configured environment
- [`dev`](./dev.md): Run in development mode with the configured environment
- [`create`](./create.md): Create a new project with initial environment setup

# Plugin Command

The `plugin` command helps you manage ElizaOS plugins - modular extensions that add capabilities to your agents.

## Usage

```bash
npx @elizaos/cli plugin <action> [options]
```

## Actions

| Action    | Description                    |
| --------- | ------------------------------ |
| `publish` | Publish a plugin to a registry |

## Publishing Plugins

The `publish` command allows you to publish an ElizaOS plugin to a registry:

```bash
npx @elizaos/cli plugin publish [options]
```

### Options

| Option                      | Description                                               |
| --------------------------- | --------------------------------------------------------- |
| `-r, --registry <registry>` | Target registry (default: "elizaOS/registry")             |
| `-n, --npm`                 | Publish to npm instead of GitHub                          |
| `-t, --test`                | Test publish process without making changes               |
| `-p, --platform <platform>` | Specify platform compatibility (node, browser, universal) |

### Publishing Process

When you run the `publish` command, ElizaOS will:

1. Validate that you're in a plugin directory
2. Check for required GitHub credentials
3. Validate the plugin package.json
4. Build the plugin
5. Publish to the specified registry (GitHub by default, or npm if specified)

### Examples

#### Test Publishing

Test the publishing process without actually publishing:

```bash
npx @elizaos/cli plugin publish --test
```

#### Publishing to npm

Publish your plugin to npm:

```bash
# First make sure you're logged in to npm
npm login

# Then publish
npx @elizaos/cli plugin publish --npm
```

#### Publishing with Platform Specification

Specify platform compatibility when publishing:

```bash
npx @elizaos/cli plugin publish --platform node
```

## Creating a Plugin

To create a new plugin, use the general `create` command with the plugin type:

```bash
npx @elizaos/cli create --type plugin
```

This will guide you through the process of creating a new plugin project with the proper structure.

## Plugin Structure

A typical ElizaOS plugin has this structure:

```
my-plugin/
├── src/
│   └── index.ts        # Plugin entry point
├── dist/               # Compiled code (generated)
├── package.json        # Plugin metadata and dependencies
└── tsconfig.json       # TypeScript configuration
```

The main plugin definition is in `src/index.ts`:

```typescript
import type { Plugin } from '@elizaos/core';

export const myPlugin: Plugin = {
  name: 'my-plugin',
  description: 'My custom plugin for ElizaOS',

  // Plugin components
  actions: [],
  services: [],
  providers: [],
  models: {},

  // Initialization function
  async init(config) {
    // Setup code
  },
};

export default myPlugin;
```

## Requirements for Publishing

Before publishing a plugin, ensure:

1. Your plugin name should include `plugin-` (e.g., `@elizaos/plugin-discord`)
2. A complete package.json with name, version, and description
3. GitHub credentials if publishing to the ElizaOS registry
4. npm login if publishing to npm

## Related Commands

- [`create`](./create.md): Create a new plugin
- [`project list-plugins`](./projects.md): List available plugins to install
- [`project add-plugin`](./projects.md): Add a plugin to your project
- [`project remove-plugin`](./projects.md): Remove a plugin from your project

# Project Command

The `project` command helps you manage ElizaOS projects, including their plugins and configurations.

## Usage

```bash
npx @elizaos/cli project <action> [options]
```

## Actions

| Action          | Description                                        |
| --------------- | -------------------------------------------------- |
| `list-plugins`  | List available plugins to install into the project |
| `add-plugin`    | Add a plugin to the project                        |
| `remove-plugin` | Remove a plugin from the project                   |

## Managing Plugins

### Listing Available Plugins

View all plugins that are available to install:

```bash
npx @elizaos/cli project list-plugins
```

You can filter the results by type:

```bash
npx @elizaos/cli project list-plugins --type adapter
```

### Options for list-plugins

| Option              | Description                              |
| ------------------- | ---------------------------------------- |
| `-t, --type <type>` | Filter by type (adapter, client, plugin) |

### Adding Plugins

Add a plugin to your project:

```bash
npx @elizaos/cli project add-plugin @elizaos/plugin-discord
```

This will:

1. Find the plugin in the registry
2. Install the plugin into your project
3. Add it to your project's dependencies

### Options for add-plugin

| Option            | Description                              |
| ----------------- | ---------------------------------------- |
| `--no-env-prompt` | Skip prompting for environment variables |

### Removing Plugins

Remove a plugin from your project:

```bash
npx @elizaos/cli project remove-plugin @elizaos/plugin-discord
```

This will:

1. Uninstall the plugin from your project
2. Remove it from your project's dependencies

## Examples

### Discovering and installing plugins

```bash
# List all available plugins
npx @elizaos/cli project list-plugins

# Install a specific plugin
npx @elizaos/cli project add-plugin @elizaos/plugin-telegram
```

### Managing multiple plugins

```bash
# Add multiple plugins
npx @elizaos/cli project add-plugin @elizaos/plugin-discord
npx @elizaos/cli project add-plugin @elizaos/plugin-pdf

# Remove a plugin you no longer need
npx @elizaos/cli project remove-plugin @elizaos/plugin-pdf
```

## Plugin Configuration

After adding a plugin, you'll need to configure it in your project's code:

```typescript
// In your src/index.ts file
import { createProject } from '@elizaos/core';
import { discordPlugin } from '@elizaos/plugin-discord';

const project = createProject({
  name: 'my-project',
  plugins: [
    discordPlugin, // Use the imported plugin
  ],
  // Plugin-specific configuration
  discord: {
    token: process.env.DISCORD_BOT_TOKEN,
    guildId: process.env.DISCORD_GUILD_ID,
  },
});

export default project;
```

## Troubleshooting

### Plugin Not Found

If a plugin can't be found in the registry:

```
Plugin @elizaos/plugin-name not found in registry
```

Check for:

1. Typos in the plugin name
2. Network connectivity issues
3. Registry availability

### Installation Problems

If a plugin fails to install:

1. Check that you have the necessary permissions
2. Ensure you have a stable internet connection
3. Check for compatibility issues with your project

## Related Commands

- [`create`](./create.md): Create a new project from scratch
- [`plugin`](./plugins.md): Manage plugin publishing
- [`start`](./start.md): Start your project
- [`dev`](./dev.md): Run your project in development mode

# Publish Command

The `publish` command allows you to package and publish your ElizaOS plugins or projects to the ElizaOS registry and npm, making them available for others to use.

## Usage

```bash
npx @elizaos/cli publish [options]
```

## Options

| Option                      | Description                                               |
| --------------------------- | --------------------------------------------------------- |
| `-r, --registry <registry>` | Target registry (default: "elizaOS/registry")             |
| `-n, --npm`                 | Publish to npm instead of GitHub                          |
| `-t, --test`                | Test publish process without making changes               |
| `-p, --platform <platform>` | Specify platform compatibility (node, browser, universal) |
| `--dry-run`                 | Generate registry files locally without publishing        |
| `--skip-registry`           | Skip publishing to the registry                           |

## Publishing Process

When you run the `publish` command, ElizaOS will:

1. Detect if your package is a plugin or project
2. Validate the package structure and configuration
3. Check for GitHub credentials (if publishing to GitHub)
4. Build the package
5. Publish to npm or GitHub based on options
6. Update the ElizaOS registry with your package metadata

### Project Type Detection

ElizaOS automatically detects if your package is a plugin or project by:

- Checking if the package name contains `plugin-`
- Looking for type definitions in `package.json`
- Analyzing the package exports
- Examining the package description

For plugins, the package name should include `plugin-` (e.g., `@elizaos/plugin-discord`).

## Platform Compatibility

You can specify the platform compatibility of your package:

```bash
# Specify that your plugin works in Node.js only
npx @elizaos/cli publish -p node

# Specify that your plugin works in browsers only
npx @elizaos/cli publish -p browser

# Specify that your plugin works everywhere (default)
npx @elizaos/cli publish -p universal
```

## Publishing Targets

### Publishing to npm

Make your component available on the npm registry:

```bash
npx @elizaos/cli publish -n
```

Before publishing to npm, make sure you're logged in:

```bash
npm login
```

### Publishing to GitHub

By default, packages are published to GitHub:

```bash
npx @elizaos/cli publish
```

This requires GitHub credentials, which you'll be prompted for if not already configured.

## Testing Before Publishing

### Test Mode

Run tests without actually publishing:

```bash
npx @elizaos/cli publish -t
```

This will:

1. Validate the package structure
2. Check your GitHub credentials
3. Test the build process
4. Not actually publish anything

### Dry Run

Generate registry files locally without publishing:

```bash
npx @elizaos/cli publish --dry-run
```

This creates the registry metadata files locally for inspection.

## Registry Integration

By default, your package will be submitted to the ElizaOS registry when publishing. This makes it discoverable by other ElizaOS users.

If you don't want to publish to the registry:

```bash
npx @elizaos/cli publish --skip-registry
```

## Examples

### Publishing a Plugin to GitHub

```bash
# Navigate to plugin directory
cd my-plugin

# Publish to GitHub
npx @elizaos/cli publish
```

### Publishing to npm

```bash
# Navigate to your package directory
cd my-package

# Publish to npm
npx @elizaos/cli publish -n
```

### Testing the Publishing Process

```bash
# Test the publishing process without making changes
npx @elizaos/cli publish -t
```

### Publishing with Platform Specification

```bash
# Publish a Node.js plugin
npx @elizaos/cli publish -p node
```

## Troubleshooting

### Authentication Issues

If you encounter authentication errors:

```bash
# For npm publishing
npm login

# For GitHub publishing, the CLI will guide you through setting up credentials
```

### Package Validation Failures

If your package fails validation:

1. Ensure your `package.json` contains name, version, and description
2. For plugins, ensure the name includes `plugin-`
3. Make sure your package has a proper entry point

## Related Commands

- [`plugin`](./plugins.md): Manage plugin publishing
- [`project`](./projects.md): Manage projects

# Start Command

The `start` command launches an ElizaOS project or agent in production mode. It initializes the agent runtime, loads plugins, connects to services, and starts handling interactions.

## Usage

```bash
npx @elizaos/cli start [options]
```

## Options

| Option               | Description                                                          |
| -------------------- | -------------------------------------------------------------------- |
| `-p, --port <port>`  | Port to listen on (default: 3000)                                    |
| `-c, --configure`    | Reconfigure services and AI models (skips using saved configuration) |
| `--character <path>` | Path or URL to character file to use instead of default              |
| `--build`            | Build the project before starting                                    |

## Starting a Project

When you run the `start` command, ElizaOS detects what's in your current directory:

1. If a project is detected, it loads all agents and plugins defined in the project
2. If a plugin is detected, it loads the default character with your plugin for testing
3. If neither is detected, it loads the default ElizaOS character

ElizaOS will:

1. Load and validate the configuration
2. Initialize the database system
3. Load required plugins
4. Start any configured services
5. Process knowledge files if present
6. Start the HTTP API server
7. Initialize agent runtimes
8. Begin listening for messages and events

```bash
# Navigate to your project directory
cd my-agent-project

# Start the project
npx @elizaos/cli start
```

## Environment Variables

The `start` command will look for an `.env` file in the project directory and load environment variables from it. You can also set environment variables directly:

```bash
# Set environment variables directly
OPENAI_API_KEY=your-api-key npx @elizaos/cli start
```

## Project Detection

ElizaOS automatically detects projects in the current directory by looking for:

1. A `package.json` with an `eliza.type` field set to `project`
2. A main entry point that exports a project configuration with agents
3. Other project indicators in the package metadata

## Plugin Testing

If you're developing a plugin, you can test it by running the `start` command in the plugin directory:

```bash
# In your plugin directory
cd my-plugin
npx @elizaos/cli start
```

This will load the default ElizaOS character with your plugin enabled for testing.

## Building Before Starting

To build your project before starting it:

```bash
npx @elizaos/cli start --build
```

This will compile your TypeScript files and prepare the project for execution.

## Examples

### Basic startup

```bash
cd my-agent-project
npx @elizaos/cli start
```

### Starting with configuration

```bash
npx @elizaos/cli start --configure
```

### Starting with a custom port

```bash
npx @elizaos/cli start --port 8080
```

### Starting with a custom character

```bash
npx @elizaos/cli start --character path/to/character.json
```

## Related Commands

- [`dev`](./dev.md): Run in development mode with hot reloading
- [`env`](./env.md): Configure environment variables
- [`plugin`](./plugins.md): Manage plugins
- [`project`](./projects.md): Manage projects

# Test Command

The `test` command allows you to run tests for your ElizaOS projects, plugins, and agents. It helps ensure your implementations work correctly before deployment.

## Usage

```bash
npx @elizaos/cli test [options]
```

## Options

| Option             | Description                                            |
| ------------------ | ------------------------------------------------------ |
| `--project`, `-p`  | Path to project directory (default: current directory) |
| `--file`, `-f`     | Specific test file to run                              |
| `--suite`          | Specific test suite to run                             |
| `--test`, `-t`     | Specific test to run                                   |
| `--watch`, `-w`    | Watch files and rerun tests on changes                 |
| `--verbose`        | Show detailed test output                              |
| `--json`           | Output results in JSON format                          |
| `--timeout`        | Timeout in milliseconds for each test (default: 5000)  |
| `--fail-fast`      | Stop after first test failure                          |
| `--no-compilation` | Skip TypeScript compilation                            |
| `--config`, `-c`   | Path to test configuration file                        |

## Test Structure

ElizaOS tests are organized in three levels:

1. **Test Files**: Physical files containing test suites
2. **Test Suites**: Groups of related tests with a unique name
3. **Tests**: Individual test cases that verify specific functionality

Tests are defined in plugins or projects using a structured format:

```typescript
// Example test structure from a plugin
const tests = [
  {
    name: 'plugin_test_suite',
    tests: [
      {
        name: 'example_test',
        fn: async (runtime) => {
          // Test implementation
          if (runtime.character.name !== 'Eliza') {
            throw new Error('Expected character name to be "Eliza"');
          }
        },
      },
      {
        name: 'should_have_action',
        fn: async (runtime) => {
          // Another test
          const actionExists = plugin.actions.some((a) => a.name === 'EXAMPLE_ACTION');
          if (!actionExists) {
            throw new Error('Example action not found in plugin');
          }
        },
      },
    ],
  },
];
```

## Running Tests

### Basic Test Execution

Run all tests in the current project:

```bash
# Navigate to your project
cd my-agent-project

# Run all tests
npx @elizaos/cli test
```

### Running Specific Tests

Target specific test suites or individual tests:

```bash
# Run a specific test suite
npx @elizaos/cli test --suite plugin_test_suite

# Run a specific test
npx @elizaos/cli test --suite plugin_test_suite --test example_test

# Run tests from a specific file
npx @elizaos/cli test --file src/tests/agent.test.ts
```

### Watch Mode

Automatically rerun tests when files change:

```bash
npx @elizaos/cli test --watch
```

## Test Output

The test command produces output showing test results:

```
PASS  Test Suite: plugin_test_suite (2 tests)
  ✓ example_test (15ms)
  ✓ should_have_action (3ms)

FAIL  Test Suite: agent_test_suite (3 tests)
  ✓ agent_initialization (20ms)
  ✓ message_processing (45ms)
  ✗ knowledge_retrieval (30ms)
    Error: Expected 3 knowledge items but got 2

Test Suites: 1 failed, 1 passed, 2 total
Tests:       1 failed, 4 passed, 5 total
Time:        1.5s
```

## Writing Tests

### Project Tests

Project tests typically verify agent behavior, knowledge retrieval, and integration with plugins:

```typescript
// Example project test
export default {
  name: 'agent_behavior_tests',
  tests: [
    {
      name: 'responds_to_greeting',
      fn: async (runtime) => {
        const agent = runtime.getAgent('assistant');
        const response = await agent.processMessage({
          content: { text: 'Hello' },
          userId: 'test-user',
        });

        if (!response.content.text.includes('hello') && !response.content.text.includes('Hi')) {
          throw new Error('Agent did not respond to greeting properly');
        }
      },
    },
  ],
};
```

### Plugin Tests

Plugin tests verify the functionality of actions, services, and providers:

```typescript
// Example plugin test
export const testSuite = {
  name: 'discord_plugin_tests',
  tests: [
    {
      name: 'registers_discord_service',
      fn: async (runtime) => {
        const service = runtime.getService('discord');
        if (!service) {
          throw new Error('Discord service not registered');
        }
      },
    },
    {
      name: 'handles_discord_messages',
      fn: async (runtime) => {
        // Test implementation
      },
    },
  ],
};
```

## Test Hooks

ElizaOS tests support hooks for setup and teardown:

```typescript
export default {
  name: 'database_tests',
  beforeAll: async (runtime) => {
    // Setup test database
    await runtime.db.migrate();
  },
  afterAll: async (runtime) => {
    // Clean up test database
    await runtime.db.clean();
  },
  beforeEach: async (runtime, test) => {
    // Setup before each test
    console.log(`Running test: ${test.name}`);
  },
  afterEach: async (runtime, test) => {
    // Cleanup after each test
  },
  tests: [
    // Test cases
  ],
};
```

## Test Assertions

Tests should make assertions to verify behavior:

```typescript
test('check_knowledge_retrieval', async (runtime) => {
  const query = 'What is our refund policy?';
  const results = await runtime.knowledge.search(query);

  // Check count
  if (results.length === 0) {
    throw new Error('No knowledge results found');
  }

  // Check relevance
  if (!results[0].text.includes('refund') && !results[0].text.includes('return')) {
    throw new Error('Knowledge results not relevant to query');
  }
});
```

## Examples

### Testing a Complete Project

```bash
# Run all tests
npx @elizaos/cli test

# Run with detailed output
npx @elizaos/cli test --verbose
```

### Testing During Development

```bash
# Watch for changes and automatically rerun tests
npx @elizaos/cli test --watch

# Focus on a specific test while debugging
npx @elizaos/cli test --suite agent_suite --test message_handling --watch
```

### CI/CD Integration

```bash
# Run tests in CI environment
npx @elizaos/cli test --json > test-results.json
```

## Troubleshooting

### Tests not found

If tests aren't being discovered:

```bash
# Check test discovery with verbose logging
npx @elizaos/cli test --verbose

# Try specifying the test file directly
npx @elizaos/cli test --file src/tests/main.test.ts
```

### Tests timing out

For long-running tests:

```bash
# Increase test timeout
npx @elizaos/cli test --timeout 10000
```

### TypeScript errors

If TypeScript compilation is failing:

```bash
# Build the project first
npx @elizaos/cli project build

# Then run tests without recompilation
npx @elizaos/cli test --no-compilation
```

## Related Commands

- [`dev`](./dev.md): Run your project in development mode
- [`start`](./start.md): Start your project in production mode
- [`project`](./projects.md): Manage project configuration

# Update Command

The `update` command checks for and updates ElizaOS dependencies in your project or plugin to the latest compatible versions. This helps you keep your ElizaOS installation current with the latest features, improvements, and bug fixes.

## Usage

```bash
npx @elizaos/cli update [options]
```

## Options

| Option         | Description                                       |
| -------------- | ------------------------------------------------- |
| `--check`      | Check for available updates without applying them |
| `--skip-build` | Skip building the project after updating          |

## Update Process

When you run the `update` command, ElizaOS will:

1. Detect whether you're in a project or plugin directory
2. Find all ElizaOS packages in your dependencies
3. Update them to match the current CLI version
4. Install the updated dependencies
5. Build the project with the new dependencies (unless `--skip-build` is specified)

## Project Type Detection

The update command automatically detects whether you're working with:

1. **Project**: A complete ElizaOS project with agents
2. **Plugin**: An ElizaOS plugin that provides extensions

It determines this by checking the package.json metadata, particularly:

- If the package name starts with `@elizaos/plugin-`
- If keywords include `elizaos-plugin`

## Workspace References

The command properly handles workspace references in monorepo setups:

- Packages with versions like `workspace:*` are identified as workspace references
- These are skipped during the update process as they're meant to be managed by the monorepo

## Examples

### Check For Updates

Check what updates are available without applying them:

```bash
npx @elizaos/cli update --check
```

Example output:

```
Checking for available updates...
Current CLI version: 1.3.5
To apply updates, run this command without the --check flag
```

### Update Dependencies

Update all ElizaOS dependencies to match the CLI version:

```bash
npx @elizaos/cli update
```

Example output:

```
Detected project directory
Current CLI version: 1.3.5
Found 5 ElizaOS packages: @elizaos/core, @elizaos/runtime, @elizaos/knowledge, @elizaos/models, @elizaos/plugin-discord
Updating @elizaos/core to version 1.3.5...
Updating @elizaos/runtime to version 1.3.5...
Updating @elizaos/knowledge to version 1.3.5...
Updating @elizaos/models to version 1.3.5...
Updating @elizaos/plugin-discord to version 1.3.5...
Dependencies updated successfully
Installing updated dependencies...
Project successfully updated to the latest ElizaOS packages
```

### Skip Building

Update dependencies but skip the build step:

```bash
npx @elizaos/cli update --skip-build
```

## Version Management

The update command will automatically handle different types of updates:

- For minor or patch updates, it will proceed automatically
- For major version updates that might include breaking changes, it will ask for confirmation

## Best Practices

Here are some recommended practices when updating ElizaOS dependencies:

1. **Check First**: Use `--check` to see what updates are available before applying them
2. **Backup Your Project**: Always make a backup of your project before updating
3. **Test After Updating**: Make sure your project works correctly after updating
4. **Review Changelogs**: Check the ElizaOS changelog for any breaking changes in new versions

## Troubleshooting

### Dependency Resolution Problems

If you encounter issues with dependency resolution:

```bash
# Run the command with full Node.js error stack traces
NODE_OPTIONS=--stack-trace-limit=100 npx @elizaos/cli update
```

### Build Failures

If the build fails after updating:

```bash
# Skip the automatic build and build manually with more verbose output
npx @elizaos/cli update --skip-build
npx @elizaos/cli build --verbose
```

### Version Mismatches

If you need a specific version rather than the latest:

```bash
# Manually install the specific version needed
npm install @elizaos/core@1.2.3
```

## Related Commands

- [`build`](./projects.md): Build your project manually
- [`start`](./start.md): Start your project with updated dependencies
- [`dev`](./dev.md): Run your project in development mode