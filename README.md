# Trooth MCP server

The public **Trooth Network** connector: a remote, read-only [MCP](https://modelcontextprotocol.io) server that lets any AI assistant check a company's witnessed trust record, take a neutral read of a domain's public security surface, verify a signed Trust Ledger Token, or ask about Trooth.

- **Endpoint:** `https://api.trooth.co/public/mcp`
- **Transport:** streamable HTTP
- **Auth:** none (public, read-only)
- **Website:** https://trooth.co
- **Official MCP Registry:** `io.github.trooth-eng/trooth-network`

## Tools

- `trooth_public_trust_profile` — a company's witnessed Trust Profile or Network standing, with provenance.
- `trooth_outside_in_read` — a live, neutral read of a domain's public security surface.
- `trooth_verify` — re-verify a signed Trooth Trust Ledger Token.
- `trooth_ask` — ask about Trooth (products, methodology, pricing).

Every answer names its source and date. Witnessed evidence is labeled apart from what a company declares, and anything unsettled comes back marked unknown rather than guessed. Read-only, no account.

## Add it

**ChatGPT** — Settings → Apps → Create → paste the endpoint as the MCP server URL.

**Claude** — Settings → Connectors → Add custom connector → paste the endpoint.

**Cursor, Cline, VS Code, or any MCP client** — add a remote MCP server pointing at `https://api.trooth.co/public/mcp`.

## About this repo

This repository documents the hosted Trooth connector. `server.json` is the record published to the official MCP Registry. The connector runs as a hosted service; there is no package to install.

Learn more at https://trooth.co and https://trooth.co/developers.
