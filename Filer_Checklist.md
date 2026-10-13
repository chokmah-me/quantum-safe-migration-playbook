# Filing the October 22 Plan — a filer's checklist

*Ships with the Quantum-Safe Migration Playbook v1.3.0. Companion to the
Agency PQC Migration Plan Template in the same release. All dates trace to
EO 14412, OMB M-26-15, or its appendices — checked against the primary
documents on 2026-10-13.*

## The deadline

- [ ] Plan addressed to **OMB and the National Cyber Director**, submitted
  to **PQC@omb.eop.gov** (M-26-15 §3.D).
- [ ] Submitted by **October 22, 2026** — 120 days from the memo's date
  (June 24, 2026).
- [ ] Treated as a **dynamic document**: file the honest version now; the
  memo expects it to mature. A thin-but-real plan beats a polished late
  one.

## The nine sections — done means

1. **Prioritization strategy** — the method is stated in one paragraph,
   and every top-tier system carries a risk-based justification and its
   memo category (high impact · HVA · highly-sensitive · CRQC-vulnerable ·
   data mission-sensitive in 2030).
2. **Phase timelines** — one milestone table per phase (all five phases,
   2026–2035), every milestone with a date, an owner, and exit evidence.
   Phase 3 lands by Dec 31, 2030; Phase 4 by Dec 31, 2031.
3. **TLS 1.3 milestones** — endpoint inventory, testing milestones, and a
   hybrid key-exchange plan, aimed at the **January 2, 2030** support
   deadline (M-26-15 App. A §4).
4. **Inventory** — each discovery method named with its tool and coverage;
   results in a central CBOM; a refresh cadence stated (continuous, not a
   snapshot).
5. **Crypto-agile architecture** — library, configuration, negotiation,
   and key-management layers each state how an algorithm gets swapped
   without a redesign.
6. **Third-party coordination** — vendor answers verified, contract gaps
   listed with renewal dates, FedRAMP shared-responsibility delineated.
7. **Funding + personnel estimate** — by cost driver and phase, ranges
   with assumptions logged, and **the same numbers as the budget request**.
8. **Risk management strategy** — a register with likelihood, impact,
   owner, and contingency per risk, plus the trigger events that would
   force a re-plan.
9. **Governance** — all seven Appendix B roles **named**, reporting line
   to leadership stated, decision rights and the plan-revision process
   written down.

## Cross-checks before you hit send

- [ ] **No NSS-track dates.** Every date traces to EO 14412 §4(b),
  M-26-15 §3, or App. A §4. CNSA 2.0 dates (national security systems,
  governed since June 2026 under NSPM-12) do not belong in this plan.
- [ ] **The migration lead is named** — the role EO 14412 §2(h) defines
  and §4(a) required agencies to fill within 30 days of June 22, 2026.
- [ ] **Testing is a section, not a sentence.** GAO's October 2026 review
  (GAO-27-108740) found no agency had tested PQC; §3's testing milestones
  are where your plan answers that.
- [ ] **IR 8547 cited as what it is**: NIST IR 8547, an initial public
  draft — the memo directs alignment with it or its successor.
- [ ] **Numbers reconcile**: §7 estimate = budget request = §2 milestone
  resourcing. Three documents, one set of numbers.

## If you are reading this in the final week

Work the sections in this order — it front-loads what reviewers check
first and what GAO found missing:

- **Day 1:** §9 governance names + §1 method paragraph. Owners make every
  later section assignable.
- **Day 2:** §4 inventory methods (write the methods you actually run;
  coverage honesty beats coverage theater).
- **Day 3:** §2 phase tables + §3 TLS milestones, dates against the
  anchors above.
- **Day 4:** §7 estimate with the CFO's office — do not let this slip;
  it is the section agencies most often leave blank.
- **Day 5:** §5 agility + §6 third-party from §7's vendor facts.
- **Day 6:** §8 risk register (an afternoon, using the template's seed
  rows).
- **Day 7:** cross-checks above, one full read-through, submit.
