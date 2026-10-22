# AGENTS.md — quantum-safe-migration-playbook

PQC migration knowledge base (v1.3.0): corpus, federal timeline, filer checklist, plan template. Document bundle, not a toolchain.

## Commands

No build. Read `Corpus.md`. Optional maintainer mint (needs `ZENODO_TOKEN`):

```powershell
pwsh ./mint-zenodo-v1.3.0.ps1
```

RAG ingest: follow `INTEGRATION.md`, then run golden queries in `evals/` before anyone uses the system.

## Work rules

- Canonical public record is the Zenodo deposit. GitHub ships the same files as the tarball.
- Fact IDs in `Corpus.md` are the traceable claims. Do not paraphrase them into un-IDed assertions.
- Four risk owners (hardware, latency/bandwidth, vendor readiness, discovery) are the operational claim. A plan missing one has a gap.

## Pointers

- `README.md` — who uses which file
- `Corpus.md` — source of truth
- `Federal_Timeline.md` / `Filer_Checklist.md` / `Plan_Template.md`
- `ZENODO.md` — concept vs version
