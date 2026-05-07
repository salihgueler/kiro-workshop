# Step 8 — Kiro CLI

> **Goal:** Take Kiro out of the IDE and into the terminal. Explore the Kiro CLI as a full development experience — interactive chat, custom agents, shell translation, session management, and MCP from the command line.

---

## 8.1 — Kiro Beyond the IDE

Everything you've done so far has been inside the Kiro IDE. But not every developer lives in an IDE. Some prefer the terminal. Some work over SSH. Some want Kiro alongside their existing tmux/vim/emacs workflow.

The **Kiro CLI** is a full-featured agent experience for the terminal. It's not a stripped-down version — it has the same agent, same models, same steering, hooks, MCP, and skills as the IDE. Just a different interface.

---

## 8.2 — Installing the CLI

### macOS / Linux

```bash
curl -fsSL https://cli.kiro.dev/install | bash
```

### Windows (PowerShell)

```powershell
irm 'https://cli.kiro.dev/install.ps1' | iex
```

After installation, authenticate:

```bash
kiro-cli login
```

This opens your browser for authentication. You can use the same account (Google, GitHub, Builder ID, or IAM Identity Center) as the IDE. For remote/SSH environments, it uses a device flow — shows a code and URL you complete on another device.

### Verify it works

```bash
kiro-cli whoami    # Check auth status
kiro-cli version   # Check version
kiro-cli doctor    # Diagnose common issues
```

---

## 8.3 — Interactive Chat

The simplest way to start:

```bash
cd tic-tac-toe
kiro-cli
```

This drops you into an interactive chat session with a rich terminal UI — syntax-highlighted code, interactive panels, and visual tool progress. It's not a bare-bones text prompt; it's a polished terminal experience.

### Ask questions directly

You can also pass a question as an argument to skip the interactive startup:

```bash
kiro-cli chat "Explain the game logic in Board.tsx"
```

### Try it out

Open a terminal, `cd` into the tic-tac-toe project, and run `kiro-cli`. Ask it to do something with the game:

```
Explain how the win detection works in the tic-tac-toe game
```

Then ask it to make a change:

```
Add a move counter that shows how many moves have been played in the current game
```

Notice that it reads files, writes code, runs commands, and follows the steering files you set up in Step 4 — all from the terminal.

---

## 8.4 — Shell Translation

One of the CLI's most useful features for day-to-day work. Translate natural language into shell commands:

```bash
kiro-cli translate "find all TypeScript files modified today"
```

Kiro outputs the actual shell command. You can generate multiple options:

```bash
kiro-cli translate -n 3 "compress all log files older than 30 days"
```

This is handy even outside of project work — it's a smarter `man` page.

---

## 8.5 — Running Shell Commands Inline

Inside a chat session, prefix any command with `!` to run it directly without going through the AI:

```bash
!npm run build
!git status
!npm test
```

Output streams in real time. TTY commands like `vim`, `ssh`, and `top` get full terminal access. Long output collapses to a head + tail view — press `Ctrl+O` to expand.

This means you never have to leave the chat session to run a quick command.

---

## 8.6 — Multi-Line Input and the Editor

For longer prompts, you have several options:

- **Shift+Enter** — Insert a newline (works in iTerm2, Ghostty, Kitty, Warp, Zed)
- **Ctrl+J** — Insert a newline (works in all terminals including tmux)
- **Alt+Enter** — Insert a newline (works in Terminal.app and Ghostty)
- **`/editor`** — Opens your default editor (vi by default) for composing longer prompts

The `/editor` command is great when you want to write a detailed multi-paragraph prompt without fighting the terminal input.

---

## 8.7 — Slash Commands in Chat

Inside a chat session, type `/` to see available slash commands:

| Command    | What it does                                       |
| ---------- | -------------------------------------------------- |
| `/model`   | Switch the AI model mid-conversation               |
| `/tools`   | View and search available tools                    |
| `/agent`   | Switch to a different custom agent                 |
| `/compact` | Compact the conversation to free up context        |
| `/context` | Manage file context                                |
| `/chat`    | Session management (new, resume, save, load)       |
| `/editor`  | Open editor for multi-line input                   |
| `/reply`   | Open editor with the last assistant message quoted |
| `/help`    | Show all available commands                        |

