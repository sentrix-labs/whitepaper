# Sentrix Whitepaper

Foundational paper for [Sentrix Chain](https://sentrixchain.com) — a Layer-One blockchain for the real economy.

Author: Satya Kwok &lt;satya@sentrixchain.com&gt;

## Contents

- [`sentrix-whitepaper-en.tex`](sentrix-whitepaper-en.tex) — English LaTeX source
- [`sentrix-whitepaper-en.pdf`](sentrix-whitepaper-en.pdf) — English PDF (LaTeX-rendered, canonical)
- [`sentrix-whitepaper-en.md`](sentrix-whitepaper-en.md) — English Markdown source
- [`sentrix-whitepaper-en.html`](sentrix-whitepaper-en.html) — English HTML (browser-readable)
- [`sentrix-whitepaper-id.tex`](sentrix-whitepaper-id.tex) — Bahasa Indonesia LaTeX source
- [`sentrix-whitepaper-id.pdf`](sentrix-whitepaper-id.pdf) — Bahasa Indonesia PDF (LaTeX-rendered)
- [`sentrix-whitepaper-id.md`](sentrix-whitepaper-id.md) — Bahasa Indonesia Markdown source
- [`sentrix-whitepaper-id.html`](sentrix-whitepaper-id.html) — Bahasa Indonesia HTML

## Building from source

```bash
pdflatex sentrix-whitepaper-en.tex
pdflatex sentrix-whitepaper-en.tex   # second pass for cross-references
```

Requires `texlive-latex-recommended`, `texlive-latex-extra`, `lmodern`, and (for the ID build) `texlive-lang-other`.

## Versions

- **v1.2.2** (current, final unless chain hard-fork) — Technical-accuracy
  audit pass against the live `sentrix-labs/sentrix` codebase. Fixed the
  transaction format (single canonical wire format with `to_address`
  sentinel routing, not the Ethereum-style `op_type` field), corrected
  the signing claim (canonical-JSON SHA-256 over eight content fields,
  not EIP-155 RLP), corrected fee distribution (50% accrued to all
  precommit signers pro-rata via `PROTOCOL_TREASURY` escrow, not paid
  directly to the proposer), and noted in the risk disclosures that the
  on-chain consensus-jail dispatch is currently disabled with manual
  jailing as the operational path.
- **v1.2.1** — LaTeX source (.tex) added, contact email
  `satya@sentrixchain.com`, "Focus Statement" preamble making explicit
  that Sentrix is financial infrastructure for the real economy and not
  a speculation venue.
- **v1.2** — Adds genesis allocation addresses (on-chain verifiable),
  validator onboarding requirements (§7.5), incident response model (§7.6),
  privacy posture (§8.7), expanded comparison with Aptos/Sui/Near, full
  governance section (§11) covering current binary-release coordination +
  SentrixSafe multisig + future on-chain governance + non-governable
  invariants, risk disclosures (Appendix B), legal notice (Appendix C),
  and About the Author.
- **v1.1** — Vision, mission, and protocol depth: state transition function,
  BFT safety/liveness, network model, transaction lifecycle, slashing matrix,
  long-range attack defense, comparison table, parameters appendix.
- **v1.0** (initial) — Vision-first foundational document; tagline "Where real assets live."

## Citation

```
Kwok, S. (2026). Sentrix: A Layer-One Blockchain for the Real Economy.
Sentrix Labs. https://github.com/sentrix-labs/whitepaper
```

## License

The whitepaper text is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
You may share, adapt, and build on this work with attribution.

The Sentrix Chain implementation is licensed separately — see
[sentrix-labs/sentrix](https://github.com/sentrix-labs/sentrix) for protocol code (BUSL-1.1).
