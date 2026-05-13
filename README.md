# Kiro Workshop

A hands-on workshop that walks you through the full Kiro platform by building, extending, and testing a tic-tac-toe app. You start from an empty folder and a picture, and finish with a full-stack, tested application plus working specs, skills, steering, hooks, powers, and a terminal-based agent workflow.

This repository contains the workshop guides. The tic-tac-toe app itself is built during the workshop, using Kiro.

## Prerequisites

- [Kiro](https://kiro.dev) installed (IDE, or the CLI for Step 8)
- Node.js and npm (for the Vite + React + TypeScript scaffold and the Express backend)
- A Kiro account signed in (Google, GitHub, Builder ID, or IAM Identity Center)
- A whiteboard (physical or digital) to sketch out the expected features of the tic-tac-toe game before Step 1. Jot down the board, the X and O markers, turn indicator, win/draw detection, and a reset button. Snap a photo of it, that picture becomes your starting spec in Step 1.

## How to Use

Work through the steps in order. Each file in `steps/` is self-contained and ends with a recap and a pointer to the next step.

Start here: [`steps/1_kicking_things_off.md`](steps/1_kicking_things_off.md)

## Workshop Steps

| #   | Step                                                          | Focus                                                                                                              |
| --- | ------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| 1   | [Kicking Things Off](steps/1_kicking_things_off.md)           | Tour the Kiro IDE, use Vibe mode and Autopilot, scaffold a React + Vite + TypeScript tic-tac-toe app from an image |
| 2   | [Spec-Driven Development](steps/2_spec_driven_development.md) | Create a Spec and move through requirements → design → tasks to add a Node.js + Express backend                    |
| 3   | [Adding Skills](steps/3_adding_skills.md)                     | Install Anthropic's frontend-design skill and Vercel's React best practices skill, then redesign the UI            |
| 4   | [Steering](steps/4_steering.md)                               | Generate foundational steering (product, tech, structure), author custom conventions, and use inclusion modes      |
| 5   | [Hooks](steps/5_hooks.md)                                     | Automate with `fileEdited`, `preToolUse`, and `postTaskExecution` hooks for builds and accessibility review        |
| 6   | [Powers](steps/6_powers.md)                                   | Install a Power (Netlify, Amplify, Supabase, CDK, or another) and use it for deployment, data, or infra            |
| 7   | [MCP and Testing](steps/7_mcp_and_testing.md)                 | Configure the Playwright MCP server and have Kiro play and verify the game in a real browser                       |
| 8   | [Kiro CLI](steps/8_kiro_cli.md)                               | Use Kiro from the terminal: interactive chat, shell translation, sessions, custom agents, and MCP management       |

## What You Will Build

By the end of the workshop you will have:

- A tic-tac-toe web app (React + Vite + TypeScript) scaffolded from an image
- A Node.js + Express backend with game results, leaderboard, and history endpoints
- A Kiro Spec in `.kiro/specs/` documenting requirements, design, and tasks
- Steering files in `.kiro/steering/` encoding your project conventions
- Hooks in `.kiro/hooks/` automating builds and reviews
- An installed Power (or more) for deployment or data
- The Playwright MCP server configured in `.kiro/settings/mcp.json` for browser testing

## Repository Layout

```
.
├── README.md
├── .gitignore
└── steps/
    ├── 1_kicking_things_off.md
    ├── 2_spec_driven_development.md
    ├── 3_adding_skills.md
    ├── 4_steering.md
    ├── 5_hooks.md
    ├── 6_powers.md
    ├── 7_mcp_and_testing.md
    └── 8_kiro_cli.md
```

Generated files produced during the workshop (`node_modules/`, `dist/`, `.vite/`, `.playwright-mcp/`, etc.) are already covered by `.gitignore`.
