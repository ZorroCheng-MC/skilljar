# Claude Code in Action — Study Guide

_Full course notes compiled from all lessons._

---


## Introduction


### Introduction

This video is still being processed. Please check back later and refresh the page.
Uh oh! Something went wrong, please try again.


### What is a coding assistant?

This video is still being processed. Please check back later and refresh the page.
Uh oh! Something went wrong, please try again.


### Claude Code in action

Claude Code = AI coding assistant that functions as a collaborative engineer on projects, not just a code generator.

Key capabilities: project setup, feature design, code writing, testing, deployment, error fixing in production.

Setup workflow:
- Download project, open in editor
- Run \`claude\` command to launch
- Ask Claude to read README and execute setup directions
- Run \`init\` command = Claude scans codebase for architecture/coding style, creates claude.md file
- claude.md = automatically included context for future requests

Memory types: Project (shared), Local, User memory files.

Context management:
- Use # symbol to add specific notes to memory
- Can manually edit claude.md or rerun init to update
- Claude can handle Git operations (staging, committing)

Effective prompting strategies:

Method 1 - Three-step workflow:
1. Identify relevant files, ask Claude to analyze them
2. Describe feature, ask Claude to plan solution (no code yet)
3. Ask Claude to implement the plan

Method 2 - Test-driven development:
1. Provide relevant context
2. Ask Claude to suggest tests for the feature
3. Select and implement chosen tests
4. Ask Claude to write code until tests pass

Core principle: Claude Code = effort multiplier. More detailed instructions = significantly better results. Treat as collaborative engineer, not just code generator.


### Claude Code setup

Claude Code = terminal-based coding assistant program that helps with code-related tasks

Core capabilities = search/read/edit files + advanced tools (web fetching, terminal access) + MCP client support for expanded functionality via MCP servers

Setup process:
1. Install Node.js (check with "npm help" command)
2. Run npm install to install Claude Code
3. Execute "claude" command in terminal to login to Anthropic account

Full setup guide = docs.anthropic.com

MCP client functionality = can consume tools from MCP servers to extend capabilities beyond basic file operations


### Project setup

CLI-based chatbot project = teaches MCP client-server interaction through hands-on implementation

Project components:
- MCP client = connects to custom MCP server
- MCP server = provides 2 tools (read document, update document)
- Document collection = fake documents stored in memory only

Key distinction: Normal projects implement either client OR server, not both. This project implements both for educational purposes.

Setup process:
1. Download CLI_project.zip starter code
2. Extract and open in code editor
3. Follow readme.md setup directions
4. Add API key to .env file
5. Install dependencies (with/without UV)
6. Run project: "uv run main.py" or "python main.py"
7. Test with chat prompt

Expected outcome = working chat interface that responds to basic queries, ready for MCP feature additions.


## Core Workflow


### Adding context

This video is still being processed. Please check back later and refresh the page.
Uh oh! Something went wrong, please try again.


### Making changes

This video is still being processed. Please check back later and refresh the page.
Uh oh! Something went wrong, please try again.


### Course satisfaction survey

Loading...


### Controlling context

This video is still being processed. Please check back later and refresh the page.
Uh oh! Something went wrong, please try again.


### Custom commands

This video is still being processed. Please check back later and refresh the page.
Uh oh! Something went wrong, please try again.


### MCP servers with Claude Code

This video is still being processed. Please check back later and refresh the page.
Uh oh! Something went wrong, please try again.


### Github integration

This video is still being processed. Please check back later and refresh the page.
Uh oh! Something went wrong, please try again.


## Hooks


### Introducing hooks

This video is still being processed. Please check back later and refresh the page.
Uh oh! Something went wrong, please try again.


### Defining hooks

This video is still being processed. Please check back later and refresh the page.
Uh oh! Something went wrong, please try again.


### Implementing a hook

This video is still being processed. Please check back later and refresh the page.
Uh oh! Something went wrong, please try again.


### Gotchas around hooks

You may notice that after running the
`npm run setup`
command there are two
`settings.json`
files in the
`.claude`
directory. Let me explain what's going on there.
The Claude Code documentation lists some recommendations around hooks security:
One of the recommendations is to use absolute paths (rather than relative paths) for scripts. This helps mitigate
path interception
and
binary planting
attacks.
This recommendation also makes it much more challenging to share
`settings.json`
files. The reason is simple: the absolute path to any of the hook scripts on
your
machine will likely be different from the absolute
path
on my machine, simply because we will probably place the project in separate directories.
To solve this problem, our project has a
`settings.example.json`
file. Inside of it, the script references contain a
`$PWD`
placeholder. When we run
`npm run setup`
, some dependencies are installed, but it also runs an
`init-claude.js`
script placed inside the scripts directory. This script will replace those
`$PWD`
placeholder with the absolute path to the project on your machine, copy the
`settings.example.json`
file, and rename it to
`settings.local.json`
.
This script allows us to share settings.json files but still use the recommended absolute paths!


### Useful hooks!

This video is still being processed. Please check back later and refresh the page.
Uh oh! Something went wrong, please try again.


### Another useful hook

There are more hooks beyond the
`PreToolUse`
and
`PostToolUse`
hooks discussed in this course. There are also:
`Notification`
- Runs when Claude Code sends a notification, which occurs when Claude needs permission to use a tool, or after Claude Code has been idle for 60 seconds
`Stop`
- Runs when Claude Code has finished responding
`SubagentStop`
- Runs when a subagent (these are displayed as a "Task" in the UI) has finished
`PreCompact`
- Runs before a compact operation occurs, either manual or automatic
`UserPromptSubmit`
- Runs when the user submits a prompt, before Claude processes it
`SessionStart`
- Runs when starting or resuming a session
`SessionEnd`
- Runs when a session ends
Here's the confusing part:
- The stdin input to your commands will change based upon the type of hook being executed (
`PreToolUse`
,
`PostToolUse`
,
`Notification`
, etc)
- The
`tool_input`
contained in that will differ based upon the tool that was called (in the case of
`PreToolUse`
and
`PostToolUse`
hooks)
For example, here's a sample of some stdin input to a hook, where the hook is a
`PostToolUse`
that was watching for uses of the
`TodoWrite`
tool. For reference, that is the tool that Claude uses to keep track of to-do items.
`{
  "session_id": "9ecf22fa-edf8-4332-ae85-b6d5456eda64",
  "transcript_path": "<path_to_transcript>",
  "hook_event_name": "PostToolUse",
  "tool_name": "TodoWrite",
  "tool_input": {
    "todos": [{ "content": "write a readme", "status": "pending", "id": "1" }]
  },
  "tool_response": {
    "oldTodos": [],
    "newTodos": [{ "content": "write a readme", "status": "pending", "id": "1" }]
  }
}`
And for comparison, here's an example of the input to a
`Stop`
hook:
`{
  "session_id": "af9f50b6-f042-4773-b3e2-c3a4814765ce",
  "transcript_path": "<path_to_transcript>",
  "hook_event_name": "Stop",
  "stop_hook_active": false
}`
As you can see, the stdin input to your command will differ significantly based upon the hook (
`PreToolUse`
,
`PostToolUse`
,
`Stop`
, etc)
and
the matcher used (in the case of
`PreToolUse`
and
`PostToolUse`
). This can make writing hooks challenging - you might not know the exact structure of the input to your command!
To handle this challenge, try making a helper hook like this:
`"PostToolUse": [ // Or "PreToolUse" or "Stop", etc
  {
    "matcher": "*",
    "hooks": [
      {
        "type": "command",
        "command": "jq . > post-log.json"
      }
    ]
  },
]`
Notice the provided command. It will write the input to this hook to the
`post-log.json`
file, which allows you to inspect exactly what would have been fed into your command!
This makes it a lot easier for you to understand what data your command should inspect.


## SDK and Wrap Up


### The Claude Code SDK

This video is still being processed. Please check back later and refresh the page.
Uh oh! Something went wrong, please try again.


### Summary and next steps

This video is still being processed. Please check back later and refresh the page.
Uh oh! Something went wrong, please try again.
