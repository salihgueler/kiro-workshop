# Step 6 — Powers

> **Goal:** Extend Kiro with Powers — curated tool bundles that give the agent access to external services. Install a power, use it to do something real with your tic-tac-toe game, and see how powers activate dynamically based on context.

---

## 6.1 — Why Powers?

So far, everything you've done has been local — Kiro reads files, writes code, runs commands on your machine. But real projects need external services: deployment platforms, databases, observability tools, payment providers.

You _could_ set up raw MCP servers for each of these. But that creates two problems:

1. **Context overload** — Five MCP servers might load 100+ tool definitions into every conversation, eating 40% of the context window before you type a word.
2. **Missing expertise** — Kiro has the tools but doesn't know the best practices. It can call the Stripe API but doesn't know to use idempotent keys.

Powers solve both. A Power bundles:

- **MCP server config**: The tools and connection details
- **POWER.md**: Steering that teaches Kiro _how_ to use the tools well
- **Hooks/steering**: Optional automation and workflow guidance

They load **dynamically** — only when your conversation mentions relevant keywords. Talk about "deploy," the deployment power activates. Switch to "database," the DB power loads instead. No manual toggling.

---

## 6.2 — The Powers Ecosystem

Kiro has a growing catalog of curated powers from launch partners and the community. Here's a sampling organized by what you'd use them for:

### Deployment & Infrastructure

| Power                          | Provider  | What it does                                          |
| ------------------------------ | --------- | ----------------------------------------------------- |
| **Deploy with Netlify**        | Netlify   | Deploy React/Next.js/Vue apps to Netlify's global CDN |
| **AWS Infrastructure as Code** | AWS       | Build infrastructure with CDK and CloudFormation      |
| **AWS Amplify**                | AWS       | Full-stack apps with auth, data, storage, functions   |
| **Terraform**                  | HashiCorp | Infrastructure as Code with Terraform                 |
| **AWS SAM**                    | AWS       | Serverless applications with SAM                      |
| **ECS Express Mode**           | Community | Deploy containers to AWS ECS with an HTTPS endpoint   |

### Databases

| Power                 | Provider   | What it does                                         |
| --------------------- | ---------- | ---------------------------------------------------- |
| **Supabase (hosted)** | Supabase   | Postgres, auth, storage, real-time subscriptions     |
| **Supabase (local)**  | Supabase   | Local Supabase development environment               |
| **Neon**              | Neon       | Serverless Postgres with branching and scale-to-zero |
| **Aurora PostgreSQL** | AWS        | Aurora-specific best practices                       |
| **ClickHouse**        | ClickHouse | Analytics database management                        |

### Design & Frontend

| Power       | Provider | What it does                                          |
| ----------- | -------- | ----------------------------------------------------- |
| **Figma**   | Figma    | Implement designs from Figma files as production code |
| **Miro**    | Miro     | Use Miro boards as source of truth for architecture   |
| **Bria AI** | Bria     | AI image generation, editing, background removal      |

### Observability & Security

| Power                 | Provider | What it does                                    |
| --------------------- | -------- | ----------------------------------------------- |
| **Datadog**           | Datadog  | Query logs, metrics, traces for debugging       |
| **Snyk**              | Snyk     | Security scanning and vulnerability remediation |
| **AWS Observability** | AWS      | CloudWatch, CloudTrail, Application Signals     |

### Payments

| Power            | Provider     | What it does                     |
| ---------------- | ------------ | -------------------------------- |
| **Stripe**       | Stripe       | Payments, subscriptions, billing |
| **Checkout.com** | Checkout.com | Global payments API              |

---

## 6.3 — Installing a Power

Powers install with one click — no JSON config files, no CLI setup.

### From the Kiro panel

1. Open the **Powers** section in the Kiro panel (or use Command Palette → "Kiro: Configure Powers")
2. Browse available powers
3. Click **Install** on the one you want
4. Kiro handles the MCP server setup automatically

### From kiro.dev