These mirror many of the IDE's features — model switching, tool discovery, and context management — all from the keyboard.

---

## 8.8 — Context Management

Control what files Kiro sees in the current session:

```bash
/context show              # See what's in context with per-file token usage
/context add "src/**/*.ts" # Add files by glob pattern
/context remove src/app.js # Remove a file
/context clear             # Remove all context rules
```

This is the CLI equivalent of the `#file` and `#folder` context providers in the IDE — but with glob patterns for more precise control.

---

## 8.9 — Session Management

The CLI saves every conversation automatically. You can resume, list, and manage sessions:

```bash
# Resume the most recent session in this directory
kiro-cli chat --resume

# Pick from a list of previous sessions
kiro-cli chat --resume-picker

# List all saved sessions for this directory
kiro-cli chat --list-sessions

# Resume a specific session by ID
kiro-cli chat --resume-id abc123-def456
```

Inside a session, you can also:

```bash
/chat new              # Start a fresh conversation (saves current)
/chat new "add a timer"  # Start fresh with an initial prompt
/chat resume           # Pick a previous session to resume
/chat save ./session.json   # Export session to a file
/chat load ./session.json   # Load a session from a file
```

Sessions are stored per directory, so each project has its own history.

### Exporting conversations (IDE comparison)

In the IDE, you can export a conversation by right-clicking the chat tab and selecting **Export Conversation** — it saves as a markdown file. The CLI equivalent is `/chat save`, which exports to JSON. Both are useful for sharing sessions with teammates or keeping a record of how a feature was built.

---

## 8.10 — Custom Agents

Create specialized agent configurations for different workflows:

```bash
kiro-cli agent list                       # List available agents
kiro-cli agent create code-reviewer       # Create a new agent
kiro-cli agent edit code-reviewer         # Edit its config
kiro-cli agent set-default code-reviewer  # Set as default
```

Then use it:

```bash
kiro-cli chat --agent code-reviewer "Review the latest changes to the game logic"
```

Custom agents let you define different system prompts, tool permissions, and behaviors for different tasks — a code reviewer agent, a documentation agent, a debugging agent.

---

## 8.11 — MCP from the Terminal

Manage MCP servers without touching JSON files:

```bash
kiro-cli mcp list                      # List configured servers
kiro-cli mcp add --name playwright \
  --command "npx" \
  --scope workspace                    # Add a server
kiro-cli mcp status --name playwright  # Check connection status
kiro-cli mcp remove --name playwright  # Remove a server
kiro-cli mcp import --file config.json workspace  # Import from file
```

This is useful when you're setting up a project on a new machine or configuring servers in a remote environment.

---

## 8.12 — Inline Suggestions

The CLI can provide ghost-text suggestions as you type commands in your shell:

```bash
kiro-cli inline enable     # Turn on inline suggestions
kiro-cli inline disable    # Turn off
kiro-cli inline status     # Check current status
```

---

## 8.13 — Kiro Command Router

If you use both the IDE and CLI, the command router lets you control what `kiro` does:

```bash
kiro-cli integrations install kiro-command-router

# Set CLI as the default for the `kiro` command
kiro set-default cli

# Or keep IDE as the default
kiro set-default ide
```

After this:

- `kiro` → launches your default (CLI or IDE)
- `kiro-cli` → always launches CLI
- `kiro ide` → always launches IDE

---

## 8.14 — Housekeeping Commands

A few more commands that round out the CLI experience:

### Update

```bash
kiro-cli update                  # Update to the latest version
kiro-cli update --non-interactive  # Update without confirmation (for scripts)
```

### Theme

```bash
kiro-cli theme --list    # See available themes
kiro-cli theme dark      # Set dark theme
kiro-cli theme light     # Set light theme
kiro-cli theme system    # Follow system preference
```

### Settings

```bash
kiro-cli settings list           # View current settings
kiro-cli settings list --all     # View all available settings with descriptions
kiro-cli settings open           # Open settings file in your editor
kiro-cli settings telemetry.enabled false  # Set a specific setting
```

### Diagnostics

