# Miller ⚡

> An agent-ready, production-grade starting point for real web applications.

**Miller** gives Codex and other coding agents a deterministic way to create, inspect, configure, run, and prepare a NestJS and Next.js application for production—while keeping you in full control of credentials and external changes.

---

## 🤖 Create an App with Codex

Open Codex in the directory where you keep your projects and paste one line:

```text
Follow [https://usemiller.dev/start.md](https://usemiller.dev/start.md) and create my app.
Codex will ask for the application name, derive its slug and directory, create the project, inspect its local and production configuration, and walk you through only the decisions and credentials that need human input. It will plan external changes before asking for approval and will never need secret values pasted into the conversation.

Read the agent creation guide

Browse all Miller products

Give an agent the complete Miller index

The same workflow is available as the reusable create-miller agent skill. Once generated, every project includes its own AGENTS.md and use-miller skill for ongoing development and operations.

🚀 Quick Start
Prerequisites
Node.js 24

pnpm 11

Docker

Google OAuth credentials (required)

Stripe account (when billing is enabled)

Note: Authentication is self-hosted in the NestJS API; Google is the only sign-in provider.

From a Fresh Installation
A fresh application can be created directly from your terminal:

Bash
pnpm create miller@latest my-app
The publishable package lives in libs/create-miller and bundles the matching Miller template, ensuring an installer version cannot silently fetch a different starter version.

Setup & Development Workflow
Bash
pnpm install
pnpm run mill -- report --profile all --json
pnpm run mill -- setup --only auth --dry-run --json
pnpm run mill -- setup --only auth --apply --yes --json
pnpm run mill -- doctor --deep
pnpm run mill:dev
Working from Source Checkout
To create a separate application from this source checkout:

Bash
pnpm run mill -- create ../my-app --name "My App" --slug my-app
cd ../my-app
pnpm install
pnpm run mill -- doctor
Inspect the project in a form suitable for an agent:

Bash
pnpm run mill -- report --profile all --json
🛠️ Miller CLI Reference
All commands are non-interactive and locate the project root automatically. Configuration is stored in miller.config.json.

Security Note: Use --dry-run before mutations and --json when calling Miller from an agent or automation. Setup credentials are read from the process environment or ignored Terraform variable files; they are never accepted as command arguments or returned in JSON. See libs/project-setup/README.md for the complete setup contract.

Bash
pnpm run mill -- doctor --json
pnpm run mill -- report --profile all --deep --json
pnpm run mill -- setup --from-env --dry-run --json
pnpm run mill -- setup --from-env --apply --yes --json
pnpm run mill -- env sync --json
pnpm run mill -- configure --name "My App" --slug my-app --dry-run --json
pnpm run mill -- add ai --dry-run --json
pnpm run mill -- verify --scope backend
Agent Reporting (mill report --profile all)
This serves as the agent-facing inventory. It reports:

Local vs. production configuration separately.

Which capabilities are enabled.

Where credentials belong and how to obtain them.

Which capabilities are ready and whether Terraform is configured.

Which local services exist or are running.

The next safe command for every gap.

🔒 This reports key names and state only—never raw configuration values.

🔐 Auth & Administration
Authentication & Sessions
Better Auth supports browser session cookies and signed bearer tokens.

The same NestJS auth contract works seamlessly with the included Next.js frontend and independent browser SPAs.

The first successful Google login with a verified email links to the single existing application user with that email. Existing memberships, subscriptions, and domain data stay on that user row; ambiguous duplicate-email matches fail closed.

Admin Operations
The admin API is mounted by NestJS at /api/auth/admin/*.

Configured owners receive the Better Auth admin role and Miller's global backend permissions.

Setup: Set SUPER_USER_EMAILS in the ignored local backend environment or app_super_user_emails in production variables, then visit /super-admin for identity and application operations.

Local Development Startup
pnpm run mill:dev waits for local containers, builds the backend, applies pending database migrations, and only then starts both applications. A fresh checkout therefore gets the Better Auth and admin schema without a separate manual migration step.

📂 Applications & Architecture
apps/backend — NestJS API and production application capabilities.

apps/frontend — Next.js web application.

apps/backend-e2e — Authenticated API verification.

libs/project-setup — Source for the mill CLI.

skills/use-miller — Reusable agent workflow.

AI Core Capability
The optional apps/backend/src/ai-core capability supplies provider-neutral model and tool interfaces. It does not force an AI vendor or expose application actions without normal parsing and authorization.

📖 Further Reading
Product Intent (PRODUCT.md)

System Architecture (ARCHITECTURE.md)

UI Design Principles (DESIGN.md)

Coding Agent Instructions (AGENTS.md)