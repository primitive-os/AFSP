# AFSP v0.1 conformance vectors

Language-neutral test vectors for AFSP v0.1, built from the Appendix A
session (Alice's certified AI assistant opens a high-yield savings account at
Sunrise Digital Bank; the bound package is validated at
`2026-08-18T14:24:15Z`). Use them to check that another implementation
canonicalizes, signs, verifies and validates exactly as the reference
implementation does.

Everything here is generated deterministically by Primitive's reference
implementation, from fixed seeds, identifiers and timestamps (ECDSA
signatures use RFC 6979 deterministic nonces), and re-verified against it on
every change. Only public keys are included. **All keys are test keys.**
Never use them outside tests.

## Layout

| Path | Contents |
|---|---|
| `manifest.json` | The package validation cases, the document index, the trust inputs and the verifier policy |
| `trust/trust-list.json` | The signed AFSP trust list (platform, agent registry, S4/S5 provider keys, Participation Registry entries) |
| `trust/keys/*.jwk.json` | Every party's public test key (P-256 JWK with `kid`); `administrator.jwk.json` is the key the trust list is pinned to |
| `documents/` | Every positive document of the session: S1–S5, tier key certificates, authorization tokens, scope expansion, Servicing Intent Package, unbound and bound packages, reverse inquiry, institution response, clearance response, session token, audit record, Return Trip, registry entry |
| `packages/` | One package per validation case (positive and negative) |
| `reports/` | The reference `ValidationReport` for each case (informative; `detail` messages are not normative) |
| `jcs/` | RFC 8785 canonicalization cases: `<id>.input.json` and `<id>.canonical.json` |
| `signatures/` | Single-document signature cases: kind, document, public key, signing input, expected result |

## Package validation cases (`manifest.json` → `cases`)

Each case gives:

- `inputs.package`, `inputs.trust_list`, `inputs.administrator_key` — files
  relative to this directory;
- `institution` — the receiving institution's registered identifier. Its
  registered keys (for VAL-04a nonce binding) come from the trust list;
- `request` — what the institution asks the package to authorize:
  `action_class` (default `INITIATE`, the clearance handshake),
  `product_category` (or `null`) and `clearance_granted`;
- `now` / `now_unix` — the verification time. Use this, never the clock;
- `prior_presentations` (optional) — earlier presentations of the same
  package to the **same** verifier (same consumed-package registry), each
  with its own `now` and `expected`. `val-04-replay` presents the package
  once (ACCEPTED) and then again (replay);
- `expected.outcome` — `ACCEPTED`, `PACKAGE_REJECTED` or `SESSION_TERMINATED`;
- `expected.check`, `expected.code`, `expected.status` — the first check that
  failed (or, when none failed, the first that warned), its stable error
  code, and `FAIL` / `WARN`. `null` for a clean pass;
- `expected.checks` — the status of every check that ran (`PASS`, `FAIL`,
  `WARN`, `SKIPPED`). Checks run in the order STRUCTURE, VAL-04a, VAL-01,
  VAL-02, VAL-03, VAL-04, VAL-05, VAL-06, VAL-07, VAL-08, VAL-08a, REF-01,
  VAL-09, VAL-10; after a package-level failure no later check runs;
- `schema_valid` — whether the package validates against
  `schemas/pcaa-package.schema.json`. Most negative packages are
  structurally valid (their failures are cryptographic or semantic); the
  S4 raw-data field, the short `package_id` and the wrong `afsp_version` are
  not;
- `spec` — the requirement the case exercises.

The verifier policy is in `manifest.json` → `policy`: 30 s clock skew, S4
epsilon at most 1.0 and k at least 5, NETWORK_MARKETPLACE Tier 1
authorizations accepted, 86400 s trust-list grace period.

### Using the cases from another implementation

For each case:

1. Load the trust list and verify it against the administrator key at `now`
   (with the `max_stale_s` grace period).
2. Create a verifier for `institution` with an empty consumed-package
   registry.
3. Run every `prior_presentations` entry, then the case itself, with your
   own code, at the given times and `request`.
4. Compare the outcome, the first failing (or warning) check id and its error
   code with `expected`. An implementation SHOULD also match
   `expected.checks`; it need not reproduce the reports' `detail` text.

Error codes: `malformed`, `invalid_argument`, `bad_signature`,
`untrusted_key`, `not_yet_valid`, `expired`, `validity_too_long`, `replay`,
`wrong_binding`, `scope`, `not_certified`, `state`, `host`.

### Covered requirements

