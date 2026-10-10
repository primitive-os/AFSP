# AFSP v0.1 JSON Schemas

JSON Schema (draft 2020-12) for every AFSP v0.1 document exchanged in a
session. Each schema's `$id` is
`https://agenticfinanceprotocol.org/schemas/v0.1/<name>.schema.json`; `$ref`s
between schemas are relative, so the directory works offline with any
registry that maps `$id`s to these files.

Every schema carries an `x-afsp-status`:

- `v0.1` — the document is defined by the AFSP v0.1 Public Review Draft and
  the schema follows the specification's field names.
- `reference-proposal` — v0.1 leaves the format to the Technical Working
  Group (§18). The schema documents the proposal of Primitive's reference
  implementation, for the working group to adopt, change or replace. It is
  not normative AFSP.

| Schema | Document | Specification | Status |
|---|---|---|---|
| [`common.schema.json`](common.schema.json) | AFSP v0.1 common definitions | §3.4, §4.3 | normative v0.1 |
| [`pcaa-package.schema.json`](pcaa-package.schema.json) | PCAA attestation package | §6.1, §6.2 | normative v0.1 |
| [`s1-agent-provenance.schema.json`](s1-agent-provenance.schema.json) | S1 agent provenance attestation | §6.2 VAL-05/06, §18 | reference proposal |
| [`s2-signal.schema.json`](s2-signal.schema.json) | S2 consumer authorization signal (envelope) | §7.3, §7.4, §6.2 VAL-07/08/08a | reference proposal |
| [`consumer-authorization-token.schema.json`](consumer-authorization-token.schema.json) | Consumer authorization token | §7.3 | normative v0.1 |
| [`tier-key-certificate.schema.json`](tier-key-certificate.schema.json) | Tier key certificate | §7.2, §7.4 (pending AFSP-07) | reference proposal |
| [`scope-expansion-token.schema.json`](scope-expansion-token.schema.json) | Scope expansion token | §9.2 | reference proposal |
| [`servicing-intent-package.schema.json`](servicing-intent-package.schema.json) | Servicing Intent Package | §5.8, Appendix A.8 | reference proposal |
| [`s3-federated-referral.schema.json`](s3-federated-referral.schema.json) | S3 federated referral attestation | §1.3, §3.3 | reference proposal |
| [`s4-financial-profile.schema.json`](s4-financial-profile.schema.json) | S4 financial profile attestation | §8.1–§8.4, §6.2 VAL-09 | normative v0.1 |
| [`s5-provider-score.schema.json`](s5-provider-score.schema.json) | S5 provider score submission | §11.2 | normative v0.1 |
| [`s5-composite.schema.json`](s5-composite.schema.json) | S5 composite score | §11.3, §11.4 | reference proposal |
| [`reverse-inquiry.schema.json`](reverse-inquiry.schema.json) | Reverse inquiry payload | §9.3 | normative v0.1 |
| [`institution-response.schema.json`](institution-response.schema.json) | Institution response | §9.4 | normative v0.1 |
| [`clearance-response.schema.json`](clearance-response.schema.json) | Clearance response | §10.2, Appendix A.8 | reference proposal |
| [`session-token.schema.json`](session-token.schema.json) | AFSP session token | §10.3 | normative v0.1 |
| [`audit-record.schema.json`](audit-record.schema.json) | Audit record | §10.4 | reference proposal |
| [`return-trip.schema.json`](return-trip.schema.json) | Return Trip confirmation | §13.2 | normative v0.1 |
| [`registry-entry.schema.json`](registry-entry.schema.json) | Participation Registry entry | §12.2 | normative v0.1 |
| [`trust-list.schema.json`](trust-list.schema.json) | AFSP trust list | §6.2, §12.2, §17 (distribution of keys) | reference proposal |
| [`validation-report.schema.json`](validation-report.schema.json) | Package validation report | §6.2 | reference proposal |

## Design rules

- **Open objects.** Objects allow additional members so documents stay
  forward compatible with Type 1 extensions (§4.1).
- **One closed object.** The S4 `signals` object and each of its six signals
  are closed (`additionalProperties: false`): §8.1 data minimization permits
  only the derived signals of §8.3, so an attestation carrying raw data (for
  example an `account_balance_usd` member) is invalid.
- **Structure only.** Schemas check shape, registered values (§4.3),
  identifier and timestamp formats. They do not check signatures, validity
  window lengths (3600 s, 1800 s), `signals_present` consistency, bindings
  between signals, or trust — those are the verifier's job (§6.2) and are
  covered by the conformance vectors in `../vectors`.
- **Signatures.** Signature blocks require `algorithm: "ECDSA-P256"` (§3.4)
  and a base64 `signature`. The signing input is the RFC 8785 canonical form
  of the document with only the signature value removed (reference proposal).

## Validating

Any Draft 2020-12 validator works. Register every file in this directory
by its `$id`, then validate each document in [`../vectors`](../vectors) against the
schema for its kind; `vectors/manifest.json` records which vectors are
deliberately schema-invalid (`schema_valid: false`).

## License

Copyright © 2026 CiDR Technologies, Inc. D/B/A Primitive. All rights reserved.

Published as part of the AFSP Specification under the [AFSP Specification License](../LICENSE).
