
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
