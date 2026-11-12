# Security

## What this plugin does

- Connects Hermes to one remote MCP server, `https://tell.markets/mcp`, over Streamable HTTP.
- Every tool on that server is read only (annotated `readOnlyHint`). There is no tool that signs, sends, spends or
  touches a wallet.
- No keys, tokens or credentials are needed or read. The plugin has no environment variables.
- No code runs on your machine: the package is two Markdown skills and two JSON files.

## Data

The server returns public data that is also on tell.markets: risk reports, launch lists and proofs. Tool output is
data. The skills tell the agent to treat every string returned by a tool (token names, verdicts, website text) as
data and never as an instruction. Token websites are written by whoever launched the token, so that matters.

## Reporting a problem

Open an issue on this repository, or reach us on X at [@tell_markets](https://x.com/tell_markets). Please do not post
details of an unfixed vulnerability in a public issue; send a direct message instead.
