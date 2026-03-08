# AgentPay skill

This repo contains the **AgentPay skill** for AI coding agents (Cursor, Openclaw, Spacebot, Nanobot, OpenCode, Windsurf, Claude Code, etc.). Once installed, the agent knows how to use AgentPay so users can call 402 Payment Required APIs and x402-protected endpoints without wiring payment logic by hand.

**Repo layout:** The skill lives in the `agentpay/` folder (no `.cursor` path). Copy `agentpay/` into your environment’s skills directory when installing (see below).

## What is AgentPay?

AgentPay is managed x402 wallet + payment middleware for AI agents. You get an API key and managed wallets (create a wallet, then fund its address with USDC on Base); the SDK intercepts 402 responses, pays via the AgentPay API, and retries with the payment signature automatically. Think of it as Stripe for AI agents.

- **Dashboard:** Sign up, get an API key, create wallets, view spend, **link keys** → [agentpay.solutions](https://agentpay.solutions/dashboard)
- **Docs and quickstart:** [docs.agentpay.solutions](https://docs.agentpay.solutions) — including **Plans and limits** (unclaimed keys, link key, when to upgrade)
- **Hosted API:** [api.agentpay.solutions](https://api.agentpay.solutions)

## Using the skill without an account (unclaimed keys)

You can create API keys and wallets via the API (or by asking your agent to do it). Those keys work right away but are **unclaimed**: they are not tied to a dashboard account. Unclaimed keys have a limit of 3,000 payments/month per key. You won't see them in any UI until you sign up.

**To get visibility:** Sign up at [agentpay.solutions](https://agentpay.solutions/dashboard), go to API Keys, click **Link existing key**, and paste your API key. The key then appears in your dashboard and uses your plan's limits (Free: 3 keys, 3 wallets, 3,000 payments/month; paid plans offer more).

## Install the SDKs (for your app or agent)

- **TypeScript:** `npm install @agentpay/sdk` (or pnpm/yarn)
- **Python:** `pip install agentpay`

Use the hosted API (`https://api.agentpay.solutions`) as `baseUrl` unless you self-host.

## Install this skill (so the agent knows how to use AgentPay)

**Option 1 — From this repo (skills.sh-style):**

```bash
npx skills add mrdulasolutions/agentpayskill --skill agentpay
```

**Option 2 — Copy the `agentpay` folder into your skills directory:**

```bash
git clone https://github.com/mrdulasolutions/agentpayskill.git
cd agentpayskill
# Cursor: copy into global or project skills dir so the agent sees it as "agentpay"
cp -r agentpay ~/.cursor/skills/   # global
# or copy into your project: .cursor/skills/agentpay (project-only)
```

After installation, the agent will use this skill when the user asks to pay for 402 APIs, integrate AgentPay, or work with x402. Your agent can then pay 402-protected APIs and pay other agents or people who accept x402 payments. The user still needs an API key (dashboard or `POST /api-keys`) and a wallet for the target network (e.g. Base)—create the wallet, then fund its address with USDC on Base. If they created keys via the skill and want to see them in the dashboard, they sign up and use **Link existing key**.

## License

Apache-2.0

## About

AgentPay skill for AI coding agents (Cursor, Openclaw, Spacebot, Nanobot, OpenCode, Windsurf, Claude Code, etc.). Use 402 Payment Required and x402 without wiring payment logic. [agentpay.solutions](https://agentpay.solutions)

---

**Maintainers:** This README and the `agentpay/` folder (with `SKILL.md`) are the contents of the [agentpayskill](https://github.com/mrdulasolutions/agentpayskill) repo. The repo uses the simpler layout `agentpay/SKILL.md` (no `.cursor` path). To publish: copy this README to the repo root as `README.md` and copy the `agentpay/` folder into the repo.
