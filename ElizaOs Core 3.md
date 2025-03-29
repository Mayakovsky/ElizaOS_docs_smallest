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
