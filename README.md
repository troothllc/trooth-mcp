# trooth-mcp

The Trooth Network over the Model Context Protocol. Four read-only tools, public data, no key.

Trooth operates the Trooth Network: one public, signed, machine-readable record per company, carrying its identity, products and demos, commercial terms, domain and marketing links, people, documents, security and privacy posture, AI practices, procurement terms and relationships. It is Trooth's only product and it is free.

DNS says where a company is. A TLS certificate says the connection is authentic. The Trooth Network says who the company is and what it does with your data.

**Trooth witnesses and dates facts. It does not score, rate, rank or certify anyone.** No tool here returns a number that sums a company up, and no such number exists. An agent that wants one is being asked to invent it.

## What is in this repository

A license, this README and `server.json`. There is no server code here, and there is nothing to install.

The server runs in the Worker behind `api.trooth.co`, which is a different repository. `server.json` is the manifest published to the official MCP Registry. What this repository is, therefore, is the registry record plus the description of a surface that lives somewhere else, and the honest thing to do is say which of the two any given fact came from.

Everything below was read from a committed file. The lines marked **live** were observed against the endpoint itself on 2026-09-24 by Trooth's verification script (`scripts/verify-public-mcp.mjs` in the site repository, not in this one), which writes its report to `docs/rebuild-package/PUBLIC_MCP_VERIFICATION.md` there. In that run nothing failed: the report lists 85 results that matched what was expected and 11 informational readings. That run is anonymous, from one network location, and says nothing about a later time.

## The endpoint

| | |
|---|---|
| URL | `https://api.trooth.co/public/mcp` |
| Method | `POST` only. **Live:** `GET` and `DELETE` both answer 405 with a JSON body saying so. |
| Transport | Streamable HTTP, JSON-RPC 2.0, one JSON response per POST. No SSE stream, no batches. **Live:** a batch request is refused with `-32600`, malformed JSON with `-32700`. |
| Sessions | None, under any revision. **Live:** no `Mcp-Session-Id` header is ever minted. |
| Authentication | **None.** No key, no cookie, no account. |
| Protocol revision | `2026-07-28` is what the server leads with. `2025-06-18`, `2025-03-26` and `2024-11-05` are still answered in full. **Live:** `server/discover` returned exactly those four. |
| Server identity | **Live:** `serverInfo` is `{"name":"trooth-mcp","version":"1.1.0"}`. |
| Capabilities | **Live:** `{"tools":{"listChanged":false},"resources":{"listChanged":false,"subscribe":false},"prompts":{"listChanged":false}}` and nothing else. |
| CORS | **Live:** `https://trooth.co` is granted. A foreign origin is granted nothing, on the request and on the OPTIONS check before it. |

`2026-07-28` removed protocol-level sessions and the `initialize` handshake and added a required `server/discover` RPC. Which era a request belongs to is decided by the presence of the reserved `_meta` key, not by the version value, so an unknown future version is refused rather than quietly served as an old one. **Live:** a `2026-07-28` request that omits the `MCP-Protocol-Version` header is refused with `-32020`, and so is an `Mcp-Method` header that contradicts the body.

An older client is not stranded. **Live:** `initialize` asking for `2025-06-18` gets `2025-06-18` back, asking for `2024-11-05` gets `2024-11-05`, and an unrecognized version falls back to `2025-06-18` rather than failing.

## The four tools

Four, and there are no others. **Live:** `tools/list` returned exactly these names. Each takes one required string argument. Each answers with prose in `content[0].text` for a person and the same answer as a typed record in `structuredContent` for a machine, from a declared `outputSchema`. All four are annotated `readOnlyHint: true`, `destructiveHint: false`, `idempotentHint: true`, `openWorldHint: true`.

### `trooth_public_trust_profile`

Argument `company`: a domain or a Trooth slug.

