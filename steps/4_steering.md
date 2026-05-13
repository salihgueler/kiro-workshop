# Step 4 — Steering

> **Goal:** Teach Kiro about your project's conventions using Steering files. Generate foundational docs, create custom steering for your API and component patterns, and see how inclusion modes control when context loads.

---

## 4.1 — Why Steering?

In Step 3 you installed Skills — general knowledge from the community (design principles, React patterns). Skills are great, but they don't know anything about _your_ project.

Steering fills that gap. It's how you teach Kiro:

- What tech stack you're using and why
- How your project is organized
- What coding conventions your team follows
- How your API endpoints should be structured
- What patterns to use (and what to avoid)

Without steering, every new chat session starts from scratch — you'd have to re-explain your conventions each time. With steering, Kiro remembers. It's like onboarding a new teammate who actually reads the wiki.

---

## 4.2 — Where Steering Files Live

Steering files are markdown files stored in specific locations:

| Scope         | Location            | Applies to        |
| ------------- | ------------------- | ----------------- |
| **Workspace** | `.kiro/steering/`   | This project only |
| **Global**    | `~/.kiro/steering/` | All your projects |

Workspace steering takes priority over global steering if they conflict. This lets you set global defaults ("I always use TypeScript") while overriding per-project ("this project uses Tailwind, not CSS modules").

---

## 4.3 — Generating Foundational Steering Files

Kiro can auto-generate three foundational files that capture the basics of your project. Let's do that now.

### How to generate them

1. Open the **Steering** section in the Kiro panel (left sidebar)
2. Click the **Generate Steering Docs** button (or click `+` → **Protect steering files**)
3. Kiro analyzes your codebase and generates three files

### The three foundational files

**`product.md`** — What this project is about

Kiro will describe the tic-tac-toe game: its purpose, target users, key features (game board, leaderboard, game history), and the fact that it's a workshop demo project.

**`tech.md`** — The technology stack

Kiro will document: React + Vite + TypeScript for the frontend, Node.js + Express for the backend, in-memory data store, and any libraries it installed during the build.

**`structure.md`** — How the project is organized

Kiro will map out the file structure: where components live, where API routes are defined, where styles go, and the naming conventions already in use.

### What to notice

- Kiro didn't just list files — it understood the _architecture_. It knows the frontend and backend are separate, it knows which components handle game logic vs display.
- These files are **always included** by default — every future chat session will have this context automatically.
- You can (and should) edit them. If Kiro got something wrong or missed a convention, fix it. These are your docs.

---

## 4.4 — Creating Custom Steering Files

Foundational files cover the basics. Custom steering files let you encode specific conventions. Let's create two.

### Custom file 1: API conventions

Create a new steering file for your backend API patterns.

1. In the Steering section, click `+`
2. Choose **Workspace** scope
3. Name it `api-conventions.md`

Write something like:

````markdown
---
inclusion: fileMatch
fileMatchPattern: "server/**/*.ts"
---

# API Conventions

## Endpoint structure

- All endpoints are prefixed with `/api/v1/`
- Use plural nouns for resources: `/api/v1/games`, `/api/v1/players`
- Use HTTP methods correctly: GET for reads, POST for creates, PUT for updates, DELETE for deletes

## Response format

All API responses follow this shape:

\```json
{
"success": true,
"data": { ... },
"error": null
}
\```

On error:

\```json
{
"success": false,
"data": null,
"error": { "code": "NOT_FOUND", "message": "Game not found" }
}
\```

## Error handling

- Always return appropriate HTTP status codes (200, 201, 400, 404, 500)
- Never expose internal error details to the client
- Log errors server-side with timestamps

## Naming

- Route files: `<resource>.routes.ts`
- Handler functions: `get<Resource>`, `create<Resource>`, `update<Resource>`
````

### Custom file 2: Component patterns

Create another for your React component conventions:

```markdown
---
inclusion: fileMatch
fileMatchPattern: ["src/components/**/*.tsx", "src/components/**/*.ts"]
---

# Component Conventions

## File structure

- One component per file
- Component file name matches the export: `GameBoard.tsx` exports `GameBoard`
- Co-locate styles with components: `GameBoard.tsx` + `GameBoard.css`

## Component patterns

- Use functional components with hooks (no class components)
- Props interface named `<Component>Props`: `GameBoardProps`
- Destructure props in the function signature
- Keep components focused — if it's doing too much, split it

## State management

- Use `useState` for local component state
- Lift state up to the nearest common ancestor when shared
- Use functional setState when the new value depends on the previous value

