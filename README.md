# AFSP: Agentic Financial Services Protocol

AFSP is an open industry protocol for one moment: when an AI agent arrives at a regulated financial institution on behalf of a person. Before the agent reaches the institution's open banking APIs, AFSP lets the institution verify who the agent is, who it is acting for, and that the person is present and in control.

AFSP governs one question: should this AI agent session proceed to the open banking API layer? KYC, underwriting and account-opening decisions stay with the institution. AFSP is a standard, not a product.

## Status

**v0.1 Public Review Draft, published September 28, 2026. Open for public comment through November 16, 2026.**

- Specification: [AFSP v0.1 Public Review Draft](https://agenticfinanceprotocol.org) at agenticfinanceprotocol.org

Version 0.1 covers the first use case: agent-assisted opening of a new deposit account.

## The five trust signals

| Signal | Question | What it verifies |
|---|---|---|
| **S1** | Who is this agent? | The AI agent is a certified, registered, unmodified software build from an accountable operator |
| **S2** | On whose behalf, and did they authorize it? | A real human, on their enrolled device, explicitly authorized this specific session |
| **S3** | Has their identity been verified? | A trusted network institution has previously completed KYC on this consumer (cryptographic attestation only, no raw data) |
| **S4** | Are they financially real? | The consumer's behavioral financial signals are consistent with an established consumer profile |
| **S5** | Are they present right now? | The session is currently human-operated within normal behavioral parameters |

The signals are bound into a single cryptographically signed attestation package (the PCAA package). The receiving institution evaluates it in real time against its own risk framework and returns a clearance decision: proceed to its own KYC, request additional verification, or decline the session. Nothing reaches the institution's banking systems until the institution clears the session.

## How it works

1. **Consumer and agent.** The consumer authorizes the session, and exactly what the agent may do, with a hardware-bound biometric check on their enrolled device.
2. **Attestation package.** Certified signal providers produce S1–S5, which are bound into the signed PCAA package (Section 6 of the specification).
3. **Institution.** The receiving institution verifies the package and evaluates each signal against its own risk-decisioning framework.
4. **Existing controls.** KYC, credit and fraud decisions proceed as they do today. AFSP's role ends at the clearance decision.

Version 0.1 defines the architecture and information model. The protocol binding (transport, endpoints, message serialization, signature format and error handling), a reference implementation and conformance tests will be developed with the Technical Working Group, starting in November 2026. First proofs of concept against the network specifications are targeted for Q1 2027.

### Scope

AFSP does not make credit, fraud, KYC or servicing decisions. Those controls stay with the institution; AFSP runs before them. AFSP is designed to align with existing regulatory frameworks, including Section 1033 and FFIEC guidance, and gives examiners a tamper-evident record of who authorized what.

## Get involved

- **Comment on the specification** by November 16, 2026: open a [specification comment](https://github.com/primitive-os/AFSP/issues/new?template=spec-comment.yml) or email [afsp@primitive.com](mailto:afsp@primitive.com). GitHub issues are public; send confidential comments by email.
- **Join the Technical Working Group.** Participation is open to all organizations with a bona fide interest in the agentic banking channel. Working groups begin in November 2026. Contact [afsp@primitive.com](mailto:afsp@primitive.com).
- **Become a Founding Endorser.** This requires a written letter of endorsement and a commitment to participate in the Technical Working Group; no production implementation is required. Contact [afsp@primitive.com](mailto:afsp@primitive.com). Founding Endorsers will be announced ahead of the November 16, 2026 comment deadline.
- **Prototype against it.** The specification and its worked example are public. Production implementations require AFSP certification, and patent claims essential to implementing AFSP are licensed on FRAND terms (see [License and intellectual property](#license-and-intellectual-property)).

## Extending AFSP

Any organization may propose an extension: a minor extension (Type 1), a major extension (Type 2) or an implementation profile (Type 3). [Open an extension proposal](https://github.com/primitive-os/AFSP/issues/new?template=extension-proposal.yml). See [CONTRIBUTING.md](CONTRIBUTING.md) for what to include and how proposals are reviewed.

## Governance

AFSP v0.1 is published and governed by Primitive as founding author and interim protocol administrator. Founding Endorsers have standing participation in the AFSP Technical Working Group, which is open to all organizations with a bona fide interest. Governance will transfer to the AFSP Foundation, an independent non-profit targeted for formation in early 2027, with final timing set by the independent entity itself. See the [working group charter](CHARTER.md).

## License and intellectual property

The AFSP specification is published under fair, reasonable and non-discriminatory (FRAND) licensing terms. Anyone may reproduce the specification without payment of royalties to Primitive, subject to attribution.

The technical mechanisms described in the specification are the subject of patent applications filed by CiDR Technologies, Inc. D/B/A Primitive. Primitive is willing to license the patent claims essential to implementing AFSP, on FRAND terms, to any organization implementing AFSP in good faith and complying with the specification. The license terms, including whether any royalty applies, will be published before v1.0. See [LICENSE](LICENSE), [PATENTS.md](PATENTS.md) and Section 17.5 of the specification.

AFSP certification, the certification mark and inclusion in the Participation Registry are governed by the Network Participation Agreement. Publishing the specification grants no rights to the AFSP name or certification mark.

## Contact

[afsp@primitive.com](mailto:afsp@primitive.com) · [agenticfinanceprotocol.org](https://agenticfinanceprotocol.org)
