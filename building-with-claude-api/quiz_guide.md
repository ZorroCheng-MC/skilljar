# Building with the Claude API — Quiz Guide

_All quiz questions with answer choices (correct answer marked with ✓)._

---


## Accessing Claude with the API


### Quiz on accessing Claude with the API


**1. You want to send a request to Claude's API. What's the minimum information you must include?**

- A) Only the API key and your question
- B) Just your message text
- C) API key, model name, messages, and max tokens
- D) Your name, email, and message


**2. You ask Claude "What is pizza?" and it answers. Then you ask "What toppings are popular?" but Claude doesn't understand what you're referring to. What's the problem?**

- A) Your internet connection is slow
- B) Claude is broken
- C) You asked too quickly
- D) Claude doesn't remember previous messages


**3. When Claude processes your text, what's the first thing it does?**

- A) Generates a response immediately
- B) Checks if it's appropriate content
- C) Breaks it into smaller chunks called tokens
- D) Translates it to another language


**4. Users complain your chat app feels slow because they wait 20 seconds staring at a loading spinner, then all the generated text appears at once. What can fix this?**

- A) Asking shorter questions
- B) Using a faster internet connection
- C) Enabling response streaming
- D) Using a different web browser


**5. You're building a web app that talks to Claude. Where should you store your API key?**

- A) In your mobile app that users install
- B) In a text file on the user's computer
- C) On your server that users can't access
- D) In your JavaScript code that users download


**6. You're building a math tutor bot. You want Claude to give hints instead of direct answers. What should you use?**

- A) Setting a very low word limit
- B) Using all capital letters in your messages
- C) A system prompt explaining the tutor role
- D) Asking users to be more specific


**7. You want Claude to give very predictable, consistent answers for a factual Q&A app. What temperature setting should you use?**

- A) Temperature doesn't matter for facts
- B) Low temperature (near 0.0)
- C) Medium temperature (around 0.5)
- D) High temperature (near 1.0)


**8. You're building an app that needs clean JSON from Claude with no extra text or formatting. How do you get just the raw JSON?**

- A) Send the request multiple times and pick the best one
- B) Ask Claude very nicely to only return JSON
- C) Combine prefilled messages and stop sequences
- D) Use a very high temperature setting


## Prompt evaluation


### Quiz on prompt evaluation


**1. You wrote a prompt and tested it once. It worked fine, so you deployed it to production. What's the main risk with this approach?**

- A) Users will provide unexpected inputs that break it ✓
- B) The prompt will become too expensive
- C) The prompt will work too slowly
- D) Other developers won't understand it


**2. You need test cases for your prompt evaluation. You have two options: write them by hand or use Claude to generate them. Which model should you use for generation?**

- A) The most expensive model available
- B) Multiple models combined
- C) A faster model like Haiku ✓
- D) The same model you're testing


**3. You're running a prompt evaluation workflow. You've used Claude to generate some responses. What's the next step?**

- A) Deploy to production
- B) Rewrite the original prompt
- C) Create more test questions
- D) Feed the responses through a grader ✓


**4. You want to measure how well your prompts actually work in practice. Which approach should you focus on?**

- A) Using more examples
- B) Prompt engineering techniques
- C) Writing longer prompts
- D) Prompt evaluation methods ✓


**5. You're using a model grader to evaluate responses. To get better scores than just middle-range numbers, what should you ask for alongside the score?**

- A) Just the numerical score
- B) Comparison to other responses
- C) Strengths, weaknesses, and reasoning ✓
- D) A longer explanation


**6. Which type of grader uses another AI model to assess the quality of outputs?**

- A) Model grader ✓
- B) Human grader
- C) Syntax grader
- D) Code grader


## Prompt engineering techniques


### Quiz on prompt engineering techniques

_Quiz questions could not be extracted._


## Tool use with Claude


### Quiz on tool use with Claude


**1. How can you tell if Claude wants to make another tool call in a conversation?**

- A) Check if the response contains the word "tool"
- B) Check if the response is longer than usual
- C) Look at the stop_reason field for "tool_use" ✓
- D) Count the number of message blocks


**2. When Claude uses a tool, what type of message structure does it return?**

- A) Multi-block messages with text and tool use blocks ✓
- B) Simple text-only responses
- C) JSON data without any text
- D) Error messages only


