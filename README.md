# Container Management Skill

> Docker container operations for AI agents — run, stop, inspect, exec, manage images, networks, and volumes with safety-first lifecycle management via native Docker socket.

[![Skill Standard](https://img.shields.io/badge/standard-agentskills.io-blue)](https://agentskills.io)
[![MCP Server](https://img.shields.io/badge/mcp--server-mcp--containers-green)](https://github.com/zavora-ai/mcp-containers)
[![ADK-Rust Enterprise](https://img.shields.io/badge/ADK--Rust-Enterprise-purple.svg)](https://enterprise.adk-rust.com)
[![License](https://img.shields.io/badge/license-Apache--2.0-orange)](LICENSE)

## What This Skill Does

This skill orchestrates 42 Docker tools into **safe container lifecycle workflows** — always inspect before acting, always check logs before restarting, never force-remove running containers.

| Workflow | Tool Calls | What It Achieves |
|----------|-----------|------------------|
| Container Status | 1-3 | List + stats + inspect |
| Logs & Diagnostics | 2-4 | Logs + processes + resources + filesystem |
| Run Container | 3-4 | Pull → run → verify startup |
| Lifecycle | 2-3 | Stop → remove (safe order) |
| Execute Inside | 1 | Run commands in container |
| Image Management | 2-3 | Pull, tag, inspect |
| Networks | 2-3 | Create, connect, inspect |
| Volumes | 1-2 | Create, inspect |

### Without this skill:
- Containers force-killed without checking logs
- Running containers removed (data loss)
- Images pulled without checking if already local
- No resource monitoring before scaling decisions

### With this skill:
- Logs checked before every restart (understand first)
- Stop before remove enforced (safe lifecycle)
- Images verified locally before running
- Stats checked before resource decisions

## Installation

```bash
git clone https://github.com/zavora-ai/skill-container-management.git \
  ~/.skills/skills/container-management
```

## Requirements

**Required:** `mcp-containers` (42 tools via Docker socket)

**Cross-MCP:**
- `mcp-observability` — container health alerts
- `mcp-cicd` — deploy new versions
- `mcp-database` — DB container health checks

## Folder Structure

```
container-management/
├── SKILL.md                       # 174 lines — 8 workflows + safety rules
├── scripts/
│   └── validate.py                # Container config validator
├── references/
│   ├── tool-sequences.md          # 42 tools across 5 categories
│   ├── cross-mcp-workflows.md     # Containers + Obs + CI/CD + DB
│   └── examples.md                # Status, troubleshoot, deploy
├── README.md
└── LICENSE
```

## Example

**User:** "The API container keeps crashing"

**Agent behavior:**
1. Gets recent logs — finds "ECONNREFUSED :5432"
2. Checks stats — memory/CPU fine
3. Inspects container — finds network mismatch

**Result:**
```
API can't connect to database (ECONNREFUSED :5432).
Resources fine (45% mem, 2% CPU).
Cause: API and DB on different networks.
Fix: connect_network(api, db_network)
```

## Success Criteria

| Metric | Target |
|--------|--------|
| Safety | Never remove running containers |
| Diagnostics | Logs + stats + top in one investigation |
| Deploy speed | Pull → stop → run → verify in 4 calls |

## MCP Server Compatibility

| Category | Tools | Count |
|----------|-------|:-----:|
| Containers | list, inspect, run, stop, kill, remove, restart, pause, unpause, rename, logs, exec, stats, top, changes, wait, update | 17 |
| Images | list, pull, remove, inspect, tag, history, save, load | 8 |
| Networks | list, create, remove, inspect, connect, disconnect | 6 |
| Volumes | list, create, remove, inspect | 4 |
| Compose | up, down | 2 |
| System | info, prune, events, df, version | 5 |
| **Total** | | **42** |

## Contributors

| [<img src="https://github.com/jkmaina.png" width="80px;" alt=""/><br /><sub><b>James Karanja Maina</b></sub>](https://github.com/jkmaina) |
|:---:|

## License

Apache-2.0

---

Part of the [ADK-Rust Enterprise](https://enterprise.adk-rust.com) skills ecosystem. Built with ❤️ by [Zavora AI](https://zavora.ai)
