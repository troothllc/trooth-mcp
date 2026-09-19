# trooth-mcp

The Trooth Network over the Model Context Protocol. Four read-only tools, public data, no key.

Trooth operates the Trooth Network: one public, signed, machine-readable record per company, carrying its identity, products and demos, commercial terms, domain and marketing links, people, documents, security and privacy posture, AI practices, procurement terms and relationships. It is Trooth's only product and it is free.

DNS says where a company is. A TLS certificate says the connection is authentic. The Trooth Network says who the company is and what it does with your data.

**Trooth witnesses and dates facts. It does not score, rate, rank or certify anyone.** No tool here returns a number that sums a company up, and none of them exists. An agent that wants one is being asked to invent it.

## What is in this repository

A licence, this README and `server.json`. There is no server code here, and there is nothing to install.

The server runs in the Worker behind `api.trooth.co`, which is a different repository. `server.json` is the manifest published to the official MCP Registry. What this repository is, therefore, is the registry record plus the description of a surface that lives somewhere else, and the honest thing to do is say which of the two any given fact came from.

Everything below was read from a committed file. The lines marked **live** were observed against the endpoint itself on 2026-09-19 by Trooth's verification script — `scripts/verify-public-mcp.mjs` in the site repository, not in this one — which writes its report to `docs/rebuild-package/PUBLIC_MCP_VERIFICATION.md` there: 83 checks passed, 0 failed, 8 informational. That run is anonymous, from one network location, and says nothing about a later time.

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

An older client is not stranded. **Live:** `initialize` asking for `2025-06-18` gets `2025-06-18` back, asking for `2024-11-05` gets `2024-11-05`, and an unrecognised version falls back to `2025-06-18` rather than failing.

## The four tools

Four, and there are no others. **Live:** `tools/list` returned exactly these names. Each takes one required string argument. Each answers with prose in `content[0].text` for a person and the same answer as a typed record in `structuredContent` for a machine, from a declared `outputSchema`. All four are annotated `readOnlyHint: true`, `destructiveHint: false`, `idempotentHint: true`, `openWorldHint: true`.

### `trooth_public_trust_profile`

Argument `company`: a domain or a Trooth slug.

Returns the company's published Trust Profile if it has one — signed evidence, re-checked on a schedule — or its Network standing from a signed scan. When Trooth holds nothing, the answer is an honest absence carrying `provenance: "honest_absence"` and a `claim_url`, which is not a guess and not a negative finding about the company.

It reads the same source as `GET /api/network/profile?q=`, deliberately, so the machine answer and the human page cannot disagree about the same company.

### `trooth_outside_in_read`

Argument `domain`.

A neutral read of that domain's public surface at the moment of the call: HTTPS and TLS reachability, the common security headers, `security.txt`. These are **observations**. They are not witnessed evidence, they are not a grade, and the response labels them that way. An outside-in read says what a stranger can see from the internet. It says nothing about what the company runs.

The input is hardened, and this is the one tool where that matters, because it is the only one that makes an outbound request on a caller's behalf. **Live:** an IPv4 literal, an IPv6 literal, `localhost`, the cloud metadata address `169.254.169.254`, a hostname resolving into `10.0.0.0/8` and a hostname resolving to loopback are each refused as `bad_input` with the reason named. A URL is reduced to its host before anything is read.

### `trooth_verify`

Argument `token`: a Trust Ledger Token in `tlt2.` or `tlt.` form, or its JTI.

Re-runs both signatures and answers valid, expired, revoked or tampered. The limit travels in the answer: Trooth's signature attests the signing event and the payload at issuance, and never the truthfulness of the claims inside it. A caller that reads a valid token as a verified claim has made the one mistake this product exists to prevent.

**Live:** an unknown token and outright garbage both come back as an honest absence. Neither is ever reported valid.

### `trooth_ask`

Argument `question`.

Answers questions about Trooth itself — what the Network is, what one record carries, what it costs, how witnessing works. It is not a question-answering service about a company; a question about a company is answered by the three tools above, which cite what they read. **Live:** a question outside the curated corpus comes back `out_of_scope` rather than invented.

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
| `trooth://methodology` | how identity is confirmed and how facts are witnessed |
| `trooth://provenance-labels` | the closed vocabulary every fact carries |
| `trooth://verify-a-vendor` | the four-step reading sequence |

Prompts: `vendor_trust_check`, `verify_trust_token`, `before_you_trust`.

## Discovery

