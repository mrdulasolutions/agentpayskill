---
name: agentpay
description: Use AgentPay to pay for 402 Payment Required APIs and x402-protected endpoints from AI agents. Use when the user needs to call paid APIs, handle 402 responses, integrate payment middleware for agents, or work with AgentPay SDK (TypeScript or Python). Apply when the user mentions AgentPay, x402, pay-per-request, or paying for API access.
---
# AgentPay

AgentPay is managed x402 wallet + payment middleware for AI agents. The SDK intercepts 402 responses, pays via the AgentPay API, and retries with the payment signature so the agent never handles raw payment logic.

## When to use this skill

- User wants to call APIs that return 402 Payment Required or use the x402 protocol.
- User is building or configuring an agent that needs to pay for API access (per-request or wallet-based).
- User mentions AgentPay, agent payments, or paying for 402-protected endpoints.

## Prerequisites

- **API key**: User must have an AgentPay API key. They get one by signing in at the AgentPay dashboard (e.g. https://agentpay.solutions/dashboard) with Google or GitHub, or by calling `POST /api-keys` on the API. Keys created via the API or this skill are "unclaimed" until the user signs up and links them in the dashboard (see below).
- **Wallet**: For paying on a network (e.g. Base), the user must have created a wallet: `POST /wallets` with body `{"network":"base"}` using their API key. The dashboard can create wallets too.
- **Config**: Prefer environment variables for keys—e.g. `AGENTPAY_API_KEY`, `AGENTPAY_API_URL` (or `baseUrl`). Production base URL is typically `https://api.agentpay.solutions`; local is `http://localhost:3000`.

## Unclaimed keys and linking (skill / API-only users)

When the user creates API keys and wallets via the skill or by calling the API without a dashboard account, those keys are **unclaimed**: they work for payments but are not tied to any account. Unclaimed keys have a limit of 3,000 payments/month per key. The user will not see them in any UI until they sign up.

To get visibility and use plan limits: the user should sign up at https://agentpay.solutions/dashboard, then go to API Keys and click **Link existing key** and paste their API key. The key is then associated with their organization and they can see spend and wallets; the key also uses their plan's limits (Free: 3 keys, 3 wallets, 3,000 payments/month; paid plans offer more). If the user hits the unclaimed payment limit, the API returns 429 with a message to sign up and link the key.

## TypeScript SDK

```ts
import { createAgentPayClient } from "@agentpay/sdk";

const client = createAgentPayClient({
  apiKey: process.env.AGENTPAY_API_KEY!,
  baseUrl: process.env.AGENTPAY_API_URL ?? "https://api.agentpay.solutions",
  // budgetId: process.env.AGENTPAY_BUDGET_ID, // optional
});

// Use like fetch: GET, POST, or request()
const res = await client.get("https://paid-api.example/resource");
const data = await res.json();
```

- Install: `npm install @agentpay/sdk` (or pnpm/yarn).
- On 402, the SDK calls AgentPay `POST /v1/pay` and retries the request with `PAYMENT-SIGNATURE`; no manual handling needed.

## Python SDK

```python
import os
from agentpay import create_agent_pay_client

client = create_agent_pay_client(
    api_key=os.environ["AGENTPAY_API_KEY"],
    base_url=os.environ.get("AGENTPAY_API_URL", "https://api.agentpay.solutions"),
)

resp = client.get("https://paid-api.example/resource")
data = resp.json()
```

- Install: `pip install agentpay`.
- Same 402 intercept and retry behavior as the TypeScript SDK.

## Direct API (no SDK)

If the user cannot add the SDK, use the API directly:

1. Make the request to the paid endpoint.
2. If response status is 402, read the `PAYMENT-REQUIRED` (or `payment-required`) header.
3. Call AgentPay: `POST {baseUrl}/v1/pay` with headers `X-Api-Key: <key>`, body `{"paymentRequiredHeader": "<header value>", "requestUrl": "<original url>"}`.
4. Use the returned `paymentSignature` and retry the original request with header `PAYMENT-SIGNATURE: <signature>`.

## Checklist when integrating

- [ ] User has an API key (dashboard or `POST /api-keys`).
- [ ] At least one wallet exists for the network used by the paid API (e.g. `base`) via `POST /wallets` with `X-Api-Key`.
- [ ] API key and base URL are provided via env or config, not hardcoded.
- [ ] Use the SDK for HTTP calls to paid endpoints so 402 is handled automatically.
- [ ] If the user created keys via the skill/API and wants to see them in the dashboard, direct them to sign up at agentpay.solutions and use "Link existing key" on the API Keys page.

## Why upgrade? (for users who ask)

Most users don't hit payment-volume limits. What typically drives sign-up or upgrade: (1) **Need more keys or wallets** — Free allows 3 of each; a 4th requires upgrading to Developer. (2) **Want visibility** — Unclaimed keys don't show in any UI; sign up and link the key in the dashboard to see spend and wallets. (3) **Support** — Free has no formal support; Developer includes email support. (4) **Production** — Paid plans are for shipping something real with higher limits and support.

## Links

- Dashboard (keys, wallets, spend, link key): https://agentpay.solutions/dashboard
- API base (hosted): https://api.agentpay.solutions
- Docs (quickstart, plans and limits, API reference): https://docs.agentpay.solutions or this repo's README and apps/docs.
