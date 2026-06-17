# MCP Architecture — My Notes

## The core problem it solves

Every AI integration today is custom-built. Want Claude to read your Notion? Custom code.
Want it to query your DB? More custom code. Want both? Double the maintenance.

MCP says: build one server per data source, following one standard. Any AI client that speaks MCP can use any MCP server. Done.

---

## The three primitives

### 1. Resources
Read-only data the model can access. Like GET endpoints — here's the data, model can read it.

Examples: files, database rows, API responses, config files

### 2. Tools
Functions the model can *call* and get results back.

Examples: send_email(), create_github_issue(), run_sql_query()

This is where it gets powerful — the model can actually *do* things, not just read.

### 3. Prompts
Pre-written prompt templates that users can invoke through the client.

Examples: "summarize this document", "review this PR", "explain this error"

---

## Transport layer

MCP works over two transports:
- **stdio** — for local servers (runs as a subprocess of the client)
- **HTTP + SSE** — for remote/cloud servers

Local servers are dead simple to set up. Remote ones need auth handling.

---

## Security model

- Servers run in their own process, isolated
- Clients explicitly approve which servers to connect to
- No server can reach beyond what it's given access to
- User is always in the loop for tool calls (Claude asks before executing)

---

## Why "two-way" matters

Old RAG pipelines: model reads data → generates text. One direction.

MCP: model reads data AND calls tools AND gets results back → generates text with real actions taken. Bidirectional.

This is the shift from "AI that knows things" to "AI that does things."

---

## Client-Server handshake (simplified)

```
Client                        Server
  |                             |
  |------ initialize() -------->|
  |<----- capabilities ---------|
  |                             |
  |------ list_tools() -------->|
  |<----- [tool definitions] ---|
  |                             |
  |------ call_tool(name) ----->|
  |<----- tool result ----------|
```

The client discovers what the server can do at runtime — no hardcoding needed.
