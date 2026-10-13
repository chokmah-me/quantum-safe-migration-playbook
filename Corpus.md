# Quantum-Safe Migration Playbook: Corpus

> **Version:** 1.3.0 · **Last reviewed:** October 2026
> **Purpose:** Retrieval-augmented knowledge base for advising on Post-Quantum Cryptography (PQC) migration.
> **How to read fact IDs:** `§N.M` = section N, fact M. Use these IDs when citing in answers.

---

## Scope

**In scope.** Operational PQC migration for enterprise environments: standards awareness, infrastructure impact, discovery, pilot design, procurement, skills, and program pacing.

**Out of scope (do not answer from this corpus):**
* Physics of quantum computing, qubit architectures, decoherence, error correction.
* Quantum Key Distribution (QKD) and quantum networks (different technology, different threat model).
* Specific vendor product recommendations or purchasing advice.
* Legal or regulatory compliance interpretation for specific jurisdictions.
* Predictions of exact Q-Day (the date a cryptographically relevant quantum computer arrives).

If a question falls outside scope, say so and suggest where the user should look instead.

---

## §1. Strategic Urgency

**§1.1** The migration is driven by the mismatch between how long today's public-key encryption will hold and how long the secrets it protects must stay private.

**§1.2** NIST published the first three final PQC standards on August 13, 2024: **FIPS 203 (ML-KEM)** for key encapsulation, **FIPS 204 (ML-DSA)** and **FIPS 205 (SLH-DSA)** for digital signatures. **HQC** was selected March 11, 2025 as a backup KEM. **FIPS 206 (FN-DSA / FALCON)** is in development as an additional signature option (no public draft as of October 2026).

**§1.3** NSA's CNSA 2.0 sets binding federal timelines: software and firmware signing exclusive use of PQC by **2030**; traditional network equipment (VPNs, routers) by **2030**; web browsers, servers, and cloud services by **2033**; operating systems by **2033**. Treat these as floor dates for federal supply chains and anchor dates for everyone else.

**§1.4** Q-Day estimates have shortened materially in 2025–2026. Neutral-atom architectures and improved error correction cut the qubit count needed to break P-256 to roughly ten thousand. Cloudflare now targets **2029** for full internal PQ readiness (down from typical 2035+ industry estimates a year earlier). Plan for compressed timelines.

**§1.5** Strategic goal: move from reactive patching to **preventive engineering**. Success is defined as continued normality (no outages, no headlines) through the transition. See also §8.

*Cross-ref:* §3 (why long-life secrets make timing urgent), §9 (how to push vendors on dates).

---

## §2. Current State of Deployment

**§2.1** The internet is already migrating quietly, and the client side moved first. By September 2026, roughly **70% of browser-generated traffic** reaching Cloudflare used post-quantum hybrid key agreement, up from 52% of human-initiated traffic in December 2025. Origin connections lag far behind: only about **15% of origins** negotiate hybrid ML-KEM. The jump came from browser defaults (Chrome, Edge, Firefox) and Apple iOS 26 (September 2025).

**§2.2** The user experience is invisible: no banners, no prompts. Backward-compatible TLS 1.3 hybrid key agreement (X25519 + ML-KEM) negotiates silently.

**§2.3** Key agreement is the solved half. Authentication (PQ certificates, PQ-signed firmware) is the unsolved half. As of early 2026, no public post-quantum certificates are broadly issued. CA/Browser Forum has not finalized the hybrid certificate format. Early CA pilots are expected in 2026; broad trust across browsers likely 2027+.

*Cross-ref:* §5 (why certificates are the harder problem), §9 (asking vendors about both halves).

---

## §3. Threat Model: Harvest Now, Decrypt Later (HNDL)

**§3.1** HNDL is an asymmetric attack: adversaries archive high-value ciphertext today and decrypt once quantum computers are viable. The attacker's cost is storage; the defender's loss is everything that was secret.

**§3.2** The risk equation is **data lifetime vs. Q-Day**. Any secret whose value outlasts the arrival of a cryptographically relevant quantum computer is already exposed if intercepted today.

**§3.3** High-risk assets (must stay secret 10–20+ years):
* Intellectual property and trade secrets
* State records, classified intelligence, biometrics
* Signed firmware (signatures have to hold for the life of the device)
* Healthcare records, long-term legal contracts
* Source code and cryptographic root material

**§3.4** Low-risk assets (expendable or short-lived): ephemeral session data, daily chat logs, short-horizon telemetry. HNDL does not apply to data whose value expires before decryption is possible.

