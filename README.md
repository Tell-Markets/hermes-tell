# Tell for Hermes Agent

Two [Hermes Agent](https://github.com/NousResearch/hermes-agent) skills and a one-block MCP config that bring
[Tell](https://tell.markets), risk checks for token launches on Robinhood Chain, into your own self-hosted agent.
Your model, your machine, your chat. Tell never sees a key and never sends a transaction.

| Skill | What it does |
|---|---|
| `tell-check` | Paste any token and ask if it's safe. Tell answers with a 0–100 risk score and every red flag behind it: owner powers, linked wallets, snipers, a deployer who already sold, a website that belongs to another token. Compare two tokens, or list today's Pons launches. |
| `tell-launch-watch` | A cron job that checks new Pons launches every hour and sends the ones with real holders and no high-risk flags to your chat. Silent when there's nothing. |

## Install

The quickest way installs the MCP server and both skills together (Portable Agent Plugins v1):

```
hermes plugins install Tell-Markets/hermes-tell
hermes plugins enable tell
```

Skills are then namespaced `tell:tell-check` and `tell:tell-launch-watch`.

### Or set it up by hand

1. Add the Tell MCP server to `~/.hermes/config.yaml`:

   ```yaml
   mcp_servers:
     tell:
       url: "https://tell.markets/mcp"
       trust: untrusted   # every tool is read only, so nothing ever prompts
   ```

   Run `/reload-mcp` in a session (or restart `hermes gateway`). Check it with `hermes mcp test tell`: you should
   see 5 tools.

2. Install the skills:

   ```
   hermes skills install Tell-Markets/hermes-tell/skills/tell-check
   hermes skills install Tell-Markets/hermes-tell/skills/tell-launch-watch
   ```

### Try it

> /tell-check is 0xc13630c6b85fd1767b484acf38b50314e08a59fc safe to buy?

> /tell-check compare these two tokens: 0x… and 0x…

> /tell-check which Pons launches today have no high-risk flags?

## Launch alerts in Telegram

```
hermes cron create "every 1h" \
  "Run the tell-launch-watch skill. Report launches worth a look, otherwise reply [SILENT]." \
  --skill tell-launch-watch --deliver telegram
```

Every run is read only, so it works unattended with Hermes' default deny-on-approval cron policy. A message looks like:

```
◆ Tell · $TARMAC · 60/100 caution · 25 holders · MC $7.1K · top flag: "Fresh wallet: 3 transactions ever" · https://tell.markets/t/0x690e…
```

## Tools

| Tool | |
|---|---|
| `check_token` | One token, `quick` (free, ~3s) or `full` (~10–30s: holders, deployer, website, AI read) |
| `compare_tokens` | Two tokens side by side |
| `recent_launches` | Pons launches of the last 24h with scores |
| `verdict_proof` | A token's past verdicts with Merkle proofs against roots anchored on-chain every hour |
| `pricing` | What's free, what costs USDG over x402 |

## What it can and cannot do

- **Can:** check any token on Robinhood Chain (and Elysium), compare tokens, list recent Pons launches with scores,
  and prove a past verdict against Tell's on-chain ledger.
- **Cannot:** sign, send, spend or read a key. There is no tool for it.
- **Full checks:** a few a day are free. After that `check_token` says so and `quick` keeps working; full checks
  are 0.03 USDG each over x402 on [the HTTP API](https://tell.markets/docs).

## Links

- Tell: https://tell.markets · API and MCP docs: https://tell.markets/docs
- The engine is open source: https://github.com/Tell-Markets/tell
- X: [@tell_markets](https://x.com/tell_markets) · Telegram: [@tell_marketsrh](https://t.me/tell_marketsrh)

A risk score describes what Tell found. It can be wrong, and it is not financial advice. Tell is a community
plugin, not affiliated with or endorsed by Nous Research or Robinhood.