`https://trooth.co/.well-known/mcp.json` is Trooth's own descriptor, and that file says so in its own first line. **MCP defines no `.well-known` path for advertising a server.** The specification's answer to "what is this server" is the in-band `server/discover` RPC, which needs the URL you already have, and a Server Card document is an active working-group proposal (SEP-2127, Draft) whose experimental shape is `GET <mcp-url>/server-card`, not that path. The descriptor is published because a fixed address a person or an agent can guess is worth having, and it is named as Trooth's own so nobody cites it as a convention.

There is no OAuth metadata beside it, and that absence is written down rather than left to be discovered. The MCP specification makes an authorized HTTP server an OAuth 2.0 protected resource; none of that applies here, because this server takes no credential and the four tools read data already public to any browser. So there is no protected resource, no protected-resource metadata and no authorization server, and the descriptor's `protectedResourceMetadata` is `null` rather than a URL nobody serves.

## The registry record

Published as `io.github.trooth-eng/trooth-network`. `io.github.*` is the registry's GitHub-authenticated namespace, which is why no signing key and no DNS proof were ever needed.

`server.json` in this repository is version `1.1.1`. **Live:** the manifest read back from the registry on 2026-09-19 was `io.github.trooth-eng/trooth-network 1.1.1`, and it points at `{"type":"streamable-http","url":"https://api.trooth.co/public/mcp"}`.

The manifest version and the running server's version are two different numbers on purpose, and since 2026-09-17 they have not matched: the publish was refused with `400 cannot-publish-duplicate-version` because `1.1.0` was already on the registry, so the payload went out as `1.1.1` while the Worker kept reporting `1.1.0`. Both are read back on every verification run, so neither can drift without somebody being told.

## What this server does not offer

**No long-running work, and no task handle.** `2026-07-28` changed the core lifecycle and removed sessions; it does not define a task lifecycle. Long-running work in MCP lives in a separate extension identified as `io.modelcontextprotocol/tasks`, published in draft beside the revision rather than inside it, and opt-in on both sides. **Live:** this server's capabilities are tools, resources and prompts and nothing else. No task capability, no task method, no `resultType` discriminator.

Stated so nobody has to infer it: a client that supports the Tasks extension needs no special handling here and should offer none. All four tools are bounded reads that answer in the ordinary `tools/call` response. Trooth never returns a task handle and invents no field the revision it negotiated does not define. A client that does not support the extension is in exactly the same position, which is the point of saying so.

**No authenticated surface.** There is no signed-in company or buyer an agent can act as. Nothing here writes. Earlier plans listed a further ten public tools, eight buyer tools and eight company tools; none of them is built, and the reason is the same for all of them.

**No unknown tool runs.** **Live:** a call to a tool that does not exist answers `-32602` and executes nothing. A call with no arguments at all degrades to `bad_input` rather than throwing.

## Monitoring

A scheduled job POSTs `tools/list` to the live endpoint every five minutes. Be clear about what that proves and what it does not: it checks the HTTP status code and discards the body, so it catches the endpoint being down and would not catch it answering 200 with the wrong tools in it. The catalogue and the served surface are compared properly by the verification run above, which is not continuous.

The output-schema contract is pinned against the Worker by a test in the site repository, so a change on one side cannot pass unnoticed on the other.

## Add it

**ChatGPT:** Trooth Network is listed in the ChatGPT app directory. [Open the listing](https://chatgpt.com/plugins/plugin_asdk_app_6a9361d2ee7481919a8c706c3ff5388f) and press Try in chat. To add it by hand instead: Settings, then Apps, then Create, then paste the endpoint as the MCP server URL.

**Claude:** Settings, then Connectors, then Add custom connector, then paste the endpoint.

**Cursor, Cline, VS Code, or any other MCP client:** add a remote MCP server pointing at `https://api.trooth.co/public/mcp`. There is no key to configure.

## What earlier versions of this file got wrong

Named rather than quietly corrected, because a reader who acted on one of them deserves to know which line moved.

- The `status` table omitted `unavailable`. The server declares thirteen values and the table listed twelve, so a client branching exhaustively on that table had an unhandled case.
- The `provenance` table listed six values and omitted `input_error`, which is the label on every answer any of the four tools gives to a malformed argument. It is the one a client is most likely to meet first.
- `server.json` here and the copy of the same payload served at `https://trooth.co/.well-known/mcp-server.json` both carry version `1.1.1` and carry **different** `description` fields. Two files claiming to be the same published record at the same version cannot both be it. Which one the registry holds is not settled from this repository, and it is written down here rather than left looking tidy.
- `LICENSE` in this repository is MIT. Every other public Trooth repository is Apache 2.0, and the organisation profile says all of them are. One of those two has to change, and until it does this line is the only place the disagreement is recorded.

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

Trooth automates. Trooth never signs for you.
