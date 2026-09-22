# AI Fluency – Day 1: Chatbot, Workflow & AI Agent

## Overview

This project is part of my AI Fluency learning journey.

The Day 1 project demonstrates the difference between three approaches to solving the same set of tasks:

1. **LLM-based Chatbot**
2. **Rule-Based Workflow**
3. **AI Agent with Tools**

The project also includes a challenge that compares how a fixed workflow and an AI agent handle a new budget-based question.

---

## Objective

The main objective is to understand how an AI system progresses from a simple LLM chatbot to a tool-using AI agent.

The project demonstrates:

* Direct interaction with an LLM
* Rule-based decision making
* Tool calling
* Multi-step reasoning
* Dynamic task execution
* Comparison between workflows and agents

---

## Systems Implemented

### System 1 – LLM Chatbot

The chatbot directly sends the questions to the language model.

**Provider:** Groq
**Model:** `openai/gpt-oss-20b`

The model can generate natural-language responses, but it does not automatically have access to the course-fee data used by the other systems.

For example, when asked:

> What is the fee for AI202?

the chatbot responded that it needed information about the institution or program.

For the welcome-message question, however, it successfully generated a suitable response because that task did not require external course data.

---

### System 2 – Rule-Based Workflow

The workflow uses predefined rules and course-fee data.

Course fees used in the project:

| Course |     Fee |
| ------ | ------: |
| CS101  | ₹12,000 |
| AI202  | ₹18,000 |
| DS303  | ₹15,000 |

The workflow successfully handles predefined fee-related operations.

Example:

```text
Q: What is the fee for AI202?

A: Fee for AI202: Rs. 18,000
```

It can also calculate a scholarship:

```text
CS101 + AI202
= ₹12,000 + ₹18,000
= ₹30,000

After 10% scholarship
= ₹30,000 × 0.90
= ₹27,000
```

However, the workflow is limited by the rules that have been explicitly implemented.

---

### System 3 – AI Agent

The AI agent can use tools to obtain information and perform calculations.

The agent used the following tool:

```text
get_course_fee(course_code)
```

It can also use a calculator tool for mathematical operations.

For example:

```text
get_course_fee('AI202') -> 18000
```

and:

```text
calculator('(12000 + 18000) * 0.9') -> 27000.0
```

The agent can decide which tools are needed and execute multiple steps to answer a question.

---

## Agent Execution Example

For the question:

> What is the total fee for CS101 and AI202 after a 10% scholarship?

The agent performed:

```text
Step 1:
get_course_fee(CS101)
→ ₹12,000

Step 2:
get_course_fee(AI202)
→ ₹18,000
```

It then calculated:

```text
₹12,000 + ₹18,000 = ₹30,000

10% scholarship = ₹3,000

Final fee = ₹27,000
```

---

## Challenge

The challenge question was:

> I can pay Rs. 30,000. Which two courses can I take together within this budget?

### Workflow Result

```text
Sorry, I can only answer questions about course fees.
```

The workflow could not handle this new type of question because the required rule was not implemented.

### Agent Result

```text
You can take CS101 (₹12,000) and AI202 (₹18,000)
together, which totals ₹30,000.
```

The agent retrieved the relevant course fees and determined a valid combination within the budget.

---

## Project Structure

```text
day_1/
│
├── chatbot.py
├── workflow.py
├── agent.py
├── challenge.py
├── tools.py
├── config.py
├── requirements.txt
├── README.md
├── analysis.md
├── .gitignore
│
└── output/
    └── project output files
```

---

## Technologies Used

* Python
* Groq API
* `openai/gpt-oss-20b`
* Python Virtual Environment
* `python-dotenv`
* Git
* GitHub

---

## Environment Setup

### 1. Create a virtual environment

```powershell
python -m venv .venv
```

### 2. Activate the environment

```powershell
.\.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```powershell
python -m pip install -r requirements.txt
```

### 4. Configure the API key

Create a `.env` file:

```text
GROQ_API_KEY=your_api_key_here
```

The `.env` file should remain private and must not be uploaded to GitHub.

---

## Running the Project

Run each system separately:

### Chatbot

```powershell
python chatbot.py
```

### Rule-Based Workflow

```powershell
python workflow.py
```

### AI Agent

```powershell
python agent.py
```

### Challenge

```powershell
python challenge.py
```

### Tools

```powershell
python tools.py
```

---

## Key Learning

The main lesson from this project is the difference between a chatbot, a workflow, and an agent.

A chatbot mainly generates responses using an LLM.

A workflow follows predefined steps and rules.

An agent can select and use tools dynamically to complete a task.

This project demonstrates that distinction using the same course-fee data and progressively more capable approaches.

---

## Future Improvements

Possible improvements include:

* Adding more courses
* Supporting multiple budget combinations
* Adding more tools
* Adding conversation memory
* Adding input validation
* Improving error handling
* Creating a web interface
* Adding more complex agent tasks
* Logging agent tool calls and results
