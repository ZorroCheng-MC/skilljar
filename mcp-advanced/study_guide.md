# Model Context Protocol: Advanced Topics — Study Guide

_Full course notes compiled from all lessons._

---


## Advanced Patterns


### Let's get started!

_Video lesson — no text notes._


### Sampling

Sampling allows an MCP server to access a language model like Claude **through** the connected MCP client, rather than calling the API directly.

**The problem it solves**

When a tool needs to generate text (e.g. summarising Wikipedia results), the server has two options:

- **Option 1 — Direct LLM call:** The server calls Claude directly. This works, but requires the server to hold an API key, adds complexity, and means a public server pays for any user's token generation.
- **Option 2 — Sampling:** The server asks the client to run a prompt on its behalf. The client already has a connection to Claude, so it simply makes one additional request and returns the result. No API key on the server, no token-cost exposure.

**How it works**

1. Inside a tool function, the server calls `create_message()` and passes a list of messages (the prompt).
2. The MCP framework wraps this into a `CreateMessageRequest` and sends it to the client.
3. The client's **sampling callback** receives the messages, calls Claude, and returns a `CreateMessageResult`.
4. The server receives the generated text and continues.

**When to use it**

Sampling is most useful for publicly accessible MCP servers where you do not want to expose an API key or accept token costs on behalf of arbitrary users.


### Sampling walkthrough

_Video lesson — no text notes._


### Log and progress notifications

Logging and progress notifications give users visibility into what a long-running tool is doing, preventing the appearance of a stalled or broken application.

**Server side**

Inside any tool function, the last argument is a `context` object that provides:

- `context.info("message")` — emits a log statement to the client
- `context.report_progress(current, total)` — emits a progress update

Calling these functions automatically sends the corresponding notification message to the connected client.

**Client side**

Write two callback functions and register them:

- A **logging callback** — receives log notification objects; display them however suits your UI (print to terminal for CLI, push to browser for web apps)
- A **progress callback** — receives progress notifications; passed to the `call_tool` invocation

Both callbacks are optional. You do not have to display them; they are purely UX enhancements.


### Notifications walkthrough

_Video lesson — no text notes._


### Roots

Roots let users grant an MCP server access to a **specific set of files and folders**, providing both permission control and a way for Claude to know where to look.

**The problem without roots**

If a user asks Claude to "convert biking.mp4", Claude has no way to locate that file on a complex filesystem. Requiring fully-qualified paths is inconvenient.

**How roots work**

1. When launching the server, the user passes allowed directories as command-line arguments (e.g. `uv run server.py ~/Desktop`). These become the server's roots.
2. The server exposes a `list_roots` tool (or injects roots into a prompt) so Claude can discover what it's allowed to access.
3. Every tool that accesses files implements an `is_path_allowed(path)` check: fetch the root list from the client and verify the requested path falls within one of them. Deny access otherwise.

**Important:** No Anthropic SDK automatically enforces roots. The enforcement logic must be written into your own tools.

**Two benefits of roots**

- **Permission boundary** — the server cannot read files the user didn't grant access to
- **Focused search** — Claude doesn't need fully-qualified paths; it can call `list_roots`, read that directory, and discover the target file autonomously


### Roots walkthrough

_Video lesson — no text notes._


### Survey

_Video lesson — no text notes._


---


## Transport Layer


### JSON message types

Clients and servers communicate by exchanging JSON objects called **messages**. There are two categories:

| Category | Description | Example |
|----------|-------------|---------|
| **Request-result pairs** | Always come in pairs; one side sends a request, the other replies with a result | `call_tool_request` → `call_tool_result` |
| **Notifications** | One-way events; no response expected | `progress_notification`, `logging_message_notification` |

**Client messages vs server messages**

The MCP spec (defined in `schema.ts` in the MCP spec repo) explicitly separates messages into client-originated and server-originated:

- *Client requests* — e.g. `call_tool_request`, `initialize_request`
- *Server requests* — e.g. `create_message_request` (used for sampling), `list_roots_request`
- *Server notifications* — e.g. progress, logging

**Key takeaway:** Some messages flow from the **server to the client**. This becomes critical when choosing a transport, because HTTP makes server-initiated messages difficult.


### The STDIO transport

