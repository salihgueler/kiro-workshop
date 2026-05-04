# Step 7 — MCP and Testing with Playwright

> **Goal:** Configure a raw MCP server manually, install the Playwright MCP, and use it to test our tic-tac-toe game by having Kiro play it in a real browser.

---

## 7.1 — What Is MCP?

We've already used MCP indirectly — Powers bundle MCP servers behind the scenes. But you can also configure MCP servers directly for tools that don't have a Power yet, or when you want full control.

**Model Context Protocol (MCP)** is an open protocol that lets Kiro communicate with external servers to access specialized tools, prompts, and resources. Think of it as a plugin system: each MCP server provides a set of tools that Kiro can call during a conversation.

With MCP, Kiro can:

- Browse the web and interact with web pages
- Query databases
- Call external APIs
- Access specialized knowledge bases
- Run custom tools you build yourself

---

## 7.2 — Why Playwright MCP?

We've built a tic-tac-toe game. We've added a backend, redesigned the UI, set up conventions, and automated our workflow. But we haven't actually _tested_ it properly.

The **Playwright MCP server** gives Kiro the ability to control a real browser — navigate to pages, click elements, fill forms, take screenshots, and inspect the DOM. That means we can ask Kiro to:

- Open our game in a browser
- Play a full game by clicking cells
- Verify the win/draw detection works
- Check the leaderboard shows correct stats
- Take screenshots of the results

It's like having a QA engineer who can actually use the app, not just read the code.

---

## 7.3 — Configuring the Playwright MCP Server

MCP servers are configured in `mcp.json` files. There are two scopes:

| Scope             | Location                    | Applies to        |
| ----------------- | --------------------------- | ----------------- |
| **Workspace**     | `.kiro/settings/mcp.json`   | This project only |
| **User (global)** | `~/.kiro/settings/mcp.json` | All projects      |

### Add the Playwright MCP server

Create or edit `.kiro/settings/mcp.json`:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"],
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

That's it. Save the file and Kiro will automatically detect the new server and connect to it.

### Verify the connection

1. Open the **MCP Servers** section in the Kiro panel
2. You should see "playwright" with a green connection indicator
3. Click the server name to see the available tools

### What tools does Playwright MCP provide?

The Playwright MCP server gives Kiro a rich set of browser automation tools:

| Tool                       | What it does                                                                        |
| -------------------------- | ----------------------------------------------------------------------------------- |
| `browser_navigate`         | Go to a URL                                                                         |
| `browser_snapshot`         | Capture an accessibility snapshot of the page (better than screenshots for actions) |
| `browser_click`            | Click an element                                                                    |
| `browser_type`             | Type text into an input                                                             |
| `browser_take_screenshot`  | Take a visual screenshot                                                            |
| `browser_evaluate`         | Run JavaScript on the page                                                          |
| `browser_console_messages` | Read console output                                                                 |
| `browser_network_requests` | Inspect network traffic                                                             |
| `browser_hover`            | Hover over elements                                                                 |
| `browser_select_option`    | Select dropdown options                                                             |
| `browser_fill_form`        | Fill multiple form fields at once                                                   |
| `browser_wait_for`         | Wait for text to appear or disappear                                                |

---

## 7.4 — Testing the Game

Make sure your dev server is running (`npm run dev`), then open a Vibe chat and try:

### Test 1: Play a full game

```
Open our tic-tac-toe game at http://localhost:5173 in the browser.
Play a full game — make moves for both X and O.
Try to get X to win with a diagonal.
Take a screenshot of the winning state.
```

### What to watch for

Kiro will:

1. Navigate to the game URL
2. Take a snapshot to understand the page structure
3. Click cells one by one, alternating X and O
4. Watch for the win message to appear
5. Take a screenshot showing the final board state

This is a great visual moment for the audience — they can see Kiro literally playing the game in a real browser.

### Test 2: Verify the leaderboard

```
After the game, navigate to the leaderboard page.
Verify that the win was recorded correctly.
Take a screenshot of the leaderboard.
```

### Test 3: Test edge cases

```
Play a game that ends in a draw — fill all cells without either player winning.
Verify the draw message appears.
Then click the reset button and confirm the board clears.
```

### Test 4: Check accessibility

```
Take a snapshot of the game page and check if all interactive elements
have proper accessibility labels. Report any issues.
```

The `browser_snapshot` tool captures an accessibility tree, not just a visual screenshot. Kiro can analyze it for missing labels, non-semantic elements, and keyboard navigation gaps.

