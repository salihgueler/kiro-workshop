# Step 5 — Hooks

> **Goal:** Set up event-driven automation with Agent Hooks. Create hooks that lint on save, verify accessibility before file writes, and run the build after spec tasks complete.

---

## 5.1 — Why Hooks?

In Step 4 we taught Kiro our conventions with Steering. But conventions only work if they're enforced. Right now, nothing stops Kiro (or us) from writing code that breaks the rules.

Hooks close that gap. They're automated triggers that fire when something happens in the IDE — a file is saved, a tool is about to run, a spec task finishes. When the trigger fires, Kiro either runs a shell command or sends itself a prompt.

Think of hooks as CI/CD for your editor. Instead of waiting for a pipeline to catch problems after you push, hooks catch them _while you're working_.

---

## 5.2 — How Hooks Work

Every hook has two parts:

1. **When** — The event that triggers it
2. **Then** — The action to take

### Trigger events

| Event               | Fires when...                                                 |
| ------------------- | ------------------------------------------------------------- |
| `fileEdited`        | You save a file                                               |
| `fileCreated`       | A new file is created                                         |
| `fileDeleted`       | A file is deleted                                             |
| `promptSubmit`      | You send a message to the agent                               |
| `agentStop`         | The agent finishes a turn                                     |
| `preToolUse`        | Before Kiro is about to use a tool (read, write, shell, etc.) |
| `postToolUse`       | After Kiro uses a tool                                        |
| `preTaskExecution`  | Before a spec task starts                                     |
| `postTaskExecution` | After a spec task completes                                   |
| `userTriggered`     | You click a manual trigger button                             |

### Actions

| Action       | What it does                                            |
| ------------ | ------------------------------------------------------- |
| `askAgent`   | Sends a prompt to Kiro — Kiro reads it and acts on it   |
| `runCommand` | Executes a shell command — output is captured and shown |

---

## 5.3 — Creating Hooks

There are three ways to create a hook:

### 1. Ask Kiro (natural language)

1. Open the **Agent Hooks** section in the Kiro panel
2. Click `+`
3. Select **Ask Kiro to create a hook**
4. Describe what you want: "Run the linter every time I save a TypeScript file"
5. Review the generated config and click **Save Hook**

### 2. Manual form

1. Click `+` → **Manually create a hook**
2. Fill in: title, description, event type, file patterns or tool types, action, and the prompt or command
3. Click **Create Hook**

### 3. Command Palette

`Cmd+Shift+P` → "Kiro: Open Kiro Hook UI"

For this workshop, we'll use the natural language approach — it's faster and shows off Kiro's ability to generate hook configs from plain English.

---

## 5.4 — Hook 1: Build on Save

Let's start simple. We want to run the TypeScript compiler every time we save a `.ts` or `.tsx` file to catch type errors immediately.

### What to say

In the hook creation dialog:

```
Run "npm run build" every time a TypeScript file is saved.
```

### What Kiro generates

```json
{
  "name": "Build on Save",
  "version": "1.0.0",
  "description": "Runs the build when TypeScript files are saved to catch type errors early",
  "when": {
    "type": "fileEdited",
    "patterns": ["*.ts", "*.tsx"]
  },
  "then": {
    "type": "runCommand",
    "command": "npm run build"
  }
}
```

### What to point out

- The hook is a simple JSON file stored in `.kiro/hooks/`
- `fileEdited` + `patterns` means it only fires for TypeScript files, not every save
- `runCommand` runs the build in the background — you see the output but it doesn't block your work
- If the build fails, you'll see the errors immediately

### See it fire

Make a small change to a `.ts` file — add a type error on purpose (like assigning a string to a number). Save the file. Watch the hook fire and the build fail with a clear error message.

Fix the error, save again, and the build passes. Instant feedback loop.

---

## 5.5 — Hook 2: Accessibility Review on Write

This one's more interesting. We want Kiro to review every file it writes for accessibility issues _before_ the write happens.

### What to say

```
Before Kiro writes any file, review the changes for accessibility issues —
missing aria labels, non-semantic HTML, keyboard navigation gaps.
If there are problems, fix them before writing.
```

### What Kiro generates

```json
{
  "name": "Accessibility Review",
  "version": "1.0.0",
  "description": "Reviews file writes for accessibility compliance before they happen",
  "when": {
    "type": "preToolUse",
    "toolTypes": ["write"]
  },
  "then": {
    "type": "askAgent",
    "prompt": "Before writing this file, review the changes for accessibility issues: missing aria-labels on interactive elements, non-semantic HTML (div instead of button/nav/main), missing keyboard navigation support, insufficient color contrast considerations, and missing alt text on images. If you find issues, fix them in the content before proceeding with the write."
  }
}
```

### What to point out

