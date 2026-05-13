# Step 2 — Spec-Driven Development

> **Goal:** Introduce Specs as Kiro's structured approach to building features. Create a spec for adding a backend to the tic-tac-toe game, walk through the three-phase workflow, and execute the tasks.

---

## 2.1 — Why Specs?

In Step 1 you vibed. You threw a picture at Kiro and said "build this" — and it worked great for scaffolding. But what happens when the feature gets more complex?

- What exactly are the requirements?
- How should the backend API look?
- What about error handling, edge cases, data models?
- How do you track progress across multiple tasks?

This is where **Spec-driven development** comes in. Instead of jumping straight into code, Kiro helps you think through the feature first — then builds it systematically.

### Vibe vs Spec: When to use which

|               | Vibe                                  | Spec                                                            |
| ------------- | ------------------------------------- | --------------------------------------------------------------- |
| **Structure** | Freeform conversation                 | Three-phase workflow                                            |
| **Artifacts** | Just code                             | `requirements.md` → `design.md` → `tasks.md`                    |
| **Best for**  | Quick prototypes, simple changes, Q&A | Complex features, team collaboration, anything production-bound |
| **Tracking**  | No formal tracking                    | Task-by-task progress with status updates                       |

Think of it this way: **Vibe is a whiteboard sketch. Spec is the blueprint.**

---

## 2.2 — What You're Building

Your tic-tac-toe game works, but it's purely client-side. Let's add a backend so you can:

- **Save game results**: Track who won, who lost, and draws
- **Leaderboard API**: Serve win/loss stats
- **Game history**: Store and retrieve past games

This is a perfect candidate for a Spec because it touches multiple layers (API design, data model, frontend integration) and has clear requirements you can define upfront.

---

## 2.3 — Creating a Spec

### How to start

There are two ways to create a Spec:

1. **From the Kiro panel** — Click the `+` button under the **Specs** section in the sidebar
2. **From chat** — Open a new session and instead of **Vibe**, select **Spec**

Either way, Kiro will ask you three things:

1. **Describe what you want to do** — This is your prompt
2. **Feature or Bug?** — Choose **Feature**, you're building something new
3. **Requirements** or **Technical Design** — Choose **Requirements**, you want to gather requirements first

### The prompt

Type something like:

```
Add a backend to the tic-tac-toe game.
I want a Node.js/Express API that:
- Saves game results (winner, players, moves, timestamp)
- Provides a leaderboard endpoint with win/loss/draw stats
- Provides a game history endpoint to retrieve past games
- Uses an in-memory data store for now (no database setup needed)
The React frontend should display the leaderboard and game history.
```

Enter the prompt and press enter. Tell Kiro it's a **Feature** and you want to work on **Requirements**. Then watch what happens.

---

## 2.4 — Phase 1: Requirements

Kiro generates `requirements.md` inside `.kiro/specs/<spec-name>/`.

This file contains **user stories** with **acceptance criteria** written in a structured format. You'll see something like:

- **As a player**, I want my game results saved automatically so I can track my performance
- **As a player**, I want to see a leaderboard so I can compare my stats with others
- **As a player**, I want to browse game history so I can review past matches

Each user story has specific acceptance criteria — testable conditions that define "done."

### What to notice

- Kiro didn't just echo your prompt back. It **expanded** your idea into structured requirements with edge cases you might not have thought of.
- The format follows the EARS (Easy Approach to Requirements Syntax) standard.
- You can **edit the requirements** — add stories, remove ones you don't want, tweak acceptance criteria.
- When you're happy, tell Kiro to proceed to design.

### Iterating on requirements

This is a conversation. If something's missing or wrong, just say:

```
Add a requirement for resetting the leaderboard.
Also, the game history should include the full board state for each move, not just the final result.
```

Kiro updates the requirements file. When you're satisfied, move to the next phase in the `requirements.md` file and choose **Continue** and **Generate Design**.

---

## 2.5 — Phase 2: Design

Kiro generates `design.md` — the technical architecture for implementing the requirements.

This typically includes:

- **System architecture**: How the frontend and backend communicate
- **API design**: Endpoints, request/response shapes, status codes
- **Data model**: What the game result and leaderboard objects look like
- **Sequence diagrams**: Flow of a game being saved, leaderboard being fetched
- **Error handling**: What happens when things go wrong
- **Testing strategy**: What to test and how

### What to notice

- The design doc references the requirements, there's traceability between what you want and how you'll build it.
- Sequence diagrams are generated in Mermaid syntax — Kiro renders them visually.
- The API design gives you concrete endpoint definitions before any code is written.
- Again, you can **iterate** — "Use a different API structure" or "Add pagination to the history endpoint."

