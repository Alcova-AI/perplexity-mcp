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

This server supports only the `stdio` protocol for MCP communication.

### Recommended: Use as MCP command with Claude Code

Adding to Claude Code:

```sh
claude mcp add-json perplexity-mcp '{"type":"stdio","command":"perplexity-mcp","env":{"PERPLEXITY_API_KEY":"pplx-YOUR-API-KEY-HERE"}}'
```

That's it! You can now use Perplexity in Claude Code.

### Recommended: Use as MCP command with Claude Desktop

Adding to Claude Desktop:

1. Exit the Claude Desktop MCP config:

```sh
code ~/Library/Application\ Support/Claude/claude_desktop_config.json
```

2. Add the Perplexity MCP server:

```diff
  {
    "mcpServers": {
+        "perplexity-mcp": {
+            "command": "perplexity-mcp",
+            "args": [
+                "--model",
+                "sonar-pro",
+                "--reasoning-model",
+                "sonar-reasoning-pro"
+            ],
+            "env": {
+                "PERPLEXITY_API_KEY": "pplx-YOUR-API-KEY-HERE"
+            }
+        }
    }
  }
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



## License

MIT
