# ai_os_tutorAssistant
# OS Socratic Tutor

An AI-powered Operating Systems tutor that teaches using the **Socratic method** — it never gives direct answers, always guides you to think.

Built with Google Gemini (free tier), Sentence Transformers, and ChromaDB.

---

## How it works

```
generate_os_dataset_gemini.py
        │
        │  asks Gemini to write 304 student-teacher dialogues
        ▼
os_socratic_dataset.jsonl   ← your training data on disk
        │
        │  loaded + embedded into ChromaDB at startup
        ▼
use_as_fewshot_gemini.py
        │
        │  student asks question → find 3 similar examples
        │  → inject as style guide → Gemini responds
        ▼
  Socratic tutor chat (terminal)
```

Every answer follows:
- **ANALOGY** — a real-world comparison
- **FACT** — the precise technical concept
- **QUESTION** — a probing question to make you think

---

## Quickstart

### 1. Clone and install

```bash
git clone https://github.com/YOUR_USERNAME/os-tutor.git
cd os-tutor
pip install -r requirements.txt
```

### 2. Get a free Gemini API key

Go to [aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey) — no billing needed.

### 3. Set up your environment

```bash
cp .env.example .env
# Open .env and paste your key
```

### 4. Generate the dataset

```bash
python scripts/generate_dataset.py
```

Takes ~15 minutes. Produces `data/os_socratic_dataset.jsonl` with ~300 examples across 38 OS topics.

Want to verify quality first? Run 3 sample examples:

```bash
python scripts/preview_samples.py
```

### 5. Chat with the tutor

```bash
python scripts/tutor.py
```

Then ask anything:
```
You: what is a deadlock?
You: i dont understand semaphores
You: why does round robin even matter
```

Type `quit` to exit.

---

## Project structure

```
os-tutor/
├── README.md
├── requirements.txt          # all dependencies
├── .env.example              # template for your API key
├── .gitignore                # keeps secrets + data out of git
│
├── config/
│   └── topics.py             # all 38 OS topics + variations
│
├── scripts/
│   ├── generate_dataset.py   # creates os_socratic_dataset.jsonl
│   ├── preview_samples.py    # quick 3-example quality check
│   └── tutor.py              # interactive Socratic chat
│
└── data/
    └── .gitkeep              # folder tracked, data files ignored
```

---

## Topics covered

| Category | Topics |
|---|---|
| Process Management | process vs thread, context switching, fork/exec, zombie processes, IPC |
| Scheduling | FCFS, Round Robin, SJF, Priority, Multilevel feedback queue |
| Synchronization | race conditions, mutex, semaphores, monitors, deadlock, Banker's algorithm |
| Memory | paging, segmentation, virtual memory, page replacement, TLB, thrashing |
| File Systems | file allocation, inodes, disk scheduling, RAID, I/O buffering |
| OS Concepts | kernel vs user mode, system calls, multiprogramming |

---

## Tech stack

| Component | Tool | Why |
|---|---|---|
| LLM | Gemini 2.0 Flash | Free tier, fast, good quality |
| Embeddings | `all-MiniLM-L6-v2` | Lightweight, runs locally |
| Vector DB | ChromaDB | In-memory, no setup needed |
| Dataset format | JSONL | Standard for LLM fine-tuning |

---

## For your presentation

This project demonstrates:
- **RAG** (Retrieval-Augmented Generation) — finding relevant context before answering
- **Few-shot prompting** — teaching style by example, not by fine-tuning
- **Semantic search** — finding similar questions by meaning, not keyword matching
- **Synthetic data generation** — using an LLM to build a domain-specific dataset

The same architecture used in production RAG systems at scale.
