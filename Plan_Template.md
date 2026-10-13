# Agency PQC Migration Plan — Template

**Ships with the Quantum-Safe Migration Playbook v1.3.0 (October 2026).
Built directly on OMB Memorandum M-26-15, Appendix B: requirements read
at the primary source, and every load-bearing assertion claims-traced
before release. Bracketed prompts are for the filer to complete.**

## How to use this template

- One section per element OMB Memorandum M-26-15, Appendix B, requires in the
  initial plan submission, in the memo's order (§§1–9 below), plus the
  governance role table inside §9 as Appendix B specifies.
- Every federal requirement is cited to its source section. Guidance drawn
  from the Playbook corpus is cited by its fact IDs (§N.M) so a reviewer can
  trace any claim back to the corpus.
- Submission facts (M-26-15 §3.B, §3.D): submit to OMB and the Office of the
  National Cyber Director at **PQC@omb.eop.gov**, no later than 120 days from
  the memo's date of June 24, 2026 — **October 22, 2026**. The plan is a
  dynamic document that matures over time. Align the plan with **NIST IR
  8547** (initial public draft, or successor). National security systems
  are out of the memo's scope (§1).

## Frame: the five phases (M-26-15 §3.D)

| Phase | Window | Focus |
|---|---|---|
| 1 | 2026–2027 | Strategy, planning, discovery: inventory, assessment, awareness and training, governance, accountable officials |
| 2 | 2027–2028 | Pilots and early migration of prioritized systems |
| 3 | 2028–2030 | Prioritized migration: PQC key establishment for HVA / high-impact / highly-sensitive / CRQC-vulnerable systems; cryptographic agility everywhere |
| 4 | 2031 | Signature migration for the same system classes |
| 5 | 2035 | Full migration of remaining systems, risk- and availability-based |

Binding anchors: PQC key establishment on HVAs and high impact systems by
**December 31, 2030** and PQC digital signatures by **December 31, 2031**
(EO 14412 §4(b)); overall objective of mitigating as much quantum risk as
feasible by December 31, 2030 (M-26-15 §3.A).

*Template note: Appendix B's second element says "all three migration
phases" while §3.D defines five. This template covers all five; filers
should do the same rather than inherit the discrepancy.*

---

## §1. System prioritization strategy (risk-based justification)

**Requirement:** Appendix B element 1; prioritization categories at M-26-15
§3.C.2.

**Write:**
- [ ] The prioritization method, stated in one paragraph: which risk
  factors, in which order, produce the ranked list.
- [ ] The ranked system list, each entry tagged with its memo category:
  high impact system · High Value Asset · highly-sensitive data ·
  CRQC-vulnerable · asymmetric access-control system (PKI) using an
  Appendix A algorithm · data expected to remain mission-sensitive in 2030.
- [ ] Per-system justification for the top tier: data lifetime, exposure,
  replacement difficulty.

**Playbook support:** the corpus's risk method is data lifetime vs Q-Day
(§3.2), with high-risk asset classes at §3.3 and expendable classes at §3.4;
§3.5 sets the sequencing rule (key agreement before authentication, because
that is where harvest-now-decrypt-later bites); §7.3 risk-scores by data
lifetime rather than asset class. Map the memo's categories onto that
method rather than inventing a parallel one.

**Failure mode to avoid:** a ranked list with no stated method. GAO's
October 2026 review (GAO-27-108740) found prioritized inventory among the
practices agencies had not completed; a defensible method is the deliverable,
not the list alone.

## §2. Timelines and milestones — migration phases

**Requirement:** Appendix B element 2.

**Write:** one milestone table per phase: milestone · target date · owner
(role from §9) · exit evidence (the artifact that proves it happened).

- [ ] Phase 1 milestones should include: governance stood up (§9), inventory
  methodology operational (§4), this plan submitted.
- [ ] Phase 3 milestones must land on or before the December 31, 2030 key
  establishment anchor; Phase 4 on or before December 31, 2031.
- [ ] Every milestone names an owner. Unowned milestones are wishes
  (Playbook README: a plan without an owner for each risk has a gap).

**Playbook support:** §7.3 (translating timelines into sprint plans and
roadmaps); §8 pilot pacing feeds Phase 2. Note the track distinction:
§1.3's CNSA 2.0 dates govern national security systems (whose governance
frame since June 12, 2026 is NSPM-12); this plan runs on
the EO 14412 / M-26-15 civilian track. Do not mix the two date sets.

## §3. Timelines and milestones — TLS 1.3 testing and deployment

**Requirement:** Appendix B element 3. Deadline: support TLS 1.3 (RFC
9846) or a successor **not later than January 2, 2030** (M-26-15 App. A
§4, per EO 14306).

