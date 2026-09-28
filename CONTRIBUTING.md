# Contributing to AFSP

## Proposing an extension

Any organization may submit an extension proposal by [opening an issue with the Extension proposal template](https://github.com/primitive-os/AFSP/issues/new?template=extension-proposal.yml). The template applies the `extension-proposal` label automatically.

Proposals must include:

- **(a)** the problem statement and use case
- **(b)** the proposed technical specification
- **(c)** backward compatibility analysis
- **(d)** security considerations specific to the extension
- **(e)** the proposing organization's identity and contact

Proposals must also give the extension type (see [Extension types](#extension-types)) and name the components, sections and registered values they affect.

## Commenting on the v0.1 Public Review Draft

Comments on any part of the specification are welcome through November 16, 2026.

- Open a [specification comment](https://github.com/primitive-os/AFSP/issues/new?template=spec-comment.yml), citing the section number.
- Or email [afsp@primitive.com](mailto:afsp@primitive.com). GitHub issues are public; use email for comments you want to keep confidential.

## Extension types

AFSP recognizes three kinds of extension (Section 4.1 of the specification):

| Type | Covers | Review path |
|---|---|---|
| **Type 1: Minor** | Optional schema fields, new values for existing enumerated types, or clarifications that don't change behavior | Working group review; incorporated into the next point release (for example v0.1 to v0.2) |
| **Type 2: Major** | A new Signal type, a new AFSP component number, a new action class, or a change to mandatory behavior | Working group consensus and a 60-day public comment period; incorporated into the next minor or major version |
| **Type 3: Profile** | A named implementation profile (for example "AFSP-EU" or "AFSP-SMB") that constrains or extends the base specification for one deployment context | Published as a companion document; doesn't modify the base specification |

Values registered under v0.1 (action classes, product categories, S5 signal types, clearance decisions and return trip outcomes) are listed in Section 4.3 and may be extended through the Type 1 process. Proposals should name the components, sections and registered values they affect.

## Review process

The AFSP Technical Working Group reviews extension proposals on a rolling basis. Proposal status is tracked with labels:

| Label | Meaning |
|---|---|
| `extension-proposal` | New proposal, awaiting triage |
| `status: under-review` | Accepted for working group review |
| `status: needs-info` | More information requested from the proposer |
| `status: accepted` | Approved for inclusion in the specification |
| `status: declined` | Not accepted; reasons given in the issue |

Specification comments are labeled `spec-comment`.

## Participation

Founding Endorsers have standing participation in the working group. Other organizations may participate as observers or contributors as defined in the [working group charter](CHARTER.md).

## Security issues

Do not report vulnerabilities in public issues. See [SECURITY.md](SECURITY.md).

## Contributor terms

The AFSP contribution terms, including any patent commitments from contributors, will be published before pull requests are accepted. Until then, please share comments and extension proposals as issues.

## Licensing

The specification is published under the terms in [LICENSE](LICENSE) and [PATENTS.md](PATENTS.md), which follow Section 17.5 of the specification: it may be reproduced without royalties, with attribution, and patent claims essential to implementing it are licensed on FRAND terms.
