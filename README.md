# RAG SQL Agent

A production-grade, end-to-end RAG SQL Agent that accepts data files,
generates SQL from natural language, self-corrects errors, and interprets results.
Powered by OpenRouter (any LLM) + DuckDB + ChromaDB + Streamlit.

## Quick Start

### 1. Clone and Install
```bash
git clone <your-repo-url>
cd rag_sql_agent
pip install -r requirements.txt
```

### 2. Configure API Key
```bash
cp .env.example .env
# Edit .env and add your OpenRouter API key
# Get a key at: https://openrouter.ai/keys
```

### 3. Run
```bash
streamlit run app.py
```

Open http://localhost:8501 in your browser.

## Supported File Formats
- CSV (auto-detects separator and encoding)
- TSV (tab-separated)
- Excel (.xlsx and .xls — all sheets loaded as separate tables)
- JSON (array or nested object format)
- JSON Lines (.jsonl)
- Parquet

## How It Works
1. Upload your data file in the sidebar
2. The agent extracts schema intelligence and embeds it in ChromaDB (vector store)
3. Ask a question in plain English
4. The agent retrieves relevant schema context via semantic search (RAG)
5. Generates DuckDB SQL using your chosen LLM via OpenRouter
6. Executes SQL against DuckDB (in-memory, no server)
7. Self-corrects if SQL fails (up to 3 attempts)
8. Interprets results in natural language

## Architecture
- **OpenRouter**: LLM provider (supports GPT-4o, Claude, Gemini, Llama, etc.)
- **DuckDB**: In-memory SQL engine — no database setup needed
- **ChromaDB**: Vector store for schema RAG
- **sentence-transformers**: Local embeddings (all-MiniLM-L6-v2, runs on CPU)
- **Streamlit**: Web UI

## Environment Variables
See `.env.example` for all configuration options.

## Project Structure
```
rag_sql_agent/
├── app.py                  # Streamlit entry point
├── requirements.txt
├── .env.example
├── agent/
│   ├── sql_agent.py        # Core orchestrator
│   ├── sql_generator.py    # LLM SQL generation
│   ├── sql_executor.py     # DuckDB execution + self-correction
│   ├── result_interpreter.py
│   └── intent_classifier.py
├── rag/
│   ├── schema_extractor.py
│   ├── vector_store.py     # ChromaDB wrapper
│   └── context_builder.py
├── data/
│   └── loader.py           # Multi-format file loader
├── config/
│   └── settings.py
└── utils/
    ├── sql_validator.py
    ├── formatters.py
    └── session_state.py
```