Returns the company's published Trust Profile if it has one. A profile Trooth has witnessed comes back as `witnessed_signed`, with the date it was last witnessed where the record carries one; a published profile with nothing witnessed comes back as `self_declared`, the company's own words. A company listed in the public directory without a published profile comes back with its directory entry, labeled `signed_scan` (the API's name for that provenance). When Trooth holds nothing, the answer is an honest absence carrying `provenance: "honest_absence"` and a `claim_url`, which is not a guess and not a negative finding about the company.

It reads the same source as `GET https://trooth.co/api/network/profile?q=`, deliberately, so the machine answer and the human page are built from the same record. The Worker caches that read for up to 120 seconds, so a change can take that long to appear here.

### `trooth_outside_in_read`

Argument `domain`.

A neutral read of that domain's public surface at the moment of the call: HTTPS and TLS reachability, the common security headers, `security.txt`. These are **observations**. They are not witnessed evidence, they are not a grade, and the response labels them that way. An outside-in read says what a stranger can see from the internet. It says nothing about what the company runs.

The input is hardened, and this is the one tool where that matters, because it is the only one that makes an outbound request on a caller's behalf. **Live:** an IPv4 literal, an IPv6 literal, `localhost`, the cloud metadata address `169.254.169.254`, a hostname resolving into `10.0.0.0/8` and a hostname resolving to loopback are each refused as `bad_input` with the reason named. A URL is reduced to its host before anything is read.

### `trooth_verify`

Argument `token`: a Trust Ledger Token in `tlt2.` or `tlt.` form, or its JTI.

Re-runs both signatures and answers `valid`, `expired`, `revoked` or `invalid`, the last when a signature does not match, which the answer tells you to treat as tampered. The limit travels in the answer: Trooth's signature covers the signing event and the payload at issuance, and never the truthfulness of the claims inside it. A caller that reads a valid token as proof that its claims are true has made the one mistake this product exists to prevent.

**Live:** an unknown token and outright garbage both come back as an honest absence. Neither is ever reported valid.

### `trooth_ask`

Argument `question`.

Answers questions about Trooth itself: what the Network is, what one record carries, what it costs, how witnessing works. It is not a question-answering service about a company; a question about a company is answered by the three tools above, which cite what they read. **Live:** a question outside the curated corpus comes back `out_of_scope` rather than invented.

## What comes back

`structuredContent` carries `status`, `provenance`, `subject`, `summary` and, where it applies, `claim_url`. **Live:** the `status` enum the server declares is exactly:

```
published  listed  unclaimed  private  observed  valid  invalid
expired  revoked  answered  out_of_scope  bad_input  unavailable
```

`provenance` is the label that keeps an observation from being read as a proof. The values the verification script holds the server to are `witnessed_signed`, `signed_scan`, `live_observation`, `honest_absence`, `withheld_by_owner`, `knowledge_base`, `input_error`, `self_declared` and `read_failed`. The live run exercised five of them: `witnessed_signed`, `live_observation`, `honest_absence`, `knowledge_base` and `input_error`, the last on every malformed argument.

Two states stay deliberately apart, and collapsing them would misrepresent a company:

- `unclaimed` with `honest_absence` means Trooth holds no record. That is an absence of evidence, not a finding.
- `private` with `withheld_by_owner` means a record exists and its owner chose not to publish it. That is a decision, not missing data.

## Resources and prompts

**Live:** three of each.

| Resource | |
|---|---|
| `trooth://methodology` | what witnessed means, how an outside-in read differs from signed evidence, and what an honest absence is |
| `trooth://provenance-labels` | what each provenance label an agent sees here means |
| `trooth://verify-a-vendor` | the four-step reading sequence |

Prompts: `vendor_trust_check`, `verify_trust_token`, `before_you_trust`.

## Discovery

`https://trooth.co/.well-known/mcp.json` is Trooth's own descriptor, and that file says so in its own first line. **MCP defines no `.well-known` path for advertising a server.** The specification's answer to "what is this server" is the in-band `server/discover` RPC, which needs the URL you already have, and a Server Card document is an active working-group proposal (SEP-2127, Draft) whose experimental shape is `GET <mcp-url>/server-card`, not that path. The descriptor is published because a fixed address a person or an agent can guess is worth having, and it is named as Trooth's own so nobody cites it as a convention.

There is no OAuth metadata beside it, and that absence is written down rather than left to be discovered. The MCP specification makes an authorized HTTP server an OAuth 2.0 protected resource; none of that applies here, because this server takes no credential and the four tools read data already public to any browser. So there is no protected resource, no protected-resource metadata and no authorization server, and the descriptor's `protectedResourceMetadata` is `null` rather than a URL nobody serves.

## The registry record

Published as `io.github.trooth-eng/trooth-network`. `io.github.*` is the registry's GitHub-authenticated namespace, which is why no signing key and no DNS proof were ever needed.

`server.json` in this repository is version `1.1.2`. **Live:** the registry read back on 2026-09-24 served `io.github.trooth-eng/trooth-network 1.1.2` as its latest version, and it points at `{"type":"streamable-http","url":"https://api.trooth.co/public/mcp"}`.

The manifest version and the running server's version are two different numbers on purpose, and since 2026-09-17 they have not matched: the publish was refused with `400 cannot-publish-duplicate-version` because `1.1.0` was already on the registry, so the payload went out as `1.1.1` while the Worker kept reporting `1.1.0`. `1.1.2`, published 2026-09-19, changed the description, and the Worker still reports `1.1.0`. Both numbers are recorded on every verification run, so a drift shows up in its report.

A `1.1.3` payload is prepared in the site repository and is not published. It is the file served at `https://trooth.co/.well-known/mcp-server.json`, and its description writes out the ampersand: "Trooth is an infrastructure and cybersecurity company providing Machine-Readable Trust." Until it is published, that file and `server.json` here differ on purpose, and `https://trooth.co/.well-known/mcp.json` declares the gap as `registry.pendingVersion`.

## What this server does not offer

**No long-running work, and no task handle.** `2026-07-28` changed the core lifecycle and removed sessions; it does not define a task lifecycle. Long-running work in MCP lives in a separate extension identified as `io.modelcontextprotocol/tasks`, published in draft beside the revision rather than inside it, and opt-in on both sides. **Live:** this server's capabilities are tools, resources and prompts and nothing else. There is no task capability and no task method. The Worker's code sets `resultType` to `"complete"` on every `2026-07-28` result.

Stated so nobody has to infer it: a client that supports the Tasks extension needs no special handling here and should offer none. All four tools are bounded reads that answer in the ordinary `tools/call` response. Trooth never returns a task handle and invents no field the revision it negotiated does not define. A client that does not support the extension is in exactly the same position, which is the point of saying so.

**No authenticated surface.** There is no signed-in company or buyer an agent can act as. Nothing here writes. Earlier plans listed a further ten public tools, eight buyer tools and eight company tools; none of them is built, and the reason is the same for all of them.

**No unknown tool runs.** **Live:** a call to a tool that does not exist answers `-32602` and executes nothing. A call with no arguments at all degrades to `bad_input` rather than throwing.

## Monitoring

A scheduled job in the site repository POSTs `tools/list` to the live endpoint every five minutes (GitHub may delay a scheduled run under load). Be clear about what that proves and what it does not: it checks the HTTP status code and that each of the four tool names appears somewhere in the body, so it catches the endpoint being down or losing a tool, and would not catch an extra tool, a changed schema or a wrong answer from a tool. The catalog and the served surface are compared properly by the verification run above, which is not continuous.

A test in the site repository holds the output contract published on [trooth.co/docs/agents](https://trooth.co/docs/agents) in two ways: against the `status` values the last verification run recorded, and against the Worker's `provenance` values when the Worker's repository is cloned beside it. Without that clone, its `provenance` check only confirms that the page publishes every `provenance` value the last run saw, and it says the fuller comparison was skipped. It is only as current as that last run.

## Add it

The steps below restate each vendor's own pages as [trooth.co/docs/agents](https://trooth.co/docs/agents) read them in September 2026. That page carries the plan limits, the removal steps and the sources.

**ChatGPT:** Trooth Network is listed in the ChatGPT app directory. [Open the listing](https://chatgpt.com/plugins/plugin_asdk_app_6a9361d2ee7481919a8c706c3ff5388f); OpenAI says whether an app is offered to you depends on your plan, region, workspace and role. To add it by hand instead you need Developer mode, which OpenAI lists for Plus, Pro, Business, Enterprise and Education accounts on the web and not for the Free plan: Settings, then Security and login, then turn on Developer mode; then open Plugins, press +, give it a name and paste the endpoint.

**Claude:** Customize, then Connectors, then Add custom connector, then paste the endpoint and press Add. If the dialog asks how people sign in, choose No sign-in. On Team and Enterprise an owner adds it for the organization first.

**Cursor, Cline, VS Code, or any other MCP client:** add a remote MCP server pointing at `https://api.trooth.co/public/mcp`. There is no key to configure.

## What earlier versions of this file got wrong

Named rather than quietly corrected, because a reader who acted on one of them deserves to know which line moved.

- The `status` table omitted `unavailable`. The server declares thirteen values and the table listed twelve, so a client branching exhaustively on that table had an unhandled case.
- The `provenance` table listed six values and omitted `input_error`, which is the label on every answer any of the four tools gives to a malformed argument. It is the one a client is most likely to meet first.
- The last published version of this file said `server.json` here and the copy served at `https://trooth.co/.well-known/mcp-server.json` both carried version `1.1.1` with different `description` fields, and that the registry held `1.1.1`. That was out of date: `server.json` here and the registry's latest record are now both `1.1.2`, published 2026-09-19; the website's copy is the prepared, unpublished `1.1.3` described under The registry record.
- The last published version of this file said the organization profile claims every public Trooth repository is Apache 2.0. That was out of date. `LICENSE` in this repository is MIT, every other repository in the profile's table is Apache 2.0, and the profile now says so: every repository in its table is Apache 2.0 except `trooth-mcp`, which is MIT.

## Security

Report a vulnerability through the [Vulnerability Disclosure Policy](https://trooth.co/security/vulnerability-disclosure-policy), or read [/.well-known/security.txt](https://trooth.co/.well-known/security.txt).

This server takes no credential, so there is none to leak through it. Every tool reads data already public to any browser. The only outbound request made on a caller's behalf is `trooth_outside_in_read`, and the refusals listed against that tool above are what keeps it from being pointed at a private address.

## Links

- The Network: [trooth.co/network](https://trooth.co/network)
- The agent pattern, written out: [trooth.co/docs/agents](https://trooth.co/docs/agents)
- The descriptor: [trooth.co/.well-known/mcp.json](https://trooth.co/.well-known/mcp.json)
- The registry payload: [trooth.co/.well-known/mcp-server.json](https://trooth.co/.well-known/mcp-server.json)
- The API reference: [trooth.co/docs/api](https://trooth.co/docs/api)
- How witnessing works, and what Trooth does not read: [trooth.co/methodology](https://trooth.co/methodology)
- Verify a signature yourself: [trooth.co/verify/keys](https://trooth.co/verify/keys)
- The command-line reader: [`troothllc/trooth-cli`](https://github.com/troothllc/trooth-cli), published on npm as **`trooth`**
- Publish your own record, free: [trooth.co/get-started](https://trooth.co/get-started)
- Contact: [trooth.co/contact](https://trooth.co/contact)

## License

MIT. See [LICENSE](LICENSE).

Trooth signs what it witnessed. It never signs on a company's behalf.
