# LangGraph + LangChain Starter

A minimal Python starter project for building stateful workflows with [LangGraph](https://langchain-ai.github.io/langgraph/) and [LangChain](https://python.langchain.com/).

## Requirements

- Python 3.10 or newer
- An OpenAI API key, if you use the example model

## Setup

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
export OPENAI_API_KEY="your-api-key"
```

On Windows PowerShell, use:

```powershell
$env:OPENAI_API_KEY = "your-api-key"
```

## Minimal example

The following graph calls a LangChain chat model and returns its response:

```python
from typing import TypedDict

from langchain_openai import ChatOpenAI
from langgraph.graph import END, START, StateGraph


class State(TypedDict):
    message: str
    response: str


model = ChatOpenAI(model="gpt-4o-mini")


def call_model(state: State) -> dict[str, str]:
    response = model.invoke(state["message"])
    return {"response": response.content}


builder = StateGraph(State)
builder.add_node("call_model", call_model)
builder.add_edge(START, "call_model")
builder.add_edge("call_model", END)
graph = builder.compile()

result = graph.invoke({"message": "Explain LangGraph in one sentence."})
print(result["response"])
```

Save the example as `main.py` and run it with:

```bash
python main.py
```

## Project ideas

- Add conditional edges for routing between agents or tools.
- Add checkpointing for resumable workflows.
- Replace `ChatOpenAI` with another LangChain chat model integration.
- Add LangSmith tracing with `LANGSMITH_API_KEY` and `LANGCHAIN_TRACING_V2=true`.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
