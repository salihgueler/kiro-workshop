# Step 8 — Kiro CLI

> **Goal:** Take Kiro out of the IDE and into the terminal. Show the Kiro CLI as a full development experience — interactive chat, custom agents, shell translation, session management, and MCP from the command line.

---

## 8.1 — Kiro Beyond the IDE

Everything we've done so far has been inside the Kiro IDE. But not every developer lives in an IDE. Some prefer the terminal. Some work over SSH. Some want Kiro alongside their existing tmux/vim/emacs workflow.

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
cd my-tic-tac-toe-project
kiro-cli
```

This drops you into an interactive chat session with a rich terminal UI — syntax-highlighted code, interactive panels, and visual tool progress. It's not a bare-bones text prompt; it's a polished terminal experience.

### Ask questions directly

You can also pass a question as an argument to skip the interactive startup:

```bash
kiro-cli chat "Explain the game logic in Board.tsx"
```

### What to demo

Open a terminal, `cd` into the tic-tac-toe project, and run `kiro-cli`. Ask it to do something with the game:

```
Explain how the win detection works in our tic-tac-toe game
```

Then ask it to make a change:

```
Add a move counter that shows how many moves have been played in the current game
```

Show that it reads files, writes code, runs commands, and follows the steering files we set up in Step 4 — all from the terminal.

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

## 8.6 — Context Management

Control what files Kiro sees in the current session:

```bash
/context show              # See what's in context with per-file token usage
/context add "src/**/*.ts" # Add files by glob pattern
/context remove src/app.js # Remove a file
/context clear             # Remove all context rules
```

This is the CLI equivalent of the `#file` and `#folder` context providers in the IDE — but with glob patterns for more precise control.

---

## 8.7 — Session Management

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

---

## 8.8 — Custom Agents

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

## 8.9 — MCP from the Terminal

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

## 8.10 — Inline Suggestions

The CLI can provide ghost-text suggestions as you type commands in your shell:

```bash
kiro-cli inline enable     # Turn on inline suggestions
kiro-cli inline disable    # Turn off
kiro-cli inline status     # Check current status
```

---

## 8.11 — Kiro Command Router

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

## 8.12 — What We Just Demonstrated

| Kiro Feature           | How we used it                                            |
| ---------------------- | --------------------------------------------------------- |
| **Kiro CLI**           | Full interactive chat in the terminal                     |
| **CLI installation**   | One-line install on macOS/Linux/Windows                   |
| **Interactive chat**   | Asked questions and made changes to the game              |
| **Shell translation**  | Translated natural language to shell commands             |
| **Inline commands**    | Ran shell commands with `!` without leaving chat          |
| **Context management** | Controlled file context with `/context` and glob patterns |
| **Session management** | Resumed, listed, saved, and loaded sessions               |
| **Custom agents**      | Created specialized agent configurations                  |
| **MCP from CLI**       | Managed MCP servers via command line                      |
| **Inline suggestions** | Ghost-text completions in the shell                       |
| **Command router**     | Configured `kiro` to launch CLI or IDE                    |

---

## Key Takeaway

The Kiro CLI isn't a secondary tool — it's a first-class development experience. Everything you can do in the IDE chat, you can do in the terminal. For developers who live in the command line, it's the natural way to work with Kiro. And because it shares the same steering, hooks, MCP, and skills, switching between IDE and CLI is seamless.

---

## What's Next

We've used Kiro interactively — in the IDE and now in the terminal. In the next step, we'll remove the human from the loop entirely with **headless mode**, running Kiro in CI/CD pipelines to automate code reviews, test generation, and build troubleshooting.