1. Go to [kiro.dev/powers](https://kiro.dev/powers/)
2. Find the power you want
3. Click **Install** — it opens Kiro and installs directly

---

## 6.4 — Try It: Pick Something and Go

You've got a working tic-tac-toe game with a backend. Now pick a direction and let a Power handle the heavy lifting. Here are some paths — choose whichever sounds interesting to you.

### Option A: Deploy to Netlify

Install the **Netlify** power, then ask Kiro:

```
Deploy the tic-tac-toe game to Netlify.
Set up the build configuration and get me a live URL.
```

Kiro will:

- Activate the Netlify power (triggered by keywords "deploy" and "Netlify")
- Use Netlify's MCP tools to create a site and configure the build
- Follow best practices from the power's built-in POWER.md steering
- Deploy and give you a live URL

### Option B: Deploy to AWS with Amplify

Install the **AWS Amplify** power, then ask:

```
Deploy the tic-tac-toe game to AWS using Amplify.
Set up hosting for the frontend and a serverless backend.
```

Kiro will:

- Activate the Amplify power
- Set up Amplify Gen 2 with TypeScript
- Configure hosting, backend functions, and data models
- Deploy to AWS

### Option C: Add a real database with Supabase

Install the **Supabase** power, then ask:

```
Replace the in-memory data store with Supabase.
Set up a Postgres database for game results and the leaderboard.
Use Supabase's auth for player accounts.
```

Kiro will:

- Activate the Supabase power
- Create tables for games and players
- Set up Row Level Security policies
- Integrate the Supabase client into the React frontend
- Replace the Express in-memory store with Supabase queries

### Option D: Deploy infrastructure to AWS with CDK

Install the **AWS Infrastructure as Code** power, then ask:

```
Create a CDK stack to deploy the tic-tac-toe backend as a Lambda function
behind API Gateway, with a DynamoDB table for game storage.
```

Kiro will:

- Activate the AWS IaC power
- Generate a CDK stack with Lambda, API Gateway, and DynamoDB
- Follow AWS Well-Architected best practices
- Validate the CloudFormation template

### Option E: Something else entirely

The catalog has 50+ powers. Security scanning with Snyk? Observability with Datadog? Image generation with Bria? Install whatever catches your eye and point Kiro at it.

---

## 6.5 — How Dynamic Activation Works

After installing a power, try this sequence to see the dynamic loading in action:

1. **Start a conversation about something unrelated** — "Explain the game logic in App.tsx." The power stays dormant. No extra tools loaded.
2. **Then mention the power's domain** — "Now deploy this to Netlify." The power activates. Kiro loads the MCP tools and POWER.md steering.
3. **Switch topics again** — "Add a new game mode." The power deactivates. Context is freed up.

This is the difference between Powers and raw MCP servers. With raw MCP, all tools load all the time. With Powers, Kiro loads what's relevant and unloads what's not.

---

## 6.6 — What's Inside a Power

If you're curious, here's the anatomy. Every power is a folder with:

```
my-power/
├── POWER.md           # Steering: what the tools do, when to use them, best practices
├── mcp.json           # MCP server configuration (tools + connection details)
└── steering/          # Optional: additional workflow guides
    └── deployment.md
```

**POWER.md** is the brain. It tells Kiro:

- What tools are available and what they do
- When to use each tool
- Best practices and common pitfalls
- Workflow sequences ("first create the project, then configure the build, then deploy")

**mcp.json** is the muscle. It defines the MCP server that provides the actual tools.

Together, they give Kiro both the _ability_ and the _knowledge_ to use external services well.

---

## 6.7 — Powers vs Skills vs Steering vs MCP

Now that you've seen all four knowledge systems, here's how they compare:

|                    | Powers                   | Skills                        | Steering               | Raw MCP             |
| ------------------ | ------------------------ | ----------------------------- | ---------------------- | ------------------- |
| **What**           | Tool bundles + knowledge | Instruction packages          | Project knowledge      | Tool servers        |
| **Loading**        | Dynamic (keyword)        | On-demand (description match) | Configurable (4 modes) | Always loaded       |
| **Includes**       | MCP + steering + hooks   | Instructions + scripts        | Markdown guidance      | Tools only          |
| **External tools** | Yes                      | No                            | No                     | Yes                 |
| **Best practices** | Bundled                  | Bundled                       | You write them         | Not included        |
| **Best for**       | Service integrations     | Reusable workflows            | Project conventions    | Custom tool servers |

The mental model: **Steering** is your project wiki. **Skills** are community playbooks. **Powers** are service integrations with built-in expertise. **Raw MCP** is for when you need something custom.

---

## 6.8 — Recap

| Kiro Feature           | How you used it                                                 |
| ---------------------- | --------------------------------------------------------------- |
| **Powers**             | Installed and used a curated power for deployment/database/etc. |
| **One-click install**  | Installed a power from the Kiro panel                           |
| **Dynamic activation** | Saw powers loading/unloading based on conversation context      |
| **MCP integration**    | Powers configured MCP servers automatically                     |
| **POWER.md steering**  | Kiro followed best practices bundled with the power             |
| **Powers ecosystem**   | Browsed the catalog of available powers                         |

---

## Key Takeaway

Powers are the bridge between Kiro and the outside world. They give Kiro access to deployment platforms, databases, observability tools, and more — with the expertise to use them well. And because they load dynamically, you can have dozens installed without slowing anything down.

Going from "I want to deploy this" to "it's deployed" takes minutes, not hours of configuration.

---

## What's Next

You've covered all the major Kiro features: Vibe mode, Specs, Skills, Steering, Hooks, and Powers. Time to wrap up — let's review what you built, what you learned, and where to go from here.

→ [Step 7: MCP and Testing](./7_mcp_and_testing.md) — Set up custom MCP servers and explore property-based testing.
