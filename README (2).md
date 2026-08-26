# Multi-Agent System

A simple multi-agent pipeline built in Python. A `PlannerAgent` lays out the
steps, a `ResearchAgent` and `KnowledgeAgent` gather evidence, a
`ReasoningAgent` produces an answer, and a `CriticAgent` verifies it —
retrying (replanning) up to a configurable number of times if verification
fails.

## Structure

- `agent.py` — main script containing all agent classes and the
  `Orchestrator` that runs the pipeline.

## Agents

| Agent | Role |
|---|---|
| `PlannerAgent` | Breaks the user's query into a plan |
| `ResearchAgent` | Collects external evidence |
| `KnowledgeAgent` | Looks up internal knowledge |
| `ReasoningAgent` | Analyzes evidence and drafts an answer |
| `CriticAgent` | Verifies the answer and approves/rejects it |
| `Orchestrator` | Runs the full pipeline, with retries on rejection |

## Usage

```bash
python agent.py
```

You'll be prompted to enter a question; the system will run through the
plan → research → knowledge → reasoning → critique loop (up to 3 attempts)
and print the final answer, confidence score, and retry count.

## Requirements

No external dependencies — uses only the Python standard library
(`dataclasses`, `typing`).
