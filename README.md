# LangGraph_1

A collection of hands-on LangGraph learning projects — each script is a self-contained example exploring a different core LangGraph concept: sequential pipelines, conditional routing, parallel branches with reducers, tool-calling agent loops, and human-in-the-loop workflows.

## Contents

| File | Concept | What it does |
|---|---|---|
| `states.py` | State design | Reference notes on the four ways to define LangGraph state — `TypedDict`, Pydantic `BaseModel`, a `dataclass`, and `MessagesState`. Not a runnable script; a quick-reference cheat sheet. |
| `sequential_workflow.py` | Sequential graph | A 3-stage linear pipeline (`editor → scriptwriter → translator`) that takes raw text, cleans it up, turns it into a punchy video-script hook, then localizes it into Hinglish. |
| `conditional_RAG.py` | Conditional routing + RAG | A CLI "College Assistant" that classifies a student's question as `academic`, `fee`, or `general`, retrieves context from the relevant PDF via FAISS + HuggingFace embeddings when needed, and answers accordingly. |
| `app.py` | Conditional routing + RAG (UI) | The Streamlit version of `conditional_RAG.py` — same classify → retrieve → respond graph, wrapped in a chat UI with programme selection, query-type badges, and chat history. |
| `parallel_reducers.py` | Parallel branches + reducers | Runs three independent LLM checks (toxicity, copyright/originality risk, cultural sensitivity) on a piece of text in parallel, merging their outputs into one shared state field via a custom reducer. |
| `iterative_tools.py` | Tool-calling agent loop | A writer/reviewer LinkedIn post generator: a writer agent (optionally searching the web via Tavily) drafts a post, a separate reviewer LLM grades it against a rubric, and the loop retries with feedback until approved or a max attempt count is reached. |
| `humaninloop.py` | Human-in-the-loop | Same writer/reviewer LinkedIn post concept as `iterative_tools.py`, but the reviewer is replaced by a human: the graph pauses with `interrupt()`, prints the draft, and waits for you to type `approved` or feedback before resuming. |

## Setup

1. **Clone and install dependencies**

   ```bash
   git clone https://github.com/Pushkar370/LangGraph_1.git
   cd LangGraph_1
   pip install -r requirements.txt
   ```

   `requirements.txt` covers the RAG/graph core (`langgraph`, `langchain`, `langchain-community`, `langchain-groq`, `langchain-huggingface`, `langchain-text-splitters`, `faiss-cpu`, `pypdf`, `python-dotenv`). Depending on which script you run, you'll also need:
   - `streamlit` — for `app.py`
   - `langchain-tavily` — for `iterative_tools.py`
   - `langchain-google-genai` and/or `langchain-openai` — for whichever writer LLM you configure in `iterative_tools.py`

   ```bash
   pip install streamlit langchain-tavily langchain-google-genai langchain-openai
   ```

2. **Set up environment variables**

   Create a `.env` file in the repo root with whichever keys the script you're running needs:

   ```
   GROQ_API_KEY=your_key_here
   GOOGLE_API_KEY=your_key_here      # if using Gemini models
   OPENAI_API_KEY=your_key_here      # if using OpenAI models
   TAVILY_API_KEY=your_key_here      # for web search in iterative_tools.py
   ```

3. **Run a script**

   ```bash
   # CLI college assistant
   python conditional_RAG.py

   # Streamlit college assistant
   streamlit run app.py

   # Sequential pipeline (editor -> scriptwriter -> translator)
   python sequential_workflow.py

   # Parallel safety-check analyzer
   python parallel_reducers.py

   # Iterative writer/reviewer agent loop
   python iterative_tools.py

   # Human-in-the-loop writer/reviewer
   python humaninloop.py
   ```

   `conditional_RAG.py` and `app.py` expect `academics_handbook.pdf` and `fee_structure.pdf` (included in the repo) in the working directory — these form the sample knowledge base for the RAG retrieval.

## Notes

- These are learning/practice projects built while working through core LangGraph patterns — state design, conditional edges, parallel fan-out with reducers, tool-calling loops, and interrupt-based human review — rather than a single production application.
- Model choices (Groq, Gemini, OpenAI) are easily swappable — each script instantiates its LLM(s) near the top of the file.
