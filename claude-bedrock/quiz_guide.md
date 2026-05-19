# Claude with Amazon Bedrock — Quiz Guide

_All quiz questions with answer choices (correct answer marked with ✓)._

---


## Accessing Claude with the API


### Quiz on working with the API


**1. You're building a chat app that talks to Claude. Where should you put your API key?**

- A) In the user's browser
- B) On your secure server
- C) In your website's JavaScript code
- D) In a public GitHub repository


**2. What is the primary purpose of a system prompt when working with Claude?**

- A) To authenticate API requests to the Anthropic service
- B) To provide instructions that customize Claude's tone, style, and approach
- C) To limit the number of tokens Claude can generate in a response
- D) To store the conversation history between multiple requests


**3. Your users complain that your chat app feels slow because they wait 20 seconds staring at a loading spinner, then a bunch generated text suddenly appears. What feature should you add to fix this?**

- A) Shorter prompts
- B) Response streaming
- C) Multiple chatbots
- D) Faster internet connection


**4. You want Claude to generate only clean JSON code without any explanations or markdown formatting. Which combination of techniques works best?**

- A) Request shorter responses
- B) Ask nicely in the prompt
- C) Use high temperature and long prompts
- D) Prefill with "{" and use "```" as a stop sequence


**5. You're building a chatbot to answer factual questions about your company. You want consistent, reliable answers every time. What temperature setting should you use?**

- A) 0.1 (low temperature)
- B) It doesn't matter
- C) 0.8 (high temperature)
- D) 1.0 (high temperature)


**6. Claude reads your message "I love quantum physics." What happens first?**

- A) Claude breaks the text into smaller pieces called tokens
- B) Claude researches quantum physics
- C) Claude writes a response immediately
- D) Claude translates it to another language


**7. You ask Claude "What's the best programming language?" but you want it to specifically argue for Python. What technique helps you control this?**

- A) Use a higher temperature setting
- B) Ask the question multiple times
- C) Make the text bigger
- D) Add an assistant message starting with "Python is the best because"


## Prompt Evaluation


### Quiz on prompt evaluations


**1. You've learned techniques for writing better prompts, but now you want to measure how well they actually work. What do you need?**

- A) More prompt engineering techniques
- B) Prompt evaluation methods
- C) More training data
- D) A faster AI model


**2. You need test cases for your prompt evaluation but don't want to write them all by hand. What's a good alternative?**

- A) Ask users to create test cases
- B) Only test with one example
- C) Skip testing and deploy immediately
- D) Use Claude to generate test cases automatically


**3. You write a prompt and test it twice with your own inputs. It looks good, so you deploy it. What's the main risk?**

- A) The prompt will work too slowly
- B) The prompt will become too expensive
- C) The AI model will stop working
- D) Users might provide unexpected inputs that break it


**4. In a typical evaluation workflow, what happens right after you feed your prompts through Claude?**

- A) You change the prompt and start over
- B) You feed the responses through a grader
- C) You create a new dataset
- D) You deploy the prompt to production


## Prompt Engineering


### Quiz on prompt engineering


**1. You're giving your AI a long document to analyze along with your instructions. What would help the AI understand your prompt better?**

- A) Put everything in one big paragraph
- B) Write the document in all capital letters
- C) Use XML tags like `<document>` and `<instructions>`
- D) Separate sections with lots of blank lines


**2. You want your AI to write a book review. Which approach would be more helpful?**

- A) "Write a book review that's 300 words, includes plot summary, mentions two characters, and gives a rating"
- B) "Write a book review that's good and interesting"
- C) "Tell me what you think about books in general"
- D) "Write something about the book I just read"


**3. You want to improve a prompt that isn't working well. What should you do first after writing your initial prompt?**

- A) Test it and measure how well it performs
- B) Add more examples immediately
- C) Rewrite it completely from scratch
- D) Make it longer and more detailed


**4. Your evaluation report shows your prompt scored poorly on "missing calorie information" across multiple test cases. What does this tell you?**

