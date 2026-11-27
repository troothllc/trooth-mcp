# Directory submission materials

Listing copy for the Trooth MCP server, written against the live `tools/list` (42 tools, read 2026-11-27) and registry record `co.trooth/trooth` 2.0.0. Nothing here has been submitted. Each section is sized to that directory's form; the shared values come first so every listing says the same thing.

## Shared values

| Field | Value |
| --- | --- |
| Name | Trooth |
| Server URL | `https://mcp.trooth.co` |
| Transport | Streamable HTTP, JSON-RPC 2.0, `POST` only, no sessions |
| Protocol revisions | `2026-07-28` (stateless), `2025-11-25`, `2025-06-18`, `2025-03-26`, `2024-11-05` |
| Registry name | `co.trooth/trooth` |
| Repository | `https://github.com/troothllc/trooth-mcp` |
| Website | `https://trooth.co` |
| Documentation | `https://trooth.co/docs/agents` |
| Privacy (what a query sends and what Trooth keeps) | `https://trooth.co/docs/query-privacy` |
| Privacy policy | `https://trooth.co/privacy` |
| Support and security contact | `security@trooth.co`, `https://trooth.co/security` |
| Authentication | None for the public record tools. Trust Profile and Semanto tools need an Open Authorization (OAuth) access token for a person signed in to trooth.co; the scopes are listed at `https://mcp.trooth.co/.well-known/oauth-protected-resource`. |
| Icon | `https://trooth.co/brand/trooth-badge-light-512.png` (light), `https://trooth.co/brand/trooth-badge-dark-512.png` (dark) |
| License | MIT (this repository) |

**Tagline** (50 characters): `One public record per company, read by your AI.`

