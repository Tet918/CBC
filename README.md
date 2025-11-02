# CBC — Compressed Block Chain

CBC (Compressed Block Chain) is an open specification that defines a canonical, deterministic compressed block format and companion proof structures to reduce blockchain storage and bandwidth while preserving verifiability and interoperability.

Elevator pitch
- One-sentence: CBC is an open, Creative‑Commons specification that shrinks blockchain data so more people can run full nodes, apps sync faster, and networks become cheaper and greener.
- Call to action: Read the spec, try the test vectors, or help build reference implementations.

What’s in this repository
- SPEC.md — the plain-language specification and rationale.
- ONE_PAGER.md — one-page summary and benefits.
- CONTRIBUTING.md — how to help (reviews, spec changes, code).
- LICENSES:
  - LICENSE_DOCS (CC0) — for spec and docs.
  - LICENSE_CODE (MIT) — recommended for any code contributed.

Why CBC?
- Reduces node storage and bandwidth costs.
- Improves sync time and block propagation.
- Enables better mobile/light client experiences.
- Designed to be optional and backwards-compatible (e.g., sidecar/on-wire compression).

How to get started
1. Read SPEC.md for the design and invariants.
2. Clone the repo, open an issue or PR with thoughts or improvements.
3. If you’re a developer: add reference code in /implementations (suggested languages: Rust, Go).
4. If you want to help but don’t code: review the spec, reach out to maintainers, recruit implementers, or write test vectors.

Contact / Governance
- Maintainers: (initially community-led). See CONTRIBUTING.md.
- Email: (put your contact email here) or open an issue to start a conversation.

Status
- Concept/spec stage. Seeking technical collaborators and reference implementations.