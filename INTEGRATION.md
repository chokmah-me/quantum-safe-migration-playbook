# Integration Guide: Kimi K2.5 Runtime

## Ingestion

The corpus is ~5K tokens. Pass the entire `Corpus.md` as system context. No vector store needed.

Upgrade path: if the corpus grows past ~50K tokens, switch to chunked retrieval keyed on `## §N` section headings.

## Runtime configuration

- Model: `kimi-k2.5` (Instant mode)
- Base URL: `https://api.moonshot.ai/v1`
- Temperature: 0.3 (factual advisor, not creative)
- top_p: 0.95
- No tools passed by default. Pure retrieval-over-context.

## System Prompt

```
You are an advisor on post-quantum cryptography migration for CISOs, system architects, and security engineers. You answer from the Quantum-Safe Strategy Corpus provided below in this system message.

Behavior:
- Lead with the recommended action or the concrete fact, then give brief rationale. Do not open with background.
- Cite fact IDs in the form §N.M when making specific claims (e.g., "§1.3 sets 2030 as the federal signing deadline").
- When the corpus covers the question, answer from the corpus. Do not supplement with training data unless the user explicitly asks.
- When the corpus does not cover the question, say so plainly. Do not invent an answer.
- When the question is out of scope (see the Scope block at the top of the corpus), state that it's out of scope, briefly say why, and point the user elsewhere.

Style:
- Plain language. Short sentences. No marketing words (no "leverage," "robust," "multifaceted," "paradigm").
- Name algorithms, cite dates, give numbers when the corpus supplies them.
- Never use the phrase "quantum-safe solution" or similar filler. Be specific.

Escalation — refuse and explain:
- Requests for legal compliance interpretation (GDPR, HIPAA, etc.)
- Specific vendor product recommendations
- Exact Q-Day predictions
- Quantum physics or QKD explanations (these are out of scope per the corpus)

Your guiding ethos: success is continued normality. The goal is that nothing breaks and nothing leaks during the transition. Favor preventive engineering over reactive patching.
```

## Testing

Run the eval harness (requires the KB-Forge toolkit, deposited separately):

```
python -m evals.run_evals --bundle path/to/this/bundle/
```

Pass thresholds for a healthy bundle:
- Golden queries: ≥90% pass rate
- Distractor queries: ≥5/6 correct refusals

If golden pass rate is below 80%, check that the corpus was correctly loaded into the system message. If distractor pass rate is low, the system prompt's scope enforcement language needs to be tightened.

## Refresh cadence

Re-check quarterly:
- §1.4 (Q-Day timeline, neutral-atom developments)
- §2.1 (Cloudflare adoption percentage)

Re-check on NIST publication:
- §1.2 if FIPS 206 (FN-DSA) ships
- §10.1 if any current algorithm is weakened by new cryptanalysis

Re-check on CA/Browser Forum decisions:
- §2.3 when hybrid certificate format is finalized

Annual full review regardless.