**Short description** (99 characters, the registry record's): `Machine-Readable Company Profiles: find, search and read any company's Trust Profile, fact by fact.`

**Full description:**

> The Trooth Network holds one public record per company: identity, products, commercial terms, people, documents, security and privacy posture, artificial intelligence (AI) practices, procurement terms and relationships. Every fact says who said it, in one of four words (witnessed, public record, attested or declared), and carries its date where one is known. A section a company has not published is marked not published, never filled in, and Trooth keeps three negatives apart: no, not_published and not_read.
>
> With this server an assistant can find a company's record from its domain or name, search and list the Network, read a whole Trust Profile or one buyer question answer, see how a record changed, read a company's declared AI and data-use practices, take a live read of a domain's public HTTPS surface, and check the signatures on a Trust Ledger Token. A signed-in company can also fill and publish its own Trust Profile and set up Semanto: 12 of the 42 default tools change something, carry no read-only hint, and the client asks before running them. Results include text companies wrote about themselves; the server labels it as data, not instructions.
>
> Only the witness statement is signed: Trooth signs witness statements and, for workspaces that create a delegation, delegation receipts and action evidence references. It does not sign the profile, its facts or a company's declarations. No account is needed to read public records.

## Tools, one line each

| Tool | Title | Changes something |
| --- | --- | --- |
| `trooth_setup` | Set up Semanto and the Trust Profile, one step at a time | no |
| `trooth_hub_sync` | Send the signed facts to one listing hub | yes |
| `enable_toolset` | Show one more group of tools | no |
| `search_tools` | Find a tool by what it does | no |
| `call_tool` | Run a read-only tool by name | no |
| `call_write_tool` | Run a tool that changes something, by name | yes |
| `trooth_ai_data_use_disclosures` | Read a company's published AI and data-use declarations | no |
| `trooth_api_details` | Read one Trooth API operation's parameters | no |
| `trooth_api_search` | Search the Trooth API by keyword | no |
| `trooth_ask` | Ask about Trooth (the Trooth Network, what a record carries, pricing, methodology) | no |
| `trooth_company_changes` | Read how a company's record changed | no |
| `trooth_get_fact` | Read one fact or buyer question from a company's record | no |
| `trooth_inspect` | What one object ID names and where it is stored | no |
| `trooth_list_companies` | List companies on the Trooth Network by published facts | no |
| `trooth_my_company_record` | Read the caller's own workspace's company record | no |
| `trooth_outside_in_read` | Live read of a domain's HTTPS and response headers | no |
| `trooth_profile_checklist` | List what is left before the workspace's Trust Profile is published | no |
| `trooth_profile_diff` | Show where the workspace's Trust Profile draft and published page differ | no |
| `trooth_profile_domain_check` | Check the company's domain proof | yes |
| `trooth_profile_domain_start` | Start proving the company's domain | yes |
| `trooth_profile_get` | Read the workspace's Trust Profile draft | no |
| `trooth_profile_import` | Import a page of the company's website into the workspace's Trust Profile draft | yes |
| `trooth_profile_publish` | Publish the workspace's Trust Profile draft | yes |
| `trooth_profile_remediation_plan` | Plan the changes that close the open checklist items | no |
| `trooth_profile_update` | Change fields of the workspace's Trust Profile draft | yes |
| `trooth_profile_validate` | Check changes and the publish rules without saving | no |
| `trooth_public_trust_profile` | Read a company's complete Trust Profile | no |
| `trooth_resolve_company` | Find a company's Trooth Network record | no |
| `trooth_search_companies` | Search the Trooth Network for companies | no |
| `trooth_verify` | Check a signed copy or a Trust Ledger Token | no |
| `trooth_semanto_status` | Setup status and next step | no |
| `trooth_semanto_setup_read_again` | Read the website again | yes |
| `trooth_semanto_setup_choose` | Choose Basic or the full signed copy | yes |
| `trooth_semanto_review_get` | The facts to review | no |
| `trooth_semanto_setup_confirm` | Confirm the reviewed version | yes |
| `trooth_semanto_sign` | Sign and publish | yes |
| `trooth_semanto_install_guide` | How to put it on the site | no |
| `trooth_semanto_install_check` | Check my site | yes |
| `trooth_semanto_page` | Read one signed page | no |
| `trooth_semanto_facts` | A company's signed facts | no |
| `trooth_semanto_answer` | Answer a question from a company's signed record | no |
| `trooth_semanto_statement_check` | Check a statement against the signed record | no |

## Example prompts

1. "Find the Trooth Network record for stripe.com and tell me whether it says Stripe trains AI models on customer data, with the provenance and date."
2. "Read trooth.co's Trust Profile summary and list which sections are not published."
3. "Where does Acme store customer data, and can a customer choose the region? Cite the anchor."
4. "List witnessed companies on the Trooth Network in the Fintech industry with an EU data region."
5. "What changed on trooth.co's record since 2026-10-01?"
6. "Run vendor due diligence on example.com." (prompt `vendor_due_diligence`)
7. "Take a live read of example.com's public security headers."

A reviewer needs no account. A call that always returns content: `trooth_ask` with `{"question": "What is Trooth?"}`, or `trooth_outside_in_read` with `{"domain": "example.com"}`.

## Anthropic connector directory

| Field | Value |
| --- | --- |
| Connector name (100 max) | Trooth |
| Short description (55 max) | One public record per company, read by your AI. |
| Long description (2,000 max) | The full description above. |
| Server URL | `https://mcp.trooth.co` |
| Authentication | No sign-in for the public record tools. Workspace and Semanto tools use OAuth through the authorization server named at `https://mcp.trooth.co/.well-known/oauth-protected-resource`; a call without a token answers 401 with `WWW-Authenticate`. |
| Tool titles and annotations | Present on every tool (see the table above). |
| Large results | `trooth_public_trust_profile` sets `_meta["anthropic/maxResultSizeChars"]` to 200,000; `detail: "summary"` returns a short answer. |
| Privacy policy | `https://trooth.co/docs/query-privacy` and `https://trooth.co/privacy` |
| Support contact | `security@trooth.co`, `https://trooth.co/security` |
| Documentation | `https://trooth.co/docs/agents` |
| Example prompts | 1, 2, 3, 6 and 7 above. |

## Smithery

Smithery lists remote servers by URL. Use the shared values, plus:

- Display name: Trooth
- Description: the short description.
- Connection: Streamable HTTP at `https://mcp.trooth.co`; no configuration fields.
- Authentication: none for public records; OAuth for workspace and Semanto tools.
- Tools are read from `tools/list`; the table above is the copy to check against.

## Glama

The listing is claimed through `glama.json` in this repository (maintainer `trooth-eng`) and the connector claim the server publishes. Use:

- Name: Trooth
- Description: the full description.
- Category: Security; Research; Developer Tools.
- Hosting: remote, `https://mcp.trooth.co`.
- Support: `security@trooth.co`.

## mcp.so

- Name: Trooth
- URL: `https://mcp.trooth.co`
- Repository: `https://github.com/troothllc/trooth-mcp`
- Description: the short description, then the tool table.
- Tags: company records, vendor due diligence, trust, security, privacy.

## PulseMCP

- Server name: Trooth
- Remote endpoint: `https://mcp.trooth.co` (Streamable HTTP)
- Registry name: `co.trooth/trooth`
- Summary: the short description.
- Description: the full description.
- Authentication: none for public records; OAuth for workspace and Semanto tools.
- Contact: `security@trooth.co`, `https://trooth.co/security`.

## Before submitting

- Check that `https://trooth.co/docs/query-privacy` is live: it is the privacy link every listing above uses.
- Confirm the live `tools/list` still shows 42 tools; if not, regenerate the tool table below from it.
