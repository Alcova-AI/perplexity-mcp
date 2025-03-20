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

## License

MIT