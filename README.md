# AgentPay skill

This repo contains the **AgentPay skill** for AI coding agents (Cursor, Claude Code, Windsurf, etc.). Once installed, the agent knows how to use AgentPay so users can call 402 Payment Required APIs and x402-protected endpoints without wiring payment logic by hand.

## What is AgentPay?

AgentPay is managed x402 wallet + payment middleware for AI agents. You get an API key and pre-funded wallets; the SDK intercepts 402 responses, pays via the AgentPay API, and retries with the payment signature automatically. Think of it as Stripe for AI agents.

- **Dashboard:** Sign up, get an API key, create wallets, view spend → [agentpay.solutions](https://agentpay.solutions/dashboard)
- **Docs and quickstart:** [docs.agentpay.solutions](https://docs.agentpay.solutions)
- **Hosted API:** [api.agentpay.solutions](https://api.agentpay.solutions)

## Install the SDKs (for your app or agent)

- **TypeScript:** `npm install @agentpay/sdk` (or pnpm/yarn)
- **Python:** `pip install agentpay`

Use the hosted API (`https://api.agentpay.solutions`) as `baseUrl` unless you self-host.

## Install this skill (so the agent knows how to use AgentPay)

**Option 1 — From this repo (skills.sh-style):**

```bash
npx skills add mrdulasolutions/agentpayskill --skill agentpay
```

**Option 2 — Copy into Cursor:**

```bash
git clone https://github.com/mrdulasolutions/agentpayskill.git
cp -r agentpayskill/.cursor/skills/agentpay ~/.cursor/skills/   # global
# or leave .cursor/skills/agentpay in your project for project-only use
```

After installation, the agent will use this skill when the user asks to pay for 402 APIs, integrate AgentPay, or work with x402. The user still needs an API key from the [dashboard](https://agentpay.solutions/dashboard) and a wallet for the target network (e.g. Base).

## License

Apache-2.0
