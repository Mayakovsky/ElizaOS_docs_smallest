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
