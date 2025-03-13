# Open AI Agent SDK Experiment

This OpenAI Agent is created by a LangGraph Agent with a vector search tool. The OpenAI Agent SDK documentation been embedded in the vector store to serve as a reference for the Agent.

The LangGraph Agent returned wrongly:<br>
<code>async def write_to_markdown(ctx: RunContextWrapper, content: str, filename: str = "output.md")</code>
<br>
Correct declaration:<br>
<code>async def write_to_markdown(ctx: RunContextWrapper, content: str, filename: str)</code><br>

If you want to execute it in Jupyter Notebook add
```from dotenv import load_dotenv
import os

# Load environment variables
load_dotenv()
```
<code>================================== Ai Message ==================================
<br>
To create an AI Agent using the OpenAI Agent SDK that can search the web in real-time, extract data based on user prompts, and write it to a markdown file, we will follow these steps:
<br>
1. **Install Required Libraries**: We need to install the `openai-agents` package.
2. **Set Up API Keys**: Ensure the OpenAI API key is set up in your environment.
3. **Create the Agent**: Define an agent using the OpenAI Agent SDK.
4. **Define Tools**: Use `WebSearchTool` for web searching and `FunctionTool` for writing to a markdown file.
5. **Run the Agent**: Use the `Runner` to execute the agent.

Here's the complete code to achieve this:

```python
# Install the required package
# !pip install openai-agents

import asyncio
from agents import Agent, Runner, function_tool, WebSearchTool
from agents.run_context import RunContextWrapper

# Define a function tool to write data to a markdown file
@function_tool
async def write_to_markdown(ctx: RunContextWrapper, content: str, filename: str = "output.md") -> str:
    """Writes the given content to a markdown file."""
    with open(filename, "w") as f:
        f.write(content)
    return f"Data written to {filename}"

# Create the agent
agent = Agent(
    name="WebSearchAgent",
    instructions="Search the web for the user's query and write the results to a markdown file.",
    tools=[
        WebSearchTool(),
        write_to_markdown
    ]
)

# Define the main function to run the agent
async def main():
    user_query = "Latest news on AI technology"
    result = await Runner.run(agent, user_query)
    print(result.final_output)

# Run the main function
await main()
```

### Libraries to Install
- `openai-agents`

### Required API Keys
- OpenAI API Key: Set this in your environment as `OPENAI_API_KEY`.

This code sets up an agent that uses a web search tool to find information online and a function tool to write the results to a markdown file. The agent is executed using the `Runner` class, which handles the asynchronous execution of the agent's tasks.
</code>
