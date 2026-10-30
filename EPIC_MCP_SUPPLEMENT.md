# Supplementary Note: open.epic MCP Documentation Integration (Post-Publication)

**Status: post-publication implementation note — 2026-10-30.**
This file is a supplement added after the paper in this repository was
published. It is **not part of the paper, does not change the paper's
methods, data, or results, and reports no new study findings.** It records
follow-on implementation work that FHIR infrastructure projects — including
projects of the kind this paper studies — can adopt when building against
Epic's APIs.

## What was published by Epic

Epic publishes a remote Model Context Protocol (MCP) server under the
open.epic programme:

| Item | Value |
|---|---|
| Server | "open.epic MCP", version 4.0.9 (verified live 2026-10-30) |
| URL | `https://mcp.open.epic.com` |
| Transport | Streamable HTTP |
| Authentication | None documented |
| Source | https://fhir.epic.com/documentation?docid=mcp |

## What the server is — and is not

The server returns **documentation content only**: API specifications,
interface summaries, additional documentation such as data-sharing playbooks
and tutorials, and FAQs — including Epic on FHIR content.

It is **not** a patient-data or FHIR-resource server. It does not query
`Patient`, `Observation`, `Encounter`, or any other FHIR resource, and it is
not a SMART on FHIR / OAuth endpoint. Patient data continues to flow only
through a project's own FHIR R4 client with SMART on FHIR authorisation —
never through this MCP server.

Epic explicitly states the server is **not designated to handle protected
health information (PHI) or confidential information**, and that the user is
responsible for anything transmitted to it. In practice: only
documentation-style queries (API names, resource *type* names, spec topics)
should ever be sent — never patient identifiers or real FHIR resource JSON.
Use is subject to the open.epic Terms of Use.

## Live tools (discovered via MCP `tools/list`, 2026-10-30)

| Tool | Arguments | What it does |
|---|---|---|
| `search_documents` | `query` (string, ≤ 500 chars) | Keyword/phrase search across Epic documentation; returns documents with id, url, title, and snippet. |
| `get_document` | `document_id` (e.g. `api:x`, `tutorial:x`, `faq:x`, `interface:x:x`); optional `offset` (int) | Returns the full contents of one document; paginate with `offset` / `next_offset` in 20,000-character pages when a document is large. |

## Why it is relevant as follow-on work

The paper this repository accompanies is about the plumbing layer under
clinical AI: FHIR servers, synthetic data, and federated infrastructure, and
about the care with which standards-adjacent code is changed. A
documentation-lookup integration of this kind fits that theme as
implementation practice, not as research:

- Developers contributing to FHIR infrastructure can retrieve the current
  Epic API specification for the resource types they are working on (for
  example `MedicationRequest`, `DocumentReference`, or `Claim`) from inside
  their AI client, instead of relying on remembered or stale spec details.
- Because the server carries no patient data and no credentials, it can be
  wired into developer tooling without changing a project's security or
  governance posture for clinical data — provided the no-PHI rule above is
  respected.
- Recording which spec version a validation or implementation decision was
  checked against makes that decision re-checkable when Epic's documentation
  changes — the same auditability concern the paper raises for infrastructure
  contributions generally.

## Scope note

No code, data, figures, or text from the paper were modified to add this
note, and nothing here should be cited as part of the paper's results. The
paper remains as published (Zenodo DOI:
https://doi.org/10.5281/zenodo.23549199).
