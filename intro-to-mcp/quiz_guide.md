# Introduction to Model Context Protocol — Quiz Guide

_All quiz questions with answer choices (correct answer marked with ✓)._

---


## MCP Primitives


### Final assessment on MCP


**1. You're building an MCP client to connect your application to an MCP server. What are the two main components you need?**

- A) A frontend and a backend
- B) A database and a web server
- C) A REST API and a GraphQL endpoint
- D) An MCP Client class and a Client Session ✓


**2. Your MCP client needs to find out what tools are available from an MCP server. What message type should it send?**

- A) ListToolsRequest ✓
- B) CallToolRequest
- C) ToolDiscoveryRequest
- D) GetToolsMessage


**3. You've built an MCP server and want to test if your tools work correctly before connecting to a full application. What's the easiest way to do this?**

- A) Write separate test scripts for each tool
- B) Test everything manually in the terminal
- C) Use the built-in MCP Inspector with `mcp dev mcp_server.py` ✓
- D) Connect directly to Claude first


**4. You're building a chat app where users ask Claude about their GitHub data. Without MCP, what's the main problem you'd face?**

- A) GitHub doesn't allow API access
- B) You'd have to write and maintain all the GitHub tool code yourself ✓
- C) Claude can't understand GitHub data
- D) Users can't ask questions about repositories


**5. You're deciding how to implement a new feature in your MCP server. Users should be able to click a button to trigger a "summarize document" workflow. Which MCP primitive should you use?**

- A) Resources - because you need to fetch document data
- B) Functions - because it involves processing
- C) Prompts - because users control when to start the workflow ✓
- D) Tools - because the AI needs new capabilities


**6. You're using the Python MCP SDK to create a tool that reads files. What's the easiest way to define this tool?**

- A) Use the @mcp.tool() decorator on a Python function ✓
- B) Create a separate configuration file
- C) Write JSON schemas manually
- D) Send HTTP requests to register the tool


**7. You want to create a resource that fetches different documents based on their ID, like docs://documents/report.pdf. What type of resource should you use?**

- A) A templated resource with parameters in the URI ✓
- B) A tool instead of a resource
- C) A direct resource with a static URI
- D) A database query resource