The STDIO transport runs client and server as separate processes on the **same machine**, communicating via stdin/stdout.

- Client → server: write to server's **stdin**
- Server → client: write to server's **stdout**

**Initialization sequence** (required by the MCP spec before any other messages):

1. Client sends `initialize_request`
2. Server replies with `initialize_result`
3. Client sends `initialized_notification` (no response expected)

After this, the connection is live and any message type can flow in either direction.

**Why STDIO is simple**

All four communication scenarios work trivially:

| Scenario | Mechanism |
|----------|-----------|
| Client initiates request to server | Write to stdin |
| Server responds to client | Write to stdout |
| Server initiates request to client | Write to stdout |
| Client responds to server | Write to stdin |

Either side can initiate at any time. **This bidirectional flexibility does not carry over to the HTTP transport.**

**Limitation:** Client and server must run on the same physical machine.


### The StreamableHTTP transport

The StreamableHTTP transport sends MCP messages over HTTP, enabling **remotely hosted servers** that any client can connect to.

**The core problem**

Standard HTTP is request-response: the client initiates all requests. A server cannot push a message to a client unprompted. This breaks:

- **Sampling** — requires a server-initiated `CreateMessageRequest`
- **Progress notifications** — server-pushed
- **Logging notifications** — server-pushed

**Two configuration flags** (both default to `false`):

| Flag | Effect when `true` |
|------|-------------------|
| `stateless_http` | No session ID issued; GET SSE pathway disabled; sampling/progress/logging unavailable; initialization steps skipped |
| `json_response` | POST responses return final JSON only; no streaming; no intermediate log/progress messages |

Setting either flag to `true` degrades functionality. If features work locally with STDIO but break after switching to HTTP, these flags are the first place to look.


### StreamableHTTP in depth

The transport solves the server→client problem using **SSE (Server-Sent Events)** — a long-lived HTTP response the server can write to at any time.

**Full connection flow**

1. **Initialize** — Client sends `initialize_request`; server replies with `initialize_result` containing the `Mcp-Session-Id` header. All subsequent requests must include this session ID.
2. **Initialized notification** — Client sends `initialized_notification` to complete the handshake.
3. **GET SSE (persistent channel)** — Client optionally sends a GET request; server holds the response open as an SSE stream. The server uses this channel to send server-initiated messages (sampling requests, some notifications).
4. **Tool calls** — Client sends a POST request with a `call_tool_request`. The server opens a **second** SSE response for this specific request, sends related messages, then closes it automatically when the `call_tool_result` is sent.

**Where each message type is delivered**

| Message | Channel |
|---------|---------|
| Progress notifications | Persistent GET SSE response |
| Log messages | POST SSE response (tied to the tool call) |
| `call_tool_result` | POST SSE response |
| Sampling (`create_message_request`) | Persistent GET SSE response |

**Session ID**

- Assigned per client on the first `initialize_result`
- Included in every follow-up request as a header
- Allows the server to route server-initiated messages to the correct client
- Not used with STDIO transport


### State and the StreamableHTTP transport

**Why set `stateless_http = true`?**

When horizontally scaling with a load balancer, incoming requests may be routed to different server instances. The persistent GET SSE connection (needed for sampling) may live on instance A, while a POST tool call lands on instance B. Coordinating sampling across instances is complex.

Setting `stateless_http = true` sidesteps this by eliminating the session concept entirely:

- No session ID issued
- No GET SSE pathway → no sampling, no server-initiated progress/logging
- No initialization handshake required (saves 2 round trips)
- Each request is fully independent — safe to route to any instance

**Why set `json_response = true`?**

Replaces streaming SSE responses with a single plain-JSON response. The client waits for the tool to finish and receives only the final result — no intermediate log or progress messages.

**Trade-off summary**

| Flag | Gains | Loses |
|------|-------|-------|
| `stateless_http = true` | Horizontal scalability, no init overhead | Sampling, progress, logging, subscriptions |
| `json_response = true` | Simple non-streaming clients | Intermediate messages, real-time feedback |

**Development advice**

Develop with the same transport you plan to use in production. Switching from STDIO to HTTP late in development commonly surfaces broken sampling or notification features that worked fine locally.


### Wrapping up

_Video lesson — no text notes._
