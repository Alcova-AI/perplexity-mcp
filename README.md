# Perplexity MCP Server

A Model Context Protocol (MCP) server for the Perplexity API written in Go.

## Description

This MCP server provides integration with the Perplexity API, allowing AI assistants like Claude to interact with Perplexity's large language models via the Sonar API. It implements a tool called `perplexity_ask` that accepts conversation messages and returns a chat completion from Perplexity's model.

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

Before running the server, set your Perplexity API key as an environment variable:

```sh
export PERPLEXITY_API_KEY=your-api-key-here
```

Run the server:

```sh
perplexity-mcp
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

To use this server with MCP clients:

1. Start the server using the instructions above
2. Configure your MCP client to use this server
3. The client can then call the following tools:
   - `perplexity_ask` for search-based queries using Sonar Pro
   - `perplexity_reason` for complex reasoning tasks using Sonar Reasoning Pro

## Usage Examples

### With Claude Code (CLI)

Claude Code can directly use the Perplexity MCP tools when properly configured:

```bash
# First, start the MCP server
export PERPLEXITY_API_KEY=your-api-key
perplexity-mcp

# In another terminal, use Claude Code with MCP support
claude "Write a summary of the latest developments in quantum computing" \
  --mcp-server perplexity:perplexity-mcp
```

When starting Claude Code, you can specify the MCP server with:

```bash
claude --mcp-server perplexity:perplexity-mcp
```

### With Claude Desktop

In Claude Desktop, first enable Perplexity in the Models & Tools section of the settings. Once enabled, you can ask Claude to use the tools:

"Use Perplexity to search for information about recent breakthroughs in fusion energy."

"Use the Perplexity reasoning tool to analyze the environmental impact of different energy sources."

### With Cursor

In Cursor, you can use the Perplexity MCP tools within your development workflow:

1. Connect Cursor to the Perplexity MCP server in the settings
2. Ask Cursor to use Perplexity for research or reasoning tasks:

"Use Perplexity to find information about best practices for Go error handling."

"Use Perplexity reasoning to analyze the tradeoffs between different database choices for a high-throughput API."

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