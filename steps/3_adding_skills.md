# Step 3 — Adding Skills

> **Goal:** Install community skills to level up Kiro's output, Anthropic's frontend-design skill for stunning UI and Vercel's React best practices for production-quality code. Then use them to redesign the tic-tac-toe game.

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

Skills activate automatically when your request matches their description. You can also mention the skill's domain explicitly in your prompt to ensure it activates (e.g., "follow React best practices" or "use the frontend design skill").

---

## 3.2 — The Two Skills You're Installing

### 1. Anthropic's Frontend Design Skill

**What it does:** Guides Kiro to create distinctive, production-grade frontend interfaces that avoid generic "AI slop" aesthetics. It pushes for bold design choices — unique typography, cohesive color themes, animations, spatial composition, and textural details.

**Why you want it:** Your tic-tac-toe game probably looks... fine. Generic. This skill will make Kiro think like a designer before writing CSS — choosing an aesthetic direction, picking distinctive fonts, adding motion and atmosphere.

**Key principles it teaches Kiro:**

- Think about purpose, tone, and differentiation before coding
- Choose bold typography (no Inter, no Roboto, no Arial)
- Commit to a cohesive color theme with dominant colors and sharp accents
- Add motion — page load animations, hover states, micro-interactions
- Create atmosphere with textures, gradients, shadows, and depth
- Never produce cookie-cutter, predictable layouts

### 2. Vercel's React Best Practices Skill

**What it does:** A comprehensive performance optimization guide with 70 rules across 8 categories, maintained by Vercel Engineering. Covers everything from eliminating waterfalls to bundle size optimization to re-render prevention.

**Why you want it:** Your tic-tac-toe game is small, but the patterns matter. This skill ensures Kiro writes React code the way Vercel's engineers would — proper component structure, efficient state management, no unnecessary re-renders.

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

Select to install the additional Agent for **Kiro**.

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

Now put these skills to work. Open a Vibe chat and try:

```
Redesign the tic-tac-toe game UI. Make it visually stunning —
I want it to feel like a premium, polished experience, not a generic tutorial app. Use the installed Skills.
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

### Compare the difference

If you saved a screenshot of the game before installing skills, compare it side-by-side with the redesigned version. The contrast should be dramatic — from "tutorial project" to "something you'd actually want to play."

### Web Tools in Action

During the redesign, Kiro might need to look things up — a Google Font that matches the aesthetic, a CSS animation technique, or a color palette tool. Kiro has built-in **web tools** that let it search the internet and fetch content from URLs in real time.

Try asking:

```
Find a distinctive Google Font pairing that fits a retro-futuristic aesthetic for the game.
```

Kiro will search the web, find font options, and apply them. This is useful anytime Kiro needs current information — library docs, API references, design inspiration — that goes beyond its training data.

---

## 3.5 — How Skills Activate

Skills activate **automatically** based on relevance. When your prompt matches a skill's description (defined in its `SKILL.md` frontmatter), Kiro loads the full instructions into context for that conversation.

For example:
- Asking to "redesign the UI" or "make it visually stunning" triggers the **frontend-design** skill because its description mentions "frontend interfaces," "styling," and "beautifying."
- Writing or refactoring React components triggers **vercel-react-best-practices** because its description mentions "React components," "performance improvements," and "refactoring."

You don't need to do anything special — just describe what you want, and the relevant skills activate. If you want to be explicit, mention the skill's domain in your prompt (e.g., "follow React best practices" or "use the frontend design skill").

---

## 3.6 — Other Ways to Give Kiro Context

Skills are one of several ways to feed Kiro extra knowledge. You'll cover the others in later steps:

- **Skills**: Community instruction packages (what you just installed). Activate automatically based on your request.
- **Steering**: Project-specific conventions you write yourself. Things like "our API always returns this shape" or "use functional components only." Lives in `.kiro/steering/`. Covered in **Step 4**.
- **Powers**: External service integrations (deployment, databases, observability) bundled with MCP tools and best-practice guidance. Covered in **Step 6**.

Beyond these, you can also pull context into any chat manually:

- Type `#` in the chat input to attach a specific **file**, **folder**, **terminal output**, **git diff**, or **problems** from your editor
- Drag and drop **images** or **documents** (PDF, DOCX) directly into the chat
- Reference `#Problems` to share current diagnostics with Kiro

This means Kiro's knowledge comes from layers: automatic context (skills, steering), explicit context (what you attach with `#`), and external tools (powers). Together they keep conversations focused without you having to re-explain your project every time.

---

## 3.7 — Recap

| Kiro Feature                     | How you used it                                                         |
| -------------------------------- | ----------------------------------------------------------------------- |
| **Agent Skills**                 | Installed two community skills to enhance Kiro's capabilities           |
| **skills.sh / npx skills**       | Used the CLI to install skills from GitHub repos                        |
| **Progressive disclosure**       | Skills loaded automatically when your request matched their descriptions|
| **Automatic activation**         | Skills activated based on prompt relevance, no manual invocation needed |
| **Vibe mode**                    | Used conversational chat to redesign the game with skills active        |
| **Web tools**                    | Kiro searched the web for fonts, colors, or techniques during redesign  |
| **Skills vs Steering vs Powers** | Clarified the three knowledge systems                                   |

---

## What's Next

Your game looks great and follows React best practices. In the next step, you'll set up **Steering** to teach Kiro about your specific project conventions — so every future change follows your patterns automatically.