**3. What is the main purpose of a JSON schema when working with Claude tools?**

- A) To format the final response for users
- B) To tell Claude what arguments your function expects and how to use it ✓
- C) To store the results of tool function calls
- D) To encrypt data between Claude and your server


**4. What problem does the batch tool solve?**

- A) It makes tools run faster
- B) It translates tool results into different languages
- C) It reduces the number of back-and-forth communications when multiple tools are needed ✓
- D) It automatically fixes errors in tool responses


**5. What is the correct sequence of steps in the tool use workflow?**

- A) Initial Request → Tool Request → Data Retrieval → Final Response ✓
- B) Tool Request → Initial Request → Final Response → Data Retrieval
- C) Final Response → Initial Request → Tool Request → Data Retrieval
- D) Data Retrieval → Tool Request → Initial Request → Final Response


**6. Claude can only access information from its training data by default. What allows Claude to get current, real-time information?**

- A) Making educated guesses based on patterns
- B) Searching through its training data more carefully
- C) Asking the user to provide more details
- D) Using tools to access external information ✓


**7. What makes Claude's built-in text editor and web search tools different from custom tools?**

- A) Claude provides the schema, but you may still need to implement some functionality ✓
- B) They require special API keys
- C) They only work with specific file types
- D) They cost more to use


## Features of Claude


### Quiz on features of Claude


**1. What is the Files API used for?**

- A) Scanning files for viruses and malware
- B) Compressing large files to reduce API costs
- C) Converting files between different formats automatically
- D) Uploading files ahead of time and referencing them later instead of encoding them directly in messages ✓


**2. You're making many requests with the same large system prompt. What feature would make your requests faster and cheaper?**

- A) PDF processing
- B) Citations
- C) Extended thinking
- D) Prompt caching ✓


**3. What is the primary purpose of citations in Claude?**

- A) To create a clear trail from Claude's response back to specific parts of source documents ✓
- B) To compress large documents for faster processing
- C) To count the number of words in a document
- D) To automatically generate footnotes for academic papers


**4. When Claude uses extended thinking, what two parts do you get in the response?**

- A) Reasoning process and final answer ✓
- B) Problem and solution
- C) Input and output
- D) Question and answer


**5. You want Claude to analyze a PDF document. What's the main difference from sending an image?**

- A) Change the type to "document" and media_type to "application/pdf" ✓
- B) PDFs cost more to process
- C) You can only send text, not images in PDFs
- D) PDFs require special permission


**6. What is a key limitation of Claude's Code Execution tool?**

- A) It can only run JavaScript code
- B) It has no network access and runs in an isolated Docker container ✓
- C) It requires users to provide their own execution environment
- D) It can only process text files


**7. You want to cache your system prompt. What's the minimum requirement for caching to work?**

- A) You must make at least 5 requests
- B) You must use extended thinking
- C) The content must be under 500 tokens
- D) The content must be at least 1024 tokens long ✓


## Model Context Protocol


### Quiz on Model Context Protocol


**1. You've created an MCP server and want to test your tools before connecting them to Claude. What's the best way to do this?**

- A) Testing isn't needed for tools, Claude can figure out how to use them
- B) Connect to Claude immediately
- C) Use the MCP Inspector in your browser ✓
- D) Test in production


**2. You're building a document system where users can type @document_name to reference files. What MCP feature is best for exposing the document contents?**

- A) Tools
- B) Clients
- C) Prompts
- D) Resources ✓


**3. Your MCP server and client need to communicate. What's the most common way they connect during development?**

- A) Through a database
- B) Over the internet
- C) Through standard input/output on the same machine ✓
- D) Using email


**4. You're building a chatbot that needs to access GitHub data. What is the main benefit of using MCP instead of writing your own GitHub integration?**

- A) MCP requires less memory
- B) MCP handles the tool definitions and execution for you ✓
- C) MCP only works with GitHub
- D) MCP makes your chatbot run faster


**5. You want to create a tool for your MCP server that reads document contents. Using the Python SDK, what's the easiest way to define this tool?**

- A) Write a complex JSON schema manually
- B) Send an HTTP request
- C) Use the @mcp.tool decorator on a function ✓
- D) Create a separate configuration file


