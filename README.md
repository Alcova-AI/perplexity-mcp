# Perplexity MCP Server

A Model Context Protocol (MCP) server for the Perplexity API written in Go. This server enables AI assistants like Claude (Code and Desktop) and Cursor to seamlessly access Perplexity's powerful search and reasoning capabilities directly from their interfaces.

## Description

The Perplexity MCP Server acts as a bridge between AI assistants and the Perplexity API, allowing them to:

1. **Search the web and retrieve up-to-date information** using Perplexity's Sonar Pro model via the `perplexity_ask` tool
2. **Perform complex reasoning tasks** using Perplexity's Sonar Reasoning Pro model via the `perplexity_reason` tool

This integration lets AI assistants like Claude access real-time information and specialized reasoning capabilities without leaving their interface, creating a seamless experience for users.

### Key Benefits

- **Access to real-time information**: Get current data, news, and information from the web
- **Enhanced reasoning capabilities**: Leverage specialized models for complex problem-solving tasks
- **Seamless integration**: Works natively with Claude Code, Claude Desktop, and Cursor
- **Simple installation**: Quick setup with Homebrew, Go, or pre-built binaries
- **Customizable**: Configure which Perplexity models to use for different tasks

## Installation

### Using Homebrew (macOS and Linux)

```sh
brew tap alcova-ai/tap
brew install perplexity-mcp
```

### Using Go

```sh
go install github.com/Alcova-AI/perplexity-mcp@latest
```

### From Source

Clone the repository and build manually:

```sh
git clone https://github.com/Alcova-AI/perplexity-mcp.git
cd perplexity-mcp
go build
```

### From Binary Releases

