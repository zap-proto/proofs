# ZAP Protocol — Proofs

> **Docs:** [Formal proofs](https://zap-proto.dev/docs) · part of the [ZAP Protocol](https://zap-proto.io)


Formal proofs accompanying the [papers](../papers/). Each proof is a standalone LaTeX document with theorems, lemmas, and explicit reductions/advantage bounds.

| # | Proof | Pages | Theorems | Top-level result |
|---|---|---|---|---|
| 1 | [pq-hybrid-kem](./pq-hybrid-kem/proof.tex) | 8 | T1 (Z-Wing IND-CCA2), T2 (ZAP+Z-Wing IND-CCA2-AKE) | Adv ≤ 2 n_P n_S · Adv_ZWING + n²_S/2^|nonce| + q²_H/2^(n+1) |
| 2 | [zero-copy-throughput](./zero-copy-throughput/proof.tex) | 10 | T1 (Throughput Bound), T2 (Tail-Latency Independence) | Crossover point n* ≈ 3.7 KiB; ZAP O(1) field access vs Protobuf Ω(n) |
| 3 | [transport-vs-jwt](./transport-vs-jwt/proof.tex) | 10 | T1 (ZAP upper bound), T2 (JWT lower bound), Cor (strict dominance) | Adv_JWT ≥ max{...EUF-CMA, JWKS, alg-conf, kid-conf, replay, leak} |
| 4 | [rns-identity-binding](./rns-identity-binding/proof.tex) | — | T1 (Name-spoofing), T2 (Connection-binding), T3 (Continuity), Sybil lemma | Concrete bound ≤ 2^-97 + p_root under ML-KEM-768 + ML-DSA-65 |
| 5 | [streaming-tail-latency](./streaming-tail-latency/proof.tex) | — | T1 (Tail-Latency Independence), T2 (Throughput Composition), FEC lemma | P[L(s_i,f) > t \| F_j] = P[L(s_i,f) > t] |
| 6 | [fix-trading-channel](./fix-trading-channel/proof.tex) | — | T1 (steady-state), T2 (resumption), Sequence lemma | T_rtt_zap(M) ≤ T_rtt_tls(M) - (Δ_ser + Δ_parse) - κ₁·Δs |
| 7 | [agent-communication](./agent-communication/proof.tex) | 6 | T1 (non-repudiation), T2 (impersonation), T3 (provenance chain), replay lemma | Adv_nonrep ≤ Adv_eufcma(ML-DSA-65) + q_h²/2^257 |
| 8 | [composability](./composability/proof.tex) | 8 | T1 (category), T2 (initial object), T3 (tight composition), Cor (chain bound) | ε_Q ≤ ε_P + ρ_φ + δ_φ; chain of n tight embeddings is negligible |

## Concrete take-aways

**ZAP-base alone provides** (via Z-Wing handshake): IND-CCA2-AKE security `≤ 2^-87` under standard cryptographic assumptions on X25519, ML-KEM-768, and ML-KEM-1024. Surviving any one component gives you full security.

**Every sub-protocol inherits** ZAP-base's confidentiality and authentication via the categorical composition theorem (proof 8). A FIX session running over ZAP-RNS-resolved peers over ZAP-base gets PQ confidentiality "for free" — no FIX-specific PQ analysis required.

**JWT-based service auth** has provably larger attacker advantage than transport-layer mutual auth in any realistic instantiation (proof 3). The strict-dominance corollary cites independently realisable attack channels (alg=none, kid confusion, JWKS poisoning, etc.) that don't exist for ZAP.

**Tail latency** of stream `s_i` in a multiplexed `zap-ws` connection is conditionally independent of other streams' substrate-packet events (proof 5). FEC `(k+m, k)` recovers ≤ m losses with overhead `m/k`, eliminating the retransmission RTT in the tail.

## Build

```sh
cd <topic> && make    # → proof.pdf
make clean
```

## License

CC-BY-4.0 for the proof text.