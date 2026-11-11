# Antigravity Multi-Agent Development Workflow

A stack-agnostic, global multi-agent orchestration setup for **Google Antigravity**. It coordinates specialized subagents—from UI design and implementation to automated code review, security/leak auditing, and testing—for single-project or multi-project repositories.

---

## Architecture & Agent Roles

When triggered via `/orchestrator`, the main agent inspects your prompt and workspace (`AGENTS.md` and dependency manifests) to dynamically dispatch isolated subagent pipelines:

| Agent | Role & Strict Boundary |
| :--- | :--- |
| **`orchestrator`** | **Pipeline Coordinator.** Analyzes the prompt and workspace structure, defines shared contracts for multi-project tasks, and routes work through strict quality gates. Never writes application code. |
| **`ui-designer`** | **UI/UX Specialist.** Uses the Stitch MCP server to retrieve layouts and design tokens, generating presentation-layer boilerplate matching your frontend framework. Never writes business logic or backend code. |
| **`developer`** | **Implementation Engineer.** Auto-discovers your language, state management, architecture, and package manager from `AGENTS.md` and manifest files, then implements features and fixes review feedback. |
| **`code-reviewer`** | **Read-Only Quality Auditor.** Audits modified files against `AGENTS.md`, architectural conventions, and native linters/analyzers. Outputs `STATUS: PASS` or `STATUS: FAIL`. |
| **`security-auditor`** | **Read-Only Security & Leak Auditor.** Inspects changes for injection flaws, broken auth, exposed secrets, and memory/resource leaks (unclosed streams, controllers, timers, or connections). |
| **`tester`** | **Automated QA Engineer.** Discovers your test runner and writes/executes unit, component/widget, and integration tests inside test directories only. |

### Execution Flow

```text
Prompt (/orchestrator)
  │
  ├─► [1. ui-designer]      (Conditional: UI tasks only via Stitch MCP)
  │
  ├─► [2. developer]        (Implements feature based on discovered stack)
  │
  ├─► [3. Verification Loop] ◄───────────────────────┐
  │     ├── code-reviewer   (Read-only quality check) │ Loop until BOTH
  │     └── security-auditor (Read-only leak/sec)     │ return STATUS: PASS
  │            └─► If FAIL ──► [developer fixes] ─────┘
  │
  └─► [4. tester]           (Writes & runs unit, widget, and integration tests)
               └─► If FAIL ──► [developer fixes] ──► Re-verify & Re-test
```

---

## Repository Contents

* `setup_antigravity_pipeline.sh` — One-command installation script that configures `~/.gemini/config/` with your global MCP servers, subagents, and orchestrator skill.
* `AGENTS.md` — Drop-in project context template to place in the root of your application repositories.

---

## Installation & Setup

### 1. Prerequisites
* **Google Antigravity** installed on your machine.
* **Dart SDK** (if using the Dart MCP server for Flutter/Dart projects).
* **Node.js / `npx`** (for running the Stitch MCP server).
* **Stitch API Key** exported in your shell environment (`~/.zshrc` or `~/.bashrc`):
  ```bash
  export STITCH_API_KEY="your_stitch_api_key_here"
  ```

### 2. Install Global Agents & Workflow
Clone this repository and run the setup script once to install the agents globally in `~/.gemini/config/`:

```bash
git clone <your-repo-url>
cd <your-repo-directory>
chmod +x setup_antigravity_pipeline.sh
./setup_antigravity_pipeline.sh
```

### 3. Verify Installation
Open any project in Google Antigravity and type `/agents` in the prompt bar to confirm that `ui-designer`, `developer`, `code-reviewer`, `security-auditor`, and `tester` are registered.

---

## Usage Guide

### Step 1: Add `AGENTS.md` to Your Target Project
Copy the `AGENTS.md` template from this repo into the root of your target project (or into each sub-project folder in a monorepo) and fill in your stack details, commands, and architectural rules.

### Step 2: Run `/orchestrator` in Antigravity

#### Single-Project Workspace
Open your project repository and describe the feature or bug fix:
```text
/orchestrator Implement offline caching for the user feed according to AGENTS.md
```

#### Multi-Project / Full-Stack Workspace
Mention both projects or directories in your prompt. The orchestrator will establish the shared API contract and run separate, parallel agent pipelines for each project:
```text
/orchestrator Build the user profile editing screen in the mobile app folder and the corresponding update endpoint in the backend folder.
```

#### Live Monitoring
* Open the **Subagent Panel** (or press `Alt+J` in the Antigravity CLI) at any time to inspect real-time progress or send direct guidance to an active subagent.

---

## Customization

* **MCP Servers:** Edit `~/.gemini/config/mcp_config.json` (or update the script before running) to add or modify MCP tools such as database, GitHub, or browser servers.
* **Agent Rules:** Modify the Markdown files inside `~/.gemini/config/agents/` to adjust model tiers (`pro` / `flash`), tool permissions, or strict boundary instructions.