- A) You should ignore this feedback and try something else
- B) You need to specifically tell your prompt to include calorie information
- C) The test cases are too hard
- D) The evaluation system is broken


**5. You want your AI to detect sarcasm in social media posts, but it keeps missing sarcastic comments. What would help most?**

- A) Tell it to "try harder" to find sarcasm
- B) Use bigger fonts in your prompt
- C) Show it examples of sarcastic posts with correct labels
- D) Ask it to guess when posts might be sarcastic


**6. Why is the first line of your prompt considered the most important part?**

- A) It determines which AI model will be used
- B) It determines how fast the AI will respond
- C) It sets the stage for everything that follows and should be clear and direct
- D) It controls the length of the AI's response


## Tool Use


### Quiz on tool use


**1. When Claude wants to use a tool, it sends back a response that's different from usual. What does this response contain?**

- A) Error messages only
- B) Both text blocks and tool use blocks
- C) Only tool requests with no text
- D) Only text like normal


**2. You want to create a tool that gets the current time. What type of code do you need to write?**

- A) A regular Python function
- B) A web page
- C) A database query
- D) A complex AI algorithm


**3. A user asks Claude "What day will it be 30 days from today?" To answer this, Claude needs to use multiple tools. What happens?**

- A) Claude asks the user to do the math
- B) Claude uses one tool and guesses the rest
- C) Claude calls tools in sequence - first getting today's date, then adding 30 days
- D) Claude gives up and says it can't help


**4. Sarah asks Claude "What's the weather like today?" but Claude says it doesn't have current weather data. What would solve this problem?**

- A) Waiting for Claude to update itself
- B) Giving Claude access to tools that fetch current data
- C) Asking Claude to guess the weather
- D) Training Claude on more weather information


**5. You're building a chat app with Claude. A user asks for today's stock prices, but Claude responds "I don't have access to current stock information." What's the core problem?**

- A) The user asked the wrong question
- B) Claude is broken
- C) Claude only knows information from its training data
- D) Claude needs to be restarted


**6. You want to give Claude the ability to search the web for current information. What do you need to implement?**

- A) Just a simple schema - Claude handles the searching
- B) Your own search engine
- C) A complex web scraping system
- D) Permission from Google


**7. You've written a Python function for Claude to use. What else do you need so Claude knows how to call it?**

- A) Permission from Claude
- B) A JSON schema describing the function
- C) A special license
- D) A user manual


## RAG and Agentic Search


### Quiz on Retrieval Augmented Generation


**1. You're setting up a system to handle large documents. Instead of using everything at once, you break documents into smaller pieces and search for relevant ones. What is this approach called?**

- A) The chunking approach
- B) File splitting
- C) Text summarization
- D) Document compression


**2. You try to include a massive 800-page document directly in your Claude prompt. What problems will you likely face?**

- A) There are hard limits on text length, reduced effectiveness, and higher costs
- B) The document will be perfectly processed
- C) Claude will work faster than normal
- D) Only the cost will increase slightly


**3. You have an 800-page financial report and want to ask Claude specific questions about it. What does RAG help you do?**

- A) Ask only yes/no questions
- B) Put the entire document into each prompt
- C) Summarize the whole document first
- D) Find and include only the relevant sections for each question


**4. You send the text "The cat is happy" to an embedding model. What do you get back?**

- A) A summary of the text
- B) A translation in another language
- C) A list of keywords
- D) A long list of numbers


**5. What problem does contextual retrieval solve in RAG systems?**

- A) It makes search queries run faster
- B) It reduces the storage space needed for embeddings
- C) It addresses the issue of chunks losing their connection to broader document context when documents are split
- D) It eliminates the need for vector databases


**6. You have search results from both semantic search and BM25 search. They use different scoring systems. How do you combine them into one ranked list?**

- A) Use Reciprocal Rank Fusion (RRF) based on rank positions
- B) Take the average of both scores
- C) Add the scores together directly
- D) Use only the semantic search results


