# AgentPay Pharos

> A drop-in **MCP skill** that gives AI agents a real on-chain wallet on **Pharos** — check balances, read live prices, move tokens, pay only when conditions are met, screen for scams, and buy x402 pay-per-use APIs, all from natural language.

`agentpay-pharos` is a [Model Context Protocol](https://modelcontextprotocol.io) server. Point any MCP-compatible client (Claude Desktop, Cursor, …) at it and your agent gains **20 payment & safety tools** backed by a live wallet on the Pharos network — no glue code required.

## ✨ Highlights
- **Full wallet control** — balances, transfers, wrap/unwrap, gas estimates, and history for PHRS / WPHRS, USDC, USDT, WETH.
- **Conditional & batch payments** — *"pay 5 USDC only if WETH > $2000"*, or fan out a payroll run in one call.
- **Built-in safety net** — GoPlus-powered screening of wallets, tokens, and contracts; `safe_transfer` refuses flagged recipients. No API key needed.
- **x402 native** — agents autonomously pay for metered HTTP APIs over the x402 protocol.
- **Testnet → mainnet** — develop on Atlantic, ship to Pacific by flipping one env var.

## 🧰 Tools

**Payments & wallet**

| Tool | What it does |
|---|---|
| `get_wallet_balances` | All token balances (PHRS, USDC, USDT, WETH, WPHRS) in one call |
| `get_wallet_profile` | Snapshot of holdings + recent activity |
| `send_usdc` / `send_token` | Transfer a stablecoin or any supported token |
| `safe_transfer` | Like `send_token`, but blocks known-malicious recipients |
| `batch_send` | Multi-recipient payout with a per-token summary |
| `conditional_payment` | Send only if a price condition holds |
| `multi_condition_payment` | Combine several price/threshold conditions |
| `wrap_phrs` / `unwrap_phrs` | Wrap/unwrap native PHRS ↔ WPHRS |
| `estimate_gas` | Preview gas cost before sending |
| `get_transaction_history` | Recent transactions for the wallet |

**Prices & network**

| Tool | What it does |
|---|---|
| `get_token_price` | Live price for a supported token |
| `get_network_stats` | Pharos network status / metrics |

**Safety (GoPlus)**

| Tool | What it does |
|---|---|
| `check_wallet_safety` | Risk-screen an address |
| `check_token_safety` | Detect honeypots / risky tokens |
| `check_contract_safety` | Screen a contract before interacting |

**x402 & payment requests**

| Tool | What it does |
|---|---|
| `x402_pay_for_resource` | Pay for and fetch an x402-gated HTTP resource |
| `create_payment_request` | Generate a payment request others can fulfill |
| `verify_payment_received` | Confirm an expected payment landed |

## 🚀 Install

```bash
npm install -g pharos-paygate
```

From source:
```bash
git clone https://github.com/mo726278282/agentpay-pharos.git
cd agentpay-pharos && npm install && npm run build
```

## ⚙️ Configure

```bash
cp .env.example .env
```

```dotenv
PRIVATE_KEY=your_wallet_private_key_here   # dedicated, low-value key
NETWORK=testnet                            # "testnet" (Atlantic) or "mainnet" (Pacific)
CHAIN_ID=688689
RPC_URL=https://atlantic.dplabs-internal.com
MAINNET_RPC_URL=https://rpc.pharos.xyz
FACILITATOR_URL=https://x402.org/facilitator
```

Register with your MCP client (Claude Desktop `claude_desktop_config.json`):
```json
{
  "mcpServers": {
    "agentpay-pharos": {
      "command": "pharos-paygate",
      "env": { "PRIVATE_KEY": "your_wallet_private_key_here", "NETWORK": "testnet" }
    }
  }
}
```

## 💬 Example prompts
- "What's in my wallet right now?"
- "Send 25 USDC to 0xabc…, but only if it isn't a flagged address."
- "Pay 5 USDC to 0xdef… if WETH is above $2,000."
- "Is token 0x123… safe to receive?"
- "Buy access to this x402 API and fetch the data."

## 🛡️ Safety model
Screening is powered by **GoPlus** — malicious addresses, honeypot tokens, risky contracts — no API key required. `safe_transfer` refuses high-risk recipients outright. Treat these as guardrails, not guarantees.

## 🌐 Networks

| Network | Env | Chain ID |
|---|---|---|
| Atlantic (testnet) | `NETWORK=testnet` | 688689 |
| Pacific (mainnet) | `NETWORK=mainnet` | 1672 |

## 🧑‍💻 Development
```bash
npm run dev        # run from source with tsx
npm run build      # compile to dist/
npm run inspector  # debug tools in the MCP Inspector
```

## ⚠️ Disclaimer
This software signs and broadcasts real blockchain transactions with the key you provide. Use a dedicated wallet, start on testnet, and never commit your `.env`. Nothing here is financial advice — you are responsible for every transaction your agent makes.

## 📄 License
MIT
