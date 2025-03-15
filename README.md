# Open AI Agents SDK Experiment

This OpenAI Agent was created using a LangGraph Agent with a vector search tool. The OpenAI Agent SDK documentation has been embedded in the vector store to serve as a reference for the LangGraph Agent.

The LangGraph Agent returned wrongly:<br>
```async def write_to_markdown(ctx: RunContextWrapper, content: str, filename: str = "output.md")```
<br>
Correct declaration:<br>
```async def write_to_markdown(ctx: RunContextWrapper, content: str, filename: str)```<br>

_**At the end of this page, you can compare the Agent generated by my LangGraph Agent with another Agent created using a custom OpenAI GPT. Both the LangGraph Agent and the GPT use the same Agents SDK documentation and have the same system and user prompts!**_

================================== Ai Message ==================================<br>
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


# Execution Output :: see query_reply.md


# OpenAI Agents SDK Agent generated by OpenAI GPT

_**In short nowhere near close to how it should look like!**_

================================== GPT ==================================<br>
Here’s the AI Agent implementation using the OpenAI Agent SDK. This agent will:

1. Search the web in real-time based on user queries.
2. Extract the required data.
3. Write the extracted data into a markdown file.

**Requirements**
You'll need to install the following libraries:

```pip install openai openai-agents markdownify```

**API Keys Required**

```OpenAI API key (for using the Agent SDK)```


```python
import openai
import markdownify
import json
from openai import Assistant
from openai.types.beta import Tool, FunctionTool
from typing import List
from openai.beta import tools
import requests

# Define tool for web search
def web_search(query: str) -> str:
    """Search the web and return extracted results."""
    response = requests.get(f"https://api.duckduckgo.com/?q={query}&format=json")
    if response.status_code == 200:
        data = response.json()
        return json.dumps(data.get("RelatedTopics", []))
    else:
        return "Error: Unable to fetch search results."

# Define tool for markdown writing
def write_markdown(content: str, filename: str = "output.md"):
    """Write extracted content into a markdown file."""
    with open(filename, "w", encoding="utf-8") as f:
        f.write(markdownify.markdownify(content))
    return f"Data written to {filename}"

# Define the AI agent
tools_list: List[Tool] = [
    FunctionTool(name="web_search", function=web_search, description="Search the web in real-time"),
    FunctionTool(name="write_markdown", function=write_markdown, description="Write content into a markdown file")
]

# Create the AI Assistant
assistant = Assistant.create(
    name="Web Data Extractor",
    instructions="Use tools to fetch web data and save it in markdown format.",
    tools=tools_list
)

# Example Usage
def run_agent(query: str):
    """Run the AI Agent to search the web and save data in markdown."""
    search_results = web_search(query)
    formatted_results = json.loads(search_results)
    content = "\n".join([item["Text"] for item in formatted_results if "Text" in item])
    return write_markdown(content)

# Example execution
query = "latest AI news"
result = run_agent(query)
print(result)
```
This AI Agent:

* Uses web_search to fetch real-time search results.
* Extracts relevant content.
* Writes it into a Markdown file using write_markdown.