**§3.5** Priority action: fix key agreement first (§2), because that's where HNDL bites. Authentication upgrades can lag without immediate HNDL exposure.

*Cross-ref:* §1.4 (why Q-Day compression changes this math), §6 (finding long-life secrets).

---

## §4. Infrastructure Impact: Keys Are Bigger, Handshakes Are Heavier

**§4.1** PQC is not a drop-in replacement. ML-KEM public keys and ciphertexts are roughly kilobyte-scale, versus tens of bytes for X25519. ML-DSA signatures are several kilobytes versus ~64 bytes for Ed25519.

**§4.2** Network impact comes from two mechanisms: inflated handshake payloads that push past single-packet limits (forcing extra round-trips), and larger certificate chains. Measured latency overhead in real-world TLS handshakes is typically modest for key agreement alone (often single-digit ms added) but can be sharply worse on lossy or high-latency links. Expect noticeable regression on mobile, satellite, and constrained networks.

**§4.3** Audit before you migrate: performance budgets, CDN configurations, MTU assumptions, QUIC vs. TCP behavior, and mobile radio performance. A "fine on my laptop" pilot hides the problems that matter.

**§4.4** PQ authentication is where the real weight lands. Certificate chains with ML-DSA signatures add tens of kilobytes per handshake. This is why browsers are holding off on PQ certs by default even as they enable PQ key agreement.

*Cross-ref:* §5 (hardware limits that amplify this), §8 (measure-before-you-claim-win).

---

## §5. Hardware Bottlenecks: The Plumbing Problem

**§5.1** Old hardware assumptions break under PQC loads. This turns expected software updates into hardware refresh cycles.

**§5.2** Memory and processing limits hit:
* VPN appliances (buffer sizes fixed for small keys)
* Smartcards and HSMs (limited RAM, limited flash)
* Industrial controllers, PLCs, SCADA endpoints (decade-old silicon)

**§5.3** Trust store limits hit: many bootloaders allocate fixed space for root certificates. PQ roots don't fit. Devices that can't update their bootloader can't trust PQ CAs.

**§5.4** Proprietary ASICs are the worst case: silicon that hardcodes RSA or ECC math cannot be software-patched to run ML-KEM or ML-DSA. Affected devices must be replaced, not updated.

**§5.5** HSMs need firmware that supports the new algorithms, often with accompanying hardware audits before certification. This is a procurement lead-time issue, not a configuration issue.

*Cross-ref:* §9 (RFP language to catch this early), §6 (how to find these devices in your fleet).

---

## §6. Discovery: Finding Where Crypto Hides

**§6.1** Inventory quality determines project duration more than algorithm choice. You cannot migrate what you cannot find.

**§6.2** Common hiding places for undocumented cryptography:
* **Cron jobs and scheduled tasks** with hardcoded keys or cert paths
* **Service meshes** (Istio, Linkerd, Consul) with embedded mTLS configs
* **Mobile device management and provisioning profiles** with forgotten client certs
* **Telemetry and log pipelines** using TLS with pinned certs or opaque agents
* **Vendor black boxes**: appliances where "there's one integration certificate somewhere"
* **CI/CD signing keys**, container image signing, artifact repositories
* **IoT and OT fleets**: cameras, sensors, badge readers, HVAC

**§6.3** Discovery tactics that work:
* Network-side passive TLS inventory (what cipher suites and key types are actually negotiated)
* Endpoint-side crypto library inventory (SBOM + crypto library versions)
* Certificate transparency logs for your own domains
* Source code scanning for hardcoded algorithm names, OIDs, and library calls

**§6.4** Cryptographic Bill of Materials (CBOM) is the emerging artifact. Treat it like an SBOM: generate, version, and diff it over time.

*Cross-ref:* §7 (who does this work), §9 (making vendors disclose their crypto).

---

## §7. Skills: Translating Math to Ops

**§7.1** The needed role bridges cryptographic standards and enterprise IT operations. It is neither a pure cryptographer job nor a pure DevOps job.