---

## 7.5 — Bugfix Specs: When Testing Finds a Bug

If one of the Playwright tests reveals a bug — say the leaderboard shows the wrong win count, or diagonal wins aren't detected — this is the perfect moment to show **Bugfix Specs**.

In Step 2 we used Feature Specs. Bugfix Specs are the other type, designed for systematically diagnosing and fixing bugs.

### How to create a Bugfix Spec

1. Open a **Spec** session (switch from Vibe in the session picker)
2. Choose **Bug** instead of Feature
3. Describe the bug:

```
The leaderboard shows incorrect win counts. When X wins a game,
the leaderboard sometimes credits the win to O instead.
I found this by playing the game with Playwright and checking the leaderboard.
```

### What Kiro generates

Instead of `requirements.md`, you get `bugfix.md` with:

- **Current behavior** — What's happening now (wrong player credited)
- **Expected behavior** — What should happen (correct player gets the win)
- **Unchanged behavior** — What should NOT change (draw detection, game history, reset)

Then the same `design.md` → `tasks.md` flow as Feature Specs, but focused on the fix.

### Why this matters

Bugfix Specs prevent the classic "fix one thing, break another" problem. By explicitly defining what should _not_ change, Kiro is less likely to introduce regressions. The testing → bug discovery → bugfix spec → fix → re-test cycle is a complete quality loop.

---

## 7.6 — MCP Configuration Deep Dive

### The mcp.json structure

```json
{
  "mcpServers": {
    "<server-name>": {
      "command": "npx",
      "args": ["<package-name>@latest"],
      "env": {
        "API_KEY": "your-key-here"
      },
      "disabled": false,
      "autoApprove": ["tool_name_1", "tool_name_2"]
    }
  }
}
```

| Field         | What it does                                           |
| ------------- | ------------------------------------------------------ |
| `command`     | The executable to run (e.g., `npx`, `uvx`, `node`)     |
| `args`        | Arguments passed to the command                        |
| `env`         | Environment variables for the server process           |
| `disabled`    | Toggle the server on/off without removing the config   |
| `autoApprove` | Tools that don't need user confirmation before running |

### Scope precedence

If the same server is defined in both workspace and global configs, the workspace config wins. This lets you set global defaults and override per-project.

### Using `#mcp` in chat

You can reference MCP tools, prompts, and resources in chat using the `#mcp` context provider:

```
#mcp:playwright take a screenshot of the current page
```

---

## 7.7 — MCP vs Powers

Now that we've configured a raw MCP server, the difference with Powers is clear:

|                    | Raw MCP                             | Powers                                     |
| ------------------ | ----------------------------------- | ------------------------------------------ |
| **Setup**          | Manual JSON config                  | One-click install                          |
| **Tools**          | Always loaded                       | Loaded dynamically by keyword              |
| **Best practices** | Not included                        | Bundled in POWER.md                        |
| **Hooks/steering** | Not included                        | Can be bundled                             |
| **Best for**       | Custom tools, tools without a Power | Service integrations with curated guidance |

Use raw MCP when you need a specific tool (like Playwright) that doesn't have a Power. Use Powers when one exists — you get the tools plus the expertise.

---

## 7.8 — What We Just Demonstrated

| Kiro Feature               | How we used it                                           |
| -------------------------- | -------------------------------------------------------- |
| **MCP configuration**      | Manually configured Playwright MCP in `mcp.json`         |
| **MCP tools**              | Used browser automation tools to test the game           |
| **Browser testing**        | Played the game, verified leaderboard, tested edge cases |
| **Accessibility snapshot** | Used `browser_snapshot` to check a11y                    |
| **Screenshots**            | Captured visual evidence of test results                 |
| **`#mcp` context**         | Referenced MCP tools in chat                             |
| **Bugfix Specs**           | Created a bugfix spec when testing revealed a bug        |
| **MCP vs Powers**          | Clarified when to use each approach                      |

---

## Key Takeaway

MCP is the raw protocol that connects Kiro to external tools. Powers are curated bundles built on top of MCP. For testing, the Playwright MCP server turns Kiro into a QA engineer who can actually use your app — clicking buttons, filling forms, and verifying behavior in a real browser.

The configuration is a single JSON file. The payoff is being able to say "test my app" and watch Kiro do it.

---

## What's Next

We've been working inside the IDE this whole time. In the next step, we'll take Kiro to the terminal with the **Kiro CLI** — a full agent experience for developers who prefer the command line.