**Write:**
- [ ] Inventory of TLS-terminating endpoints (feeds from §4): servers,
  load balancers, VPN concentrators, API gateways, cloud terminators.
- [ ] Testing milestones: lab validation · pilot cohort · production waves,
  each with dates and pass criteria.
- [ ] The hybrid key exchange plan (App. A §4 mechanism: classical + PQC
  key shares combined into the session keys) and where hybrid is or is not
  used, with reasons.

**Playbook support:** §2.2 (hybrid key agreement already negotiates
silently in mainstream browsers — the client side is largely solved);
§8.3 (enable hybrid as the safety net); §8.4 (the measurement set: latency
percentiles, CPU/memory, error and fallback rates, packet-size effects);
§4 (why handshake size growth matters on lossy or constrained links — test
there, not just on the LAN).

## §4. Cryptographic inventory — methodologies and automated tools

**Requirement:** Appendix B element 4; technical basis at M-26-15 App. A §6.

**Write:**
- [ ] Each discovery method used, the tool(s) for it, and its coverage:
  software composition analysis over SBOMs · SAST/DAST code scanning ·
  network scanning for protocols and cipher suites · passive TLS
  observation · endpoint crypto-library inventory · certificate
  transparency log review for agency domains · source scanning for
  hardcoded algorithms and key material.
- [ ] Where the results live: the data should populate a **central CBOM**
  (App. A §6), versioned and
  diffed over time (Playbook §6.4), feeding monitoring dashboards.
- [ ] Update cadence: the inventory is continuous, not a one-time snapshot.
  State the refresh interval and the trigger events (new system, major
  version, contract award).

**Playbook support:** §6 is the corpus's deepest section — §6.2's hiding
places (scheduled tasks with hardcoded keys, service meshes, MDM profiles,
telemetry pipelines, vendor black boxes, CI/CD signing keys, IoT/OT fleets)
is a ready-made coverage checklist; §6.3's tactics align with the memo's
automation list. §6.1 is the section's thesis: inventory quality determines
project duration more than algorithm choice.

## §5. Cryptographically agile architecture plan

**Requirement:** Appendix B element 5; technical basis at M-26-15 App. A §5.

**Write:** how each layer achieves algorithm swap without redesign —
- [ ] Cryptographic libraries: modern, provider-style architectures that
  admit new algorithms by configuration (App. A §5).
- [ ] Configuration-driven cryptography: algorithm choice in external
  configuration, never compiled in.
- [ ] Protocol negotiation: capability discovery with downgrade protection;
  devices configurable to accept only approved algorithms.
- [ ] Key management: KMS/HSM able to generate, store, and manage classical
  and PQC keys side by side, with PQC support native.
- [ ] Hybrid architecture decisions (App. A §3): where hybrid is used as a
  transition stopgap, with its tradeoffs evaluated, not assumed.

**Playbook support:** agility enters the corpus as a procurement clause
(§9.2 — swap algorithms without firmware replacement) and a design rule
(§10.1 — design for the algorithm you will swap in later); §8.3 hybrid as
safety net. This section turns those into architecture commitments.

## §6. Third-party coordination plan

**Requirement:** Appendix B element 6; basis at M-26-15 §3.C.4.

**Write:**
- [ ] Vendor engagement: which vendors have been asked for PQC roadmaps,
  what they answered, and the verification applied — shipped-and-verified
  beats promised (Playbook §9.3: FIPS 140-3 certificate numbers, CAVP
  certificate IDs, named production customers; §9.4 red flags).
- [ ] Contract coverage: the §9.2 clause set (hybrid support, algorithm
  agility, dated FIPS 203/204/205 roadmap, test vectors, CBOM delivery on
  request) mapped against current contracts; gaps listed with renewal dates.
- [ ] CISA's PQC product-categories list (§3.C.4): requirements for product
  categories on the list include PQC integration — state how procurements
  check the list.
- [ ] FedRAMP cloud: shared-responsibility delineation with each authorized
  CSP; note the CISA / Dept. of War / GSA-led effort for multi-agency
  CSP, SaaS, PaaS, and IaaS offerings and how the agency tracks it.
- [ ] Watch item: the FAR Council proposed rule due **December 19, 2026**
  (EO 14412 §6(c)) would require covered contractors to comply with NIST
  FIPS, including PQC, by December 31, 2030 once finalized — state who
  owns tracking it.

## §7. Funding and personnel resources estimate

**Requirement:** Appendix B element 7. **New in v1.3.0** — the Playbook
corpus covers skills (§7) but has no estimation method; this scaffold is
built for the memo. (GAO-27-108740 found funding among the three practices
no agency had fully addressed; an explicit estimate is the point.)

**Write the estimate by cost driver, per phase, with assumptions logged:**

