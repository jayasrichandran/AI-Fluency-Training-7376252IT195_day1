# AI Fluency – Day 1 Analysis

## 1. Purpose of the Experiment

The purpose of this experiment was to understand the difference between:

* An LLM-based chatbot
* A rule-based workflow
* An AI agent

The same type of course-fee information was used to observe how each approach handles factual questions, calculations, and a new problem that was not explicitly defined in the workflow.

---

## 2. Data Used

The project uses three course-fee values:

| Course |     Fee |
| ------ | ------: |
| CS101  | ₹12,000 |
| AI202  | ₹18,000 |
| DS303  | ₹15,000 |

These values are used by the workflow and the agent tools.

---

# 3. System 1 – LLM Chatbot

## Approach

The chatbot directly interacts with the language model.

The model used was:

```text
Provider: Groq
Model: openai/gpt-oss-20b
```

The chatbot does not automatically have the course-fee data available as a tool or knowledge source.

## Observed Results

### Question 1

```text
What is the fee for AI202?
```

The chatbot did not provide the expected ₹18,000 value.

Instead, it asked for information about the institution or program.

### Question 2

```text
What is the total fee for CS101 and AI202 after a 10% scholarship?
```

The chatbot again requested the original fee values instead of retrieving them from the project data.

### Question 3

```text
Is DS303 more expensive than CS101, and by how much?
```

The chatbot could not determine the answer because the required fee information was not provided to it.

### Question 4

```text
Write a two-line welcome message for new AI students.
```

The chatbot successfully generated a suitable response because this task did not require access to the course-fee data.

## Analysis

This demonstrates an important point:

An LLM can generate natural-language responses, but having an LLM alone does not automatically give it access to the application's private or structured data.

---

# 4. System 2 – Rule-Based Workflow

## Approach

The workflow uses predefined rules for course-fee questions.

It can retrieve known fees and perform specific calculations.

## Observed Results

### AI202 Fee

```text
Fee for AI202: ₹18,000
```

The workflow successfully answered the question because the corresponding rule was available.

### Scholarship Calculation

For CS101 and AI202:

```text
CS101 = ₹12,000
AI202 = ₹18,000

Total = ₹30,000
```

After a 10% scholarship:

```text
₹30,000 × 0.90
= ₹27,000
```

The workflow successfully returned:

```text
Total fee: Rs. 27,000
```

### DS303 vs CS101

The workflow returned:

```text
Sorry, I do not have a rule for this type of question.
```

This shows that the workflow can only perform the operations that have been explicitly programmed.

### Welcome Message

The workflow returned:

```text
Sorry, I can only answer questions about course fees.
```

Again, this behavior is expected because the workflow was designed for course-fee tasks.

---

# 5. System 3 – AI Agent

## Approach

The AI agent has access to tools.

The main tool used for course information is:

```text
get_course_fee(course_code)
```

The project also contains a calculator tool.

The agent can determine which tools it needs and use them as part of solving a question.

---

## Example 1 – Retrieve a Course Fee

Question:

```text
What is the fee for AI202?
```

The agent performed:

```text
step 1:
get_course_fee({'course_code': 'AI202'})
→ 18000
```

It then generated:

```text
The fee for AI202 is ₹18,000.
```

---

## Example 2 – Multiple Tool Calls

Question:

```text
What is the total fee for CS101 and AI202 after a 10% scholarship?
```

The agent performed:

```text
step 1:
get_course_fee({'course_code': 'CS101'})
→ 12000

step 2:
get_course_fee({'course_code': 'AI202'})
→ 18000
```

The combined amount was:

```text
₹12,000 + ₹18,000
= ₹30,000
```

The 10% scholarship was:

```text
₹30,000 × 10%
= ₹3,000
```

Final amount:

```text
₹30,000 - ₹3,000
= ₹27,000
```

The agent returned the correct result.

---

## Example 3 – Comparison

Question:

```text
Is DS303 more expensive than CS101, and by how much?
```

The agent performed:

```text
step 1:
get_course_fee({'course_code': 'DS303'})
→ 15000

step 2:
get_course_fee({'course_code': 'CS101'})
→ 12000
```