If you have any changes, Kiro updates the design file. When you're satisfied, move to the next phase in the `design.md` file and choose **Continue** and **Generate Tasks**.

---

## 2.6 — Phase 3: Tasks

Kiro generates `tasks.md` — a list of discrete, executable implementation tasks.

Each task is:

- **Specific**: "Create the Express server with game results POST endpoint"
- **Ordered**: Dependencies are respected (backend before frontend integration)
- **Trackable**: Status updates in real-time (not started → in progress → completed)

### Executing tasks

You have two options:

1. **Run all tasks** — Click the play button and Kiro works through them sequentially
2. **Run one at a time** — Click individual tasks to execute them selectively

### Supervised mode for task execution

This is a great moment to **switch to Supervised mode**. Supervised mode is simply **Autopilot toggled off** — look for the Autopilot toggle in the chat input area and turn it off. That's it, you're now in Supervised mode.

With Supervised mode active, run a task. Kiro will:

1. Write the code for that task
2. Pause and show you the changes as **hunks**
3. Wait for you to **accept**, **reject**, or **discuss** each hunk

This gives you fine-grained control. Accept the API route definition but reject the error handling approach? You can do that. Want to discuss a specific line? Click "Chat inline" on that hunk.

### What to notice during execution

- Watch the task status update in real-time in the Specs panel
- Check the diff view — Kiro is creating backend files, modifying the frontend, adding API calls
- If a task fails or produces something unexpected, just tell Kiro in chat and it adjusts
- The tasks reference the design doc — Kiro is following the plan, not improvising

### Checkpoints: Your Safety Net

As Kiro executes tasks, it creates **checkpoints** — snapshots of your project state that you can revert to at any time. If a task produces bad output or breaks something:

1. Click **Restore** to go back to the checkpoint before that task
2. This undoes both the file changes _and_ the context additions from that task
3. Re-run the task, or give Kiro different instructions and try again

This is different from just undoing file changes — checkpoints restore the full conversation state too. Think of it as a save point in a game. If the boss fight goes badly, reload and try a different strategy.

**Try it out:** After a task completes, intentionally ask Kiro to make a bad change ("rewrite the API to use XML instead of JSON"). Then revert to the checkpoint and watch the project snap back to its previous state. It's a good safety net to have.

You can switch back to **Autopilot** and **Run all Tasks**.

---

## 2.7 — The Three Files

After the spec workflow, you have three artifacts in `.kiro/specs/<spec-name>/`:

```
.kiro/specs/tic-tac-toe-backend/
├── requirements.md    # What you're building (user stories + acceptance criteria)
├── design.md          # How you're building it (architecture + API + data model)
└── tasks.md           # Step-by-step implementation plan (with status tracking)
```

These aren't throwaway chat messages. They're **living documents** that:

- Serve as documentation for the feature
- Can be shared with teammates for review
- Provide context for future changes ("why did we build it this way?")
- Can be referenced in chat using `#spec` context provider

---

## 2.8 — Run and Verify

Once all tasks are complete:

1. Start the backend: `npm run dev` (or however the server is configured)
2. Play a few games in the browser
3. Check the leaderboard — your wins and losses should appear
4. Check game history — past games should be listed

If something's not working, use `#terminal` in chat to share the error output with Kiro and ask it to fix the issue.

---

## 2.9 — Recap

| Kiro Feature             | How you used it                                                   |
| ------------------------ | ----------------------------------------------------------------- |
| **Spec sessions**        | Created a full feature spec with three phases                     |
| **Requirements phase**   | Generated user stories with acceptance criteria from a prompt     |
| **Design phase**         | Got architecture, API design, sequence diagrams, and data models  |
| **Tasks phase**          | Discrete, trackable implementation tasks with real-time status    |
| **Task execution**       | Ran tasks with Autopilot and Supervised modes                     |
| **Supervised mode**      | Reviewed code changes hunk-by-hunk during task execution          |
| **`#` context providers**| Used `#terminal` to share errors; `#file`, `#folder`, `#problems` to pull context into chat |
| **Iterative refinement** | Edited requirements and design through conversation before coding |
| **Checkpoints**          | Reverted to a previous state when a task produced bad output      |

---

## Key Takeaway

Vibe mode is fast and fun — great for getting started. But when the feature matters, Specs give you the structure to think it through before building. Requirements catch missing edge cases. Design docs prevent architectural mistakes. Tasks give you a clear path from plan to code.

The best part: you can mix both. Vibe to explore, Spec to build.

---

## What's Next

You have a full-stack tic-tac-toe game with a backend. In the next step, you'll look at how to teach Kiro about your project's conventions using **Skills** — portable instruction packages that shape how Kiro writes code.
