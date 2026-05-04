# Step 3 — Adding Skills

> **Goal:** Install community skills to level up Kiro's output — Anthropic's frontend-design skill for stunning UI and Vercel's React best practices for production-quality code. Then use them to redesign our tic-tac-toe game.

---

## 3.1 — What Are Skills?

Skills are portable instruction packages that follow the open [Agent Skills](https://agentskills.io/) standard. They give Kiro specialized knowledge it doesn't have out of the box — design principles, framework best practices, deployment workflows, you name it.

The key idea is **progressive disclosure**:

1. **Discovery** — At startup, Kiro loads only the name and description of each installed skill (lightweight).
2. **Activation** — When your request matches a skill's description, Kiro loads the full instructions into context.
3. **Execution** — Kiro follows the instructions, pulling in scripts or reference files only as needed.

This means you can have dozens of skills installed without bloating every conversation. Kiro only loads what's relevant.

### Where skills live

| Scope         | Location          | Use case                               |
| ------------- | ----------------- | -------------------------------------- |
| **Workspace** | `.kiro/skills/`   | Project-specific workflows             |
| **Global**    | `~/.kiro/skills/` | Personal workflows across all projects |

### How to use them

Skills activate automatically when your request matches their description. You can also invoke them explicitly by typing `/` in chat and selecting the skill from the slash command list.

---

## 3.2 — The Two Skills We're Installing

### 1. Anthropic's Frontend Design Skill

**What it does:** Guides Kiro to create distinctive, production-grade frontend interfaces that avoid generic "AI slop" aesthetics. It pushes for bold design choices — unique typography, cohesive color themes, animations, spatial composition, and textural details.

**Why we want it:** Our tic-tac-toe game probably looks... fine. Generic. This skill will make Kiro think like a designer before writing CSS — choosing an aesthetic direction, picking distinctive fonts, adding motion and atmosphere.

**Key principles it teaches Kiro:**

- Think about purpose, tone, and differentiation before coding
- Choose bold typography (no Inter, no Roboto, no Arial)
- Commit to a cohesive color theme with dominant colors and sharp accents
- Add motion — page load animations, hover states, micro-interactions
- Create atmosphere with textures, gradients, shadows, and depth
- Never produce cookie-cutter, predictable layouts

### 2. Vercel's React Best Practices Skill

**What it does:** A comprehensive performance optimization guide with 70 rules across 8 categories, maintained by Vercel Engineering. Covers everything from eliminating waterfalls to bundle size optimization to re-render prevention.

**Why we want it:** Our tic-tac-toe game is small, but the patterns matter. This skill ensures Kiro writes React code the way Vercel's engineers would — proper component structure, efficient state management, no unnecessary re-renders.

**Key categories (by priority):**

| Priority | Category                | Examples                                                               |
| -------- | ----------------------- | ---------------------------------------------------------------------- |
| CRITICAL | Eliminating Waterfalls  | `Promise.all()` for independent ops, defer await into branches         |
| CRITICAL | Bundle Size             | Direct imports (no barrel files), dynamic imports for heavy components |
| HIGH     | Server-Side Performance | `React.cache()` for dedup, minimize client serialization               |
| MEDIUM   | Re-render Optimization  | Functional `setState`, derived state during render, split hooks        |
| MEDIUM   | Rendering Performance   | Content-visibility for lists, hoist static JSX, ternary over `&&`      |

---

## 3.3 — Installing the Skills

### Frontend Design (from skills.sh)

Open your terminal and run:

```bash
npx skills add https://github.com/anthropics/skills --skill frontend-design
```

This downloads the skill from Anthropic's GitHub repo and installs it into your `.kiro/skills/` directory.

### React Best Practices (from Vercel)

```bash
npx skills add https://github.com/vercel-labs/agent-skills --skill vercel-react-best-practices
```

This installs Vercel's React optimization skill the same way.

### Alternative: Import via Kiro panel

You can also install skills through the UI:

1. Open the **Agent Steering & Skills** section in the Kiro panel
2. Click `+` and select **Import a skill**
3. Choose **GitHub** and paste the repository URL
4. Select the skill you want to import

### Verify installation

After installing, you should see both skills in the **Agent Steering & Skills** section of the Kiro panel. You can also check the filesystem:

```
.kiro/skills/
├── frontend-design/
│   └── SKILL.md
└── vercel-react-best-practices/
    ├── SKILL.md
    ├── AGENTS.md
    └── rules/
        ├── async-parallel.md
        ├── bundle-barrel-imports.md
        └── ... (70 rule files)
```

---

## 3.4 — Using the Skills: Redesign the Game

Now let's put these skills to work. Open a Vibe chat and try:

```
Redesign the tic-tac-toe game UI. Make it visually stunning —
I want it to feel like a premium, polished experience, not a generic tutorial app.
Keep all the existing game logic and backend integration intact.
```

### What to watch for

With the **frontend-design** skill active, Kiro will:

- Start with a **design thinking** phase — choosing an aesthetic direction before writing code
- Pick distinctive fonts (not Inter or Roboto)
- Create a bold color palette with CSS variables
- Add animations — board entrance, piece placement, win celebration
- Build atmosphere with textures, shadows, or gradients
- Make the layout feel intentional, not default

With the **react-best-practices** skill active, Kiro will:

- Structure components to avoid unnecessary re-renders
- Use proper state patterns (functional setState, derived state)
- Keep the component tree clean (no inline component definitions)
- Use ternary operators for conditional rendering instead of `&&`

### Point out the difference

If you saved a screenshot of the game before installing skills, show it side-by-side with the redesigned version. The contrast should be dramatic — from "tutorial project" to "something you'd actually want to play."

---

## 3.5 — Invoking Skills Explicitly

Skills activate automatically based on your request, but you can also invoke them directly:

1. Type `/` in the chat input
2. You'll see `frontend-design` and `vercel-react-best-practices` in the slash command list
3. Select one to explicitly load its instructions into the current conversation

This is useful when you want to make sure a skill is active even if your prompt doesn't obviously match its description.

---

## 3.6 — Skills vs Steering vs Powers

This is a good moment to clarify the three ways to give Kiro extra knowledge:

|                 | Skills                                              | Steering                                       | Powers                         |
| --------------- | --------------------------------------------------- | ---------------------------------------------- | ------------------------------ |
| **What**        | Portable instruction packages                       | Persistent project knowledge                   | MCP tool bundles with guidance |
| **Standard**    | Open ([agentskills.io](https://agentskills.io/))    | Kiro-specific                                  | Kiro-specific                  |
| **Loading**     | On-demand (matches description)                     | Configurable (always, fileMatch, manual, auto) | Dynamic (keyword-based)        |
| **Can include** | Instructions, scripts, templates                    | Markdown guidance, file references             | MCP servers, steering, hooks   |
| **Best for**    | Reusable workflows, community packages              | Project conventions, team standards            | External tool integrations     |
| **Shareable**   | Yes — cross-tool (any Agent Skills compatible tool) | Yes — within Kiro                              | Yes — within Kiro              |

**Skills** are what you install from the community. **Steering** is what you write for your project. **Powers** are what you install for tool integrations. We'll cover Steering and Powers in later steps.

---

## 3.7 — What We Just Demonstrated

| Kiro Feature                     | How we used it                                                          |
| -------------------------------- | ----------------------------------------------------------------------- |
| **Agent Skills**                 | Installed two community skills to enhance Kiro's capabilities           |
| **skills.sh / npx skills**       | Used the CLI to install skills from GitHub repos                        |
| **Progressive disclosure**       | Skills loaded automatically when our request matched their descriptions |
| **Slash commands**               | Showed how to invoke skills explicitly with `/`                         |
| **Vibe mode**                    | Used conversational chat to redesign the game with skills active        |
| **Skills vs Steering vs Powers** | Clarified the three knowledge systems                                   |

---

## What's Next

Our game looks great and follows React best practices. In the next step, we'll set up **Steering** to teach Kiro about our specific project conventions — so every future change follows our patterns automatically.