| Driver | Basis for the estimate |
|---|---|
| Discovery + inventory tooling | Tool licenses and services sized by estate (§4 methods) |
| Hardware refresh | Devices that cannot be patched to PQC: hardcoded-crypto ASICs replaced, not updated (§5.4); trust-store/bootloader limits (§5.3); HSM firmware + hardware audit lead times (§5.5) |
| Network remediation | Endpoints where handshake growth breaks budgets (§4.2–§4.3) |
| Testing + pilots | Pilot surfaces and measurement program (§8.2, §8.4) |
| Vendor uplift | Renewal premiums, CBOM delivery, roadmap acceleration (§9.2) |
| People | Roles in §9 table, FTE by phase; skills build-vs-hire per §7.2–§7.3 |
| Program running costs | CBOM ownership, quarterly refresh, reporting (§10.2) |

- [ ] Personnel table: each §9 role · FTE by phase · filled / to hire /
  detailed · the §7.1 bridging profile (standards + operations) called out
  for the technical roles.
- [ ] Assumptions + uncertainty: ranges, not point figures; the two or
  three assumptions that move the total most, named.
- [ ] CFO tie-in: the estimate must be the same numbers that appear in the
  agency's annual budget request (Appendix B, CFO role).

## §8. Risk management strategy — migration period

**Requirement:** Appendix B element 8.

**Write:** a risk register, reviewed on a stated cadence, seeded with:

| Risk | Playbook basis | Mitigation prompt |
|---|---|---|
| Hardware cannot migrate (ASICs, bootloaders, HSMs) | §5 | Refresh pipeline in §7 budget; replacement priority per M-26-15 §3.C.3 |
| Latency/bandwidth regression in production | §4 | §8.4 measurements before claims; constrained-link testing (§3 of this template) |
| Vendor readiness overstated | §9 | §9.3 verification before reliance; dual-track plans for critical vendors |
| Discovery misses hidden crypto | §6 | §6.2 checklist as coverage audit; continuous inventory (§4) |
| Harvest-now-decrypt-later exposure during delay | §3 | §3.5 sequencing: key agreement first on long-lifetime data |
| Algorithm weakened mid-migration | §10.1 (SIKE precedent) | §5 agility commitments; hybrid safety net (§8.3) |
| Interoperability / downgrade failure at cutover | App. A §3, §5 | Negotiation design + downgrade protection (§5); pilot gates (§8) |
| Resource shortfall stalls phases | This template §7 | Phase-gated milestones (§2); CFO-aligned estimates |

- [ ] Each register row gets: likelihood · impact · owner · contingency.
- [ ] The strategy names what would make the agency re-plan (trigger
  events), not just what it fears.

## §9. Governance roles and responsibilities

**Requirement:** Appendix B element 9 and the Appendix B responsibilities
table (paraphrased):

| Role | Accountable for |
|---|---|
| CIO and CISO | Prioritization, risk acceptance, resource allocation |
| PQC Cryptographic Inventory and Migration Lead / Program Manager | Agency-wide coordination, plan alignment, reporting to senior leadership |
| PQC Technical Lead | Inventory, algorithm selection, testing, deployment (incl. hybrid where used) |
| CFO | Migration resource requirements accurately represented in the annual budget request, with CIO/CISO and the Migration Lead |
| Program Office / Requirement Owner | Vendor requirements carrying PQC-readiness and cryptographic-agility provisions |
| Security Architect | PQC integration into zero-trust components; interoperability planning |
| Application and System Owners | Implementing crypto-agile, PQC-compatible technology in their own systems |

**Write:**
- [ ] Named individual per role (not just the role), plus the reporting
  line to agency leadership.
- [ ] The **PQC migration lead** specifically: EO 14412 §2(h) defines the
  role (reports to the CIO; owns agency-wide cryptographic inventory,
  the prioritized plan, cross-agency coordination) and §4(a) required
  agencies to name theirs within 30 days of June 22, 2026. State the name
  and the date provided to OMB and the National Cyber Director.
- [ ] Decision rights: who accepts risk (§3 of the memo makes migration a
  whole-leadership accountability, not a CIO/CISO-only one), who can move
  a milestone, who approves plan revisions — the plan is dynamic, so the
  revision process belongs here.

**Playbook support:** §10.2 (a named owner for the CBOM and the roadmap);
§7.1 (the bridging role profile); the README rule — a plan without an
owner for each risk has a gap.

---

## Filer's pre-submission check

- [ ] All nine sections present, in this order, each with named owners.
- [ ] Every date in §§2–3 traces to EO 14412 §4(b), M-26-15 §3, or
  App. A §4 — no CNSA 2.0 (NSS-track) dates mixed in.
- [ ] §7's numbers match the budget request.
- [ ] §4 states an inventory refresh cadence (not a one-time snapshot).
- [ ] Submitted to PQC@omb.eop.gov by **October 22, 2026**.
