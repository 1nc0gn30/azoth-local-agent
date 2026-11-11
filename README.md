# aZoth Local Agent (`azoth-local-agent`)

Local-first autonomous ReAct agent, FastAPI web cockpit (`127.0.0.1:8136`), persistent Playwright browser sandbox, isolated Linux guest container bridge, DuckyScript macro engine, and JSON-RPC 2.0 Model Context Protocol (MCP) server.

---

## Architecture & Capabilities

1. **Multi-Engine Inference Routing (`agent.py`)**:
   - Runs offline against local Ollama (`qwen2.5-coder`, `llama3.1`, `gemma4`) or routes tasks across installed CLI/API engines (`hermes`, `agy`, `codex`, `grok_cli`, `xai`, `gemini`, `claude`).
2. **Operating Modes (`prompts/modes.py`)**:
   - `regular` (direct technical synthesis), `fun` (informal register), `think` (`<think>` chain-of-thought extraction + parallel DeepSearch citations), and `coder` (sandbox code generation and test execution).
3. **Persistent Chromium Sandbox & DuckyScript Automation (`tools/browser.py`, `tools/duckyscript.py`)**:
   - Maintains a local browser profile (`sandbox/browser_profile/`) with Bezier mouse trajectories, randomized typing cadences, DOM login inspection, session cookie export, and parameterized `.ducky` macro scripts.
4. **Python Interpreter & Linux Guest OS (`tools/code_runner.py`, `tools/vm_manager.py`)**:
   - Executes Python snippets inside `sandbox/workspace/` and manages an optional Debian containerized guest environment (`docker/Dockerfile.guest_os`) paired with a browser xterm.js PTY bridge (`/ws/terminal`).
5. **MCP Stdio Server (`python3 agent.py --mcp`)**:
   - Exposes `azoth_agent_query`, `azoth_duckyscript`, `azoth_browser_navigate`, `azoth_browser_extract`, `azoth_browser_screenshot`, `azoth_python_repl`, `azoth_shell`, `azoth_deep_search`, `azoth_x_search`, `azoth_x_trends`, `azoth_vm_exec`, and `azoth_vm_status`.

---

## Installation & Quick Start

```bash
git clone https://github.com/1nc0gn30/azoth-local-agent.git
cd azoth-local-agent
pip install -r requirements.txt

# Interactive terminal REPL (uses local Ollama by default)
python3 agent.py

# Launch FastAPI Web Cockpit on http://127.0.0.1:8136
python3 agent.py --web

# Single-shot headless execution with JSON output
python3 agent.py --query "Summarize files in sandbox/workspace" --json
```

### ZothOS System CLI (`azoth`)

```bash
azoth --version
azoth status
azoth modes
azoth --engine hermes --mode coder -q "Inspect workspace tests"
azoth web
```

---

## MCP Client Configuration

Add `agent.py --mcp` to Claude Desktop, Cursor, Hermes, or `agy`:

```json
{
  "mcpServers": {
    "azoth": {
      "command": "python3",
      "args": ["/opt/azoth-local-agent/agent.py", "--mcp"]
    }
  }
}
```

---

## Interactive REPL Commands

| Command | Description |
| :--- | :--- |
| `/mode <regular\|fun\|think\|coder>` | Switch active persona and reasoning mode |
| `/engine <name>` | Switch inference backend (`ollama`, `hermes`, `agy`, `codex`, `grok_cli`, `xai`, `gemini`, `claude`) |
| `/deep <topic>` | Execute multi-query DeepSearch synthesis with numbered source citations |
| `/x <query>` / `/trends [topic]` | Query live X/Twitter posts and topic trends |
| `/py <code>` | Execute Python code inside `sandbox/workspace/` |
| `/vm [cmd]` | Check guest container status or execute a guest shell command |
| `/browser [url]` | Open persistent Chromium profile to inspect or authenticate a site |
| `/tools` / `/engines` | Inspect registered tool schemas and detected local CLI binaries |
| `/clear` / `/quit` | Reset conversation state or exit REPL |

---

## Verification & Tests

```bash
python3 -m pytest tests/ -v
gitleaks detect --source . --no-banner --redact
```

---

## License & Support

- **Open Access**: 100% free and open-source with zero paywalls or locked sandboxes.
- **Support**: [Buy Us a Coffee via Stripe](https://buy.stripe.com/3cIfZg27PdKo2D64Ocfw40X)

Engineered by **1nc0gn30** ([@1nc0gn30](https://github.com/1nc0gn30) · [1nc0gn30.si](https://1nc0gn30.si)) · [NullAI Platform](https://nullai.tech) · [Zoth Studio](https://zoth.nullai.tech)