**§7.2** Technical skills to build or hire:
* Reading and validating X.509 certificate chains
* Parsing TLS handshakes and cipher suite negotiation
* Using crypto inventory tooling (CBOM generators, passive scanners)
* Understanding hybrid key agreement modes (what fails safe, what doesn't)

**§7.3** Operational skills that matter equally:
* Translating NIST and NSA timelines into sprint plans and roadmaps
* Risk-scoring assets by data lifetime (§3) rather than by asset class
* Vendor negotiation on crypto roadmaps (§9)
* Running controlled pilots and reading telemetry honestly (§8)

**§7.4** Career framing: this is work that succeeds quietly. Compare to Y2K remediation: the people who did it right were never celebrated because nothing broke.

---

## §8. Action Plan: Start With a Pilot

**§8.1** Do not attempt a big-bang migration. Pick a controllable surface and prove the toolchain works there first.

**§8.2** Pilot selection criteria:
* Internal service, not customer-facing
* Low traffic and low criticality (you can tolerate a regression)
* Owned end-to-end (both client and server under your control)
* Diverse enough to surface real problems (not just a trivial web service)

**§8.3** Enable **hybrid key exchange** (classical + PQ, e.g., X25519 + ML-KEM). Hybrid mode is the safety net: if the PQ algorithm is later weakened, the classical half still holds; if the classical half fails later to quantum attack, the PQ half holds.

**§8.4** Measurements to collect:
* Handshake latency (p50, p95, p99) before and after
* CPU and memory on both client and server
* Error rates, fallback rates, renegotiation rates
* Packet-size distributions and MTU-related drops

**§8.5** Produce a written post-mortem. Real numbers from a real pilot defuse skepticism faster than any vendor slide deck.

*Cross-ref:* §4 (what to measure for), §10 (making pilot findings into institutional knowledge).

---

## §9. Procurement and Vendor Management

**§9.1** Treat vendor selection as a strategic risk function, not a purchasing function.

**§9.2** Minimum RFP and renewal clauses:
* **Hybrid-mode support** for key agreement (X25519+ML-KEM or equivalent)
* **Algorithm agility**: ability to swap algorithms without firmware replacement
* **Disclosed crypto roadmap** with specific dates for FIPS 203/204/205 support
* **Test vectors and independent validation** (not just marketing claims)
* **CBOM delivery** on request, refreshed at major versions

**§9.3** Distinguish "PowerPoint promises" from "shipped and verified":
* Ask for FIPS 140-3 module validation certificate numbers covering PQC algorithms
* Ask for NIST CAVP (Cryptographic Algorithm Validation Program) certificate IDs
* Ask which of their customers are running PQC in production today

**§9.4** Red flags: vendors who conflate QKD with PQC, who cite "quantum-safe" without naming algorithms, who refuse to commit dates, or whose roadmap slips without explanation.

*Cross-ref:* §5 (hardware vendors are the highest-risk category), §1.3 (federal timelines give you leverage).

---

## §10. Continuous Practice

**§10.1** Standards will keep evolving. FIPS 206 is not yet published. HQC is being standardized. Some current algorithms may be weakened retroactively by new cryptanalysis (as happened to SIKE in 2022). Design for the algorithm you will need to swap in later, not the one you picked today.

**§10.2** Build internal practice:
* Quarterly "lunch-and-learn" reviews of new NIST publications
* A shared channel for tracking CVEs and advisories affecting crypto libraries
* A named owner for the CBOM and the migration roadmap

**§10.3** Contribute back. Crypto inventory tooling gets better when multiple organizations share findings. Open-source contributions also cement institutional knowledge that doesn't walk out the door.

---

## Provenance and Dated Facts

Statements with specific numbers or dates derive from the sources below (April 2026). Update these fields when the corpus is refreshed.

* **§1.2** NIST CSRC, PQC Standardization page and FIPS 203/204/205 final publications (Aug 13, 2024); HQC selection (Mar 11, 2025).
* **§1.3** NSA CNSA 2.0 timeline, as reported by DigiCert and NSA NSS guidance (2024–2025).
* **§1.4** Cloudflare post-quantum roadmap blog post (April 2026) citing Oratomic neutral-atom resource estimates. 2029 targets re-checked October 2026: Cloudflare reaffirmed; Google announced a matching 2029 target in March 2026.
* **§2.1** Cloudflare Radar 2025 Year in Review (Dec 2025); Automatically Secure blog post (Sept 2025); Cloudflare post-quantum visibility report (Sept 2026) for the browser/origin split.
* **§2.3** Cloudflare "State of the post-quantum Internet in 2025" (Oct 2025).
* **§4.1–4.2** ML-KEM and ML-DSA parameter sizes from FIPS 203 and 204 specifications; latency characterization from Cloudflare TLS measurement posts.

---

## Maintenance Notes (for corpus authors, not for RAG answers)

* Re-check §1.4 and §2.1 quarterly. These are the fastest-moving facts. (§2.1 refreshed October 2026 for v1.3.0.)
* If FIPS 206 is published, update §1.2 and §10.1.
* If CA/Browser Forum locks the hybrid cert format, update §2.3.
* Keep the scope block at the top authoritative. If you add a topic, add it here first.
