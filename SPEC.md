```text
CBC — Compressed Block Chain
Version: 0.1 (draft)
Last updated: 2025-11-01

Summary
CBC defines a deterministic canonical encoding for compressed blockchain blocks and associated proof structures that preserve core header invariants (block root, state root, timestamp, difficulty/weight). The goal is to reduce bandwidth and storage while keeping verification possible by full nodes and SPV/light clients.

Design goals
- Determinism: identical inputs produce identical compressed outputs across implementations.
- Verifiability: preserve root hashes and allow inclusion/exclusion proofs.
- Backwards-compatibility: initial deployment as optional/on-wire sidecar or negotiated feature.
- Minimal cryptographic changes: do not alter hashing, signature, or consensus primitives.
- Chunked/streamable: allow partial decompression for lower memory footprint.

High-level format
- Header (uncompressed, fixed fields):
  - format_version (uint16)
  - compression_algorithm (string enum, e.g., "zstd", "none")
  - uncompressed_size (uint64)
  - block_hash (32 bytes) — canonical block hash of uncompressed block
  - state_root (32 bytes) — preserved state/commit root
  - transactions_root (32 bytes) — merkle/root of tx commitments
  - proof_root (32 bytes) — root for auxiliary proof structure when used
  - flags (bitfield) — optional features (chunking enabled, sidecar-only, etc.)
- Payload:
  - compressed_blob (bytes) — zstd (recommended) of canonical serialization described below
  - optional index table (for chunk offsets)
- Footer:
  - truncated CRC32 or SHA256 of compressed payload for quick corruption detection

Canonical serialization (for compression)
- Define a deterministic serialization of the uncompressed block (canonical field order, integer varints, canonical binary encoding for signatures and addresses).
- Must produce identical bytes across implementations before compression.
- Example: canonical block = header || canonical_tx_list || canonical_withdrawals || receipts (if included)
- Test vectors must be provided.

Proofs & light clients
- CBC must preserve a proof root (e.g., merkle root/accumulator) that enables SPV proofs without decompressing full payload.
- For compressed transactions, provide auxiliary inclusion proofs or chunked access APIs so light clients can request only relevant data.

Deployment model
- Sidecar mode: nodes advertise support for CBC; they may publish both uncompressed canonical blocks and compressed sidecar blobs.
- On-wire compression: peers may negotiate compression for p2p transfers — maintain header fields uncompressed.
- Consensus changes: avoid changing consensus-critical fields. If consensus-level payloads must change, plan a hard-fork with a long transition.

Security considerations
- Decompression DoS: require size limits and streaming decompression, validate compressed size against header.
- Determinism failures: spec must include exact canonicalization rules and test vectors.
- Privacy: compression may change transaction ordering or grouping; keep fee-market semantics intact.
- Integrity: always check block_hash and cryptographic roots before accepting decompressed content.

Reference points & test vectors
- Provide canonical examples of:
  - small block with 3 txs
  - block with large receipts
  - chunked block example
- Provide expected compressed output for each example using specified algorithm and parameters.

Next steps (developer actions)
- Publish canonical test vectors and a minimal reference implementation for one language.
- Add interoperability tests and CI that validate compression/decompression round-trips.
- Provide benchmarks (compression ratio, CPU time) against representative blocks.

Acknowledgements & related work
- Bitcoin BIP152 (compact blocks), Graphene, Verkle trees, Erigon snapshotting, ZK-rollup approaches.
```