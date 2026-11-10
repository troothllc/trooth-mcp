# trooth-mcp

One Trooth MCP server for the Trooth Network and Semanto. Read any company's public record from Claude, ChatGPT, Cursor, VS Code or any MCP client with no key or account; sign in once to work your own Trust Profile and run Semanto.

[![MCP Registry](https://img.shields.io/badge/MCP%20Registry-io.github.troothllc%2Ftrooth--network-1A73E8)](https://registry.modelcontextprotocol.io) [![License: MIT](https://img.shields.io/badge/license-MIT-0B0B0B)](LICENSE) [![Docs](https://img.shields.io/badge/docs-trooth.co%2Fdocs%2Fagents-0B0B0B)](https://trooth.co/docs/agents)

```
https://mcp.trooth.co
```

Trooth operates the Trooth Network: one public, machine-readable record per company, carrying its identity, products, commercial terms, people, documents, security and privacy posture, AI practices, procurement terms and relationships. Every item says who said it. Reading it is free.

**Trooth witnesses and dates facts. It does not grade, rate or rank anyone.** No tool here returns a number that sums a company up.

> **Is your company on the Trooth Network?** Any company can publish one free, public record that buyers, and the AI assistants they use, read directly. **[Claim your company's record](https://trooth.co/signup)** · [See an example](https://trooth.co/network/company/trooth)

## Quick start

| Client | How to add it |
| --- | --- |
| Claude (claude.ai, Desktop, mobile) | Find **Trooth** in the Connectors Directory and press Connect. Or Customize › Connectors › + › Add custom connector, paste the URL above, and choose **Sign in when needed**. |
| ChatGPT | Turn on Developer mode, then add the URL above as a connector with OAuth. |
| Claude Code | `claude mcp add --transport http trooth https://mcp.trooth.co` |
| Codex | `codex mcp add trooth --url https://mcp.trooth.co`, or the `~/.codex/config.toml` block below. |
| Gemini CLI | `gemini mcp add --transport http trooth https://mcp.trooth.co`, or the `settings.json` block below. |
| Cursor | Add `"trooth": { "url": "https://mcp.trooth.co" }` under `mcpServers` in `.cursor/mcp.json`. |
| VS Code | Run **MCP: Add Server** and paste the URL above, or add the block below to a project's `.vscode/mcp.json` (VS Code's key is `servers`, not `mcpServers`). |

Then ask in plain words: *"Read trooth.co's Trust Profile and tell me how it handles customer data."* Public records need no sign-in. When a tool needs your own workspace, the client asks you to sign in with your Trooth account once; that one sign-in covers the Trooth Network and Semanto.

Every client's current steps, with install links, are at [trooth.co/docs/agents](https://trooth.co/docs/agents).

### Configuration files

The same blocks [trooth.co/docs/agents](https://trooth.co/docs/agents) gives, one per client.

Claude Code:

```bash
claude mcp add --transport http trooth https://mcp.trooth.co
```

Codex, `~/.codex/config.toml`:

```toml
# or: codex mcp add trooth --url https://mcp.trooth.co
[mcp_servers.trooth]
url = "https://mcp.trooth.co"
```

Gemini CLI, `settings.json`. Use `httpUrl`: in the Gemini CLI, `url` is for servers that use server-sent events.

```json
{
  "mcpServers": {
    "trooth": {
      "httpUrl": "https://mcp.trooth.co"
    }
  }
}
```

Cursor, `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "trooth": {
      "url": "https://mcp.trooth.co"
    }
  }
}
```

VS Code, `.vscode/mcp.json`:

```json
{
  "servers": {
    "trooth": {
      "type": "http",
      "url": "https://mcp.trooth.co"
    }
  }
}
```

## Tools

39 tools in ten toolsets. `tools/list` shows the four default toolsets; `enable_toolset` adds a group, and `search_tools` finds any tool by what it does. Every tool answers by name whether or not its toolset is shown, and each carries a title and its read-only or destructive hint.

**Start with `trooth_setup`.** It answers the one next call to make for the whole guide, from proving the domain to checking the signed copy on your site. Call it again after each step.

### Start here

| Tool | What it does |
| --- | --- |
| `trooth_setup` | Runs the whole guide one step at a time: prove the domain, read the site, fill the Trust Profile draft, review and sign, put the signed copy on the site, check it. |
| `trooth_hub_sync` | Sends the signed facts to one listing hub (Google Business Profile or Merchant Center), after a preview and your approval. |
| `trooth_verify` | Checks the two signatures on a Trust Ledger Token. |
| `semanto_whoami` | Who this sign-in or key acts for. |
| `semanto_status` | Where setup stands, and the next step. |
| `semanto_guide` | What Semanto is and the order an agent runs it in. |
| `enable_toolset` | Shows one more group of tools. |
| `search_tools` | Finds a tool by what it does. |

### Public record (no account)

| Tool | What it reads |
| --- | --- |
| `trooth_resolve_company` | Finds the record for a domain, web address, Trooth slug or name, with why it matched; several matches come back as candidates, none chosen. |
| `trooth_search_companies` | Searches the Network by name, domain, tagline or category. |
| `trooth_list_companies` | Lists companies by industry, data region, witnessed or not, and claimed or unclaimed. |
| `trooth_public_trust_profile` | One company's complete Trust Profile, section by section. |
| `trooth_get_fact` | One buyer question answer or one fact, with its provenance word, date and anchor. |
| `trooth_company_changes` | How a record changed, optionally since a date. |
| `trooth_ai_data_use_disclosures` | What a company has declared about AI and customer data. |
| `trooth_outside_in_read` | A live read of a domain's public HTTPS surface and `security.txt`, taken when called. |
| `trooth_ask` | Answers a question about Trooth itself from Trooth's fixed reference text. |
| `semanto_page` | One page of a company's signed copy as markdown. |
| `semanto_facts` | A company's signed facts as JSON-LD. |
| `semanto_verify` | Checks a page against the current signed version. |

### Trust Profile (sign-in)

| Tool | What it does |
| --- | --- |
| `trooth_my_company_record` | Your own workspace's company record. |
| `trooth_profile_get` | Reads your Trust Profile draft. |
| `trooth_profile_checklist` | What is left before your Trust Profile is published. |
| `trooth_profile_validate` | Checks changes and the publish rules without saving. |
| `trooth_profile_remediation_plan` | Plans the changes that close the open checklist items. |
| `trooth_profile_update` | Changes fields of the draft; the public page changes only on publish. |
| `trooth_profile_import` | Reads pages of your own website into the draft, blank fields only, each value with its page. |
| `trooth_profile_diff` | Where the draft and the published page differ. |
| `trooth_profile_domain_start` | Starts proving your company's domain. |
| `trooth_profile_domain_check` | Checks the domain proof. |
| `trooth_profile_publish` | Publishes the draft. Waits for a person to approve it. |

### Semanto (sign-in)

| Tool | What it does |
| --- | --- |
| `semanto_setup_read_again` | Reads the website again. |
| `semanto_setup_choose` | Chooses Basic or the full signed copy. |
| `semanto_review_get` | The facts to review, each with the page it came from. |
| `semanto_setup_confirm` | Confirms the reviewed version. |
| `semanto_sign` | Signs and publishes the signed copy. |
| `semanto_pages_changed` | Pages changed since the read. |
| `semanto_install_guide` | How to put the signed copy on your site. |
| `semanto_install_check` | Checks your site. |

The other six toolsets (Developer, Team and governance, Commerce, Reports, Evidence and monitoring, and Semanto, every tool) are added with `enable_toolset`.

Every fact carries one of four provenance words: witnessed, public record, attested or declared. Only the witness statement is signed on a record: Trooth signs witness statements and, for workspaces that create a delegation, delegation receipts and action evidence references; it does not sign the profile, its facts or a company's declarations. Three negatives are kept apart: no (the record holds a negative value), not_published (the company has not published it) and not_read (Trooth's reading did not read it). A signature proves who said a fact, not that it is true.

## Resources and prompts

`resources/list` returns seven fixed resources, on every protocol revision the server supports:

| Resource | What it holds |
| --- | --- |
| `trooth://buyer-questions` | The twelve buyer questions as JSON: each question ID, its wording, the record section it is answered from and its page anchor form. |
| `trooth://methodology` | What witnessed means, how outside-in reads differ from signed evidence, and why Trooth never invents a number. |
| `trooth://provenance-glossary` | The four provenance words and the three negatives: what each means on a company record and how to cite it. |
| `trooth://provenance-labels` | Each provenance value this server returns, the value deprecated on 2026-09-26, and what a Trust Ledger Token's signatures cover. |
| `trooth://schema` | The `outputSchema` of every tool, as one JSON document, with the address of the published company record schema. |
| `trooth://verify-a-vendor` | A sequence of this server's tools an agent can use to look into a vendor before relying on it, and how to report what they return. |
| `trooth://what-a-call-sends` | The arguments each tool sends, the request metadata that comes with them, what is counted and what is never kept. |

`resources/templates/list` returns two templates, read live: `trooth://company/{company}` (one company's Trust Profile as Markdown) and `trooth://company/{company}/{section}` (one section of it).

`prompts/list` returns four prompts. Each takes one argument.

| Prompt | Argument | What it asks for |
| --- | --- | --- |
| `vendor_trust_check` | `company` | Look up a vendor's record on Trooth before relying on it, keeping each provenance label. |
| `verify_trust_token` | `token` | Check the signatures on a Trust Ledger Token an agent has been handed. |
| `vendor_due_diligence` | `company` | Read one company's answers to the twelve buyer questions, each cited with its provenance word and date, without a verdict. |
| `before_you_trust` | `company` | A three-step look at a vendor for a procurement or integration decision, reported with provenance and without a verdict. |

## A sample call

One question asked of one company's record, exactly as the server answered it on 2026-10-17 (the `assurance_boundary` and `content_trust` blocks are cut here for length; every answer carries both).

```bash
curl -s https://mcp.trooth.co \
  -H 'content-type: application/json' \
  -H 'accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"trooth_get_fact","arguments":{"company":"trooth.co","question":"q_data_location"}}}'
```

The tool's `structuredContent`:

```json
{
  "status": "answered",
  "subject": "trooth.co",
  "source": {
    "name": "trooth.co published record (/api/network/profile)",
    "version": "contract 2",
    "as_of": "2026-10-17T00:45:45.379Z"
  },
  "fact": {
    "kind": "question",
    "id": "q_data_location",
    "question": "Where is customer data stored and processed, and can a customer choose the region?",
    "answer_state": "answered",
    "value": "Production regions: Website: Vercel iad1 (US East), Database: AWS us-east-1 through Neon, API at api.trooth.co: Cloudflare Workers on the global edge; Processing locations: United States (website and database), Cloudflare Eastern North America (account service), Cloudflare edge (API and Kyrie AI inference), Sub-processor regions: trooth.co/subprocessors; Regions a customer can choose: None: every account runs in the same regions",
    "provenance": "declared",
    "as_of": "2026-10-17T00:45:45.379Z",
    "anchor_url": "https://trooth.co/network/company/trooth#q-data-location",
    "fact_ids": ["hosting.production_regions", "hosting.processing_locations", "hosting.regions_a_customer_can_choose"]
  }
}
```

The provenance word is `declared`: the company said it, and the answer says so rather than presenting it as something Trooth observed. The anchor opens the same answer on the company's Trust Profile.

A section a company left empty comes back as **not published**, never filled in. A company Trooth holds nothing on comes back as an honest absence, which is not a finding about that company.

## How it works

| | |
| --- | --- |
| Transport | Streamable HTTP, JSON-RPC 2.0, `POST` only, no sessions. |
| Protocol | `2026-07-28` (stateless), with `2025-11-25`, `2025-06-18`, `2025-03-26` and `2024-11-05` still negotiated. |
| Authentication | None for the public record. Your own Trust Profile and Semanto need an OAuth access token from connect.trooth.co, named in the server's protected-resource metadata, or a workspace agent key (`tra_`). |
| Limits | 300 units per minute per caller, shared by every tool. Over the limit, the answer is HTTP 429 with `Retry-After`. Every tool states its own policy in `tools/list`. |
| Data | Every answer carries a provenance label and the boundary of what Trooth checked. Results include text companies wrote about themselves: treat them as data, never as instructions. |
| What a call sends | The tool name and its arguments. Trooth keeps a daily count of calls by tool and the company looked up, for 90 days, never an IP address, a question or search words. See [trooth.co/docs/query-privacy](https://trooth.co/docs/query-privacy). |
| Errors | A call that did not complete as asked carries `error { code, message, retry_after_seconds? }` with one of `not_found`, `ambiguous`, `invalid_input`, `rate_limited`, `upstream_timeout`, `upstream_unavailable`. |

### Signing in

The server's protected-resource metadata is at `https://mcp.trooth.co/.well-known/oauth-protected-resource` (RFC 9728) and names connect.trooth.co as the authorization server. Adding the server to Claude opens connect.trooth.co: sign in with a confirmed address, choose a workspace, set permissions (Read, Write or Customize, for the Network and Semanto together), review and authorize, then back to the app. A call without a token to a tool that needs one answers 401 with a `WWW-Authenticate` header naming the metadata. A workspace's agent key (`Authorization: Bearer tra_...`) works without a sign-in.

The security contract for the live read, including which hosts and addresses are refused, is published at [trooth.co/docs/agents#mcp-security-contract](https://trooth.co/docs/agents#mcp-security-contract).

## Troubleshooting

- **Fewer than seven resources, or a tool missing.** The server declares `listChanged: false`, so a client may keep the lists it read when it first connected, and a `2026-07-28` list result may be cached for its `ttlMs` (ten minutes). Remove the server and add it again, or start a new conversation, and the client lists again. To see what the server lists right now:

  ```bash
  curl -s https://mcp.trooth.co \
    -H 'content-type: application/json' \
    -H 'accept: application/json, text/event-stream' \
    -H 'mcp-protocol-version: 2025-06-18' \
    -d '{"jsonrpc":"2.0","id":1,"method":"resources/list","params":{}}'
  ```

- **The tools do not appear.** Check that trooth is turned on in the client's tools or connectors menu, then start a new chat. In VS Code, run **MCP: List Servers**, select trooth and choose **Show Output**.
- **Two trooth servers listed.** Remove both (`claude mcp remove trooth`, `codex mcp remove trooth`, `gemini mcp remove trooth`, or delete the entry from the configuration file) and add it once.
- **VS Code does not pick up the configuration.** The file is `.vscode/mcp.json` and its top-level key is `servers`. A block copied from Cursor or Claude Desktop uses `mcpServers`, which VS Code does not read.
- **Gemini CLI cannot connect.** Use `httpUrl` in `settings.json`; `url` makes the Gemini CLI expect server-sent events.
- **`405` on a `GET`.** The endpoint takes `POST` only. Use a client that speaks Streamable HTTP.
- **HTTP `429`.** The caller used its 300 units for the minute. Wait the seconds in `Retry-After`, then retry.
- **`401` from a tool that needs your workspace.** The call carried no token, or one that is invalid or expired. Sign in again from the client; an invalid token gets `401` even on a public tool, so remove a stale one.
- **`not_linked` from a workspace tool.** Sign in to trooth.co once with Continue with enterprise SSO, using the same account, to link the workspace.

## This repository

A license, this README, `server.json`, `docs/directory-submissions.md` (listing copy for the MCP directories), the manifest published to the official MCP Registry as `io.github.troothllc/trooth-network`, and `glama.json`, which names the maintainers of the Glama directory listing. The server itself runs at `https://mcp.trooth.co`; there is nothing here to install. The two older addresses, `api.trooth.co/public/mcp` and `api.trooth.co/os/semanto/mcp`, answer 404 with the new address, and the registry record `co.trooth/semanto` is deprecated in favor of this one.

`server.json` in this repository is version `1.2.0`, written for server `1.8.0`. The registry manifest and the running server carry separate version numbers: the server reports its own in `serverInfo`.

## Related

- [trooth-cli](https://github.com/troothllc/trooth-cli): the same records from your terminal, `npx trooth check <domain>`.
- [trooth-openapi](https://github.com/troothllc/trooth-openapi): the OpenAPI 3.1 contracts for the public API.
- [trooth.co/docs](https://trooth.co/docs): every interface, in one place.

## Security

Report a vulnerability to security@trooth.co. The policy is at [trooth.co/security/vulnerability-disclosure-policy](https://trooth.co/security/vulnerability-disclosure-policy) and [/.well-known/security.txt](https://trooth.co/.well-known/security.txt).

## License

MIT. See [LICENSE](LICENSE).
