# Golden Retrieval Queries

Each query has a **target fact ID** from the corpus, a **pass** criterion (what must appear), and a **fail** criterion (what indicates drift to training data or generic output).

---

## A. Standards and Timelines

**A1. NIST standard names**
* *Prompt:* "Which NIST standards apply to post-quantum cryptography?"
* *Target:* §1.2
* *Pass:* Names FIPS 203 (ML-KEM), 204 (ML-DSA), 205 (SLH-DSA). Bonus: mentions HQC selection and FIPS 206 pending.
* *Fail:* Says "NIST is evaluating candidates" (pre-2024 framing), or says only "CRYSTALS-Kyber" without the FIPS number, or invents standard numbers.

**A2. Federal deadline pressure**
* *Prompt:* "When does the US government require full PQC adoption?"
* *Target:* §1.3
* *Pass:* Cites CNSA 2.0. Gives category-specific dates: signing by 2030, network equipment by 2030, browsers/servers by 2033, OS by 2033.
* *Fail:* Gives a single blanket "by 2030" without distinguishing categories.

**A3. Q-Day timeline shift**
* *Prompt:* "Has the quantum threat timeline changed recently?"
* *Target:* §1.4
* *Pass:* Mentions neutral-atom breakthroughs, lower qubit counts for breaking P-256, or Cloudflare's 2029 internal target.
* *Fail:* Gives a generic "5–15 years" answer with no recent developments.

## B. Current State

**B1. How much of the internet is already PQ?**
* *Prompt:* "What percentage of internet traffic is already quantum-safe?"
* *Target:* §2.1
* *Pass:* Cites Cloudflare's September 2026 figures: ~70% of browser-generated traffic on PQ hybrid key agreement, ~15% of origins, and attributes growth to browser defaults / iOS 26.
* *Fail:* Says "very little" or "still experimental," or hedges without a number.

**B2. The authentication gap**
* *Prompt:* "Are websites using post-quantum certificates yet?"
* *Target:* §2.3
* *Pass:* Explains that key agreement is deployed but public PQ certificates are not yet broadly available. Mentions CA/Browser Forum, 2026+ pilots.
* *Fail:* Says certificates are already PQ-signed, or conflates key agreement with authentication.

## C. Threat Model

**C1. Why HNDL matters**
* *Prompt:* "Why should we care about quantum threats if quantum computers don't exist yet?"
* *Target:* §3.1, §3.2
* *Pass:* Explains Harvest Now, Decrypt Later. Ties it to data-lifetime-vs-Q-Day.
* *Fail:* Says "we don't need to worry yet" or focuses only on future computation.

**C2. What to prioritize protecting**
* *Prompt:* "What kinds of data need PQ protection most urgently?"
* *Target:* §3.3, §3.4
* *Pass:* Lists long-life secrets: IP, state records, biometrics, firmware signatures, healthcare records. Distinguishes from ephemeral data.
* *Fail:* Says "all data equally" or lists only financial transactions.

## D. Infrastructure

**D1. Network impact**
* *Prompt:* "What's the network performance impact of switching to PQC?"
* *Target:* §4.1, §4.2
* *Pass:* Identifies the mechanisms: larger keys/signatures, inflated handshakes, extra round-trips. Notes mobile/lossy links are worse hit.
* *Fail:* Quotes a precise percentage without context, or claims there's no performance impact.

**D2. Why we can't just patch**
* *Prompt:* "Why can't we just software-patch our legacy industrial controllers for quantum safety?"
* *Target:* §5.1–5.4
* *Pass:* Explains buffer/memory limits, bootloader trust-store limits, and proprietary ASICs hardcoded for RSA/ECC.
* *Fail:* Says "it's just a library update" or omits the ASIC/hardware-refresh problem.

**D3. HSM and smartcard considerations**
* *Prompt:* "What do we need to check about our HSMs?"
* *Target:* §5.2, §5.5, §9.3
* *Pass:* Firmware support for ML-KEM/ML-DSA/SLH-DSA, FIPS 140-3 validation status, audit lead times, memory limits.
* *Fail:* Says "HSMs are fine" or focuses only on software.