## Accessibility

- All interactive elements must have accessible labels
- Use semantic HTML elements (button, nav, main, section)
- Ensure keyboard navigation works for all game interactions
```

---

## 4.5 — Inclusion Modes Explained

Notice the `inclusion` and `fileMatchPattern` in the front matter above. Steering files support four inclusion modes that control _when_ they load into context:

### Always (default)

```yaml
---
inclusion: always
---
```

Loaded in every single interaction. Use for core standards that apply everywhere — like the foundational files.

### Conditional (fileMatch)

```yaml
---
inclusion: fileMatch
fileMatchPattern: "server/**/*.ts"
---
```

Loaded only when you're working with files that match the pattern. Your API conventions file only loads when you're touching backend code — no point cluttering context when you're working on the frontend.

### Manual

```yaml
---
inclusion: manual
---
```

Available on-demand by typing `#steering-name` in chat or selecting it from the `/` slash command menu. Use for specialized guides you only need occasionally — like a deployment checklist or migration procedure.

### Auto

```yaml
---
inclusion: auto
name: game-balance
description: Guidelines for game balance and fairness. Use when modifying game logic or win conditions.
---
```

Loaded automatically when your request matches the description. Similar to how Skills work — Kiro reads the description and decides if it's relevant.

### Why this matters

Without inclusion modes, every steering file loads into every conversation. That works fine with 3 files, but a real project might have 10–15 steering files covering API design, testing patterns, deployment, security, accessibility, and more. Inclusion modes keep context focused — Kiro only loads what's relevant to the current task.

---

## 4.6 — See It in Action

Now let's prove steering works. Open a Vibe chat and ask Kiro to add a new API endpoint:

```
Add a new endpoint to get a single game by ID.
Follow the existing API conventions.
```

### What to watch for

Because the `api-conventions.md` steering file has `fileMatch` on `server/**/*.ts`, it will load automatically when Kiro starts editing backend files. You should see Kiro:

- Use the `/api/v1/games/:id` URL pattern (not `/game/:id` or `/getGame`)
- Return the standard `{ success, data, error }` response shape
- Name the handler `getGame` (following the `get<Resource>` convention)
- Use proper HTTP status codes (200 for found, 404 for not found)
- Name the file following the existing pattern

If Kiro _doesn't_ follow a convention, that's actually a great teaching moment — update the steering file to be more specific and try again.

---

## 4.7 — File References

Steering files can reference other files in your workspace using a special syntax:

```markdown
#[[file:api/openapi.yaml]]
```

This tells Kiro to pull in the contents of that file when the steering file loads. Useful for:

- Linking to an OpenAPI spec so Kiro follows your API contract
- Referencing a component as a "template" for new components
- Pointing to config files that define project constraints

For example, you could add to your API conventions file:

```markdown
Reference the existing game routes for patterns: #[[file:server/routes/games.routes.ts]]
```

Now Kiro will see the actual code alongside the conventions — making it much more likely to match the existing style.

---

## 4.8 — AGENTS.md Support

Quick note: Kiro also supports the `AGENTS.md` standard. If you already have an `AGENTS.md` file in your repo root (common if you use Claude Code or other tools), Kiro picks it up automatically. It's always included, no front matter needed.

This means your steering setup can work across multiple AI tools — `AGENTS.md` for cross-tool conventions, `.kiro/steering/` for Kiro-specific guidance.

---

## 4.9 — Recap

| Kiro Feature              | How you used it                                                   |
| ------------------------- | ----------------------------------------------------------------- |
| **Foundational steering** | Auto-generated product.md, tech.md, and structure.md              |
| **Custom steering files** | Created API conventions and component patterns                    |
| **Inclusion modes**       | Used `always` (foundational) and `fileMatch` (conditional)        |
| **File references**       | Linked to workspace files from steering docs                      |
| **AGENTS.md**             | Mentioned cross-tool compatibility                                |
| **Steering in action**    | Added an API endpoint that followed your conventions automatically|

---

## Key Takeaway

Skills give Kiro general expertise. Steering gives Kiro _your_ expertise. Together, they mean Kiro writes code that's not just technically good — it's good _for your project_, following your patterns, your naming, your architecture.

The foundational files take 30 seconds to generate. Custom steering files take a few minutes to write. The payoff is every future interaction being more accurate and consistent.

---

## What's Next

You've taught Kiro how your project works. In the next step, you'll automate things with **Hooks** — setting up triggers that fire automatically when you save files, run tools, or complete tasks.
