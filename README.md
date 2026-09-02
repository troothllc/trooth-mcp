# Trooth MCP server

[![Listed on mcpservers.org](https://mcpservers.org/badge.svg)](https://mcpservers.org/servers/trooth-eng/trooth-mcp)

The public **Trooth Network** connector: a remote, read-only [MCP](https://modelcontextprotocol.io) server that lets any AI assistant check a company's witnessed trust record, take a neutral read of a domain's public security surface, verify a signed Trust Ledger Token, or ask about Trooth.

**Endpoint:** `https://api.trooth.co/public/mcp` (streamable HTTP, no auth, public and read-only)

**Website:** https://trooth.co

**Official MCP Registry:** `io.github.trooth-eng/trooth-network`

**In ChatGPT:** listed and approved by OpenAI. [Open Trooth Network in the app directory](https://chatgpt.com/plugins/plugin_asdk_app_6a9361d2ee7481919a8c706c3ff5388f).

**Company records:** https://trooth.co/network

## Tools

| Tool | What it returns |
| --- | --- |
| `trooth_public_trust_profile` | A company's witnessed Trust Profile or Network standing, with provenance. |
| `trooth_outside_in_read` | A live, neutral read of a domain's public security surface. |
| `trooth_verify` | Re-verify a signed Trooth Trust Ledger Token. |
| `trooth_ask` | Ask about Trooth (products, methodology, pricing). |

Every answer names its source and date. Witnessed evidence is labeled apart from what a company declares, and anything unsettled comes back marked unknown rather than guessed. Read-only, no account.

## Structured output

Every tool declares an `outputSchema` and returns `structuredContent` alongside its text, so an agent can branch on fields instead of parsing prose.

| Field | Meaning |
| --- | --- |
| `status` | `published`, `listed`, `unclaimed`, `private`, `observed`, `valid`, `invalid`, `expired`, `revoked`, `answered`, `out_of_scope`, `bad_input` |
| `provenance` | `witnessed_signed`, `signed_scan`, `live_observation`, `honest_absence`, `withheld_by_owner`, `knowledge_base` |
| `subject` | The company, domain, or token the answer is about. |
| `claim_url` | Present only when a subject has no published record and its owner could claim one. |

Two states stay deliberately apart. `unclaimed` with `honest_absence` means Trooth holds no record, which is an absence of evidence and not a finding about the company. `private` with `withheld_by_owner` means a record exists and its owner chose not to publish it, which is a decision and not missing data. Collapsing them would misrepresent a company, so they carry different values.

## Add it

**ChatGPT:** Trooth Network is a listed app. [Open the listing](https://chatgpt.com/plugins/plugin_asdk_app_6a9361d2ee7481919a8c706c3ff5388f) and press Try in chat. To add it by hand instead: Settings, then Apps, then Create, then paste the endpoint as the MCP server URL.

**Claude:** Settings, then Connectors, then Add custom connector, then paste the endpoint.

**Cursor, Cline, VS Code, or any MCP client:** add a remote MCP server pointing at `https://api.trooth.co/public/mcp`.

## About this repo

This repository documents the hosted Trooth connector. `server.json` is the record published to the official MCP Registry. The connector runs as a hosted service; there is no package to install.

Learn more at https://trooth.co and https://trooth.co/developers.
