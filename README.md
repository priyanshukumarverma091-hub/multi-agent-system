Multi-Agent System

A lightweight, extensible multi-agent orchestration framework in Python. The system breaks a user query down into a structured pipeline — planning, research, knowledge retrieval, reasoning, and critique — with automatic retry and replanning when the generated answer fails verification.

This project is designed as an architectural scaffold: the agent classes define clear responsibilities and a clean orchestration flow, making it straightforward to plug in real data sources, retrieval systems, or LLM calls in place of the current placeholder logic.

Table of Contents
Overview
Architecture
Agents
Installation
Usage
Example Run
Extending the System
Project Structure
Requirements
License
Overview

The system models a common agentic AI pattern: plan → gather evidence → reason → verify → retry if needed. An Orchestrator coordinates five specialized agents, passing a shared AgentState object between them. If the CriticAgent rejects an answer, the orchestrator triggers a replanning cycle, up to a configurable maximum number of retries.

Architecture
                 ┌───────────────┐
                 │  User Query   │
                 └───────┬───────┘
                         ▼
                 ┌───────────────┐
                 │ PlannerAgent  │
                 └───────┬───────┘
                         ▼
        ┌────────────────────────────────┐
        │            RETRY LOOP           │
        │                                  │
        │  ┌────────────────┐              │
        │  │ ResearchAgent   │              │
        │  └────────┬────────┘              │
        │           ▼                       │
        │  ┌────────────────┐              │
        │  │ KnowledgeAgent  │              │
        │  └────────┬────────┘              │
        │           ▼                       │
        │  ┌────────────────┐              │
        │  │ ReasoningAgent  │              │
        │  └────────┬────────┘              │
        │           ▼                       │
        │  ┌────────────────┐              │
        │  │  CriticAgent    │              │
        │  └────────┬────────┘              │
        │           │                       │
        │   approved? ──No──► replan ───────┘
        │           │
        └───────────┼───────────────────────┘
                    Yes
                     ▼
             ┌───────────────┐
             │ Final Answer  │
             └───────────────┘

State (AgentState) flows through every agent, accumulating plan steps, research, knowledge, reasoning, and the final answer, along with metadata like confidence score and retry count.

Agents
Agent	Responsibility
PlannerAgent	Breaks the user's query into an ordered set of steps
ResearchAgent	Gathers external evidence relevant to the query
KnowledgeAgent	Retrieves internal/domain knowledge relevant to the query
ReasoningAgent	Synthesizes research and knowledge into a reasoned answer
CriticAgent	Verifies the answer against structural checks and approves or rejects it
Orchestrator	Coordinates the full pipeline and manages the retry/replanning loop

Each agent inherits from BaseAgent, which provides consistent, prefixed logging ([PLANNER], [RESEARCHER], etc.) so pipeline execution is easy to trace in the console.

Installation

No external dependencies are required — the project uses only the Python standard library.

bash
git clone <your-repo-url>
cd multi-agent-system

Requires Python 3.7+ (for dataclasses support).

Usage

Run the script and enter a query when prompted:

bash
python agent.py
 Phase 2 Multi-Agent AI

 Enter your question: What are the benefits of renewable energy?

The orchestrator will run through the full plan → research → knowledge → reasoning → critique cycle and print the final answer, confidence score, and number of retries used.

Example Run
============================================================
🚀 MULTI-AGENT SYSTEM STARTED
============================================================

--- Attempt 1 ---
[RESEARCHER] Collecting external evidence...
[KNOWLEDGE] Searching internal knowledge...
[REASONER] Analyzing available information...
[CRITIC] Verifying the generated answer...
[CRITIC]  Answer approved.

============================================================
 FINAL ANSWER APPROVED
============================================================

Answer:
Based on the available information, the query 'What are the benefits of renewable energy?' has been analyzed.

Confidence: 0.90
Retries: 0
Extending the System

This scaffold is intended to be built on. Common next steps:

Real research — replace ResearchAgent.run() with a web search or API call.
Real knowledge retrieval — connect KnowledgeAgent to a vector store, database, or document index.
LLM-powered reasoning — swap the templated string in ReasoningAgent for a call to an LLM (e.g. the Anthropic API), passing in state.research and state.knowledge as context.
Richer critique — extend CriticAgent beyond structural checks (empty fields) to semantic checks, such as factual consistency or relevance scoring.
Configurable retries — adjust Orchestrator.MAX_RETRIES to control how many replanning cycles are allowed.
Project Structure
multi-agent-system/
├── agent.py       # All agent classes, AgentState, and the Orchestrator
├── README.md      # Project documentation
└── .gitignore     # Standard Python .gitignore
Requirements
Python 3.7+
No third-party packages
License

Add a license of your choice (e.g. MIT) here.# multi-agent-system
A Python multi-agent pipeline (Planner → Research → Knowledge → Reasoning → Critic) that answers user queries with automatic verification and retry/replanning on failure.
