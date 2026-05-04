# Step 9 — Headless Mode

> **Goal:** Run Kiro without any UI — no IDE, no interactive terminal. Demonstrate headless mode for CI/CD pipelines, automated code reviews, and scripted workflows.

---

## 9.1 — What Is Headless Mode?

Every step so far has had a human in the loop — typing prompts, reviewing changes, clicking buttons. Headless mode removes that.

**Headless mode** lets you run Kiro CLI non-interactively: pass a prompt as an argument, Kiro executes it end-to-end, and prints the result. No terminal UI, no user input, no approval dialogs.

This turns Kiro into a tool you can call from:

- **CI/CD pipelines** — GitHub Actions, GitLab CI, Jenkins, CodePipeline
- **Shell scripts** — Automate repetitive development tasks
- **Cron jobs** — Scheduled code quality checks
- **Other tools** — Any system that can invoke a CLI command

---

## 9.2 — Setting Up

### Generate an API key

Headless mode authenticates with an API key instead of a browser login.

1. Go to your [Kiro account settings](https://app.kiro.dev/)
2. Generate an API key
3. Store it securely (you'll need it as an environment variable)

### Set the environment variable

```bash
export KIRO_API_KEY=your-api-key-here
```

In CI/CD, store this as a secret (e.g., GitHub Actions secret, GitLab CI variable).

### Run your first headless command

```bash
kiro-cli chat --no-interactive "List all TypeScript files in this project"
```

That's it. Kiro reads the prompt, executes it, and prints the output to stdout.

---

## 9.3 — Tool Trust

In interactive mode, Kiro asks for permission before using certain tools (writing files, running commands). In headless mode, there's no one to ask. You grant permissions upfront with flags:

### Trust everything

```bash
kiro-cli chat --no-interactive --trust-all-tools "Write tests and run them"
```

### Trust specific categories (recommended)

```bash
kiro-cli chat --no-interactive --trust-tools=read,grep "Find all TODO comments in src/"
```

Available tool categories: `read`, `write`, `grep`, `shell`, and more.

**Best practice:** Use `--trust-tools` with specific categories instead of `--trust-all-tools`. Follow the principle of least privilege — only grant what the task needs.

---

## 9.4 — Demo: Automated Code Review

Let's use headless mode to review our tic-tac-toe codebase:

```bash
kiro-cli chat --no-interactive --trust-tools=read,grep \
  "Review the tic-tac-toe codebase for potential bugs, security issues, and performance problems. Report your findings as a structured list."
```

Kiro reads the code, analyzes it, and prints a report — all without any interactive input. This is exactly what you'd run in a PR check.

---

## 9.5 — Demo: Build Verification

```bash
kiro-cli chat --no-interactive --trust-all-tools \
  "Run npm run build for the tic-tac-toe project. If it succeeds, report the bundle size. If it fails, explain the errors and suggest fixes."
```

Kiro runs the build, interprets the output, and gives you a human-readable summary. In a pipeline, you could use the exit code to gate deployments.

---

## 9.6 — Piping Context In

One of headless mode's most powerful patterns: pipe data into the prompt for richer context.

### Review a git diff

```bash
git diff | kiro-cli chat --no-interactive "Review these changes for bugs and security issues"
```

### Troubleshoot a build failure

```bash
cat build-error.log | kiro-cli chat --no-interactive "Explain this build failure and suggest a fix"
```

### Analyze test results

```bash
npm test 2>&1 | kiro-cli chat --no-interactive "Summarize the test results. Which tests failed and why?"
```

This pattern is incredibly flexible — any text output can become context for Kiro.

---

## 9.7 — CI/CD Integration: GitHub Actions

Here's a complete GitHub Actions workflow that reviews every PR with Kiro:

```yaml
name: Kiro Code Review
on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install Kiro CLI
        run: curl -fsSL https://cli.kiro.dev/install | bash

      - name: Review PR changes
        env:
          KIRO_API_KEY: ${{ secrets.KIRO_API_KEY }}
        run: |
          kiro-cli chat --no-interactive --trust-tools=read,grep \
            "Review the changes in this PR for security issues, bugs, and code quality problems. Be concise."
```

### More CI/CD patterns

| Pattern                 | Command                                                                                                          |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **Code review on PR**   | `kiro-cli chat --no-interactive --trust-tools=read,grep "Review this PR for issues"`                             |
| **Generate tests**      | `kiro-cli chat --no-interactive --trust-all-tools "Write tests for the auth module and run them"`                |
| **Troubleshoot build**  | `cat build.log \| kiro-cli chat --no-interactive "Explain this failure"`                                         |
| **Documentation check** | `kiro-cli chat --no-interactive --trust-tools=read "Is the README up to date with the code?"`                    |
| **Security scan**       | `kiro-cli chat --no-interactive --trust-tools=read,grep "Scan for hardcoded secrets"`                            |
| **Dependency audit**    | `kiro-cli chat --no-interactive --trust-tools=read "Check package.json for outdated or vulnerable dependencies"` |

---

## 9.8 — Flags Reference

| Flag                         | What it does                                                  |
| ---------------------------- | ------------------------------------------------------------- |
| `--no-interactive`           | Run without interactive session (requires prompt as argument) |
| `--trust-all-tools`          | Auto-approve all tool calls                                   |
| `--trust-tools=<categories>` | Auto-approve specific tool categories                         |
| `--require-mcp-startup`      | Exit with error if any MCP server fails to connect            |
| `--agent <name>`             | Use a specific custom agent configuration                     |
| `-v` / `-vv` / `-vvv`        | Increase logging verbosity                                    |

---

## 9.9 — Best Practices

- **Store `KIRO_API_KEY` as a secret** — Never hardcode it in pipeline configs or commit it to source control.
- **Use `--trust-tools` over `--trust-all-tools`** — Principle of least privilege. Only grant what the task needs.
- **Add `--require-mcp-startup`** when your pipeline depends on MCP servers — fail fast instead of hanging.
- **Pipe context in** — `git diff`, build logs, test output. The more context, the better the result.
- **Check exit codes** — Use them to gate pipeline stages (deploy only if review passes).
- **Rotate API keys regularly** — Revoke unused keys from the Kiro portal.

---

## 9.10 — Limitations

- You must provide the full prompt as an argument — no mid-session follow-ups.
- Interactive slash commands (`/model`, `/agent` picker) are not available.
- Terminal UI features are disabled.
- API key authentication is required (Pro, Pro+, or Power tier).

---

## 9.11 — What We Just Demonstrated

| Kiro Feature              | How we used it                                                   |
| ------------------------- | ---------------------------------------------------------------- |
| **Headless mode**         | Non-interactive execution with `--no-interactive`                |
| **API key auth**          | `KIRO_API_KEY` environment variable                              |
| **Tool trust**            | `--trust-all-tools` and `--trust-tools` for unattended execution |
| **Automated code review** | Reviewed the tic-tac-toe codebase without interaction            |
| **Build verification**    | Ran the build and interpreted results programmatically           |
| **Piping context**        | Fed git diffs and build logs into Kiro                           |
| **GitHub Actions**        | Full CI/CD workflow for PR reviews                               |
| **Best practices**        | Least privilege, secret management, exit codes                   |

---

## Key Takeaway

Headless mode is where Kiro stops being a tool you use and becomes a tool that works for you. It runs in the background, in your pipelines, on every push — reviewing code, catching bugs, verifying builds, and enforcing standards. No human needed.

The same steering files, hooks, and MCP servers you configured in the IDE apply in headless mode too. The conventions you defined in Step 4 are enforced automatically in CI/CD.

---

## Wrapping Up the Workshop

We've now covered the full Kiro platform:

| Step | Feature              | What we did                                                           |
| ---- | -------------------- | --------------------------------------------------------------------- |
| 1    | **IDE + Vibe mode**  | Toured Kiro, scaffolded the game from a picture                       |
| 2    | **Specs**            | Added a backend with structured requirements → design → tasks         |
| 3    | **Skills**           | Installed frontend-design and React best practices skills             |
| 4    | **Steering**         | Taught Kiro our project conventions                                   |
| 5    | **Hooks**            | Automated build checks, accessibility reviews, post-task verification |
| 6    | **Powers**           | Extended Kiro with external service integrations                      |
| 7    | **MCP + Playwright** | Configured raw MCP, tested the game in a real browser                 |
| 8    | **Kiro CLI**         | Full agent experience in the terminal                                 |
| 9    | **Headless mode**    | Kiro in CI/CD pipelines — no human in the loop                        |

From a picture of a tic-tac-toe board to a full-stack, tested, deployable application with automated CI/CD — built with an audience driving the decisions. That's Kiro. 🎮