| Requirement | Cases |
|---|---|
| Positive | `accept-appendix-a`, `accept-present-with-tier1-only`, `accept-s5-absent`, `accept-s3-absent`, `accept-s5-single-provider` |
| STRUCTURE | `structure-signals-present-mismatch`, `structure-short-package-id`, `structure-unsupported-version` |
| VAL-04a | `val-04a-redirected`, `val-04a-unbound`, `val-04a-expired-nonce` |
| VAL-01 | `val-01-tampered`, `val-01-untrusted-key` |
| VAL-02 | `val-02-wrong-institution` |
| VAL-03 | `val-03-too-long`, `val-03-not-yet-valid` |
| VAL-04 | `val-04-replay` (two-step) |
| VAL-05 / VAL-06 | `val-05-s1-absent`, `val-05-suspended`, `val-05-wrong-signer`, `val-06-modified-build` |
| VAL-07 | `val-07-s2-absent`, `val-07-wrong-key`, `val-07-wrong-agent`, `val-07-too-long`, `val-07-wrong-institution`, `val-07-certificate-not-from-issuer` |
| VAL-08 / VAL-08a | `val-08-no-step-up`, `val-08-sip-tampered`, `val-08-product-out-of-scope`, `val-08-execute-without-clearance`, `val-08a-tier1-key-signs-initiate` |
| VAL-09 | `val-09-s4-absent`, `val-09-raw-data-field`, `val-09-epsilon-too-large`, `val-09-wrong-key` |
| VAL-10 (warn, accepted) | `val-10-composite-tampered` |
| REF-01 (warn, accepted) | `ref-01-s3-other-consumer` |

## Canonicalization (`jcs/`)

AFSP signs "canonical JSON"; the reference implementation proposes RFC 8785
(JCS) pending §18. Canonicalize the **bytes** of `<id>.input.json` and compare
byte for byte with `<id>.canonical.json` (UTF-8, no trailing newline). Cases
with `expected_error` must fail with `malformed`. AFSP profiles RFC 8785
with one interoperability rule (RFC 7493 §2.2): a number whose **value** is
an integer of magnitude above 2^53-1 and below 10^21 is rejected however it
is written (`9007199254740993`, `1e20` and `123456789012345680000.0` alike),
because RFC 8785 would print it as an integer literal that languages
re-parse differently. From 10^21 up the exponent form is used and accepted.
So canonicalization is idempotent. The `rfc8785-appendix-b-numbers` entry
lists each of its 18 IEEE 754 bit patterns and canonical forms; the other
five Appendix B values are in `rfc8785-appendix-b-unsafe-integers`. `jcs/` is marked `-text` in `.gitattributes` so the bytes
survive checkout.

## Signatures (`signatures/`)

Run each case by verifying `document` with `public_key` **alone**, without
the trust list's key-id filtering. So `tampered-signed-with` expects
`bad_signature`, the given key's signature no longer matching the changed
`signed_by`. In package validation, a `signed_by` naming an untrusted key
id would instead fail earlier with `untrusted_key`.

Each file gives a document `kind`, the `document`, a `public_key` and
`expected.valid` (with `expected.code` when invalid). `signing_input` is the
exact string that is signed: the RFC 8785 canonical form of the document with
the signature *value* removed — the top-level `signature` (consumer
authorization, scope expansion) or `institution_signature` (clearance
response) member, or the `signature` member of the document's signature
block (every other kind). Every other member, including `algorithm` and
`signed_by` / `signed_with`, is covered. Signatures are ECDSA-P256/SHA-256
and `algorithm` must be `"ECDSA-P256"` (§3.4). Each signature has exactly one
accepted encoding: strict DER, low-S (`s <= n/2`), standard base64 with
padding, no whitespace. The `raw-r-s-*`, `high-s-*`, `base64url-*` and
`non-minimal-der-*` cases carry a mathematically valid signature in another
encoding and must be rejected with `bad_signature`; accepting them would let
anyone re-encode a signature into a second valid string.

| Kind | Signature location |
|---|---|
| `package`, `session_token`, `audit_record` | `platform_signature` (`signed_by`: `platform.afsp.org`) |
| `agent_provenance` | `registry_signature` |
| `tier_key_certificate` | `issuer_signature` |
| `federated_referral`, `return_trip` | `institution_signature` (`signed_by`) |
| `institution_response` | `institution_signature` (`signed_with`, §9.4) |
| `financial_profile`, `trust_list` | `signature` block |
| `behavioral_score`, `behavioral_composite` | `attestation` |
| `consumer_authorization`, `scope_expansion` | top-level `signature` string |
| `clearance_response` | top-level `institution_signature` string |

## Documents (`manifest.json` → `documents`)

Each entry names the file, the schema it validates against, and, for signed
documents, its `kind` and the public key that verifies it.

## License

Copyright © 2026 CiDR Technologies, Inc. D/B/A Primitive. All rights reserved.

Published as part of the AFSP Specification under the [AFSP Specification License](../LICENSE).
