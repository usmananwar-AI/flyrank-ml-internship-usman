# Demystifying Agents and the Model Context Protocol (MCP)

The term "agent" has become the most overloaded buzzword in the AI industry, often used to describe anything from a simple automated prompt to a fully autonomous software engineer. However, from an engineering perspective—specifically following Anthropic's "Building Effective Agents" framework—there is a rigorous distinction between a *workflow* and an *agent*. Understanding this distinction, and the underlying Model Context Protocol (MCP) that connects AI to the outside world, is critical for building reliable AI systems.

### Workflows vs. Agents
The difference between a workflow and an agent comes down to who controls the routing. 

A **workflow** is a predefined, orchestrated sequence of LLM calls and programmatic actions. The path is hardcoded. Think of it like a factory assembly line: Step A extracts text, Step B formats it, and Step C saves it. The LLM performs the labor at each station, but the system's architecture dictates the movement of data between stations. 

An **agent**, on the other hand, is a system where the LLM dynamically directs its own control flow. Instead of following a rigid script, the LLM is given an objective, a set of tools, and a loop. It analyzes the current state, decides which tool to use, observes the output, and iterates until it determines the objective is complete. If a workflow is a train on a track, an agent is a self-driving car navigating traffic.

### Classifying the FL-04 Pipeline
My FL-04 build—the Source-Grounded Study Notes Pipeline—is strictly a **workflow**, not an agent. 

The process is highly orchestrated and relies on a human-in-the-loop for routing. I manually upload the data structures slides into NotebookLM (Gather), trigger a specific prompt to isolate facts (Synthesize), and then manually copy that output into a predefined Claude Project to format it into Markdown (Format). The AI never decides what to do next; it only executes the specific, isolated task it is handed at each stage. 

### The Model Context Protocol (MCP)
For an LLM to transition from a passive chatbot into an active agent, it needs to interact with external environments—databases, file systems, and APIs. Historically, every AI product built custom integrations for these tools. 

The **Model Context Protocol (MCP)** solves this by acting as the "USB-C port for AI." It is an open standard that allows developers to build a single server (e.g., a Google Drive MCP server) that any MCP-compatible AI client (like Claude Desktop) can instantly connect to. 

MCP operates on three core primitives:
1. **Tools:** Executable functions the AI can call to take action (e.g., `read_file`, `execute_sql`).
2. **Resources:** External data the AI can read to gain context, often represented with URI templates (e.g., `file:///users/database/schema`).
3. **Prompts:** Reusable instruction templates that help the AI understand how to approach specific tasks.

By standardizing these primitives, MCP allows LLMs to safely and predictably interface with local and cloud infrastructure.

### Evolving FL-04 into an Agent
To upgrade my FL-04 Study Notes workflow into a true agent, I would implement a tool-calling loop using MCP. 

Instead of manually moving data, I would provide an LLM with three MCP tools: `search_local_files`, `extract_pdf_text`, and `write_markdown_file`. I would give the agent a single overarching prompt: *"Monitor my university 'Data Structures' folder. Whenever a new PDF is added, extract its core algorithms and time complexities, format them into a Markdown study guide, and save the file."*

The agent would autonomously loop: using the search tool to find new files, the extraction tool to read them, its own reasoning to synthesize the data, and the write tool to save the `.md` file—all without human routing. This transforms a rigid, multi-step chore into an autonomous, goal-driven system.
