# Agent Systems Engineering

Hands-on experiments focused on understanding how AI agents work under the hood and how agentic systems can be designed for production-oriented use cases.

The goal of this repository is to explore agent architecture from first principles before moving into higher-level frameworks such as smolagents, LangGraph, RAG, MCP, and agent evaluation.

## Experiment 01 — Agent Loop From Scratch

Implemented a minimal tool-using agent loop using the Hugging Face Inference API.

### Architecture

User Request
    ↓
LLM
    ↓
Action Selection
    ↓
Application Executes Tool
    ↓
Observation
    ↓
LLM Continues
    ↓
Final Answer

### Key Engineering Insight

A key failure mode appeared immediately:

The model correctly selected a tool, but then generated the `Observation` itself instead of waiting for the tool to execute.

That creates an unreliable system because the model can fabricate the result of an external action.

The execution boundary should instead be:

- **LLM:** proposes the next action
- **Application:** executes the tool
- **Tool output:** becomes the source of truth
- **LLM:** continues using the real observation

To enforce this, generation is stopped before `Observation:`, the application executes the tool, and the real result is appended back to the conversation.

### Technologies

- Python
- Hugging Face Inference API
- LLM tool selection
- ReAct-style Thought → Action → Observation loop
- Google Colab

### Notebook

[`01_agent_loop_from_scratch.ipynb`](./01_agent_loop_from_scratch.ipynb)

## Roadmap

- Structured tool calling
- smolagents
- Custom tools
- RAG
- Agentic RAG
- LangGraph
- MCP
- Agent evaluation and observability
- Production-style enterprise agent

## References

This experiment was inspired by the Hugging Face Agents Course:

- Dummy Agent Library:
  https://huggingface.co/learn/agents-course/unit1/dummy-agent-library
- AI Agents Course:
  https://huggingface.co/learn/agents-course/