Download pre-built binaries from the [releases page](https://github.com/Alcova-AI/perplexity-mcp/releases).

## Usage

### Direct Execution

If you want to run the server directly (not recommended for most users):

1. Set your Perplexity API key as an environment variable:
   ```sh
   export PERPLEXITY_API_KEY=your-api-key-here
   ```

2. Run the server:
   ```sh
   perplexity-mcp
   ```

### Recommended: Use as MCP Server

The recommended way to use this tool is through the Claude MCP server system:

```sh
# Add the MCP server
claude mcp add perplexity-mcp perplexity-mcp

# Set your API key (only needed once)
export PERPLEXITY_API_KEY=your-api-key-here

# Use with Claude Code
claude --mcp-server perplexity-mcp
```

### Command Line Options

- `--model, -m`: Specify the Perplexity model to use for search (default: "sonar-pro")
  - Can also be set with the `PERPLEXITY_MODEL` environment variable
- `--reasoning-model, -r`: Specify the Perplexity model to use for reasoning (default: "sonar-reasoning-pro")
  - Can also be set with the `PERPLEXITY_REASONING_MODEL` environment variable

Example:

```sh
perplexity-mcp --model sonar-pro --reasoning-model sonar-reasoning-pro
```

## Tool Definitions

The server provides the following tools:

### perplexity_ask

Engages in a conversation using the Sonar API for search. Accepts an array of messages (each with a role and content) and returns a chat completion response from the Perplexity model.

**Input Schema:**

```json
{
  "messages": [
    {
      "role": "string",    // Role of the message (e.g., system, user, assistant)
      "content": "string"  // The content of the message
    }
  ]
}
```

### perplexity_reason

Uses the Perplexity reasoning model to perform complex reasoning tasks. Accepts a query string and returns a comprehensive reasoned response.

**Input Schema:**

```json
{
  "query": "string"  // The query or problem to reason about
}
```

## MCP Integration

This server implements the Model Context Protocol (MCP) using standard input/output (stdin/stdout) streams rather than HTTP, allowing for efficient integration with MCP clients.

To use this server with MCP clients:

1. Add the MCP server using `claude mcp add perplexity-mcp perplexity-mcp`
2. Set your Perplexity API key as an environment variable
3. Configure your MCP client to use this app
4. The client can then call the following tools:
   - `perplexity_ask` for search-based queries using Sonar Pro
   - `perplexity_reason` for complex reasoning tasks using Sonar Reasoning Pro

The server uses the stdin/stdout protocol for MCP communication rather than HTTP, which makes it compatible with the Claude Code MCP app system.

## Usage Examples

### With Claude Code (CLI)

Claude Code can directly use the Perplexity MCP tools when properly configured:

#### Setup

1. Add the Perplexity MCP server to Claude Code:
   ```bash
   # Using the interactive wizard
   claude mcp add perplexity-mcp perplexity-mcp
   
   # Or using add-json
   claude mcp add-json perplexity-mcp '{"type":"stdio","command":"perplexity-mcp"}'
   ```

2. Set your Perplexity API key (only needed once):
   ```bash
   export PERPLEXITY_API_KEY=your-api-key
   ```

3. Use Claude Code with the Perplexity MCP server:
   ```bash
   claude --mcp-server perplexity-mcp
   ```

   This can be added to your `~/.clauderc` configuration file to make it permanent:
   ```
   mcp-server = ["perplexity-mcp"]
   ```

#### Example Usage

```bash
# Direct query using Perplexity search
claude "Use Perplexity to search for the latest developments in quantum computing"

# Complex reasoning task
claude "Use Perplexity reasoning to analyze the potential impact of AI on healthcare over the next decade"
```

You can also specify the MCP server for a single command:
```bash
claude "What are the latest machine learning frameworks?" --mcp-server perplexity-mcp
```

### With Claude Desktop

#### Setup

1. Add the Perplexity MCP server via the Claude Desktop interface:
   - In Claude Desktop, go to Settings (gear icon)
   - Select the "Models & Tools" section
   - Navigate to Tools > Local Tools
   - Click "Add Local Tool" and select Perplexity
   - Configure with your API key when prompted

   Alternatively, you can manually set up the MCP server and set your API key:
   ```bash
   # Set your API key
   export PERPLEXITY_API_KEY=your-api-key
   
   # Add the MCP server to Claude Desktop
   # This varies by platform - you may need to use Claude Desktop's interface
   # or import using:
   claude mcp add-from-claude-desktop
   ```

#### Example Usage

Once configured, you can ask Claude to use Perplexity with prompts like:

- "Use Perplexity to search for information about recent breakthroughs in fusion energy."
- "Use Perplexity to find the latest research on quantum computing applications."
- "Use the Perplexity reasoning tool to analyze the environmental impact of different energy sources."
- "Ask Perplexity to reason through the potential economic effects of central bank digital currencies."

### With Cursor

#### Setup

1. Set up the Perplexity MCP server:
   ```bash
   # Add the MCP server (if not already added in Claude Code)
   claude mcp add perplexity-mcp perplexity-mcp
   
   # Set your API key
   export PERPLEXITY_API_KEY=your-api-key
   ```

2. In Cursor:
   - Open Settings (⌘+,) 
   - Navigate to the "AI" section
   - Find "MCP Tools Configuration"
   - Enable the Perplexity MCP tool
   - Click "Save" or "Apply"

#### Example Usage

Once configured, you can use Perplexity within Cursor's chat interface:

- "Use Perplexity to find information about best practices for Go error handling."
- "Use Perplexity to search for examples of implementing GraphQL authentication."
- "Use Perplexity reasoning to analyze the tradeoffs between different database choices for a high-throughput API."
- "Ask Perplexity to reason through the pros and cons of microservices versus monolithic architecture for my project."

Cursor will seamlessly call the Perplexity API through the MCP server, allowing you to access up-to-date information and reasoning capabilities directly in your development environment.

### JSON Format for Tool Calls

Here are the JSON formats for calling the tools directly via MCP:

#### perplexity_ask

```json
{
  "method": "tools/call",
  "params": {
    "name": "perplexity_ask",
    "arguments": {
      "messages": [
        {
          "role": "user",
          "content": "What is the capital of France?"
        }
      ]
    }
  }
}
```

#### perplexity_reason

```json
{
  "method": "tools/call",
  "params": {
    "name": "perplexity_reason",
    "arguments": {
      "query": "Analyze the pros and cons of microservice architecture versus monolithic applications."
    }
  }
}
```

## License

MIT
