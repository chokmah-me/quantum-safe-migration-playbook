# Quantum-Safe Migration Playbook (v1.3.0)

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19655681.svg)](https://doi.org/10.5281/zenodo.19655681)
[![GitHub release](https://img.shields.io/github/v/release/chokmah-me/quantum-safe-migration-playbook)](https://github.com/chokmah-me/quantum-safe-migration-playbook/releases/tag/v1.3.0)

A strategic knowledge base for the move to post-quantum cryptography (PQC). Written for CISOs, system architects, and security engineers who need to turn a theoretical cryptographic risk into a dated operational plan.

- **Concept DOI** (always latest): [10.5281/zenodo.19655681](https://doi.org/10.5281/zenodo.19655681)
- **Version DOI** (v1.3.0): [10.5281/zenodo.23347471](https://doi.org/10.5281/zenodo.23347471)
- Catalog: [chokmah.me landing](https://chokmah.me/research/quantum-safe-migration-playbook-strategic-operational-guide-23347471/)
- Citation file: [`CITATION.cff`](CITATION.cff) · changelog: [`CHANGELOG.md`](CHANGELOG.md)

Canonical public record is the Zenodo deposit (CC BY 4.0). This GitHub tree ships the same files as `quantum-safe-migration-playbook-v1.3.0.tar.gz`. The `LICENSE` file in the tree is CC-BY-SA-4.0, matching the tarball; the live Zenodo/DataCite record remains CC BY 4.0.

## What's in this bundle

| File | Who uses it | What it's for |
|---|---|---|
| `README.md` | Anyone | This overview. Start here. |
| `Corpus.md` | Strategists, AI engineers | The source of truth. Structured facts with IDs, cross-references, and scope boundaries. |
| `Plan_Template.md` | Agency plan authors | The M-26-15 Appendix B migration-plan template: one section per required element, role table, funding/personnel estimation scaffold. New in v1.3.0. |
| `Federal_Timeline.md` | Planners | Binding federal PQC dates (EO 14412, M-26-15), verified against the primary documents. New in v1.3.0. |
| `Filer_Checklist.md` | Agency filers | The October 22 filing checklist: nine done-criteria, cross-checks, final-week order. New in v1.3.0. |
| `INTEGRATION.md` | AI engineers | How to ingest the corpus into a RAG system, plus the system prompt. |
| `evals/golden_queries.md` | AI engineers | Retrieval tests with pass/fail criteria and distractors. |
| `CHANGELOG.md` | Maintainers | Version history. |
| `AI_USE_STATEMENT.md` | Anyone | How AI assistance was used in producing this release, and what it was not used for. |

The original deck (`Quantum-Safe Migration Playbook.pptx`) is the visual narrative. This bundle is the text version you feed into systems, policies, and RFPs.

## The four risks the corpus covers

The playbook's operational claim is that four risk categories, taken together, define whether a migration succeeds or slips:

1. **Hardware.** Legacy ASICs, bootloader trust stores, HSMs, VPN appliances. Covered in §5.
2. **Latency and bandwidth.** Larger keys and signatures change handshake behavior on real networks. Covered in §4.
3. **Vendor readiness.** Distinguishing shipped-and-verified from PowerPoint-promise. Covered in §9.
4. **Discovery.** Finding every place crypto already lives. Covered in §6.

If your migration plan does not have an owner for each of these, it has a gap.

## How to use this (pick one)

**If you are writing strategy or policy:** open the corpus and use the section structure as a scaffold. Copy fact IDs into your internal docs so reviewers can trace claims back.

**If you are running the playbook as an AI advisor:** follow `INTEGRATION.md` to set up retrieval. Run the golden queries in `evals/` before you let anyone use the system.

**If you are evaluating vendors:** read §9 of the corpus. The RFP clauses and the red-flags list are ready to paste into procurement documents.

**If you are building a program plan:** read §1.3 (federal timelines), §3 (what to protect first), §8 (how to pilot). That's enough to size the effort.

**If you are filing a federal PQC migration plan under OMB M-26-15:** start with `Filer_Checklist.md`, write the plan in `Plan_Template.md`, and check every date against `Federal_Timeline.md`.

## What this bundle does not give you

* Physics of quantum computing. Out of scope.
* Quantum Key Distribution (QKD). Different technology, different threat model.
* Specific product or vendor recommendations.
* Legal compliance interpretation.
* A Q-Day prediction.

The corpus says "no" to these explicitly so your RAG system doesn't invent answers.

## Version and freshness

Current: **v1.3.0**, last reviewed **October 2026**. The v1.3.0 delta adds the M-26-15 filing layer (plan template, federal timeline, filer checklist) created by EO 14412 and OMB M-26-15 after v1.2.0 shipped; see `CHANGELOG.md`.

Two facts move fast and should be re-checked every quarter: the Q-Day timeline (§1.4) and the deployment percentage (§2.1). See `CHANGELOG.md` for what changed between versions.

## Citation

Prefer the concept DOI so readers get the current file:

```bibtex
@unpublished{Bilar2026QuantumSafePlaybook,
  author       = {Bilar, Daniyel Yaacov},
  title        = {Quantum-Safe Migration Playbook (Strategic \& Operational Guide)},
  year         = {2026},
  month        = {10},
  version      = {1.3.0},
  publisher    = {Zenodo},
  doi          = {10.5281/zenodo.19655681},
  url          = {https://doi.org/10.5281/zenodo.19655681},
  note         = {Concept DOI (always latest). Pinned file: 10.5281/zenodo.23347471 (v1.3.0).}
}
```
