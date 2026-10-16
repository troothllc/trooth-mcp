# Directory submission materials

Listing copy for the Trooth public MCP server, written for server `1.8.0` and registry record `1.1.7`. Nothing here has been submitted. Each section is sized to that directory's form; the shared values come first so every listing says the same thing.

## Shared values

| Field | Value |
| --- | --- |
| Name | Trooth Network |
| Server URL | `https://api.trooth.co/public/mcp` |
| Transport | Streamable HTTP, JSON-RPC 2.0, `POST` only, no sessions |
| Protocol revisions | `2026-07-28` (stateless), `2025-11-25`, `2025-06-18`, `2025-03-26`, `2024-11-05` |
| Registry name | `io.github.troothllc/trooth-network` |
| Repository | `https://github.com/troothllc/trooth-mcp` |
| Website | `https://trooth.co` |
| Documentation | `https://trooth.co/docs/agents` |
| Privacy (what a query sends and what Trooth keeps) | `https://trooth.co/docs/query-privacy` |
| Privacy policy | `https://trooth.co/privacy` |
| Support and security contact | `security@trooth.co`, `https://trooth.co/security` |
| Authentication | None, except `trooth_my_company_record`, which needs an Open Authorization (OAuth) access token for a person signed in to trooth.co. No scope is required. |
| Icon | `https://trooth.co/brand/trooth-badge-light-512.png` (light), `https://trooth.co/brand/trooth-badge-dark-512.png` (dark) |
| License | MIT (this repository) |

**Tagline** (50 characters): `One public record per company, read by your AI.`

