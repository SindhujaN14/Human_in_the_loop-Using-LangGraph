# Groq-based Weather Search Example

This project demonstrates how to build an interactive weather search application using Groq and LangGraph, a state machine library for managing conversational state and logic flow. The app integrates with Groq's large language models (LLMs) and utilizes a weather search tool that outputs the weather for a given city.

## Dependencies

This project requires the following Python dependencies:
- `langgraph` — for managing the state graph and workflow logic
- `langchain_groq` — for integrating Groq LLMs
- `langchain_core` — for using LangChain core tools and messages

You can install the necessary dependencies using the following pip command:

```bash
pip install -U langgraph langchain_groq
Setup
Groq API Key: You need to set up your Groq API key. To do this, ensure that the GROQ_API_KEY environment variable is set in your system. The application will prompt you to input your API key if it’s not already set:

python
Copy
Edit
def _set_env(var: str):
    if not os.environ.get(var):
        os.environ[var] = getpass.getpass(f"{var}: ")
Run the Code: The script is set up to create an interactive conversation state with a weather search tool. It starts with a greeting and processes the conversation step-by-step, calling the weather search tool when needed and routing based on user responses.

The tool searches for the weather in a specified city and returns the result.
A human review step ensures that any tool output can be validated before proceeding.
How It Works
Graph Building: The application creates a state graph using LangGraph’s StateGraph. The graph consists of nodes that handle the logic of:

Calling the LLM (call_llm node)
Running the tool (run_tool node)
Performing human review (human_review_node node)
Tool Integration: The weather search tool is defined using @tool from langchain_core and is bound to the Groq model. The tool outputs a simple weather message like "Sunny!" when queried.

State Flow: The state transitions between nodes depending on whether a tool is used or human feedback is required. If no tool is called, the process ends. Otherwise, the flow continues with the necessary updates or feedback loop.

Memory and Checkpointing: The memory saver function ensures that the application retains context throughout the conversation, storing the messages and relevant data.

Visualization: The graph is visualized using Mermaid, which displays the structure of the nodes and edges that guide the conversation flow.

Example Usage
Once the dependencies are installed and the API key is set up, you can run the script. Below is an example interaction with the system:

Input:
"Hi!"
"What's the weather in SF?"
Output:
The system will respond with a search for the weather in San Francisco and output "Sunny!".
If necessary, the system will trigger a human review to confirm the result or make updates to the conversation flow.

python
Copy
Edit
# Test with weather search
initial_input = {"messages": [{"role": "user", "content": "what's the weather in SF?"}]}
thread = {"configurable": {"thread_id": "2"}}

for event in graph.stream(initial_input, thread, stream_mode="updates"):
    print(event)
    print("\n")
Graph Flow
The application’s state machine includes the following steps:

Start → Calls LLM
LLM Response → Routes to either human review or ends the process
Run Tool → Executes a tool call (e.g., weather search)
Human Review → Allows human intervention to continue, update, or provide feedback
