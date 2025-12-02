# CrewAI_Flows

This repository contains a collection of examples demonstrating CrewAI Flows, a workflow orchestration system designed to build powerful, intelligent, and reactive pipelines.
Each folder showcases a unique flow type, including linear flows, structured/unstructured state management, conditional branching, and Crew-integrated flows.

--- 

# What Are CrewAI Flows?

**CrewAI Flows** allow you to define *how your application moves from one step to the next*.
A Flow contains:

* **@start** → The entry point
* **@listen** → Reacts to results from previous steps
* **@router** → Routes execution dynamically based on state
* **and_ / or_** → Complex dependencies
* **Structured state (Pydantic)** or **unstructured dict-based state**
* **Support for Crews (Agents + Tasks)** inside a flow

Flows are useful when building:

* Multi-step processing pipelines
* AI-powered workflows
* Conditional logic chains
* Agent-driven automations
* Dynamic decision systems

---

# Setup & Installation

Follow these steps before running any flow:

## Create a Virtual Environment

```bash
python3 -m venv venv
```

## Activate the Environment

### Windows:

```bash
venv\Scripts\activate
```

### macOS / Linux:

```bash
source venv/bin/activate
```

## Install All Requirements

```bash
pip install -r requirements.txt
```
## Environment Variables (LLM Keys)

Create a `.env` file in the project root and supply your LLM provider’s API keys. Example:

```
OPENAI_API_KEY=your_openai_api_key
# or, for other providers:
GEMINI_API_KEY=your_gemini_api_key
# or
ANTHROPIC_API_KEY=your_anthropic_api_key
```

---

# Project Structure

```
crewai-flows/
│
├── basic-flows/
│   └── basic_flow.py
│
├── unstructured-flow/
│   └── unstructured_flow.py
│
├── structured-flow/
│   └── structured_flow.py
│
├── conditional-flows/
│   ├── orflow.py
│   ├── and_flow.py
│   └── routerflow.py
│
└── crewai-with-flows/
    └── (flows that use Crews, e.g. poem_flow.py + crews/)
```

---

# 1. Basic Flow

**File:** `basic_flow.py`

This example shows the simplest flow structure:

* `@start` generates a random city using `litellm`
* The city name is passed into the next step using `@listen`
* The second step generates a fun fact about that city

Best for **simple linear multi-step tasks**
hows how to pass values automatically through the flow
<img width="787" height="648" alt="image" src="https://github.com/user-attachments/assets/76af7f00-4cbd-4f73-bb5b-2eca93335da9" />
<img width="825" height="455" alt="image" src="https://github.com/user-attachments/assets/7a1998b0-ac15-4207-9cfc-44bdc099e640" />

---

# 2. Unstructured Flow

**File:** `unstructured_flow.py`

Uses a free-form **state dictionary**.

* No strict schema
* Dynamically adds keys like `"message"` and `"counter"`
* Each method updates `self.state`

Useful when working with **unknown or flexible fields**
Good for experimental pipelines

Flow sequence:

1. Initialize state
2. Update with new values
3. Add final updates and print result

<img width="805" height="665" alt="image" src="https://github.com/user-attachments/assets/1102ee4b-a9e3-40ba-8e36-ff56bb123b65" />
<img width="847" height="268" alt="image" src="https://github.com/user-attachments/assets/6e065336-3d49-4ad4-9a68-1f19fd25f47e" />

---

# 3. Structured Flow

**File:** `structured_flow.py`

Uses **Pydantic BaseModel** for strongly-typed state:

```python
class ExampleState(BaseModel):
    counter: int = 0
    message: str = ""
```

Benefits:

* Validation
* Predictable structure
* Clean, maintainable code

Ideal for production
Enforces consistent state transitions
<img width="817" height="763" alt="image" src="https://github.com/user-attachments/assets/4d22f761-b5c4-4b6a-a434-a211f2496737" />
<img width="839" height="110" alt="image" src="https://github.com/user-attachments/assets/aefd0c89-1559-4624-bf8a-988c4d8ef8b0" />

---

# 4. Conditional Flows

This folder contains three advanced control-flow patterns.

## 4.1 OR Flow

**File:** `orflow.py`

* `logger()` runs when **either** `start_method` **or** `second_method` finishes
* Uses: `@listen(or_(...))`

Good for:

* Multiple possible triggers
* Monitoring flows
* Event-based pipelines

<img width="827" height="685" alt="image" src="https://github.com/user-attachments/assets/001d94e8-3dc7-4907-aa26-2ec354e1d5be" />

---

## 4.2 AND Flow

**File:** `and_flow.py`

* `logger()` runs **only when both methods have completed**
* Uses: `@listen(and_(...))`

Great for:

* Synchronization
* Waiting for parallel tasks
* Multi-condition checks

<img width="810" height="615" alt="image" src="https://github.com/user-attachments/assets/4136054f-0369-4e27-bb25-f5871b28385d" />

---

## 4.3 Router Flow

**File:** `routerflow.py`

A dynamic branching system:

* `@router` receives state and returns `"success"` or `"failed"`
* Two methods listen for these strings

Ideal for:

* Decision trees
* Error handling
* Conditional workflows
<img width="817" height="639" alt="image" src="https://github.com/user-attachments/assets/3156968d-5768-468d-b543-ed65f6016a0b" />

---

# 5. CrewAI Flow (Flow + Crew + Agents)

**Folder:** `crewai-with-flows/`

This example shows how to integrate a full **Crew** (Agents + Tools + Tasks) inside a Flow to generate a poem.

Flow behavior:

1. Flow starts
2. Calls a Crew to generate a poem
3. Returns the poem as the final output

This demonstrates the real power of CrewAI:
Combine flows *and* intelligent agents to create fully automated pipelines.

## Running Flows

There are two primary ways to run flows:

### Via CLI

```bash
crewai flow kickoff
```
<img width="2198" height="1254" alt="image" src="https://github.com/user-attachments/assets/b6c7b4f7-1eff-497e-a8fa-c1b30cb28ee1" />
<img width="2198" height="1254" alt="image" src="https://github.com/user-attachments/assets/2824ad98-4d62-4440-ba7c-0db06c534774" />
<img width="2258" height="1404" alt="image" src="https://github.com/user-attachments/assets/1554792e-f982-4302-ad4e-c14d5c8916a7" />

### To Visualise

```bash
crewai flow plot
```

<img width="2868" height="1702" alt="image" src="https://github.com/user-attachments/assets/f07dc33e-159c-49a0-97c6-f2df64cb7a78" />


---

# Summary

This repository covers the full spectrum of CrewAI flow capabilities:

| Flow Type             | What It Demonstrates        |
| --------------------- | --------------------------- |
| **Basic Flow**        | Simple sequences            |
| **Unstructured Flow** | Flexible state handling     |
| **Structured Flow**   | Typed state with Pydantic   |
| **OR Flow**           | Event-based branching       |
| **AND Flow**          | Multi-condition triggers    |
| **Router Flow**       | Dynamic routing logic       |
| **Crew + Flow**       | Intelligent agent workflows |