**6. You want to provide users with a high-quality, pre-tested instruction for formatting documents. What MCP feature should you use?**

- A) Resources
- B) Sessions
- C) Tools
- D) Prompts ✓


## Agents and workflows


### Quiz on Agents and Workflows


**1. You're building an agent with tools. Which approach will give Claude the most flexibility to handle unexpected requests?**

- A) Give Claude only one powerful tool
- B) Provide very specific tools like "write_python_function" and "debug_code"
- C) Provide abstract tools like "read_file", "write_file", and "run_command" ✓
- D) Provide tools that only work for planned scenarios


**2. You want Claude to write a report, then check if it's good enough, and improve it if needed. What pattern are you using?**

- A) Chaining workflow
- B) Evaluator-Optimizer pattern ✓
- C) Parallelization workflow
- D) Routing workflow


**3. Your app generates different types of social media content. Programming topics need educational scripts, while sports topics need entertainment-focused content. What pattern should you use?**

- A) Route requests to specialized processing pipelines ✓
- B) Use the same prompt for everything
- C) Ask users to write their own content
- D) Always use the entertainment approach


**4. Claude keeps ignoring some of your rules when you give it a long prompt with many requirements. What workflow approach would help?**

- A) Make the prompt even longer with more rules
- B) Run everything in parallel
- C) Use a routing workflow to categorize first
- D) Chain the task into focused sequential steps ✓


**5. You need Claude to recommend the best material for a part by considering metal, plastic, ceramic, and wood options. Each material has different criteria. What's the best approach?**

- A) Chain the evaluations one after another
- B) Ask Claude to pick randomly
- C) Send separate requests for each material type in parallel ✓
- D) Put all criteria in one big prompt


**6. You need to choose between a workflow and an agent for your app. Reliability and predictable results are most important to you. Which should you pick?**

- A) Always use an agent for maximum flexibility
- B) Use a workflow since it's more reliable and testable ✓
- C) Combine both approaches equally
- D) Use whichever is easier to code


**7. You're building an app where users upload photos of damaged car parts and always get repair cost estimates. You know exactly what steps are needed each time. What should you use?**

- A) Multiple agents working together
- B) A single complex prompt
- C) An agent with many specialized tools
- D) A workflow with predetermined steps ✓


## Final assessment


### Final Assessment


**1. What is a tool function in the context of Claude's tool use system?**

- A) A special API endpoint provided by Anthropic
- B) A configuration file that defines Claude's behavior
- C) A database query that retrieves user preferences
- D) A plain function that gets executed when Claude needs additional information or needs to perform an action ✓


**2. You're improving a prompt that isn't working well. What should you do after applying a prompt engineering technique?**

- A) Start over with a completely new prompt
- B) Ask someone else to write the prompt
- C) Use prompt evaluations to see if it actually improved ✓
- D) Move on to the next technique immediately


**3. You're making a math tutoring app. You want Claude to give hints instead of direct answers. What should you use?**

- A) Ask users to say "please" in every question
- B) Use smaller fonts to make Claude quiet
- C) Send messages only on weekdays
- D) A system prompt explaining Claude should act like a tutor ✓


**4. You want Claude to write very creative, unpredictable stories. What temperature setting should you use?**

- A) 0.5 (medium)
- B) 0.0 (very low)
- C) Temperature doesn't affect creativity
- D) 1.0 (very high) ✓


**5. You want an AI to write a product description. Which opening is most clear and direct?**

- A) "I was wondering if you could maybe help me with something about products?"
- B) "Can you tell me about products and descriptions and stuff?"
- C) "Write a product description for running shoes." ✓
- D) "What do you think makes a good product description?"


**6. You want to measure how well your AI prompt actually works in practice. Which approach should you focus on?**

- A) Using multishot prompting examples
- B) Prompt engineering techniques like XML tags
- C) Writing longer, more detailed prompts
- D) Prompt evaluation with automated testing ✓


**7. You're building a web app that talks to Claude. Where should you store your API key?**

- A) On your server, hidden from users ✓
- B) In a public GitHub repository
- C) In the web browser's settings
- D) In your website's JavaScript code


**8. What is a model grader in prompt evaluation?**

- A) A human reviewer who manually scores AI outputs
- B) Another AI model used to assess the quality of outputs ✓
- C) A programmatic check that validates syntax and format
- D) A scoring system that only measures response speed


