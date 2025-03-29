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