- `preToolUse` fires _before_ Kiro writes — it's a gate, not a post-check
- `toolTypes: ["write"]` targets only write operations (not reads, shell commands, etc.)
- `askAgent` sends a prompt to Kiro itself — Kiro reviews its own work before committing it
- This is like having an accessibility reviewer built into your workflow

### See it fire

Ask Kiro to add a new UI element — maybe a settings button or a player name input. Watch the hook fire before the file write. Kiro will review the code for accessibility and fix any issues before saving.

If you want to make it dramatic, ask Kiro to create a component with a `<div onClick={...}>` and watch the hook catch it and replace it with a `<button>`.

---

## 5.6 — Hook 3: Run Build After Spec Tasks

When we're executing spec tasks (from Step 2), we want to make sure each completed task doesn't break the build.

### What to say

```
After each spec task completes, run the build to verify nothing is broken.
```

### What Kiro generates

```json
{
  "name": "Post-Task Build Check",
  "version": "1.0.0",
  "description": "Runs the build after each spec task to catch regressions early",
  "when": {
    "type": "postTaskExecution"
  },
  "then": {
    "type": "runCommand",
    "command": "npm run build"
  }
}
```

### What to point out

- `postTaskExecution` ties into the Spec workflow from Step 2
- Every time a task is marked complete, the build runs automatically
- If a task breaks something, you know immediately — not three tasks later
- This is especially valuable when running all tasks at once in Autopilot mode

---

## 5.7 — Review What Changed: `#git diff`

We've been creating hooks and making changes. Let's pause and review everything that's changed using the `#git diff` context provider.

In the chat, type:

```
#git diff What have we changed in this step? Summarize the hooks we added and any code modifications.
```

Kiro reads the current git diff and gives you a summary. This is a natural "pause and review" moment — useful anytime you want to see the big picture before committing.

Other context providers worth trying here:

- `#codebase` — "How is our hook system set up?" (Kiro finds the relevant files automatically)
- `#file .kiro/hooks/build-on-save.json` — Reference a specific hook file in your question

---

## 5.8 — Managing Hooks

### Viewing hooks

All hooks appear in the **Agent Hooks** section of the Kiro panel. Each shows:

- Name and description
- Trigger type
- Whether it's enabled or disabled

### Enabling / disabling

Click the toggle next to any hook to enable or disable it without deleting it. Useful when a hook is noisy during exploratory work but valuable during focused development.

### Hook files on disk

Hooks live in `.kiro/hooks/` as JSON files:

```
.kiro/hooks/
├── build-on-save.json
├── accessibility-review.json
└── post-task-build-check.json
```

They're version-controllable — commit them to your repo and the whole team gets the same automation.

---

## 5.9 — Hook Patterns Worth Knowing

Beyond what we built, here are patterns the audience might want to try:

| Pattern                | Event                      | Action                                                     | Use case             |
| ---------------------- | -------------------------- | ---------------------------------------------------------- | -------------------- |
| Format on save         | `fileEdited` + `*.ts`      | `runCommand`: `npx prettier --write {file}`                | Auto-format code     |
| Commit message review  | `preToolUse` + `shell`     | `askAgent`: "Review this git commit for sensitive data"    | Prevent secret leaks |
| Test after changes     | `fileEdited` + `*.test.ts` | `runCommand`: `npm test`                                   | Run tests on save    |
| Documentation reminder | `postToolUse` + `write`    | `askAgent`: "Check if this change needs a README update"   | Keep docs current    |
| Manual deploy check    | `userTriggered`            | `askAgent`: "Review the codebase for deployment readiness" | On-demand audit      |

---

## 5.10 — What We Just Demonstrated

| Kiro Feature                  | How we used it                                              |
| ----------------------------- | ----------------------------------------------------------- |
| **Agent Hooks**               | Created three hooks with different trigger types            |
| **fileEdited trigger**        | Build on save for TypeScript files                          |
| **preToolUse trigger**        | Accessibility review before file writes                     |
| **postTaskExecution trigger** | Build verification after spec tasks                         |
| **runCommand action**         | Executed shell commands automatically                       |
| **askAgent action**           | Had Kiro review its own work before writing                 |
| **Natural language creation** | Described hooks in plain English, Kiro generated the config |
| **Hook management**           | Showed enable/disable and file structure                    |
| **`#git diff` context**       | Reviewed all changes made during the step                   |
| **Context providers**         | Used `#codebase` and `#file` to reference project files     |

---

## Key Takeaway

Steering tells Kiro what the rules are. Hooks enforce them automatically. Together, they create a development environment where conventions aren't just documented — they're actively maintained every time you save a file, write code, or complete a task.

The best hooks are the ones you forget about. They just work in the background, catching issues before they become problems.

---

## What's Next

We've got conventions (Steering) and automation (Hooks). In the next step, we'll extend Kiro's capabilities with **Powers** — installing curated tool bundles that give Kiro access to external services and specialized knowledge.