**7. What is the purpose of re-ranking in RAG pipelines?**

- A) To compress the vector database for faster searches
- B) To generate better embeddings for text chunks
- C) To use an LLM to intelligently reorder search results after initial retrieval
- D) To split documents into more appropriate chunk sizes


**8. You're searching for a specific incident ID like "INC-2023-Q4-011" in your documents. Semantic search isn't finding it well. What additional search method would help?**

- A) Bigger vector database
- B) BM25 lexical search for exact term matching
- C) Longer embeddings
- D) More chunks


## Features of Claude


### Quiz on features of Claude


**1. When Extended Thinking is enabled, what two parts will Claude's response contain?**

- A) A summary block and a detail block
- B) A thinking block and a text block
- C) A draft block and a final block
- D) A question block and an answer block


**2. You ask Claude "How many marbles are in this image?" but get the wrong count. What's the best way to improve accuracy?**

- A) Ask the question in all capital letters
- B) Send a higher quality image
- C) Upload the image multiple times
- D) Provide detailed counting steps and methodology


**3. What's the minimum amount of content needed for caching to work?**

- A) Any amount of text
- B) 500 tokens
- C) 1024 tokens
- D) 2000 tokens


**4. You want to cache your tool definitions. Where should you place the cache breakpoint?**

- A) On the last tool in your list
- B) On the middle tool in your list
- C) On every tool in your list
- D) On the first tool in your list


**5. How does prompt caching work?**

- A) It makes Claude remember conversations forever
- B) It prevents Claude from making mistakes
- C) It reuses computational work from previous requests
- D) It translates messages into different languages


**6. You're building an app where users ask questions about documents. What's the main benefit of enabling citations?**

- A) It reduces the cost of each request
- B) It shows users exactly where information came from
- C) It makes Claude's responses longer
- D) It makes the app run faster


## Model Context Protocol


### Quiz on Model Context Protocol


**1. User-controlled workflows that are triggered through UI interactions like button clicks or slash commands. This definition describes which MCP primitive?**

- A) Resources
- B) Sessions
- C) Tools
- D) Prompts


**2. What does "transport agnostic" mean in the context of MCP communication?**

- A) MCP automatically chooses the fastest available network connection
- B) MCP requires specific hardware to function properly
- C) MCP only works with HTTP connections
- D) MCP clients and servers can communicate using different methods like HTTP, WebSockets, or standard input/output


**3. What are Resources in the context of MCP?**

- A) App-controlled data access for UI purposes or adding context to conversations
- B) Model-controlled functions for performing calculations
- C) User-triggered commands that start predefined workflows
- D) Server configuration settings that control performance


**4. In MCP architecture, what is the relationship between MCP Clients and MCP Servers?**

- A) MCP Clients generate AI responses while MCP Servers handle user input
- B) MCP Clients store data while MCP Servers process requests
- C) MCP Clients connect to MCP Servers that contain tools, prompts, and resources
- D) MCP Clients and MCP Servers are the same component with different names


**5. What is Model Context Protocol (MCP)?**

- A) A programming language specifically designed for AI applications
- B) A communication layer that provides Claude with context and tools without requiring tedious integration code
- C) A security protocol for encrypting AI model responses
- D) A database management system for storing AI conversations


**6. What is the MCP Server Inspector?**

- A) A command-line tool for monitoring server performance
- B) A code editor specifically designed for writing MCP servers
- C) A security tool for scanning MCP servers for vulnerabilities
- D) A browser-based interface for testing and debugging MCP servers in real-time


**7. Which of the following correctly describes Tools in MCP?**

- A) App-controlled data that populates UI elements
- B) Static configuration files that define server behavior
- C) User-controlled workflows that can be triggered on demand
- D) Model-controlled functions that Claude decides when to call


## Anthropic Apps and Agents


### Final assessment quiz

_Quiz questions could not be extracted._
