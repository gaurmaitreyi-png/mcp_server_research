# MCP Quick Reference

## Install

```bash
pip install mcp                              # Python SDK
npm install @modelcontextprotocol/sdk        # TypeScript SDK
```

---

## Minimal server skeleton (Python)

```python
from mcp.server import Server
from mcp.server.stdio import stdio_server

app = Server("server-name")

@app.call_tool()
async def call_tool(name, arguments):
    if name == "get_weather":
        return f"Weather for {arguments['city']}: Sunny, 28C"

@app.list_resources()
async def list_resources():
    return [{"uri": "data://main", "name": "Main Data"}]

async def main():
    async with stdio_server() as streams:
        await app.run(*streams)
```

---

## Claude Desktop config location

| OS | Path |
|----|------|
| macOS | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Windows | `%APPDATA%\Claude\claude_desktop_config.json` |

## Config format

```json
{
  "mcpServers": {
    "your-server-name": {
      "command": "python",
      "args": ["/absolute/path/to/server.py"]
    }
  }
}
```

---

## Useful links

| Resource | Link |
|----------|------|
| Spec | [modelcontextprotocol.io](https://modelcontextprotocol.io) |
| Python SDK | [github.com/modelcontextprotocol/python-sdk](https://github.com/modelcontextprotocol/python-sdk) |
| TS SDK | [github.com/modelcontextprotocol/typescript-sdk](https://github.com/modelcontextprotocol/typescript-sdk) |
| Pre-built servers | [github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) |
| Quickstart | [modelcontextprotocol.io/quickstart](https://modelcontextprotocol.io/quickstart) |

---

## Common gotchas

- Server path in config must be **absolute**, not relative
- Restart Claude Desktop after every config change
- stdio servers only work locally — need HTTP+SSE for remote/production
- Tool names must be unique within a server