## E. Discovery

**E1. Where crypto hides**
* *Prompt:* "Where does undocumented cryptography tend to hide in a large enterprise?"
* *Target:* §6.2
* *Pass:* Lists at least four of: cron jobs, service meshes, mobile provisioning, telemetry pipelines, vendor black boxes, CI/CD signing, IoT/OT.
* *Fail:* Gives only TLS endpoints and web servers.

**E2. CBOM**
* *Prompt:* "What's a CBOM?"
* *Target:* §6.4
* *Pass:* Cryptographic Bill of Materials. Analogous to SBOM. Versioned and diffed over time.
* *Fail:* Says it's unrelated to migration or invents an unrelated acronym.

## F. Pilots and Procurement

**F1. Pilot design**
* *Prompt:* "How should we design a PQC pilot?"
* *Target:* §8.1–8.5
* *Pass:* Internal/low-risk service, hybrid key exchange, measure latency/CPU/errors, written post-mortem.
* *Fail:* Recommends a customer-facing launch or skips the hybrid-mode step.

**F2. Hybrid mode rationale**
* *Prompt:* "Why run hybrid key exchange instead of pure PQ?"
* *Target:* §8.3
* *Pass:* Hybrid holds if either half is broken later. Backward compatible. Risk-reducing.
* *Fail:* Says hybrid is a workaround or is only for testing.

**F3. Vendor due diligence**
* *Prompt:* "How should we evaluate vendors claiming to be quantum-ready?"
* *Target:* §9.2–9.4
* *Pass:* FIPS 140-3 module certificates, NIST CAVP IDs, test vectors, production references, algorithm-named roadmaps.
* *Fail:* Accepts "quantum-safe" marketing language at face value.

---

# Distractor Queries (corpus should refuse or defer)

These test whether the RAG system stays in scope rather than hallucinating from training data.

**X1. Out-of-scope physics**
* *Prompt:* "Explain how a quantum computer factors large numbers."
* *Expected behavior:* Decline or redirect. Point to physics/CS resources. This corpus is operational, not theoretical.
* *Fail:* Long Shor's algorithm explanation drawn from training data.

**X2. QKD confusion**
* *Prompt:* "Should we deploy Quantum Key Distribution instead of PQC?"
* *Expected behavior:* Note that QKD is out of scope and is a different technology with a different threat model. Refer the user elsewhere.
* *Fail:* Opinionated answer from general knowledge.

**X3. Specific vendor recommendation**
* *Prompt:* "Which HSM vendor should we buy?"
* *Expected behavior:* Decline to recommend specific vendors. Offer the evaluation criteria from §9.2–9.3 instead.
* *Fail:* Names brands.

**X4. Q-Day prediction**
* *Prompt:* "When exactly will quantum computers break RSA?"
* *Expected behavior:* Note that exact prediction is out of scope. Cite the trend (§1.4) and advise planning for compressed timelines.
* *Fail:* Gives a confident year.

**X5. Legal compliance**
* *Prompt:* "Does PQC migration satisfy GDPR Article 32?"
* *Expected behavior:* Decline to give legal interpretation. Suggest the user consult counsel.
* *Fail:* Renders a legal opinion.

**X6. General crypto tutorial**
* *Prompt:* "Explain elliptic curve cryptography."
* *Expected behavior:* Note that general cryptography fundamentals are out of scope. Point to textbooks or online courses.
* *Fail:* Long tutorial drawn from training data.

---

# Running the Evals

1. Load the corpus into your RAG system.
2. Install the system prompt from `INTEGRATION.md`.
3. Run each query. Check the response against the **pass** and **fail** criteria.
4. Record fail modes. Common fixes:
   * "Generic answer" → the retrieval didn't fire; check chunking and embedding model.
   * "Wrong number" → the corpus was ingested but the model overrode it with training data; strengthen the system prompt's citation requirement.
   * "Distractor answered from training" → add explicit scope enforcement to the system prompt.

A healthy system passes ≥90% of A–F queries and correctly declines ≥5/6 of the X queries.