**9. You're asking an AI to analyze both customer reviews and sales data. How should you organize this information in your prompt?**

- A) Put reviews first, then sales data with no separators
- B) Mix everything together in one paragraph
- C) Write the data in different fonts
- D) Use XML tags like <reviews> and <sales_data> to separate them ✓


**10. You want to send a message to Claude through the API. Which four things do you absolutely need to include?**

- A) Temperature, creativity, speed, and language
- B) Date, time, location, and device type
- C) API key, model name, messages, and max tokens ✓
- D) Username, password, email, and phone number


**11. What is the primary purpose of tool use in Claude?**

- A) To automatically save conversation history
- B) To allow Claude to access real-time information and external systems beyond its training data ✓
- C) To make Claude respond faster to user queries
- D) To reduce the number of tokens Claude uses in responses


**12. You're running a prompt evaluation. After getting responses from Claude, what's the next step in the typical workflow?**

- A) Rewrite the prompt completely from scratch
- B) Create a new dataset with different questions
- C) Deploy the prompt to production immediately
- D) Feed the responses through a grader for scoring ✓


**13. You ask Claude "What is pizza?" and it answers. Then you ask "What toppings are popular?" but Claude doesn't know you're still talking about pizza. What's the problem?**

- A) You need to send the whole conversation history with each request ✓
- B) Claude doesn't like pizza
- C) Claude is broken
- D) You're asking too many questions


**14. What is the primary difference between an MCP Server and an MCP Client in terms of their roles?**

- A) MCP Servers contain tools, prompts, and resources while MCP Clients act as the communication bridge to access those tools ✓
- B) MCP Servers handle user authentication while MCP Clients manage permissions
- C) MCP Servers store data while MCP Clients process requests
- D) MCP Servers run on remote machines while MCP Clients only run locally


**15. Which of the following best describes Computer Use in the context of Claude?**

- A) A method for deploying Claude applications to cloud servers
- B) A billing system that tracks how much computational time Claude consumes
- C) A capability that lets Claude interact directly with desktop environments like a human would ✓
- D) A feature that optimizes Claude's processing speed on different hardware


**16. What does "transport agnostic" mean in the context of MCP communication?**

- A) MCP clients and servers can communicate using different methods like HTTP or standard input/output ✓
- B) MCP only works with encrypted connections
- C) MCP automatically chooses the fastest available network connection
- D) MCP requires specific hardware to function properly


**17. What is the primary purpose of a batch tool in Claude's tool system?**

- A) To accept multiple tool calls and execute them simultaneously ✓
- B) To encrypt multiple tool calls for security purposes
- C) To process large files that exceed normal size limits
- D) To automatically retry failed tool calls


**18. Claude responds to your request with both explanatory text and a tool use block. What type of message structure is this?**

- A) An error message that needs to be fixed
- B) A multi-block message with different content types ✓
- C) A single-block text message
- D) A malformed response that should be ignored


**19. You're building an app where users need to verify information Claude provides from documents. What feature should you enable?**

- A) Code execution
- B) Prompt caching
- C) Citations ✓
- D) Extended thinking


**20. When should you choose workflows over agents for handling user tasks?**

- A) When you're not sure what tasks or parameters you'll give to Claude
- B) When you need maximum flexibility in task completion
- C) When you want Claude to creatively combine tools in unexpected ways
- D) When you can picture the exact flow or steps Claude should go through to solve a problem ✓


**21. Which of the following best describes why environment inspection is crucial for AI agents?**

- A) It helps agents run faster and more efficiently
- B) It allows agents to observe and understand the results of their actions ✓
- C) It simplifies the agent's tool requirements
- D) It reduces the cost of running AI operations


**22. What is the Model Context Protocol (MCP)?**

- A) A security protocol for protecting AI model parameters
- B) A communication layer that provides Claude with context and tools without requiring tedious integration code ✓
- C) A programming language designed specifically for AI applications
- D) A database system for storing AI training data


**23. You keep sending the same long document to Claude with different questions. How can you make this faster and cheaper?**

- A) Compress the document first
- B) Split the document into smaller pieces
- C) Use prompt caching with cache breakpoints ✓
- D) Ask multiple questions at once
