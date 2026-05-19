# Claude Code in Action — Quiz Guide

_All quiz questions with answer choices (correct answer marked with ✓)._

---


## SDK and Wrap Up


### Quiz on Claude Code


**1. What is the fundamental limitation of language models that necessitates the use of a tool system in coding assistants?**

- A) They can only generate code in specific programming languages
- B) They have limited memory capacity for large codebases
- C) They cannot understand complex programming concepts
- D) They can only process text input/output and cannot directly interact with external systems ✓


**2. What permission configuration is required when integrating MCP servers with Claude Code in GitHub Actions?**

- A) Each MCP server tool must be individually listed in the permissions ✓
- B) No special permissions are needed if running in GitHub Actions
- C) Permissions are automatically inherited from the GitHub repository settings
- D) A single blanket permission for all MCP operations


**3. What is the primary difference between Plan Mode and Thinking Mode in Claude Code?**

- A) Plan Mode is for writing code while Thinking Mode is for debugging
- B) Plan Mode is faster while Thinking Mode is more accurate
- C) Plan Mode uses fewer tokens while Thinking Mode uses more tokens
- D) Plan Mode handles breadth (multi-step tasks) while Thinking Mode handles depth (complex logic) ✓


**4. Which of the following correctly describes the three types of Claude.md files and their usage?**

- A) Project level (debugging), Local level (testing), Machine level (production)
- B) Project level (personal use), Local level (team sharing), Machine level (repository specific)
- C) Project level (shared with team, committed), Local level (personal, not committed), Machine level (global for all projects) ✓
- D) Project level (configuration), Local level (documentation), Machine level (automation)


**5. How do you create a custom command in Claude Code that accepts runtime parameters?**

- A) Use the @parameters decorator in the command file
- B) Define parameters in settings.json configuration
- C) Add command line flags when executing the command
- D) Include $ARGUMENTS placeholder in the markdown command file ✓


**6. Which type of hook can prevent a tool call from happening if certain conditions are met?**

- A) PostToolUse hook
- B) Project hook
- C) Global hook
- D) PreToolUse hook ✓


**7. A developer wants to prevent Claude from reading sensitive .env files. Which type of hook should they set up, and what tool names would they likely match?**

- A) PostToolUse hook, matching Write and Edit
- B) PreToolUse hook, matching Write and Create
- C) PreToolUse hook, matching Read and Grep ✓
- D) PostToolUse hook, matching Read and Delete


**8. What is the primary purpose of hooks in Claude Code?**

- A) To manage project dependencies.
- B) To automatically generate new code snippets.
- C) To run commands before or after Claude executes a tool. ✓
- D) To provide a user interface for Claude.
