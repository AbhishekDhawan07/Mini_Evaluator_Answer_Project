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
- [Improvements & Future Scope](#-improvements--future-scope)
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

## 🔮 Improvements & Future Scope

The current version is a working MVP. Here are meaningful improvements that can be made:

### 🧠 Smarter Rubric Retrieval
| Current | Improvement |
|---|---|
| Keyword overlap counting | Use **semantic similarity** (embeddings) for more accurate subject matching |
| Fixed 7 rubrics | Allow **custom rubric upload** via the UI (JSON or CSV) |
| Single rubric matched | Support **multi-subject questions** that span topics |

### 📊 Better Evaluation
- **Marks out of custom values** — currently fixed at 5; allow teachers to set 2, 3, 10, etc.
- **Part-wise evaluation** — break multi-part questions (a, b, c) into individual scored sections
- **Confidence score** — have the LLM return how confident it is in the marks it awarded
- **Model answer support** — let teachers provide a reference answer for more accurate grading

### 🖥️ UI & UX Enhancements
- **Batch evaluation** — upload a CSV of questions + answers and evaluate all at once
- **Export results** — download evaluation results as PDF or Excel report
- **History tab** — save and review past evaluations within the session
- **Dark mode** — Gradio theme toggle for better readability

### 🔐 Security & Deployment
- **User authentication** — login system so teachers can have private sessions
- **Deploy to Hugging Face Spaces** — make it publicly accessible with one click
- **Deploy to Streamlit Cloud** — alternative free hosting option
- **Rate limiting** — prevent API key abuse in shared deployments

### 🗄️ Data & Storage
- **Database integration** — store evaluations in SQLite or PostgreSQL for analytics
- **Student performance tracking** — track a student's scores over multiple attempts
- **Analytics dashboard** — visualise class performance across subjects

### 🤖 Model & Performance
- **Support multiple LLM providers** — add OpenAI, Gemini, or Ollama (local) as alternatives to Groq
- **Fine-tuned model** — train a smaller model specifically on exam answer evaluation
- **Caching** — cache identical question+answer pairs to reduce API calls and costs
- **Streaming responses** — show feedback word-by-word as it generates for faster perceived speed

### 📱 Platform Expansion
- **REST API** — expose evaluation as an API endpoint so it can be integrated into school portals
- **Mobile-friendly UI** — responsive design for tablet/phone use in classrooms
- **WhatsApp / Telegram bot** — students submit answers via chat and get instant feedback

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