**Short description** (99 characters, the registry record's): `Machine-Readable Company Profiles: find, search and read any company's Trust Profile, fact by fact.`

**Full description:**

> The Trooth Network holds one public record per company: identity, products, commercial terms, people, documents, security and privacy posture, artificial intelligence (AI) practices, procurement terms and relationships. Every fact says who said it, in one of four words (witnessed, public record, attested or declared), and carries its date where one is known. A section a company has not published is marked not published, never filled in, and Trooth keeps three negatives apart: no, not_published and not_read.
>
> With this server an assistant can find a company's record from its domain or name, search and list the Network, read a whole Trust Profile or one buyer question answer, see how a record changed, read a company's declared AI and data-use practices, take a live read of a domain's public HTTPS surface, and check the signatures on a Trust Ledger Token. Every tool is read only. Results include text companies wrote about themselves; the server labels it as data, not instructions.
>
> Only the witness statement is signed: Trooth signs witness statements and, for workspaces that create a delegation, delegation receipts and action evidence references. It does not sign the profile, its facts or a company's declarations. No account is needed except for one tool that reads the signed-in person's own workspace.

## Tools, one line each

| Tool | Title | One line |
| --- | --- | --- |
| `trooth_resolve_company` | Find a company's Trooth Network record | Takes a domain, web address, Trooth slug or name and returns the record ID, canonical address and why it matched, or every candidate with no pick. |
| `trooth_search_companies` | Search the Trooth Network for companies | Searches names, domains, taglines and categories; 10 per page in slug order with a cursor. |
| `trooth_list_companies` | List companies on the Trooth Network by published facts | Lists by industry, data region, witnessed or not, and claimed or unclaimed; 20 per page in a fixed order. |
| `trooth_public_trust_profile` | Read a company's complete Trust Profile | One company's record by domain or slug, as a short summary or section by section. |
| `trooth_get_fact` | Read one fact or buyer question from a company's record | One answer with its sentence, value, provenance word, date, freshness and anchor, or no, not_published or not_read. |
| `trooth_company_changes` | Read how a company's record changed | Record versions and per-fact edits, oldest first, optionally since a date. |
| `trooth_ai_data_use_disclosures` | Read a company's published AI and data-use declarations | What a company declared about training on customer data and AI in its product, labeled as its own words. |
| `trooth_outside_in_read` | Live read of a domain's public security surface | Fetches the domain's home page and `security.txt` once and reports HTTPS, five response headers and `security.txt`. |
| `trooth_verify` | Check the signatures on a Trust Ledger Token | Checks the company's and Trooth's signatures on a token a company issued about its own claims. |
| `trooth_ask` | Ask about Trooth | Answers questions about Trooth itself from a fixed knowledge base. |
| `trooth_my_company_record` | Read your own workspace's company record (sign-in required) | The signed-in person's own workspace record: name, slug and public page address. |

Every tool is annotated `readOnlyHint: true`, `destructiveHint: false`, `idempotentHint: true`; only `trooth_outside_in_read` is `openWorldHint: true`. Every tool declares an `outputSchema` and returns `structuredContent`.

Resources: `trooth://company/{company}`, `trooth://company/{company}/{section}`, `trooth://methodology`, `trooth://provenance-glossary`, `trooth://provenance-labels`, `trooth://buyer-questions`, `trooth://schema`, `trooth://what-a-call-sends`, `trooth://verify-a-vendor`.

Prompts: `vendor_due_diligence` (Vendor due diligence), `vendors_side_by_side` (Vendors side by side), `vendor_trust_check`, `before_you_trust`, `verify_trust_token`.

## Example prompts

1. "Find the Trooth Network record for stripe.com and tell me whether it says Stripe trains AI models on customer data, with the provenance and date."
2. "Read trooth.co's Trust Profile summary and list which sections are not published."
3. "Where does Acme store customer data, and can a customer choose the region? Cite the anchor."
4. "List witnessed companies on the Trooth Network in the Fintech industry with an EU data region."
5. "What changed on trooth.co's record since 2026-10-01?"
6. "Run vendor due diligence on example.com." (prompt `vendor_due_diligence`)
7. "Set acme.com, globex.com and initech.com side by side on the twelve buyer questions." (prompt `vendors_side_by_side`)
8. "Take a live read of example.com's public security headers."

A reviewer needs no account. A call that always returns content: `trooth_ask` with `{"question": "What is Trooth?"}`, or `trooth_outside_in_read` with `{"domain": "example.com"}`.

## Anthropic connector directory

| Field | Value |
| --- | --- |
| Connector name (100 max) | Trooth Network |
| Short description (55 max) | One public record per company, read by your AI. |
| Long description (2,000 max) | The full description above. |
| Server URL | `https://api.trooth.co/public/mcp` |
| Authentication | No sign-in for ten tools. `trooth_my_company_record` uses OAuth through the authorization server named at `https://api.trooth.co/.well-known/oauth-protected-resource/public/mcp`; a call without a token answers 401 with `WWW-Authenticate`. |
| Tool titles and annotations | Present on every tool (see the table above). |
| Large results | `trooth_public_trust_profile` sets `_meta["anthropic/maxResultSizeChars"]` to 200,000; `detail: "summary"` returns a short answer. |
| Privacy policy | `https://trooth.co/docs/query-privacy` and `https://trooth.co/privacy` |
| Support contact | `security@trooth.co`, `https://trooth.co/security` |
| Documentation | `https://trooth.co/docs/agents` |
| Example prompts | 1, 2, 3, 6 and 7 above. |

## Smithery

Smithery lists remote servers by URL. Use the shared values, plus:

- Display name: Trooth Network
- Description: the short description.
- Connection: Streamable HTTP at `https://api.trooth.co/public/mcp`; no configuration fields.
- Authentication: none, except the one OAuth tool, which a Smithery client may leave unused.
- Tools are read from `tools/list`; the table above is the copy to check against.

## Glama

The listing is claimed through `glama.json` in this repository (maintainer `trooth-eng`) and the connector claim the server publishes. Use:

- Name: Trooth Network
- Description: the full description.
- Category: Security; Research; Developer Tools.
- Hosting: remote, `https://api.trooth.co/public/mcp`.
- Support: `security@trooth.co`.

## mcp.so

- Name: Trooth Network
- URL: `https://api.trooth.co/public/mcp`
- Repository: `https://github.com/troothllc/trooth-mcp`
- Description: the short description, then the tool table.
- Tags: company records, vendor due diligence, trust, security, privacy, read only.

## PulseMCP

- Server name: Trooth Network
- Remote endpoint: `https://api.trooth.co/public/mcp` (Streamable HTTP)
- Registry name: `io.github.troothllc/trooth-network`
- Summary: the short description.
- Description: the full description.
- Authentication: none (one optional OAuth tool).
- Contact: `security@trooth.co`, `https://trooth.co/security`.

## Before submitting

- Check that `https://trooth.co/docs/query-privacy` is live: it is the privacy link every listing above uses.
- Confirm the live `tools/list` shows eleven tools and `serverInfo.version` `1.8.0`.
