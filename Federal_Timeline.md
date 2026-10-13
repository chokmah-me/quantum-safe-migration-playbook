# Federal PQC Timeline — one-pager

*Ships with the Quantum-Safe Migration Playbook v1.3.0 (October 2026).
Compiled 2026-10-13. Rows marked "[V] primary-read" were verified against
the primary document text on 2026-10-13 (the M-26-15 memorandum; EO 14412
full text). Remaining rows are multi-source verified. Every load-bearing
assertion on this page passed an adversarial claims trace before release.*

## Binding federal dates

| Date | Event | Source status |
|---|---|---|
| 2024-08 | NIST finalizes first PQC standards: FIPS 203 (ML-KEM), FIPS 204 (ML-DSA), FIPS 205 (SLH-DSA) | [V] |
| 2026-06-22 | EO 14412 signed — "Securing the Nation Against Advanced Cryptographic Attacks" | [V] primary-read 2026-10-13 |
| 2026-06-24 | OMB M-26-15 issued — "Execution of the Migration to Post-Quantum Cryptography" | [V] primary-read 2026-10-13 |
| 2026-10-06 | GAO-27-108740 released: **none of 24 CFO Act agencies** fully addressed inventory, funding, or testing; only one completed inventory; none tested PQC | [V] |
| **2026-10-22** | **Agency PQC migration plans due to OMB + ONCD** (120 days from M-26-15). Plans must describe inventory methods, migration milestones, funding and personnel needs | [V] primary-read + arithmetic 2026-10-13 |
| 2026-12-19 | FAR Council proposed rule due — would require covered contractors to comply with NIST FIPS incl. PQC by 2030-12-31 | [V] primary-read (EO §6(c), 180 days) 2026-10-13 |
| 2027-01-01 | CNSA 2.0 acquisition gate: new commercial NSS must be PQC-capable (CNSS Policy 15, restated by NSA Oct 1) | [V] |
| 2027-03-19 | CISA + NIST CBOM minimum-elements guidance due (270 days after EO 14412) | [V] primary-read (EO §5(d)) + arithmetic 2026-10-13 |
| 2027-12-31 | NIST PQC migration pilot on its own systems complete | [V] primary-read (EO §4(c)) 2026-10-13 |
| 2030-01-02 | TLS 1.3 support deadline — agencies must support TLS 1.3 or successor not later than this date (M-26-15 App. A §4, per EO 14306; NOT EO 14412) | [V] primary-read 2026-10-13 |
| 2030-12-31 | PQC **key establishment** on high-value assets / high-impact systems (EO 14412); contractor FIPS compliance date (FAR rule) | [V] primary-read (EO §4(b), §6(c)) 2026-10-13 |
| 2031-12-31 | PQC **digital signatures** on HVA/HIS (EO 14412) | [V] primary-read (EO §4(b)) 2026-10-13 |
| 2035 | Remaining systems migrated (M-26-15 five-phase schedule: key-est 2028–2030, signatures 2031, remainder by 2035) | [V] primary-read (M-26-15 §3.D) 2026-10-13 |

## Why this one-pager matters now

- The Oct 22 agency plans land on a documented weak baseline (GAO, Oct 6): agencies
  must describe inventory methods, milestones, and funding/personnel needs — the
  exact three practices GAO found missing. That gap is the playbook's sales surface.
- EO 14412 applies to contractors only indirectly today (procurement / FedRAMP /
  contract clauses), but the Dec 19 FAR proposed rule is the on-ramp to direct
  contractor obligations — the v1.4.0 vendor chapter's forcing function.
- NSS are excluded from M-26-15; CNSA 2.0 is their track (2027 acquisition gate; category deadlines 2030–2033).
