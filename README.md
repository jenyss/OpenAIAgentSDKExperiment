# Open AI Agent SDK Experiment

This OpenAI Agent is created by a LangGraph Agent with a vector search tool. The OpenAI Agent SDK documentation been embedded in the vector store to serve as a reference for the Agent.

The LangGraph Agent returned wrongly:<br>
```async def write_to_markdown(ctx: RunContextWrapper, content: str, filename: str = "output.md")```
<br>
Correct declaration:<br>
```async def write_to_markdown(ctx: RunContextWrapper, content: str, filename: str)```<br>


```
================================== Ai Message ==================================
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


================================================================================
# Execution Output (see query_reply.md)

# Latest Developments in AI Technology (March 2025)

## Advancements in AI and Robotics

- **Google's Integration of AI with Robotics**: Google has announced the integration of its advanced Gemini 2.0 AI language models with robotic systems capable of performing physical actions. This initiative aims to create more versatile robots but also introduces new categories of risk as AI begins to take on physical capabilities. ([axios.com](https://www.axios.com/2025/03/12/google-humanoid-robotics-gemini-deepmind?utm_source=openai))

- **Google DeepMind's New AI Models**: Google DeepMind has introduced new AI models, Gemini Robotics and Gemini Robotics-ER, designed to enhance the practicality of general-purpose robots in real-world settings. These models enable robots to perform complex tasks such as folding origami, organizing a desk, and playing basketball. ([ft.com](https://www.ft.com/content/f0b1dff8-8936-4e05-9e0f-b1bbbb40dc02?utm_source=openai))

## Legal and Regulatory Developments

- **French Publishers Sue Meta**: French publishers and authors have filed a lawsuit against Meta, accusing the company of using their copyrighted works without permission to train its AI models. The plaintiffs demand the removal of data directories created by Meta for AI training, highlighting ongoing tensions between creative industries and tech companies over data usage. ([apnews.com](https://apnews.com/article/168b32059e70d0509b0a6ac407f37e8a?utm_source=openai))

- **Poland Seeks Reversal of AI Chip Restrictions**: The Polish government hopes to see a reversal of AI chip export restrictions imposed during the Biden administration. These restrictions have limited Poland's access to U.S.-designed AI chips. Polish officials are optimistic that the Trump administration will facilitate Poland's move to a more favorable tier after constructive dialogues. ([reuters.com](https://www.reuters.com/technology/poland-hopes-trump-will-reverse-biden-era-ai-chip-restrictions-2025-03-12/?utm_source=openai))

## AI in Retail and Government

- **AI Enhancing Retail Personalization**: During a recent event, Ulta Beauty's CMO, Kelly Mahoney, discussed the company's use of AI since 2018 to enhance data and personalize marketing based on customer shopping habits. The integration of technology is essential for maintaining consumer attention through enhanced in-store experiences and omnichannel strategies. ([axios.com](https://www.axios.com/2025/03/12/axios-event-technology-retail-personalization?utm_source=openai))

- **Dell's Federal AI Push**: John Roese, Dell's global CTO and chief AI officer, discussed accelerating federal AI adoption, emphasizing that government agencies can confidently adopt AI, as demonstrated by successful business cases. Dell aims to guide federal officials through the AI integration process, highlighting the necessity of adapting to unique data and operational requirements. ([axios.com](https://www.axios.com/2025/03/12/axios-interview-john-roese-dell-federal-ai-push?utm_source=openai))

## Recent Developments in AI Technology:  
- [Google looks to give AI its arms and legs](https://www.axios.com/2025/03/12/google-humanoid-robotics-gemini-deepmind?utm_source=openai)  
- [French publishers and authors sue Meta over copyright works used in AI training](https://apnews.com/article/168b32059e70d0509b0a6ac407f37e8a?utm_source=openai)  
- [Google DeepMind unveils new AI models in race to make robots useful](https://www.ft.com/content/f0b1dff8-8936-4e05-9e0f-b1bbbb40dc02?utm_source=openai)
