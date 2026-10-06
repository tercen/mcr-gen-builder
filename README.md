# mcr-gen-builder

The zero-human template repo: the seed every MCR generator repo forks.
Machine-created, no human commits. A generator's canonical store is its own
repo — this template carries the machinery, forks carry the generator.

- Orders and provenance: tercen/mcr#6 (genseq order 0; order-0 receipt
  comment 6013352534 — repo created by Alex under his own credential, by
  explicit order; machinery load by builder glm2 under the
  tercen-osanwe-builder App, landed by first PR).
- Anatomy: tercen/sarno `doc/lens-operator-plan.md` at `b4169eaa`
  (§1 the generator repo, §2 the data-operator repo).

## Zero-Human ruleset

- All updates by pull request. The `ci` check is required before merge.
- Merges are deployer-only.
- **Registration = the repo exists with CI green on `main`.** Nothing is
  registered in a ledger; one git read of a repo at its pinned sha returns
  the identity, the contract and the code.
- No docket store, no docket runtime, no board registration, no crossfleet
  credential — the docket concept is killed (tercen/sarno PR #345 rev 3).

## Fork-rename flow (tercen/mcr#6, tercen/sarno#344)

1. Fork this template, renamed to the generator name, in the `tercen/`
   namespace, **public** (D1(a): a private fork would force every read
   against it private).
2. Fill the skeleton by PR, `ci` required: `identity.json` (values filled,
   `protocol_version` naming the repo's protocol — `"lens/1"` for the lens
   repos), `operator_spec.json`, `code.py`, `folds/`, `fixtures/`,
   `Dockerfile`.
3. CI green on `main` = registered.

Fork descent generalizes: a new generator may fork any existing generator
repo, not only this template (tercen/mcr#7); the template is the seed node.

## The machinery this template carries

- `identity.json` — the generator-identity field list as the contract
  (tercen/osanwe#899: harness, model, effort, provider, endpoint, protocol
  version; plus the repo-side fields: generator name, repo origin, pinned
  repo sha, code hash, language, isolation intent). The template instance
  carries nulls; a fork fills them and pins the sha it answers for.
- `.github/workflows/ci.yml` — the build check on every push/PR. At
  template stage the running checks are the identity check plus the
  contract and determinacy checks reporting their template-stage skips.
  When a fork adds `operator_spec.json`, `code.py` and `fixtures/`, the
  same workflow gates them: the contract check (parse, D8-formed kinds —
  zero indexes and unclosed brackets refused) and the determinacy check
  (the fold run twice on the same fixture, byte-equal).
- Skeleton: `folds/`, `fixtures/`.

## The typed contracts (sarno side, shipped code)

The contract shape is sarno's machinery, already in shipped sarno code —
`GeneratorSpec`, `validate_view_roles`, `derive_output_schema` (lens
operator plan §5). A fork's `operator_spec.json` is the `GeneratorSpec`
JSON: the input port's `RoleSpec`s in D8 kind forms (`measurement`,
`measurement[2]`, …; a named slot renders `kind:name`), the output port's
`cardinality` and `preserved_roles`, and the lens spec's own fields
(`class_set`, `scope`, `fold`) as declared properties — the `fold`
property a name plus content hash, `<name>@<content-hash>`, resolved
against the repo's own `folds/` table.
