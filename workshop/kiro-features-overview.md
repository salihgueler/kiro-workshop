# Kiro Features Overview — Workshop Reference

> **Workshop project:** We're building a full-stack tic-tac-toe game — frontend + backend — live with the audience. The audience decides what crazy features to add (AI opponent? leaderboard? sound effects? themes?). We build it together using Kiro.

> This document covers every Kiro feature demonstrable on the **Free tier** (50 credits/month, 500 bonus credits on first sign-up for 30 days). All features listed below are available to free-tier users — the only constraint is credit budget, not feature gating.

---

## 1. The IDE Itself

Kiro is an agentic IDE built on Code OSS (the open-source base of VS Code). That means:

- Familiar editor experience — themes, keybindings, extensions, settings sync.
- Compatible with most VS Code extensions via the Open VSX registry.
- Available on **macOS, Windows, and Linux**.
- Supports importing settings from an existing VS Code installation.
- Multi-language support: Python, Java, JavaScript, TypeScript, C#, Go, Rust, PHP, Ruby, Kotlin, C, C++, shell scripting, and more.

**In our workshop:** Open Kiro, show the familiar VS Code layout, point out the Kiro-specific panels (Specs, Steering, Hooks, MCP, Powers), and create the tic-tac-toe workspace.

---

## 2. Chat — Conversational AI Coding

The chat panel (`Cmd+L` / `Ctrl+L`) is the primary interaction surface. You type natural language and Kiro reads, writes, and runs code on your behalf.

### Key capabilities

| Capability                  | Description                                                                                                                                               |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Smart intent detection**  | Kiro distinguishes "explain this" (information) from "fix this" (action) automatically.                                                                   |
| **Context providers (`#`)** | `#file`, `#folder`, `#codebase`, `#git diff`, `#terminal`, `#problems`, `#url`, `#repository`, `#current`, `#steering`, `#docs`, `#spec`, `#mcp`, `#code` |
| **Multi-language chat**     | Respond in Mandarin, French, German, Italian, Japanese, Spanish, Korean, Hindi, Portuguese, and more.                                                     |
| **Session history**         | Full history of past sessions with search, restore, and delete.                                                                                           |
| **Export conversations**    | Right-click a tab → Export Conversation (markdown).                                                                                                       |
| **Dev servers**             | Run long-running processes in the background without blocking the chat.                                                                                   |

### Context providers worth highlighting

- `#terminal` — Kiro reads your recent terminal output to debug build failures, test errors, and dependency issues.
- `#git diff` — Feed the current diff into chat for code review or explanation.
- `#codebase` — Let Kiro automatically find relevant files across the project.

**In our workshop:** Use `#codebase` to ask Kiro about the game logic, `#terminal` to debug build issues, and `#git diff` to review what we've built so far.

---

## 3. Vibe Sessions vs Spec Sessions

Kiro offers two session types, selectable from the session picker when you start a new chat.

### Vibe Sessions

- Conversational, Q&A-focused.
- Best for quick questions, explanations, exploratory coding, and prototyping.
- No formal structure — just talk and build.

### Spec Sessions

- Structured, three-phase workflow for complex features or bug fixes.
- Generates formal artifacts: `requirements.md` (or `bugfix.md`), `design.md`, `tasks.md`.
- Tasks are trackable with real-time status updates.
- Two spec types: **Feature Specs** and **Bugfix Specs**.

**In our workshop:** Vibe session to scaffold the initial tic-tac-toe game, then Spec session when the audience picks a complex feature to add.

---

## 4. Specs — Structured Development Artifacts

Specs formalize the development process. Every spec generates three files under `.kiro/specs/<spec-name>/`:

### Three-Phase Workflow

1. **Requirements / Bug Analysis**
   - Feature Specs → `requirements.md` with user stories and acceptance criteria.
   - Bugfix Specs → `bugfix.md` with current behavior, expected behavior, and unchanged behavior.

2. **Design**
   - `design.md` with system architecture, sequence diagrams, component design, error handling, and testing strategy.

3. **Tasks**
   - `tasks.md` with discrete, executable implementation tasks.
   - Tasks update in real-time as in-progress or completed.
   - Run tasks individually or all at once.

