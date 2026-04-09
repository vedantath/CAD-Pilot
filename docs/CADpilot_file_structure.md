# Project File Structure

```
CADpilot/
│
├── CLAUDE.md                          # Claude Code reads this every session
├── README.md                          # Project overview, setup instructions
├── .gitignore
├── .env.example                       # Template — never commit .env
│
├── .claude/                           # Claude Code config folder
│   ├── CLAUDE.local.md                # Your personal notes (gitignored)
│   └── commands/                      # Custom slash commands for Claude Code
│       ├── test-script.md             # /test-script — run a .lsp against AutoCAD
│       └── add-example.md             # /add-example — log a new prompt→script pair
│
├── orchestrator/                      # Phase 2 — FastAPI brain
│   ├── main.py                        # FastAPI app entrypoint
│   ├── planner.py                     # Breaks requests into sub-tasks
│   ├── router.py                      # Routes tasks to 7B vs 13B vs 34B model
│   ├── tools/
│   │   ├── __init__.py
│   │   ├── draw.py                    # draw_line(), draw_rect(), insert_block()
│   │   ├── layers.py                  # set_layer(), list_layers()
│   │   └── query.py                   # get_entities(), get_extents()
│   ├── memory/
│   │   ├── session.py                 # Rolling context window per session
│   │   └── db.py                      # SQLite session log
│   └── tests/
│       └── test_planner.py
│
├── autocad_bridge/                    # Phase 1+2 — AutoCAD connection
│   ├── com_bridge.py                  # pyautocad COM automation
│   ├── script_runner.py               # Executes .lsp / .py scripts
│   ├── state_reader.py                # Reads live DWG state back to agent
│   └── self_correct.py                # Verify → diff → retry loop (max 3)
│
├── rag/                               # Phase 3 — Standards & memory
│   ├── embed.py                       # Embed docs into ChromaDB
│   ├── retrieve.py                    # Query vector store at prompt time
│   ├── vector_store/                  # ChromaDB data (gitignored)
│   └── knowledge/
│       ├── standards.md               # Layer names, dim styles, line weights
│       ├── blocks.json                # Block library metadata
│       └── examples/                  # Prompt→script pairs (few-shot bank)
│           ├── rectangle_walls.json
│           └── insert_door_block.json
│
├── render/                            # Phase 4 — Rendering pipeline
│   ├── blender_bridge.py              # DXF → Blender via bpy
│   ├── controlnet_pipe.py             # Wireframe → SD + ControlNet
│   ├── presets/                       # Render preset configs
│   │   ├── client_presentation.json
│   │   ├── concept_sketch.json
│   │   └── photorealistic.json
│   └── output/                        # Rendered images (gitignored)
│
├── ui/                                # Phase 5 — Chat interface
│   ├── src/
│   │   ├── App.jsx
│   │   ├── ChatPanel.jsx
│   │   └── ApprovalModal.jsx          # Confirm before destructive ops
│   ├── package.json
│   └── vite.config.js
│
├── models/                            # Model config (not model files)
│   └── model_config.json              # Which model handles which task type
│
├── scripts/                           # Dev & utility scripts
│   ├── pull_models.sh                 # ollama pull all required models
│   ├── copy_models_to_usb.sh          # Copies ~/.ollama/models to USB
│   └── seed_examples.py               # Load few-shot examples into ChromaDB
│
├── tests/                             # Phase 1 baseline + regression suite
│   ├── prompts/                       # 20+ test drawing prompts
│   │   ├── simple_geometry.json
│   │   ├── hatching.json
│   │   └── block_insertion.json
│   └── eval.py                        # Scores script output correctness
│
└── docs/
    ├── architecture.md                # System overview with data flow
    ├── autocad_cheatsheet.md          # 30–50 most-used AutoLISP commands
    └── phase_log.md                   # What works, what doesn't, per phase
```