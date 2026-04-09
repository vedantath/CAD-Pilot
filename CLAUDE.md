# CADpilot

AutoCAD AI agent — natural language → AutoLISP/pyautocad scripts → renderings.
Fully local. Built on Ollama + FastAPI + ChromaDB.

## Stack
- Python 3.11
- FastAPI (orchestrator)
- Ollama (local models — see models/model_config.json)
- ChromaDB (RAG vector store)
- pyautocad / ezdxf (AutoCAD bridge)
- Blender bpy + ComfyUI (rendering)
- React + Vite (UI)

## Commands
```bash
# Start orchestrator
uvicorn orchestrator.main:app --reload

# Start UI
cd ui && npm run dev

# Run eval suite
python tests/eval.py

# Seed RAG examples
python scripts/seed_examples.py
```

## Structure
- orchestrator/    FastAPI brain — planning, routing, retries
- autocad_bridge/  COM bridge + script runner + state reader
- rag/             ChromaDB vector store + standards + few-shot examples
- render/          Blender + ControlNet pipelines
- ui/              React chat interface

## Conventions
- All AutoLISP output saved as .lsp, Python scripts as .py
- Self-correction retries capped at 3
- Never commit .env, vector_store/, or render/output/
- See docs/autocad_cheatsheet.md before generating AutoLISP

## Reference docs (read when relevant)
- @docs/architecture.md       — full system data flow
- @docs/autocad_cheatsheet.md — AutoLISP command reference
- @rag/knowledge/standards.md — layer names, dim styles, conventions