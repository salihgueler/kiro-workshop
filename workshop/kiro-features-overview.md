# Kiro Features Overview — Workshop Reference

> This document covers every Kiro feature demonstrable on the **Free tier** (50 credits/month, 500 bonus credits on first sign-up for 30 days). All features listed below are available to free-tier users — the only constraint is credit budget, not feature gating.

---

## 1. The IDE Itself

Kiro is an agentic IDE built on Code OSS (the open-source base of VS Code). That means:

- Familiar editor experience — themes, keybindings, extensions, settings sync.
- Compatible with most VS Code extensions via the Open VSX registry.
- Available on **macOS, Windows, and Linux**.
- Supports importing settings from an existing VS Code installation.
- Multi-language support: Python, Java, JavaScript, TypeScript, C#, Go, Rust, PHP, Ruby, Kotlin, C, C++, shell scripting, and more.

**Workshop demo idea:** Open Kiro, show the familiar VS Code layout, install an extension, and point out the Kiro-specific panels (Specs, Steering, Hooks, MCP, Powers).

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

**Workshop demo idea:** Open a project, ask Kiro to explain the authentication flow using `#codebase`, then ask it to fix a bug using `#terminal` after a failed build.

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

**Workshop demo idea:** Start a Vibe session to scaffold a quick prototype, then switch to a Spec session to plan and implement a more complex feature with requirements and design docs.

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

**Workshop demo idea:** Create a Feature Spec for "add user profile page" — walk through requirements generation, design doc with sequence diagram, and task execution.

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

**Workshop demo idea:** Toggle between modes live. Show Autopilot building a feature end-to-end, then switch to Supervised to demonstrate hunk-by-hunk review.

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

**Workshop demo idea:** Generate foundational steering files, then create a custom `api-standards.md` with conditional inclusion for `**/*.ts` files. Show how Kiro follows the standards automatically.

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

**Workshop demo idea:** Create a "lint on save" hook live, then create a "review write operations" preToolUse hook that checks coding standards before any file write.

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

**Workshop demo idea:** Create a simple `pr-review` skill, then invoke it with `/pr-review` in chat.

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

**Workshop demo idea:** Install the AWS Infrastructure as Code power, then ask Kiro to help build a CDK stack — show how the power activates automatically.

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

**Workshop demo idea:** Configure the AWS Documentation MCP server, then use `#mcp` in chat to search AWS docs without leaving the IDE.

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

**Workshop demo idea:** Show the model picker, switch between models, and explain the credit multiplier tradeoffs.

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

## Quick Reference: What to Demo in a Workshop

| #   | Feature                                               | Time Estimate | Credits Used |
| --- | ----------------------------------------------------- | ------------- | ------------ |
| 1   | IDE tour + extension install                          | 3 min         | 0            |
| 2   | Vibe chat — ask questions, generate code              | 5 min         | 2–5          |
| 3   | Context providers (`#file`, `#terminal`, `#git diff`) | 5 min         | 2–3          |
| 4   | Steering — generate foundational files                | 5 min         | 1–2          |
| 5   | Spec session — full feature workflow                  | 10 min        | 5–10         |
| 6   | Autopilot vs Supervised toggle                        | 3 min         | 1–2          |
| 7   | Hooks — create lint-on-save hook                      | 5 min         | 1–2          |
| 8   | MCP — configure and use a server                      | 5 min         | 1–2          |
| 9   | Powers — install and activate                         | 5 min         | 1–2          |
| 10  | Model switching + credit awareness                    | 3 min         | 0–1          |
|     | **Total**                                             | **~50 min**   | **~15–30**   |

> With the 500 bonus credits on first sign-up, workshop participants have plenty of room to follow along hands-on.

---

_Sources: [kiro.dev/docs](https://kiro.dev/docs/), [kiro.dev/pricing](https://kiro.dev/pricing/), [kiro.dev/faq](https://kiro.dev/faq/), [kiro.dev/blog](https://kiro.dev/blog/). Content was rephrased for compliance with licensing restrictions._
