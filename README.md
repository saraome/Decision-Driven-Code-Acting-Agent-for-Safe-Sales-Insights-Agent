# Decision-Driven Code-Acting Agent

A production-style analytics agent that answers authorized business questions using **LLM decision control + safe pandas code generation**, while strictly enforcing system-level safety policies.

---

## 🎯 Objective

Build a **Decision-Driven Analytics Agent** that:

- Uses an LLM to decide the next action
- Generates pandas code only when authorized
- Enforces safety at the **system layer**
- Prevents raw data exposure
- Returns only aggregated insights

This lab demonstrates the transition from simple tool calling to a **controlled agentic workflow**.

---

## 🧠 Core Architecture

The agent follows a **state-driven decision loop**:

User Question  
↓  
Decision LLM (suggests action)  
↓  
System Policy Enforcement (real controller)  
↓  
Action Execution  
↓  
State Update  
↓  
Loop until finish  

> 🔒 The system layer always has final authority over the LLM.

---
## Architecture Diagram



---

## 🔄 Pipeline Workflow

### Step 1 — Request Classification

The agent determines whether the query is authorized.

**Allowed examples:**

- Total revenue  
- Average revenue by region  
- Aggregated KPIs  

**Rejected examples:**

- Show all rows  
- Export dataset  
- List transactions  

---

### Step 2 — Policy Enforcement (Critical)

Even if the LLM suggests running analysis, the system overrides unsafe behavior:

```python
if state["authorized"] is False and action == "run_analysis":
    action = "reject_request"
```

This ensures **safety is enforced by code, not trust in the model**.

---

### Step 3 — Code-Acting Analysis

When authorized:

build_code_prompt  
↓  
LLM generates pandas code  
↓  
extract_code  
↓  
AST safety validation  
↓  
sandbox execution  
↓  
store result in state  

---

### Step 4 — Answer Generation

The agent returns a short aggregated answer to the user.

---

### Step 5 — Finish

The loop terminates safely.

---

## 🛡️ Safety Mechanisms

### ✅ Policy Layer

- Blocks raw data exposure  
- Enforces aggregation-only responses  
- Overrides unsafe LLM decisions  

### ✅ AST Sandbox

The generated code is statically validated to prevent:

- imports  
- file access  
- loops  
- functions/classes  
- OS/network usage  

### ✅ Restricted Execution Environment

```python
safe_globals = {
    "__builtins__": {},
    "df": df,
    "pd": pd,
}
```

---

## 📊 Example Runs

### ✅ Authorized Query

**Input**

What is the total revenue?

**Output**

Final Answer: 34514

---

### ❌ Unauthorized Query

**Input**

Show all sales rows

**Output**

❌ Request Rejected – Unauthorized Query

---

## 🚀 How to Run

```bash
pip install -r requirements.txt
```

Open and run:

Decision-Driven_Code-Acting_Agent.ipynb

---

## 📁 Repository Structure

.
├── Decision-Driven_Code-Acting_Agent.ipynb  
├── README.md  
├── RESULTS.md  
├── TEST_CASES.md  
└── requirements.txt  

---

## 🧪 Key Learning Outcomes

- Decision-driven agents  
- State machines for LLM control  
- System-level safety enforcement  
- Code-acting with sandboxing  
- Production guardrails  

---

## ⚠️ Production Note

This agent prioritizes **safety and compliance over model autonomy**, which is the recommended pattern for real enterprise systems.