```bash
kiro-cli doctor        # Quick check for common issues
kiro-cli diagnostic    # Full system report (OS, version, environment, config)
```

### Report issues

```bash
kiro-cli issue "Autocomplete not working in zsh"  # Create a GitHub issue
```

---

## 8.15 — Everything from the IDE Works Here Too

One thing to emphasize: the CLI isn't a separate product. It shares:

- **Steering files**: Same `.kiro/steering/` files apply
- **Hooks**: Same `.kiro/hooks/` automation fires
- **MCP servers**: Same `.kiro/settings/mcp.json` config
- **Skills**: Same `.kiro/skills/` packages activate
- **Powers**: Same installed powers activate by keyword
- **Models**: Same model selection and credit system

If you set up steering in Step 4 and hooks in Step 5, they work identically in the CLI. No reconfiguration needed.

---

## 8.16 — Autocomplete

The CLI integrates with your shell for intelligent command completion:

```bash
kiro-cli integrations install    # Set up shell autocomplete
kiro-cli integrations status     # Check if it's active
```

Once installed, tab-completion works for `kiro-cli` commands, subcommands, and flags.

---

## 8.17 — Recap

| Kiro Feature           | How you used it                                                       |
| ---------------------- | --------------------------------------------------------------------- |
| **Kiro CLI**           | Full interactive chat in the terminal                                 |
| **CLI installation**   | One-line install on macOS/Linux/Windows                               |
| **Interactive chat**   | Asked questions and made changes to the game                          |
| **Shell translation**  | Translated natural language to shell commands                         |
| **Inline commands**    | Ran shell commands with `!` without leaving chat                      |
| **Multi-line input**   | Shift+Enter, Ctrl+J, `/editor` for longer prompts                     |
| **Slash commands**     | `/model`, `/tools`, `/compact`, `/context`, `/chat`                   |
| **Context management** | Controlled file context with `/context` and glob patterns             |
| **Session management** | Resumed, listed, saved, and loaded sessions                           |
| **Session export**     | Exported conversations (CLI: `/chat save`, IDE: right-click → Export) |
| **Custom agents**      | Created specialized agent configurations                              |
| **MCP from CLI**       | Managed MCP servers via command line                                  |
| **Inline suggestions** | Ghost-text completions in the shell                                   |
| **Command router**     | Configured `kiro` to launch CLI or IDE                                |
| **Themes**             | Switched terminal UI between dark/light/system                        |
| **Settings**           | Viewed and configured CLI settings                                    |
| **Update**             | Self-updated to the latest version                                    |
| **Autocomplete**       | Shell tab-completion for commands                                     |
| **Shared config**      | Steering, hooks, MCP, skills, powers all work identically             |

---

## Key Takeaway

The Kiro CLI isn't a secondary tool — it's a first-class development experience. Everything you can do in the IDE chat, you can do in the terminal. For developers who live in the command line, it's the natural way to work with Kiro. And because it shares the same steering, hooks, MCP, and skills, switching between IDE and CLI is seamless.

---

## What's Next

That's a wrap. You've covered the full Kiro platform — from scaffolding a game with a picture, to structured specs, skills, steering, hooks, powers, MCP testing, and now the CLI. All on the free tier.

| Step | Feature              | What you did                                                          |
| ---- | -------------------- | --------------------------------------------------------------------- |
| 1    | **IDE + Vibe mode**  | Toured Kiro, scaffolded the game from a picture                       |
| 2    | **Specs**            | Added a backend with structured requirements → design → tasks         |
| 3    | **Skills**           | Installed frontend-design and React best practices skills             |
| 4    | **Steering**         | Taught Kiro your project conventions                                  |
| 5    | **Hooks**            | Automated build checks, accessibility reviews, post-task verification |
| 6    | **Powers**           | Extended Kiro with external service integrations                      |
| 7    | **MCP + Playwright** | Configured raw MCP, tested the game in a real browser                 |
| 8    | **Kiro CLI**         | Full agent experience in the terminal                                 |

From a picture of a tic-tac-toe board to a full-stack, tested, deployable application — built with you driving the decisions. That's Kiro.  

If you have more time, keep on exploring and see what else you can build!
