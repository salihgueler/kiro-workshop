# Step 1 — Kicking Things Off

> **Goal:** Tour Kiro as an IDE, introduce Vibe mode, and scaffold a React + Vite tic-tac-toe app from a single picture.

---

## 1.1 — What You See When You Open Kiro

When you open this project folder in Kiro, you'll notice it looks familiar — it's built on Code OSS, the same open-source foundation as VS Code. Your themes, keybindings, and most extensions carry over.

But look at the left sidebar. There are a few Kiro-specific sections that don't exist in VS Code:

| Panel Section    | What it does                                                                        |
| ---------------- | ----------------------------------------------------------------------------------- |
| **Specs**        | Structured development artifacts (requirements → design → tasks)                    |
| **Steering**     | Persistent project knowledge — markdown files that tell Kiro how your project works |
| **Agent Hooks**  | Event-driven automation — triggers that fire when you save files, run tools, etc.   |
| **MCP Servers**  | External tool integrations via Model Context Protocol                               |
| **Agent Skills** | Portable instruction packages that Kiro activates on demand                         |

We'll use most of these throughout the workshop. For now, just know they're there.

### The Chat Panel

Press `Cmd+L` (Mac) or `Ctrl+L` (Windows/Linux) to open the chat panel. This is where you talk to Kiro. You type natural language, and Kiro reads, writes, and runs code on your behalf.

At the top of the chat, you'll see two things:

1. **Session type picker** — Choose between **Vibe** and **Spec** sessions.
2. **Model picker** — Choose which AI model to use (Auto is the default and recommended).

---

## 1.2 — Vibe Mode: Just Talk and Build

For this first step, we're using **Vibe mode**. It's the conversational, freeform way to work with Kiro — no formal structure, no specs. You just describe what you want and Kiro builds it.

Vibe mode is great for:

- Scaffolding new projects
- Quick prototyping
- Asking questions about code
- Exploratory coding where you don't have a rigid plan yet

We'll switch to Spec mode later when we want more structure. For now, Vibe is perfect.

### Autopilot vs Supervised

Notice the **Autopilot toggle** in the chat panel. It has two modes:

- **Autopilot** (default) — Kiro works autonomously. It creates files, writes code, runs commands, and makes decisions without asking permission at every step. You can always view changes, revert, or interrupt.
- **Supervised** — Kiro pauses after each set of file edits and shows you the changes as "hunks." You accept, reject, or discuss each one before it continues.

For scaffolding, **Autopilot** is the way to go. We'll flip to Supervised later when we want to review changes carefully.

---

## 1.3 — The Starting Point: A Picture

Right now, this project folder has almost nothing in it — just this guide and a picture of what we want to build.

Take a look at the image. It shows a tic-tac-toe game UI — a 3×3 grid, X and O markers, a status display showing whose turn it is, and a reset button.

That picture is our entire specification for step 1. We're going to drag it into Kiro's chat and ask it to build this.

---

## 1.4 — Building the App

### What to do

1. Open the chat panel (`Cmd+L`)
2. Make sure you're in **Vibe** mode and **Autopilot** is on
3. **Drag the tic-tac-toe image** into the chat input (or click the attachment icon)
4. Type a prompt like:

```
Build me a tic-tac-toe game that looks like this image.
Use React with Vite and TypeScript.
Include the game logic — detecting wins, draws, and alternating turns between X and O.
Add a reset button to start a new game.
```

5. Press Enter and watch Kiro work

### What Kiro will do

Kiro will:

- Scaffold a Vite + React + TypeScript project (`npm create vite@latest`)
- Create the game board component with a 3×3 grid
- Implement the game logic (win detection, turn alternation, draw detection)
- Add a status display and reset button
- Style it to match the image
- Install dependencies and verify the build

### Things to point out while it's working

- **Kiro reads the image** — It understands the UI layout from the picture and generates matching components.
- **It runs terminal commands** — Watch the terminal output as Kiro runs `npm install`, `npm run build`, etc.
- **It creates multiple files** — Components, styles, game logic — all in one go.
- **You can see all changes** — Click "View all changes" in the chat to see a diff of everything Kiro created.

---

## 1.5 — Run It

Once Kiro finishes, run the dev server:

```bash
npm run dev
```

Open the URL in your browser (usually `http://localhost:5173`) and play a game. Click cells, watch turns alternate, see the win/draw detection work, and hit reset.

### If something's off

Just tell Kiro in the chat:

```
The board doesn't look right — the cells should be larger and have visible borders.
```

or

```
The win detection isn't working for diagonal wins. Fix it.
```

Kiro will read the existing code, understand the issue, and fix it. This is the conversational loop of Vibe mode — describe, build, iterate.

---

## 1.6 — What We Just Demonstrated

| Kiro Feature             | How we used it                                                        |
| ------------------------ | --------------------------------------------------------------------- |
| **IDE layout**           | Toured the Kiro-specific panels (Specs, Steering, Hooks, MCP, Skills) |
| **Vibe mode**            | Used conversational chat to scaffold a full app                       |
| **Image input**          | Dragged a picture into chat as the "spec"                             |
| **Autopilot**            | Let Kiro work autonomously to create the entire project               |
| **Terminal integration** | Kiro ran npm commands to scaffold, install, and build                 |
| **Model selection**      | Used Auto (or showed the model picker)                                |

---

## What's Next

We have a working tic-tac-toe game. In the next step, we'll add structure — using **Steering** to teach Kiro about our project conventions, and **Specs** to plan and build a feature the audience chooses.

But first — let's play a round. 🎮
