# 🎓 Mini Answer Evaluator

> **Rubric-based student answer evaluation — powered by Groq LLM**

A lightweight AI-powered tool that evaluates student answers against subject-specific rubrics. Enter any exam question and a student's answer - the system auto-selects the best rubric, scores the answer out of 5, and provides structured feedback. Built with Groq's `llama-3.3-70b-versatile` and a clean Gradio web UI.

---

## 📋 Table of Contents

- [About the Project](#-about-the-project)
- [Demo](#-demo)
- [Tech Stack](#-tech-stack)
- [Features](#-features)
- [Architecture & RAG Pipeline](#-architecture--rag-pipeline)
- [Knowledge Base](#-knowledge-base)
- [Project Structure](#-project-structure)
- [Prerequisites](#-prerequisites)
- [Installation & Setup](#-installation--setup)
- [Environment Variables](#-environment-variables)
- [Running the Application](#-running-the-application)
- [Usage Guide](#-usage-guide)
- [Quick Topics Supported](#-quick-topics-supported)
- [Contributing](#-contributing)
- [License](#-license)

---

## 📖 About the Project

The **Mini Answer Evaluator** is designed to help students and teachers get instant, structured feedback on exam-style answers. It uses keyword-based rubric retrieval to match the question to the right subject rubric, then sends the question, answer, and rubric to a Groq LLM which returns marks, feedback, and justification — all as structured JSON.

An optional **comparison mode** lets you see the difference between rubric-guided scoring and holistic (no-rubric) scoring side by side.

---

## 🎬 Demo

### Physics — 2nd Law of Motion

**Question Input & Rubric Matching:**

![Physics Question with Answer](Mini%20Evaluator%20Project/Physics%202nd%20Law%20of%20Motion%20-%20Question%20with%20Answer.png)

**Evaluation Result — With & Without Rubric:**

![Physics With and Without Rubrics Answer Evaluation](Mini%20Evaluator%20Project/Physics%202nd%20Law%20of%20Motion%20-%20With%20and%20without%20Rubrics%20Answer%20Evaluation.png)

---

### Mathematics — Matrix

**Question Input & Rubric Matching:**

![Matrix Question with Answer](Mini%20Evaluator%20Project/Matrix%20in%20Mathematics%20-%20Question%20with%20Answer.png)

**Evaluation Result — With & Without Rubric:**

![Matrix With and Without Rubrics Answer Evaluation](Mini%20Evaluator%20Project/Matrix%20in%20Mathematics%20-%20With%20and%20without%20Rubrics%20Answer%20Evaluation.png)

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| LLM Inference | [Groq](https://groq.com) — `llama-3.3-70b-versatile` |
| Rubric Retrieval | Keyword-based matching (custom Python) |
| Web UI | [Gradio](https://gradio.app) |
| API Key Management | `python-dotenv` |
| Language | Python 3.8+ |
| Notebook | Jupyter Notebook (`.ipynb`) |

---

## ✨ Features

- 🔍 **Automatic Rubric Retrieval** — keyword matching selects the right rubric from 6 subjects; falls back to a generic rubric for unrecognised topics.
- 🤖 **LLM-Powered Grading** — Groq's Llama 3.3 70B returns structured JSON: marks awarded, max marks, feedback, and justification.
- 📊 **Comparison Mode** — compare rubric-guided scoring vs. holistic (no-rubric) scoring side by side.
- 🖥️ **Gradio Web UI** — clean interface with pre-loaded example questions and a visual score bar.
- 💻 **Interactive CLI Mode** — run directly in the notebook terminal without opening a browser.
- ⚡ **Fast Inference** — Groq's LPU hardware delivers near-instant responses.

---

## 🏗 Architecture & RAG Pipeline

```
User Input
  │
  ▼
┌─────────────────────────┐
│   Question Tokeniser    │  Lowercases & extracts words
└────────────┬────────────┘
             │
             ▼
┌─────────────────────────┐
│   Rubric Retriever      │  Keyword overlap scoring
│   (retrieve_rubric)     │  across 6 subject rubrics
└────────────┬────────────┘
             │  Best matching rubric (or fallback)
             ▼
┌─────────────────────────┐
│   Prompt Builder        │  Combines question +
│                         │  student answer + rubric
└────────────┬────────────┘
             │
             ▼
┌─────────────────────────┐
│   Groq LLM              │  llama-3.3-70b-versatile
│   (evaluate_answer)     │  temperature = 0.2
└────────────┬────────────┘
             │  JSON: marks, feedback, justification
             ▼
┌─────────────────────────┐
│   Gradio UI / CLI       │  Displays results with
│   (display_result)      │  score bar and comparison
└─────────────────────────┘
```

---

## 📚 Knowledge Base

The system has 7 built-in rubric chunks:

| Subject | Class | Max Marks | Criteria |
|---|---|---|---|
| Physics | 12 | 5 | Definition, formula + SI units, term explanation, example |
| Mathematics | 12 | 5 | Method chosen, steps shown, intermediate calcs, final answer |
| English | 10 | 5 | Interpretation, key points, clarity, language quality |
| Chemistry | 12 | 5 | Concept/definition, equation, mechanism, real-world example |
| Biology | 12 | 5 | Definition, process description, diagram labelling, significance |
| History / SST | 10 | 5 | Facts & dates, cause-effect, key personalities, conclusion |
| General (Fallback) | Any | 5 | Relevance, coverage, clarity, language |

---

## 📁 Project Structure

```
Mini_Evaluator_Answer_Project/               ← GitHub Repository
│
├── README.md                                 ← This file (root level)
│
└── Mini Evaluator Project/                   ← Project subfolder
    ├── Mini Answer Evaluator Project .ipynb  ← Main notebook (all code)
    ├── .env.example                          ← API key template
    ├── .gitignore                            ← Git exclusions
    ├── Physics 2nd Law of Motion - Question with Answer.png
    ├── Physics 2nd Law of Motion - With and without Rubrics Answer Evaluation.png
    ├── Matrix in Mathematics - Question with Answer.png
    └── Matrix in Mathematics - With and without Rubrics Answer Evaluation.png
    └── .env file(Not uploaded here)
```

---

## ✅ Prerequisites

- Python 3.8 or higher
- `pip` package manager
- A free [Groq API key](https://console.groq.com/keys)
- Jupyter Notebook or VS Code with Jupyter extension

---

## 🚀 Installation & Setup

### 1. Clone the repository

```bash
git clone https://github.com/your-username/Mini_Evaluator_Answer_Project.git
cd Mini_Evaluator_Answer_Project/Mini\ Evaluator\ Project
```

### 2. Create a virtual environment (recommended)

```bash
python -m venv venv
source venv/bin/activate       # macOS / Linux
venv\Scripts\activate          # Windows
```

### 3. Install dependencies

```bash
pip install groq python-dotenv gradio
```

---

## 🔑 Environment Variables

Create a `.env` file in the project folder:

```bash
cp .env.example .env
```

Open `.env` and add your key:

```
GROQ_API_KEY=gsk_your_actual_key_here
```

> Get a free key at 👉 [console.groq.com/keys](https://console.groq.com/keys)
> ⚠️ **Never commit your `.env` file to GitHub.** It is already listed in `.gitignore`.

---

## ▶️ Running the Application

### Option 1 — Jupyter Notebook

```bash
jupyter notebook "Mini Answer Evaluator Project .ipynb"
```

Run all cells top to bottom. The Gradio app launches automatically in the last cell.

### Option 2 — VS Code

1. Open the project folder in VS Code
2. Click on the `.ipynb` file
3. Click **"Run All"** at the top

### Option 3 — Google Colab

1. Upload the `.ipynb` to [colab.research.google.com](https://colab.research.google.com)
2. Add your API key by running:
   ```python
   import os
   os.environ["GROQ_API_KEY"] = "gsk_your_key_here"
   ```
3. Run all cells

---

## 📖 Usage Guide

### Gradio Web UI

After running the last cell, open the printed local URL (e.g. `http://127.0.0.1:7860`) in your browser.

1. Enter your **exam question** in the Question box
2. Enter the **student's answer** in the Answer box
3. Optionally tick **"Compare with / without rubric"**
4. Click **🚀 Evaluate**
5. View the matched rubric, score, feedback, and justification

### Interactive CLI

Run the **Interactive Mode** cell in the notebook and follow the prompts:

```
Enter the question       : What is Newton's 2nd law?
Enter the student answer : Force equals mass times acceleration...
Compare with/without rubric? (y/n): y
```

### Programmatic Usage

```python
rubric = retrieve_rubric("Explain Newton's second law of motion.")

result = evaluate_answer(
    question="Explain Newton's second law of motion.",
    student_answer="F = ma. Force equals mass times acceleration.",
    rubric=rubric,
    use_rubric=True
)

print(result)
# {
#   "marks_awarded": 4,
#   "max_marks": 5,
#   "feedback": "Good answer. Explain each term in the formula for full marks.",
#   "justification": "Definition and formula correct; term explanation incomplete."
# }
```

---

## 🎯 Quick Topics Supported

| Example Question | Matched Rubric |
|---|---|
| Explain Newton's 2nd law and state its formula | Physics (Class 12) |
| What is matrix in mathematics? | Mathematics (Class 12) |
| Describe the theme of 'The Road Not Taken' | English (Class 10) |
| What is the role of enzymes in digestion? | Biology (Class 12) |
| What were the causes of the French Revolution? | History (Class 10) |
| Explain oxidation and reduction reactions | Chemistry (Class 12) |
| What are the main features of cloud computing? | General (Fallback) |

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -m "Add my feature"`
4. Push to the branch: `git push origin feature/my-feature`
5. Open a Pull Request

---

## 📄 License

MIT License — free to use, modify, and distribute.

---

## 🙏 Acknowledgements

- [Groq](https://groq.com) — ultra-fast LLM inference via LPU hardware
- [Meta Llama 3.3](https://ai.meta.com/blog/meta-llama-3/) — the underlying language model
- [Gradio](https://gradio.app) — easy-to-build ML web interfaces