Difference:

```text
₹15,000 - ₹12,000
= ₹3,000
```

The agent therefore generated the comparison using the retrieved values.

---

# 6. Challenge Analysis

The challenge was:

```text
I can pay Rs. 30,000.
Which two courses can I take together within this budget?
```

## Workflow

The workflow returned:

```text
Sorry, I can only answer questions about course fees.
```

The reason is that the workflow did not contain a rule for finding course combinations under a budget.

## Agent

The agent performed:

```text
step 1:
get_course_fee(CS101)
→ ₹12,000

step 2:
get_course_fee(AI202)
→ ₹18,000
```

It identified:

```text
₹12,000 + ₹18,000
= ₹30,000
```

Therefore, the agent returned CS101 and AI202 as a valid combination within the given budget.

## Important Observation

The agent's result demonstrates tool use and task execution beyond a single predefined question.

However, the current implementation should not be interpreted as a general optimization algorithm. The agent's ability to find combinations depends on the courses and logic available to it.

---

# 7. Tools Analysis

The `tools.py` file was tested independently.

### Course Fee Tool

```text
get_course_fee('ai202')
→ 18000
```

This tool provides the fee associated with a course code.

### Calculator Tool

```text
calculator('(12000 + 18000) * 0.9')
→ 27000.0
```

It performs arithmetic separately from the language model.

Another calculation:

```text
calculator('15000 - 12000')
→ 3000
```

This shows how an agent can delegate numerical operations to a dedicated tool.

---

# 8. Comparison

| Feature                     | LLM Chatbot                  | Rule-Based Workflow | AI Agent |
| --------------------------- | ---------------------------- | ------------------- | -------- |
| Natural-language generation | Yes                          | Limited             | Yes      |
| Access to course-fee tool   | No                           | Built-in rules      | Yes      |
| Fixed rules required        | No                           | Yes                 | No       |
| Multiple tool calls         | No                           | Predefined          | Yes      |
| Course comparison           | Not with current data access | Only if rule exists | Yes      |
| Scholarship calculation     | Not with current data access | Yes, if rule exists | Yes      |
| New budget challenge        | No                           | No                  | Yes      |
| Tool-based execution        | No                           | No                  | Yes      |

---

# 9. Main Learning

The experiment demonstrates three different levels of implementation.

### LLM

```text
Question
   ↓
LLM
   ↓
Response
```

The model is mainly responsible for generating the response.

### Workflow

```text
Question
   ↓
Predefined Rule
   ↓
Known Operation
   ↓
Response
```

The developer determines the allowed flow.

### Agent

```text
Question
   ↓
Agent
   ↓
Select Tool
   ↓
Tool Result
   ↓
Additional Tool / Reasoning
   ↓
Final Response
```

The agent can interact with tools to complete a task.

---

# 10. Problems Encountered During Development

During the environment setup, the project initially produced dependency errors.

### Missing `python-dotenv`

```text
ModuleNotFoundError: No module named 'dotenv'
```

It was resolved with:

```powershell
python -m pip install python-dotenv
```

### Missing `openai`

```text
ModuleNotFoundError: No module named 'openai'
```

It was resolved with:

```powershell
python -m pip install openai
```

These errors reinforced the importance of installing dependencies inside the active virtual environment.

---

# 11. Final Understanding

The main takeaway from Day 1 is that an AI agent is not simply an LLM chatbot.

The three systems have different responsibilities:

```text
LLM Chatbot
→ Generates responses

Workflow
→ Follows predefined steps

AI Agent
→ Uses an LLM together with tools to perform tasks
```

The project therefore provides a practical comparison of chatbot behavior, deterministic workflows, and tool-using agent behavior.

---

# 12. Future Improvements

The project can be extended by:

1. Adding more course data.
2. Creating a tool to list all available courses.
3. Creating a dedicated budget-optimization tool.
4. Supporting multiple valid course combinations.
5. Adding conversation memory.
6. Adding error handling for invalid course codes.
7. Recording tool calls for easier debugging.
8. Building a simple user interface.
9. Testing the agent with more complex tasks.
10. Adding automated evaluation of chatbot, workflow, and agent responses.
