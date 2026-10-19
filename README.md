# Dual-Persona AI Agent with LangGraph & Local Llama 3.2

An intelligent conversational agent built using **LangGraph**, **LangChain**, and a locally hosted **Llama 3.2** model via **Ollama**. The system inspects incoming user prompts, categorizes intent using structured output classification, and conditionally routes the query to either a compassionate therapist persona or a direct, fact-focused logical persona.

Video Tutorial Refrence: https://www.youtube.com/watch?v=1w5cCXlh7JQ

---

## Architecture & Workflow

The conversation flows through a directed state graph built with LangGraph:

```text
[START]
   │
   ▼
[Classifier Node] ────► Evaluates intent (Emotional vs. Logical)
   │
   ▼
 [Router Node]   ────► Evaluates state and determines the next target
   │
   ├───► (next: "therapist") ──► [Therapist Agent] ──► [END]
   │
   └───► (next: "logical")   ──► [Logical Agent]   ──► [END]

```

1. **Classifier Node (`classify_message`)**: Takes the latest user message and utilizes Pydantic structured output (`MessageClassfifier`) to classify the tone as either `emotional` or `logical`.


2. **Router Node (`router`)**: Inspects the assigned `message_type` from graph state and outputs the next node destination (`therapist` or `logical`).


3. **Conditional Edge**: Directs execution to the selected agent based on the router's resolution.


4. **Specialized Agent Nodes**:
* **`therapist_agent`**: Prioritizes emotional validation, empathy, and open-ended processing.


* **`logical_agent`**: Strips emotional fluff to provide concise, evidence-based, factual answers.





---

## Features

* **100% Local Inference**: Runs entirely offline using Ollama without sending conversational data to third-party cloud APIs.


* **Structured Output**: Employs Pydantic schemas to enforce deterministic categorical classification from the local LLM.


* **Stateful Execution**: Retains chat history throughout the interactive loop using LangGraph's state management primitives.



---

## Prerequisites

1. **Python**: Version 3.10 or higher.
2. **Ollama**: Installed and running locally.
* Download from [ollama.com](https://ollama.com).
* Pull the Llama 3.2 model:
```bash
ollama pull llama3.2

```


* Ensure the Ollama service is active (`ollama serve`).



---

## Installation

1. **Clone the repository:**
```bash
git clone <your-repo-url>
cd <your-repo-folder>

```


2. **Create and activate a virtual environment:**
```bash
python3 -m venv .venv
source .venv/bin/activate
# On Windows: .venv\Scripts\activate

```


3. **Install dependencies:**
```bash
pip install langgraph langchain langchain-ollama pydantic python-dotenv typing-extensions

```


4. **Environment Configuration (Optional):**
Create a `.env` file in the root directory if you need to specify custom environment variables:


```env
# Defaults to local Ollama instance on port 11434
OLLAMA_BASE_URL=http://localhost:11434

```



---

## Usage

Start the interactive CLI session by running `main.py`:

```bash
python main.py

```

### Example Interaction

```text
Type a Message: I've been feeling overwhelmed with work and don't know how to handle the pressure.
Assistant: It sounds like you are carrying a heavy burden right now. It is completely natural to feel overwhelmed when demands pile up. What part of the workload feels the most daunting to you at this moment?

Type a Message: What is the computational complexity of quicksort in the worst case?
Assistant: The worst-case time complexity of quicksort is O(n^2). This happens when the chosen pivot consistently splits the array into unbalanced partitions, such as when the array is already sorted and the first or last element is chosen as the pivot.

Type a Message: exit
Bye

```

---

## Project Structure

```text
.
├── .env                # Optional environment variables
├── main.py             # Core LangGraph pipeline, node definitions, and chat loop
└── README.md           # Project documentation

```