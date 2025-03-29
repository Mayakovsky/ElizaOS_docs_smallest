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
