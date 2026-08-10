# Agentic AI Workflows

A growing collection of small, hands-on tutorials exploring **agentic AI workflows**, with a particular interest in scientific workflows, HPC, distributed computing, and workflow systems.

The repository is intended to grow over time. Rather than starting with large or complicated applications, the notebooks use deliberately small examples so that the important ideas are easy to see: **agents, tools, evidence, state, actions, loops, deterministic control, LLM-based decisions, and parallel execution**.

## Tutorials

### 1. From MPI Processes to Proactive Agents

**Notebook:** `mpi_processes_to_proactive_agents.ipynb`

This seven-step tutorial starts from a familiar MPI4py scatter/gather example and gradually changes the coordination pattern:

```text
MPI4py scatter/gather
        ↓
MPI ranks described as manager and workers
        ↓
Plain Python agents requesting work
        ↓
Basic LangGraph workflow
        ↓
Proactive LangGraph validation
        ↓
Deterministic LangGraph task-control loop
        ↓
LLM-powered IoT sensor triage with LangChain
```

The first six steps deliberately use a tiny problem: adding `+1` to an array. This lets us concentrate on coordination rather than computation. The final step changes to an IoT sensor scenario because this is where an LLM has a genuinely useful role: interpreting short, potentially ambiguous diagnostic notes.

An important lesson is that **agentic does not automatically mean LLM-powered**. Several intermediate examples are agentic while remaining entirely deterministic Python. The LLM is introduced only when language interpretation becomes useful.

**Run in Google Colab:**  
https://colab.research.google.com/drive/1kzhauOBha7mnN6FeaCLuY4G9SThWGPIU?usp=sharing

A companion blog, *From MPI Processes to Proactive Agents: A Tiny Tutorial for HPC People Curious About Agentic Workflows*, explains the motivation and the seven-step journey. The repository notebook is the hands-on version referenced by that blog.

---

### 2. From a dispel4py Sensor Workflow to an Agentic AI Workflow

**Notebook:** `dispel4py_agentic_ai_sensor_tutorial.ipynb`

The second tutorial takes the **IoT sensor idea introduced in the final step of the first notebook** and explores it in much more detail using **dispel4py**.

The first notebook implements that final sensor example using LangGraph for orchestration and LangChain for the LLM interface. This notebook asks a different question:

> **What does an agentic AI workflow look like when dispel4py itself is the workflow system?**

We start from a traditional deterministic sensor workflow and evolve it into:

```text
ReadSensorDataPE
        ↓
NormalizeDataPE
        ↓
DeterministicPrecheckPE
        ↓
LLMSensorAgentPE
        ↓
ActionExecutorPE
        ↓
ResultWriterPE
```

Most Processing Elements remain ordinary deterministic workflow components. `LLMSensorAgentPE` is the strongly agentic component: it can inspect the current event, gather evidence, call bounded tools, observe tool results, reason again, and eventually submit a final decision.

The tutorial carefully distinguishes concepts that are easy to mix up:

```text
Evidence
    information available to the agent

Tools
    Python functions the agent is allowed to call

Evidence tools
    retrieve information, such as previous or neighbouring readings

Operation tools
    perform controlled operations, such as requesting another
    measurement or creating a maintenance ticket

Tool results
    information returned after a tool executes

Final decision
    the bounded conclusion that ends the agent loop
```

It then progresses through three execution styles:

```text
deterministic dispel4py workflow
        ↓
agentic AI workflow using the simple mapping
        ↓
agentic AI workflow using the multiprocessing mapping
```

The parallel example highlights an important issue: with multiprocessing, several copies of an agent PE may execute in different processes, and their local Python state is **not automatically shared**. The tutorial therefore explains how evidence and state are handled differently when moving from a sequential agent to parallel agent workers.

Nothing special is added to dispel4py to make it "agentic". dispel4py continues to provide the workflow graph, Processing Elements, data movement, and execution mappings. The agentic behaviour is implemented inside a PE using ordinary Python, an OpenAI LLM, bounded tools, state, and an agent loop.

No LangGraph or LangChain is required for this version.

**Run in Google Colab:**  
https://colab.research.google.com/drive/19EQjfyBnW3I2lCWD0oO2Udp3kq5rom8v?usp=sharing

---

## How the two notebooks fit together

```text
Notebook 1
MPI and distributed-computing perspective
        ↓
increasingly agentic coordination
        ↓
LangGraph + LangChain IoT sensor example
        │
        │ same sensor idea
        ▼
Notebook 2
dispel4py sensor workflow
        ↓
deterministic → agentic
        ↓
tools + evidence + agent loop
        ↓
parallel agentic execution
```

The first notebook asks:

> How can we move from familiar distributed computation towards increasingly adaptive, agentic coordination?

The second asks:

> Can we express an LLM-powered agentic workflow directly as a dispel4py workflow, and what changes when we execute the agents in parallel?

Together, they illustrate that **an LLM is only one component of an agentic workflow**, and that agentic behaviour can be combined with existing workflow and parallel-computing systems rather than replacing them.

## Running the notebooks

The notebooks are designed to run in **Google Colab** and contain setup instructions.

Depending on the tutorial, they use:

- Python
- mpi4py / MPI
- dispel4py
- LangGraph
- LangChain
- OpenAI Python SDK
- an OpenAI API key for the LLM-powered sections

Please do not commit API keys to this repository. The notebooks request credentials at runtime where required.

## Repository structure

```text
Agentic-AI-Workfows/
│
├── README.md
├── mpi_processes_to_proactive_agents.ipynb
└── dispel4py_agentic_ai_sensor_tutorial.ipynb
```

More notebooks will be added as the collection develops.

## What is coming next?

This repository is intended as an evolving collection rather than a finished set of examples.

Future notebooks may explore agentic patterns for scientific workflows, HPC, workflow recovery, provenance, heterogeneous tools, distributed agents, human-in-the-loop decisions, and other practical scenarios.

The goal is not to put an LLM into every workflow. It is to explore **where agentic behaviour is useful, where deterministic workflow logic is better, and how the two can work together**.

## Author

**Rosa Filgueira**  
EPCC, University of Edinburgh
