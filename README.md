# 🔍 Fraud Policy Assistant
### RAG-powered Compliance Q&A System

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://python.org)
[![LangChain](https://img.shields.io/badge/LangChain-latest-green.svg)](https://langchain.com)
[![HuggingFace](https://img.shields.io/badge/HuggingFace-Spaces-yellow.svg)](https://huggingface.co)
[![License](https://img.shields.io/badge/License-MIT-red.svg)](LICENSE)
[![Colab](https://colab.research.google.com/assets/colab-badge.svg)](YOUR_COLAB_LINK_HERE)

> *"My friend is a Compliance Officer at a US-based finance company.
> He had hundreds of PDFs to read every single day. It was exhausting him.
> I built this to solve that problem."*

---

## 🎯 What Problem Does This Solve?

Compliance officers spend hours searching through hundreds of regulatory
documents to answer simple questions like:

- *"What are the red flags for securities fraud?"*
- *"What does the SEC say about Ponzi schemes?"*
- *"What are the whistleblower protection rules?"*

**This system answers those questions in under 10 seconds — with citations.**

---

---
## 🧠 How It Works
User Question


 Embed Question   ← Sentence Transformer


 Search ChromaDB   ← Find top-5 relevant chunks

 Build Prompt     ← Question + Retrieved Context

 Qwen 2.5 LLM    ← Generate cited answer














**The technique is called RAG — Retrieval Augmented Generation.**

Instead of the AI guessing from memory, it retrieves real document
chunks first — then answers only from those chunks.
This eliminates hallucination.

---

## 🛠️ Tech Stack

| Component | Tool | Purpose |
|---|---|---|
| PDF Loading | LangChain | Load and parse compliance PDFs |
| Text Chunking | RecursiveCharacterTextSplitter | Split docs into 500-word chunks |
| Embeddings | Sentence Transformers (all-MiniLM-L6-v2) | Convert text to semantic vectors |
| Vector Database | ChromaDB | Store and search embeddings |
| LLM | Qwen 2.5 3B Instruct | Generate answers from context |
| Quantization | BitsAndBytes (4-bit NF4) | Run LLM on free GPU |
| UI | Gradio | Simple web interface |
| GPU | Google Colab (T4) | Free compute |
| Deployment | HuggingFace Spaces | Free hosting |

**Total cost: ₹0**

---

## 📁 Project Structure
fraud-policy-assistant/
│
├── app.py                  # Main RAG pipeline + Gradio UI
├── requirements.txt        # All dependencies
├── README.md               # This file
│
├── notebooks/
│   └── fraud_rag.ipynb     # Full Colab notebook
│
├── docs/                   # PDF documents (add your own)
│   └── .gitkeep
│
├── chroma_db/              # Vector store (auto-generated)
│   └── .gitkeep
│
└── assets/
└── demo.gif            # Demo screenshot/gif

---

## 🚀 Quick Start

### Option 1 — Run on Google Colab (Recommended)

Click the badge below — everything is pre-configured:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](YOUR_COLAB_LINK_HERE)

### Option 2 — Run Locally

**Step 1 — Clone the repo**
```bash
git clone https://github.com/YOUR_USERNAME/fraud-policy-assistant.git
cd fraud-policy-assistant
```

**Step 2 — Install dependencies**
```bash
pip install -r requirements.txt
```

**Step 3 — Add your PDFs**
```bash
# Drop your compliance PDFs into the docs/ folder
cp your_documents.pdf docs/
```

**Step 4 — Run**
```bash
python app.py
```

Open `http://localhost:7860` in your browser.

---

## 📄 Data Sources

This project uses free, publicly available compliance documents:

| Document | Source |
|---|---|
| SEC 2024 Financial Report | [SEC.gov](https://www.sec.gov/files/sec-2024-agency-financial-report.pdf) |
| SEC 2026 Exam Priorities | [SEC.gov](https://www.sec.gov/files/2026-exam-priorities.pdf) |
| FY2025 Whistleblower Report | [SEC.gov](https://www.sec.gov/files/fy25-annual-whistleblower-report.pdf) |
| Regulation S-P Compliance Guide | [SEC.gov](https://www.sec.gov/files/rules/final/2024/regulation-s-p-small-entity-compliance-guide.pdf) |

> You can add any PDF to the `docs/` folder and the system will
> automatically include it in the knowledge base.

---

## 💡 Key Design Decisions

**Why RAG instead of fine-tuning?**
Fine-tuning is expensive and the model forgets old knowledge.
RAG keeps documents external — easy to update without retraining.

**Why ChromaDB?**
Local, free, no infrastructure needed. Perfect for prototyping.
Scale to Pinecone or Weaviate when you hit 10M+ vectors.

**Why Qwen 2.5 instead of Llama?**
Qwen 2.5 is completely open — no gated access, no license approval.
Works immediately without HuggingFace token setup.

**Why 500-word chunks with 50-word overlap?**
Balance between context and precision. Too large = loses focus.
Too small = loses context. 500/50 is a solid baseline.

**Why 4-bit quantization?**
Reduces Qwen 2.5 3B from ~6GB to ~2.5GB VRAM.
Fits on a free Colab T4 GPU with room left for embeddings.

---

## 📊 Example Queries
Q: What is a Ponzi scheme?
A: A Ponzi scheme is a fraudulent investment scam that generates
returns for earlier investors using money from newer investors...
[Source: SEC Fraud Guidelines, Page 4]
Q: What are red flags for securities fraud?
A: Key red flags include: guaranteed high returns with no risk,
unregistered investments, secretive strategies...
[Source: SEC 2024 Financial Report, Page 12]
Q: How does the SEC detect insider trading?
A: The SEC uses sophisticated surveillance systems to monitor
unusual trading patterns before major announcements...
[Source: SEC Exam Priorities 2026, Page 7]

---

## ⚠️ Limitations

- Answers are only as good as the documents provided
- 3B parameter model may miss nuanced legal interpretations
- ChromaDB is local — not suitable for multi-user production use
- Free Colab session disconnects after ~2 hours idle

---

## 🗺️ Roadmap

- [x] Basic RAG pipeline
- [x] ChromaDB vector store
- [x] Gradio web UI
- [x] HuggingFace deployment
- [ ] Upgrade LLM to Claude or GPT-4
- [ ] Add RBI and SEBI Indian compliance documents
- [ ] Add reranker for better retrieval accuracy
- [ ] Evaluation using RAGAS framework
- [ ] Multi-document source filtering
- [ ] Chat history / multi-turn conversations

---

## 🧗 Challenges Faced

This project was not easy. Here's what I actually went through:

| Challenge | What Happened | How I Fixed It |
|---|---|---|
| Embeddings | Didn't understand why text becomes numbers | Studied sentence transformers from scratch |
| ChromaDB errors | Vector storage kept failing | Read docs, fixed chunk ID conflicts |
| LLM hallucination | Model invented fake citations | Strict prompt engineering — "only answer from context" |
| Llama gated access | 401 login errors with Meta's model | Switched to Qwen 2.5 — completely open |
| Colab disconnects | Lost embeddings on session restart | Persisted ChromaDB to Google Drive |
| bitsandbytes error | 4-bit quantization failing | Reinstall + runtime restart |

---

## 📚 What I Learned

1. **The retriever matters more than the LLM.** Wrong context in = wrong answer out, regardless of how smart the model is.

2. **Prompt discipline eliminates hallucination.** "Answer ONLY from the context below. If unsure, say I don't know." — this single instruction changed everything.

3. **Start simple, then optimize.** I wasted time trying fancy models before I had a working pipeline. Get it working first. Optimize second.

4. **Persistence beats talent.** I rebuilt this from scratch twice. The second version was 10x better because I understood my mistakes.

---

---
