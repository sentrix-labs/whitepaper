# Sentrix Whitepaper

[![CI](https://github.com/sentrix-labs/whitepaper/actions/workflows/lint.yml/badge.svg)](https://github.com/sentrix-labs/whitepaper/actions/workflows/lint.yml)
[![License](https://img.shields.io/github/license/sentrix-labs/whitepaper)](LICENSE)
[![Latest release](https://img.shields.io/github/v/release/sentrix-labs/whitepaper?include_prereleases&sort=semver)](https://github.com/sentrix-labs/whitepaper/releases/latest)


Foundational paper for [Sentrix Chain](https://sentrixchain.com) — a Layer-1 blockchain.

Author: Satya Kwok &lt;satya@sentrixchain.com&gt;

## Contents

- [`sentrix-whitepaper-en.tex`](sentrix-whitepaper-en.tex) — English LaTeX source (v1.3.0)
- [`sentrix-whitepaper-en.pdf`](sentrix-whitepaper-en.pdf) — English PDF (LaTeX-rendered, canonical)
- [`sentrix-whitepaper-en.md`](sentrix-whitepaper-en.md) — English Markdown source (v1.3.0)
- [`sentrix-whitepaper-en.html`](sentrix-whitepaper-en.html) — English HTML (browser-readable)
- [`sentrix-whitepaper-id.tex`](sentrix-whitepaper-id.tex) — Bahasa Indonesia LaTeX source (v1.3.0)
- [`sentrix-whitepaper-id.pdf`](sentrix-whitepaper-id.pdf) — Bahasa Indonesia PDF (v1.3.0)
- [`sentrix-whitepaper-id.md`](sentrix-whitepaper-id.md) — Bahasa Indonesia Markdown (v1.3.0)
- [`sentrix-whitepaper-id.html`](sentrix-whitepaper-id.html) — Bahasa Indonesia HTML (v1.3.0)

## Building from source

```bash
pdflatex sentrix-whitepaper-en.tex
pdflatex sentrix-whitepaper-en.tex   # second pass for cross-references
```

Requires `texlive-latex-recommended`, `texlive-latex-extra`, `lmodern`, and (for the ID build) `texlive-lang-other`. The English `.tex` uses `newunicodechar` mappings for math glyphs (`⌊⌋⌈⌉∈Σ` etc.) so plain `pdflatex` produces the canonical PDF without needing XeLaTeX.

## Versions

- **v1.3.0** (current) — Full revision targeting infra-grade protocol-specification
  rigor (Tendermint / Monad / Solana paper level). Each constant traces to a specific
  module path in `sentrix-labs/sentrix@v2.1.56`; Appendix A pins every parameter to
  its file of origin so future drift is detectable.

  Major upgrades over v1.2.4:

  - **§2 System Model** added: notation table, partial-synchrony assumption (Dwork-
    Lynch-Stockmeyer), adversarial model `β < 1/3` of stake-Byzantine, SMR formalism.
  - **§4 Voyager BFT** formalized: round structure with PROPOSE / PREVOTE / PRECOMMIT /
    FINALIZE phases, real timeout values from `crates/sentrix-bft/src/engine/timeouts.rs`
    (`PROPOSE 20s`, `PREVOTE 12s`, `PRECOMMIT 12s`, linear backoff `+1s` propose / `+2s`
    votes per round, capped 30s, `MAX_ROUND = 100`), locking invariant, Agreement /
    Validity / Termination theorems with proof sketches, `O(n²)` message complexity.
  - **§5 Execution Layer** expanded: pipeline diagram, three determinism axioms (D1/D2/D3),
    fully-formalised STF pseudocode, forward-looking optimistic-parallel execution model
    (Block-STM-style).
  - **§10 Performance Model** added: `TPS_max = MAX_TX_PER_BLOCK / BLOCK_TIME = 5,000`,
    latency decomposition `T_block = T_propose + T_prevote + T_precommit + T_apply`,
    scalability bounds.
  - **§11 Failure Handling** explicit protocols for partition / equivocation / downtime /
    chain recovery, with the operator-side `chain.db` rsync runbook (pulls FROM canonical
    TO stale via `ssh canonical-peer 'tar | tar -x'`).
  - **§12 Benchmark Framework** added: sequential reference engine + batched optimistic-
    parallel engine pseudocode, metrics definitions for `tps`, `p50_latency`, `p99_latency`,
    `conflict_rate`.
  - **§13 Comparative Analysis** restructured into three tables (execution model / consensus
    design / scalability approach) across Ethereum, Solana, Monad, Polygon PoS, Sentrix.
    MonadBFT correctly listed as `O(n)` per round (HotStuff-derivative with threshold
    aggregation).
  - **§14 Open Problems** expanded: parallel-execution determinism proof, light-client weak-
    subjectivity, cross-rail atomicity, NFT TokenOp activation, consensus-jail fix, EVM
    value-transfer fork-gate retirement, multi-implementation diversity, founder vesting.
  - 3 Mermaid diagrams in the Markdown source replace the v1.2.4 ASCII art (architecture,
    BFT round sequence, execution pipeline). The LaTeX source falls back to ASCII inside
    `verbatim` blocks for those three figures so the PDF renders without a TikZ pass.
  - Voice shifted strictly technical: retired "real-economy / Indonesia first / world after"
    framing from §1, §2, §9. The chain is described as what it is — an open-source EVM-
    compatible Layer-1 built in Rust — without market-strategy claims.

  Corrections of factual errors in v1.2.4 surfaced by the v1.3.0 audit:

  - Slashing constant correctly named `DOUBLE_SIGN_SLASH_BP` (v1.2.4 used the spec'd-but-
    unused name `EQUIVOCATION_SLASH_BP`).
  - Burn mechanism corrected: there is no `BURN_ADDRESS` sentinel. `burn_share = total_fee.div_ceil(2)`
    is debited from sender and never credited; supply contracts. v1.2.4 implied a sentinel.
  - `MAX_ACTIVE_VALIDATORS = 21` is a compile-time `const`, not parametric (v1.2.4 was vague).
  - `STATE_ROOT_FORK_HEIGHT = 100,000` is a compile-time `const`, not parametric.
  - Proposer selection is pure round-robin `(h + r) mod n`. The function name in the source
    is `weighted_proposer` but the body is unweighted.
  - Round advancement is **timeout-only** (per the 2026-04-17 leapfrog-stall fix); no vote-
    triggered or `RoundStatus`-triggered cross-round catch-up.

  The Bahasa Indonesia translation tracks v1.3.0 in this release.

- **v1.2.4** — Two corrections: reworked §7.2 validator-revenue formula (subsidy is split
  pro-rata across precommit signers, not paid to the proposer; only fees go to proposer).
  Fixed §11.2 SentrixSafe deployment claim — was "deployed at genesis" but actually deployed
  shortly after Voyager activation as part of the canonical contracts set.

- **v1.2.3** — Two corrections. First, the §6.4 premine table no longer carries fabricated
  sub-allocation strings ("market-making seed", "DEX liquidity bootstrap", "under multi-
  signature control") — those operational details belong in the mutable `sentrixchain.com/docs/tokenomics`
  page, not in the stable whitepaper. Second, reverted the v1.2.2 fee-routing description:
  the actual chain credits the 50% non-burn fee share directly to the block proposer's balance
  (immediate spendable). It is the block subsidy (1 SRX coinbase), not the fee, that goes
  through `PROTOCOL_TREASURY` for pro-rata distribution to precommit signers. v1.2.2
  conflated the two paths.

- **v1.2.2** — Technical-accuracy audit pass: transaction format, signing payload, and a
  `JAIL_CONSENSUS` risk note. (Note: this version introduced the fee-routing inaccuracy that
  v1.2.3 corrects.)

- **v1.2.1** — LaTeX source (.tex) added, contact email `satya@sentrixchain.com`, "Focus
  Statement" preamble.

- **v1.2** — Adds genesis allocation addresses (on-chain verifiable), validator onboarding
  requirements (§7.5), incident response model (§7.6), privacy posture (§8.7), expanded
  comparison with Aptos/Sui/Near, full governance section (§11) covering current binary-
  release coordination + SentrixSafe multisig + future on-chain governance + non-governable
  invariants, risk disclosures (Appendix B), legal notice (Appendix C), and About the Author.

- **v1.1** — Vision, mission, and protocol depth: state transition function, BFT safety/
  liveness, network model, transaction lifecycle, slashing matrix, long-range attack
  defense, comparison table, parameters appendix.

- **v1.0** (initial) — Vision-first foundational document.

## Citation

```
Kwok, S. (2026). Sentrix: A Layer-1 Blockchain — Protocol Specification (v1.3.0).
Sentrix Labs. https://github.com/sentrix-labs/whitepaper
```

## License

The whitepaper text is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
You may share, adapt, and build on this work with attribution.

The Sentrix Chain implementation is licensed separately — see
[sentrix-labs/sentrix](https://github.com/sentrix-labs/sentrix) for protocol code (BUSL-1.1).
