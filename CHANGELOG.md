# Changelog

## v1.3.0 — October 2026

The M-26-15 delta. EO 14412 (June 22, 2026) and OMB Memorandum M-26-15 (June 24, 2026) created a federal filing obligation that did not exist when v1.2.0 shipped. This release adds that filing layer; the corpus's structure and fact IDs are unchanged.

**New files:**
* `Plan_Template.md` — agency PQC migration plan template, one section per M-26-15 Appendix B element, with the Appendix B governance role table and a funding/personnel estimation scaffold (the element the corpus did not cover).
* `Federal_Timeline.md` — binding federal PQC dates, verified against the primary documents (M-26-15 read in full; EO 14412 full text).
* `Filer_Checklist.md` — the October 22 filer's checklist: done-criteria for all nine plan sections, pre-submission cross-checks, final-week work order.

**Fact updates (corpus):**
* §2.1: Cloudflare deployment updated to September 2026 figures (~70% of browser-generated traffic on PQ hybrid key agreement; ~15% of origins), replacing the December 2025 figure. Golden query B1 updated to match.
* §1.2: FIPS 206 status pinned — in development, no public draft as of October 2026.
* §1.4: 2029 full-PQ targets re-checked (Cloudflare reaffirmed; Google announced a matching target in March 2026); figures otherwise carried.

**New-document conventions:** NIST IR 8547 cited as an initial public draft; TLS 1.3 cited as RFC 9846; NSS governance referenced under NSPM-12 (June 2026).

**Corrections:**
* README bundle table: corpus filename corrected to `Corpus.md`, matching the file as shipped in v1.2.0.

## v1.2.0 — April 2026

Major restructure. Same strategic argument, far better retrieval and eval discipline.

**Corpus (`Quantum_Safe_Strategy_Corpus.md`):**
* Restructured around fact IDs (`§N.M`) so RAG systems can cite specific claims.
* Added explicit **Scope** block at the top listing what's in and out of scope.
* Added cross-references between sections (e.g., §3 points to §6 for discovery).
* Added provenance block at the bottom listing dated sources for specific numbers.
* **Fact updates:**
  * §1.2: added FIPS 203/204/205 final publication (Aug 2024), HQC selection (Mar 2025), FIPS 206 status.
  * §1.3: replaced blanket "2030 deadline" with NSA CNSA 2.0 category-specific dates (2030 for signing and network equipment; 2033 for browsers/servers/OS).
  * §1.4: new section on compressed Q-Day timelines (neutral-atom breakthroughs, Cloudflare's 2029 internal target).
  * §2.1: Cloudflare adoption updated to ~52% (Dec 2025), attribution to browser defaults and iOS 26.
  * §2.3: new section on the authentication gap (PQ certificates still not broadly deployed).
  * §3.5: added priority guidance to fix key agreement before authentication.
  * §4: replaced unsourced "~20% latency" claim with mechanism-based description and real-world caveats.
  * §6.4: added CBOM (Cryptographic Bill of Materials) concept.
  * §9.3: sharpened vendor validation criteria with FIPS 140-3 / CAVP references.

**Evals (`evals/golden_queries.md`):**
* Expanded from 4 queries to 15 golden queries plus 6 distractor queries.
* Added explicit **Pass** and **Fail** criteria per query.
* Added **Distractor Queries** section to test scope enforcement (QKD, physics, vendor names, Q-Day predictions, legal, general crypto tutorials).
* Added a "Running the Evals" section with common fix patterns.

**Integration (`INTEGRATION.md`) — new file:**
* Split out from README. Contains system prompt, chunking guidance, retrieval settings, update cadence.
* Tightened system prompt: requires fact-ID citations, forbids training-data supplementation, mandates scope enforcement, defines the operational/preventive ethos.

**README (`README.md`):**
* Rewritten for human strategic readers. No more RAG ingestion steps mixed with human-facing scenarios.
* Highlights the "Big 4 risks" structure as the operational frame.
* Explicitly lists what the bundle does not cover.

## v1.1.0 — prior

Initial RAG corpus + validation queries + README bundle.

---

## Maintenance cadence

* **Quarterly:** re-check §1.4 (Q-Day) and §2.1 (adoption %).
* **On NIST publication:** update §1.2 and §10.1 if FIPS 206 ships or if additional signature/KEM schemes are standardized.
* **On CA/Browser Forum decisions:** update §2.3 when hybrid certificate format is finalized.
* **On cryptanalysis news:** update §10.1 if any current NIST algorithm is weakened.
