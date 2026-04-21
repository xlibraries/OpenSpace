# Claude Code Configuration - RuFlo V3

## Project: OpenSpace

Self-evolving skill system for AI agents. Python + TypeScript monorepo with Flask backend, React frontend, MCP server, and LiteLLM multi-provider routing.

## Behavioral Rules (Always Enforced)

- Do what has been asked; nothing more, nothing less
- NEVER create files unless absolutely necessary
- ALWAYS prefer editing an existing file to creating a new one
- NEVER proactively create documentation files (*.md) or README files unless explicitly requested
- NEVER save working files, text/mds, or tests to the root folder
- ALWAYS read a file before editing it
- NEVER commit secrets, credentials, or .env files

## Project Structure

```
openspace/           # Core Python package
  agents/            # Agent execution
  grounding/         # Unified tool/backend layer
  skill_engine/      # Self-evolution core (FIX / DERIVED / CAPTURED modes)
  cloud/             # Community skill registry (open-space.cloud)
  communication/     # Multi-channel gateway
  mcp_server.py
  dashboard_server.py
frontend/            # React + TypeScript + Tailwind + Vite dashboard
gdpval_bench/        # Benchmark experiments
showcase/            # Demo apps
```

## Tech Stack

- **Backend**: Python, Flask, LiteLLM, MCP, SQLite, Pydantic
- **Frontend**: React, TypeScript, Tailwind CSS, Vite
- **LLM Providers**: Claude (Anthropic), OpenAI, Qwen, MiniMax via LiteLLM
- **Packaging**: pyproject.toml, requirements.txt

## File Organization

- Python source → `openspace/`
- Frontend source → `frontend/src/`
- Tests → `tests/`
- Docs → `docs/`
- Scripts → `scripts/`
- Config → `.env` (never commit), use `.env.example` as template

## Build & Test

```bash
# Install Python deps
pip install -e ".[dev]"

# Install frontend deps
cd frontend && npm install

# Run backend
python -m openspace

# Run frontend dev server
cd frontend && npm run dev

# Build frontend
cd frontend && npm run build

# Run tests
pytest

# Lint Python
ruff check .

# Lint frontend
cd frontend && npm run lint
```

- ALWAYS run tests after making code changes
- ALWAYS verify build succeeds before committing

## Architecture Rules

- Follow Domain-Driven Design with bounded contexts
- Keep files under 500 lines
- Use typed interfaces for all public APIs
- Skill engine modes: FIX (repair broken skills), DERIVED (compose new), CAPTURED (learn from usage)
- LLM calls MUST go through LiteLLM — never call provider SDKs directly
- MCP tools registered in `mcp_server.py` are the agent's grounding layer

## Security Rules

- NEVER hardcode API keys, secrets, or credentials
- Store all secrets in `.env`, reference via `python-dotenv`
- Always validate user input at system boundaries
- Sanitize file paths to prevent directory traversal
- NEVER commit `.env` files

## Concurrency: 1 MESSAGE = ALL RELATED OPERATIONS

- All related file reads/writes MUST be batched in one message
- Parallel Bash commands MUST be in one message

## 3-Tier Model Routing

| Tier | Handler | Use Cases |
|------|---------|-----------|
| **1** | Direct edit (no LLM) | Simple transforms, type annotations |
| **2** | Haiku | Low-complexity tasks (<30%) |
| **3** | Sonnet/Opus | Complex reasoning, architecture, security |

## Support

- Repo: https://github.com/xlibraries/OpenSpace
- Claude Flow docs: https://github.com/ruvnet/claude-flow