### Feature Specs vs Bugfix Specs

|                       | Feature Spec                       | Bugfix Spec                    |
| --------------------- | ---------------------------------- | ------------------------------ |
| **Starting point**    | A feature idea or user story       | A bug report or observed issue |
| **Phase 1 output**    | `requirements.md`                  | `bugfix.md`                    |
| **Workflow variants** | Requirements-First or Design-First | Single workflow                |

**In our workshop:** Create a Feature Spec for whatever the audience picks — AI opponent, leaderboard, multiplayer — and walk through the full requirements → design → tasks workflow.

---

## 5. Autopilot & Supervised Modes

### Autopilot (default)

- Kiro works autonomously: creates files, modifies code, runs commands, makes architectural decisions.
- You can **view all changes**, **revert all changes**, or **interrupt execution** at any time.

### Supervised

- Kiro yields after each turn that contains file edits.
- Changes are presented as **hunks** — you can accept, reject, or chat inline about each one.
- Per-file and per-hunk granularity.
- Accept All / Reject All for batch operations.

**In our workshop:** Use Autopilot for the initial scaffold, then switch to Supervised when executing a spec task to show hunk-by-hunk review of game logic changes.

---

## 6. Steering — Persistent Project Knowledge

Steering files (`.kiro/steering/*.md`) give Kiro persistent context about your workspace so you don't repeat yourself every session.

### Scope

| Scope         | Location                                       | Applies to             |
| ------------- | ---------------------------------------------- | ---------------------- |
| **Workspace** | `.kiro/steering/`                              | Current workspace only |
| **Global**    | `~/.kiro/steering/`                            | All workspaces         |
| **Team**      | `~/.kiro/steering/` (distributed via MDM/repo) | All team members       |

### Foundational Steering Files

Kiro can auto-generate three foundational files:

1. **`product.md`** — Product purpose, target users, key features, business objectives.
2. **`tech.md`** — Frameworks, libraries, dev tools, technical constraints.
3. **`structure.md`** — File organization, naming conventions, import patterns, architecture.

### Inclusion Modes

| Mode                 | Frontmatter                                 | Behavior                                                       |
| -------------------- | ------------------------------------------- | -------------------------------------------------------------- |
| **Always** (default) | `inclusion: always`                         | Loaded in every interaction.                                   |
| **Conditional**      | `inclusion: fileMatch` + `fileMatchPattern` | Loaded only when working with matching files.                  |
| **Manual**           | `inclusion: manual`                         | Available on-demand via `#steering-name` or `/` slash command. |
| **Auto**             | `inclusion: auto` + `name` + `description`  | Loaded when request matches the description.                   |

### File References

Link to live workspace files: `#[[file:api/openapi.yaml]]`

### AGENTS.md Support

Kiro also supports the `AGENTS.md` standard — place it in the workspace root or `~/.kiro/steering/` and it's always included.

**In our workshop:** Generate foundational steering files for the tic-tac-toe project at the start. Optionally create a `game-api.md` steering file with conditional inclusion for backend files.

---

## 7. Agent Hooks — Event-Driven Automation

Hooks automatically execute agent actions when specific IDE events occur.

### Trigger Events

| Event               | Description                   |
| ------------------- | ----------------------------- |
| `fileEdited`        | User saves a file             |
| `fileCreated`       | User creates a new file       |
| `fileDeleted`       | User deletes a file           |
| `promptSubmit`      | Message sent to the agent     |
| `agentStop`         | Agent execution completes     |
| `preToolUse`        | Before a tool is executed     |
| `postToolUse`       | After a tool is executed      |
| `preTaskExecution`  | Before a spec task starts     |
| `postTaskExecution` | After a spec task completes   |
| `userTriggered`     | Manual trigger (button click) |

### Hook Actions

| Action       | Description                |
| ------------ | -------------------------- |
| `askAgent`   | Send a prompt to the agent |
| `runCommand` | Execute a shell command    |

### Example: Lint on Save

