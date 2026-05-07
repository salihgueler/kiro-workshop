# Step 1 — Kicking Things Off

> **Goal:** Tour Kiro's IDE, introduce Vibe mode, and scaffold a React + Vite tic-tac-toe app from a single picture.

---

## 1.1 — Explore the Kiro IDE

When you open this project folder in Kiro, you'll notice it looks familiar, it's built on Code OSS, the same open-source foundation as VS Code. Your themes, keybindings, and most extensions carry over.

Open the Kiro panel in the left sidebar. There are a few Kiro-specific sections that don't exist in VS Code.

| Panel Section    | What it does                                                                        |
| ---------------- | ----------------------------------------------------------------------------------- |
| **Specs**        | Structured development artifacts (requirements → design → tasks)                    |
| **Agent Hooks**  | Event-driven automation — triggers that fire when you save files, run tools, etc.   |
| **Steering**     | Persistent project knowledge — markdown files that tell Kiro how your project works |
| **Agent Skills** | Portable instruction packages that Kiro activates on demand                         |
| **MCP Servers**  | External tool integrations via Model Context Protocol                               |

You'll use most of these throughout the workshop. For now, just note where they are.

### Open the Chat Panel

Press `Cmd+L` (Mac) or `Ctrl+L` (Windows/Linux) to open the chat panel. This is where you talk to Kiro. You type natural language, and Kiro reads, writes, and runs code on your behalf.

At the top of the chat, note two controls:

1. **Session type picker** — Choose between **Vibe** (freeform) and **Spec** (structured) sessions.
2. **Model picker** — Choose which AI model to use. **Auto** is the default and recommended.

### Switching Models

For this workshop, stick with **Auto**, it routes to the optimal model per task. You can try switching to a cheaper model (like Haiku or Qwen3) for a simple question, then switch to Sonnet or Opus for the scaffold. Watch how the credit consumption changes in your usage dashboard. The takeaway: you control the cost/quality tradeoff per interaction.

---

## 1.2 — Vibe Mode: Just Talk and Build

For this step, use **Vibe mode**. It's the conversational, freeform way to work with Kiro, no formal structure, no specs.

Vibe mode is great for:

- Scaffolding new projects
- Quick prototyping
- Asking questions about code
- Exploratory coding without a rigid plan

We'll switch to Spec mode later when we want more structure. For now, Vibe is perfect.

### Autopilot vs Supervised

Notice the **Autopilot toggle** in the chat panel. It has two modes:

- **Autopilot** (default): Kiro works autonomously. It creates files, writes code, runs commands, and makes decisions without asking permission at every step. You can always view changes, revert, or interrupt.
- **Supervised** : Kiro pauses after each set of file edits and shows you the changes as individual "hunks." You accept, reject, or discuss each one before it continues.

For scaffolding, make sure **Autopilot** is on. You'll use Supervised later when reviewing changes carefully.

---

## 1.3 — The Starting Point: A Picture

Right now, this project folder has almost nothing in it, just these guides.

Find or create an image of what you want to build. This could be a screenshot, a sketch on paper that you photograph, a wireframe, or any visual reference.Try tic-tac-toe for your first try. Take a look at the image. It shows a tic-tac-toe game UI — a 3×3 grid, X and O markers, a status display showing whose turn it is, and a reset button.

The image is your entire specification for this step, drag it into Kiro's chat and ask it to build what it sees.
---

## 1.4 — Build the App

1. Open the chat panel (`Cmd+L`).
2. Confirm you're in **Vibe** mode with **Autopilot** on.
3. Drag your image into the chat input (or click the attachment icon).
4. Type a prompt like:

   ```
   Build me a tic-tac-toe game that looks like this image.
   Use React with Vite and TypeScript.
   Include the game logic — detecting wins, draws, and alternating turns between X and O.
   Add a reset button to start a new game.
   ```

5. Press Enter and watch Kiro work.

### What to expect

Kiro will:

- Scaffold a Vite + React + TypeScript project (`npm create vite@latest`)
- Create the game board component with a 3×3 grid
- Implement game logic (win detection, turn alternation, draw detection)
- Add a status display and reset button
- Style it to match the image
- Install dependencies and verify the build

### Things to notice while it's working

- **Image understanding**: Kiro reads the UI layout from the picture and generates matching components.
- **Terminal commands**: Watch the terminal output as Kiro runs `npm install`, `npm run build`, etc.
- **Multi-file creation**: Components, styles, game logic — all created in one go.
- **Change visibility**: Click "View all changes" in the chat to see a diff of everything Kiro created.

---

## 1.5 — Run the App

Once Kiro finishes, start the dev server:

```bash
npm run dev
```

Kiro can also run dev servers in the background without blocking the chat. If Kiro started the server for you during the build, you'll see it running already — you can keep chatting while it serves the app. This is the **Dev Servers** feature: long-running processes that don't lock up your conversation.

Open the URL in your browser (usually `http://localhost:5173`) and play a game. Click cells, watch turns alternate, see the win/draw detection work, and hit reset.

### If something's off

Tell Kiro in the chat. For example:

```
The board doesn't look right — the cells should be larger and have visible borders.
```

or

```
The win detection isn't working for diagonal wins. Fix it.
```

Kiro reads the existing code, understands the issue, and fixes it. This is the conversational loop of Vibe mode — describe, build, iterate.

---

## 1.6 — Recap

| Kiro Feature             | How you used it                                                       |
| ------------------------ | --------------------------------------------------------------------- |
| **IDE layout**           | Toured the Kiro-specific panels (Specs, Steering, Hooks, MCP, Skills) |
| **Vibe mode**            | Used conversational chat to scaffold a full app                       |
| **Image input**          | Dragged a picture into chat as the "spec"                             |
| **Autopilot**            | Let Kiro work autonomously to create the entire project               |
| **Terminal integration** | Kiro ran npm commands to scaffold, install, and build                 |
| **Model switching**      | Explored the model picker and credit multiplier tradeoffs             |
| **Dev servers**          | Ran the dev server in the background without blocking chat            |

---

## What's Next

You have a working tic-tac-toe game. In the next step, you'll use **Spec-driven development** to plan and build a backend feature with structure and rigor.