```json
{
  "name": "Lint on Save",
  "version": "1.0.0",
  "when": {
    "type": "fileEdited",
    "patterns": ["*.ts", "*.tsx"]
  },
  "then": {
    "type": "runCommand",
    "command": "npm run lint"
  }
}
```

### Creating Hooks

- **Via natural language:** Describe what you want and Kiro generates the config.
- **Via form:** Manually fill out the hook form in the Kiro panel.
- **Via Command Palette:** `Cmd+Shift+P` → "Kiro: Open Kiro Hook UI".

**In our workshop:** Create a "build on save" hook for TypeScript files, and a "review write operations" preToolUse hook to check accessibility — both while building the game.

---

## 8. Agent Skills — Portable Instruction Packages

Skills follow the open [Agent Skills](https://agentskills.io/) standard. They bundle instructions, scripts, and templates into reusable packages.

### How They Work (Progressive Disclosure)

1. **Discovery** — Kiro loads only name + description at startup.
2. **Activation** — When your request matches, Kiro loads full instructions.
3. **Execution** — Kiro follows instructions, loading scripts/references as needed.

### Skill Structure

```
my-skill/
├── SKILL.md           # Required (frontmatter: name, description)
├── scripts/           # Optional executable code
├── references/        # Optional documentation
└── assets/            # Optional templates
```

### Scope

- **Workspace:** `.kiro/skills/`
- **Global:** `~/.kiro/skills/`

### Importing Skills

Import from GitHub URLs or local folders via the Kiro panel.

### Skills vs Steering vs Powers

|              | Skills                 | Steering           | Powers                |
| ------------ | ---------------------- | ------------------ | --------------------- |
| **Standard** | Open (agentskills.io)  | Kiro-specific      | Kiro-specific         |
| **Loading**  | On-demand              | Configurable modes | Dynamic by keywords   |
| **Includes** | Instructions + scripts | Markdown guidance  | MCP tools + knowledge |
| **Best for** | Reusable workflows     | Project standards  | Tool integrations     |

**In our workshop:** If time allows, create a `game-balance` skill that reviews game logic for fairness when invoked.

---

## 9. Powers — Dynamic MCP Tool Bundles

Powers solve two problems: context overload (too many MCP tools loaded at once) and missing domain expertise.

### How Powers Work

1. You start a task (e.g., "Add a database on Supabase").
2. Kiro evaluates installed powers against the task keywords.
3. Only relevant powers load into context.
4. When you move to a different domain, the previous power deactivates.

### What's in a Power

- **POWER.md** — Steering file with tool guidance.
- **MCP server configuration** — Tools and connection details.
- **Steering/hooks** — Optional automated tasks.

### Key Differentiators

- **Dynamic loading** — Tools load on-demand, not all at once.
- **One-click install** — Browse and install from the Kiro panel or kiro.dev/powers.
- **Open ecosystem** — Curated powers from Datadog, Dynatrace, Figma, Neon, Netlify, Postman, Supabase, Stripe, Strands SDK, AWS Aurora, and community contributions.

**In our workshop:** If the audience wants a leaderboard or deployment, install a relevant Power (e.g., Supabase for DB, AWS IaC for deployment) and show keyword-based activation.

---

## 10. Model Context Protocol (MCP)

MCP extends Kiro by connecting to external servers that provide tools, prompts, and resources.

### Configuration

MCP servers are configured in `mcp.json` files:

| Scope             | Location                    |
| ----------------- | --------------------------- |
| **Workspace**     | `.kiro/settings/mcp.json`   |
| **User (global)** | `~/.kiro/settings/mcp.json` |

```json
{
  "mcpServers": {
    "aws-docs": {
      "command": "uvx",
      "args": ["awslabs.aws-documentation-mcp-server@latest"],
      "env": { "FASTMCP_LOG_LEVEL": "ERROR" },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

### Capabilities

- Access specialized knowledge bases and documentation.
- Integrate with external services and APIs.
- Use server-provided prompt templates and resources via `#mcp` in chat.
- Create custom tools for specific workflows.

### MCP Servers Tab

The Kiro panel shows all configured servers with connection status and quick access to tools.

**In our workshop:** If we need external docs or tools during the build, configure an MCP server and use `#mcp` in chat.

---

## 11. Models

Kiro provides access to multiple AI models. All models are available on the free tier — the difference is credit consumption rate.

### Available Models (as of May 2026)

| Model                  | Context | Credit Multiplier | Tier Availability |
| ---------------------- | ------- | ----------------- | ----------------- |
| **Auto** (recommended) | —       | 1.0x              | All               |
| Claude Sonnet 4.0      | 200K    | 1.3x              | All               |
| Claude Sonnet 4.5      | 200K    | 1.3x              | All               |
| Claude Sonnet 4.6      | 1M      | 1.3x              | All               |
| Claude Opus 4.5        | 200K    | 2.2x              | All               |
| Claude Opus 4.6        | 1M      | 2.2x              | All               |
| Claude Haiku 4.5       | 200K    | 0.4x              | All               |
| DeepSeek 3.2           | 128K    | 0.25x             | All               |
| MiniMax M2.5           | 200K    | 0.25x             | All               |
| MiniMax M2.1           | 200K    | 0.15x             | All               |
| GLM-5                  | 200K    | 0.5x              | All               |
| Qwen3 Coder Next       | 256K    | 0.05x             | All               |

### Credit-Saving Tip for Free Tier

Use **Qwen3 Coder Next** (0.05x) or **MiniMax M2.1** (0.15x) for simple tasks to stretch your 50 credits further. Switch to **Auto** or **Sonnet** for complex work.

**In our workshop:** Use Auto or Sonnet for the main build, switch to Haiku or Qwen3 for simple tasks, and show the credit impact in real-time.

---

## 12. Additional Editor Features

### Slash Commands

Type `/` in chat to access available commands, including steering files and skills.

### Keyboard Shortcuts

| Action                    | Mac           | Windows/Linux  |
| ------------------------- | ------------- | -------------- |
| Open Chat                 | `Cmd+L`       | `Ctrl+L`       |
| Command Palette           | `Cmd+Shift+P` | `Ctrl+Shift+P` |
| Toggle Secondary Side Bar | `Cmd+Opt+B`   | `Ctrl+Alt+B`   |

### Checkpoints

Revert to a checkpoint to undo both file changes and context additions made by Kiro.

### Web Tools

Kiro can search the web and fetch content from URLs to get current information during development.

---

## 13. Free Tier Summary

| Aspect                  | Details                                                                |
| ----------------------- | ---------------------------------------------------------------------- |
| **Monthly credits**     | 50 (perpetual)                                                         |
| **First sign-up bonus** | 500 credits for 30 days                                                |
| **Credit consumption**  | Fractional (0.01 increments), based on task complexity                 |
| **Feature access**      | All features — Specs, Steering, Hooks, Skills, Powers, MCP, all models |
| **Spec sessions**       | Yes (consume credits)                                                  |
| **Vibe sessions**       | Yes (consume credits)                                                  |
| **Model access**        | All models available (credit multiplier varies)                        |
| **Student plan**        | 1,000 credits/month free for 1 year (eligible university students)     |

---

## Workshop Flow: Building Tic-Tac-Toe Together

This is a **freeform, audience-driven** workshop. We build a full-stack tic-tac-toe game live, and the audience decides what features to add. The Kiro features get showcased naturally as we build.

### Phase 1 — Setup & First Moves (~15 min)

| Step | What we do                                                                                   | Kiro feature showcased       |
| ---- | -------------------------------------------------------------------------------------------- | ---------------------------- |
| 1    | Quick IDE tour — panels, chat, model picker                                                  | **IDE basics, Models**       |
| 2    | Generate foundational steering files for the project                                         | **Steering**                 |
| 3    | Vibe chat: "Scaffold a tic-tac-toe game with a React frontend and a Node.js/Express backend" | **Vibe sessions, Autopilot** |
| 4    | Run the app, show it working                                                                 | **Dev servers, `#terminal`** |

### Phase 2 — Spec-Driven Feature (audience picks) (~15 min)

| Step | What we do                                                                                                           | Kiro feature showcased                    |
| ---- | -------------------------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| 5    | Ask the audience: "What feature should we add?" (AI opponent, multiplayer, leaderboard, themes, sound effects, etc.) | Audience engagement                       |
| 6    | Create a **Feature Spec** for the chosen feature                                                                     | **Specs (requirements → design → tasks)** |
| 7    | Walk through generated requirements and design doc                                                                   | **Spec artifacts**                        |
| 8    | Execute tasks — toggle to **Supervised mode** for a task or two to show hunk review                                  | **Task execution, Supervised mode**       |

### Phase 3 — Hooks & Automation (~10 min)

| Step | What we do                                                            | Kiro feature showcased              |
| ---- | --------------------------------------------------------------------- | ----------------------------------- |
| 9    | Create a hook: "Run `npm run build` after every TypeScript file save" | **Hooks (fileEdited → runCommand)** |
| 10   | Create a hook: "Review all write operations for accessibility"        | **Hooks (preToolUse → askAgent)**   |
| 11   | Show hooks firing in real-time as we keep building                    | **Hook automation in action**       |

### Phase 4 — Wild Card Round (audience picks again) (~15 min)

| Step | What we do                                                                                 | Kiro feature showcased                |
| ---- | ------------------------------------------------------------------------------------------ | ------------------------------------- |
| 12   | Ask: "What else should we add? Go wild."                                                   | Audience engagement                   |
| 13   | Build it using Vibe chat + context providers (`#file`, `#codebase`, `#git diff`)           | **Context providers, Vibe coding**    |
| 14   | If relevant, install a Power (e.g., AWS IaC for deployment, Supabase for a leaderboard DB) | **Powers**                            |
| 15   | Show model switching — use a cheaper model for simple tasks, Opus for complex ones         | **Model selection, credit awareness** |

### Phase 5 — Wrap-Up (~5 min)

| Step | What we do                                                      | Kiro feature showcased      |
| ---- | --------------------------------------------------------------- | --------------------------- |
| 16   | Review `#git diff` of everything we built                       | **Context providers**       |
| 17   | Show session history and export the conversation                | **Session history, export** |
| 18   | Recap: features used, credits consumed, what the audience built | All features                |

### Audience-Driven Feature Ideas (have these ready as suggestions)

If the audience needs a nudge, suggest these:

- 🤖 **AI opponent** — Minimax algorithm or call an LLM to decide moves
- 🏆 **Leaderboard** — Backend API + database (great for showing Powers/MCP)
- 🎨 **Themes** — Dark mode, neon, retro, emoji-only board
- 🔊 **Sound effects** — Play sounds on move, win, draw
- 👥 **Multiplayer** — WebSocket-based real-time play
- ⏱️ **Move timer** — Countdown per turn
- 📊 **Game replay** — Record and replay past games
- 🌍 **Internationalization** — Multi-language UI (shows Kiro's multi-language chat too)
- 💬 **In-game chat** — Players can trash-talk each other
- 🎯 **Larger boards** — 4x4, 5x5 grids with configurable win conditions

### Kiro Features Naturally Covered During the Build

| Kiro Feature             | When it shows up                     |
| ------------------------ | ------------------------------------ |
| Chat + context providers | Every interaction                    |
| Vibe sessions            | Scaffolding, quick features          |
| Spec sessions            | The main audience-chosen feature     |
| Autopilot mode           | Fast building phases                 |
| Supervised mode          | Reviewing a tricky change            |
| Steering                 | Project setup, coding standards      |
| Hooks                    | Automation during development        |
| Powers                   | If we integrate an external service  |
| MCP                      | If we connect external tools         |
| Models                   | Switching for cost/quality tradeoffs |
| Skills                   | If we create a reusable workflow     |
| Checkpoints              | If we need to revert a bad change    |

> **Credit budget:** With the 500 bonus credits on first sign-up, participants have plenty of room. The presenter should use ~30–50 credits for the full session. Audience members following along on their own machines will use similar amounts.

---

_Sources: [kiro.dev/docs](https://kiro.dev/docs/), [kiro.dev/pricing](https://kiro.dev/pricing/), [kiro.dev/faq](https://kiro.dev/faq/), [kiro.dev/blog](https://kiro.dev/blog/). Content was rephrased for compliance with licensing restrictions._
