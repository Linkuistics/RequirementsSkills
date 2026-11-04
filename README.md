# RequirementsSkills

RequirementsSkills is the requirements pack of
[Grove](https://github.com/Linkuistics/grove). It ships Grove's requirements
session kinds as agent skills. Grove's core supplies the spine that each kind
loads.

## Kinds

| Kind | Skill | Purpose |
| --- | --- | --- |
| `requirements` | `grove-requirements` | Establishes what to build, in the human's own words, and decides whether the open questions need a full interview |
| `review-requirements` | `grove-review-requirements` | Reads one requirements artifact adversarially and writes findings, not fixes |
| `integrate-review-requirements` | `grove-integrate-review-requirements` | Triages the findings of one `review-requirements` leaf and applies the real ones |

`grove-requirements/grilling.md` holds the interview procedure. It is a
resource of the `grove-requirements` skill, not a kind.

The review and integration kinds belong to families whose members ship in
several packs. Their family procedures therefore stay in Grove's spine, as
`references/review.md` and `references/integrate-review.md`. Each of the two
kinds loads the `grove` skill and then reads its family file there.

The ADR, spec and glossary formats also stay in Grove's core, in the spine's
`ADR-FORMAT.md`, `SPEC-FORMAT.md` and `CONTEXT-FORMAT.md`. The moved text names
those files, and each route resolves them in the `grove` skill.

## Layout

The source directory is `plugins/requirements`. It is the Claude Code plugin
root and the Ravel source directory. Its only root is `skills`, which holds the
three kind skills. The repository root holds the files that the pack does not
deliver: this README, the licence and the Taskfile.

Grove's external pack contract, `docs/specs/external-pack-contract.md` in the
Grove repository, states the layout, the references between skills and the
conformance rules.

## Install

Each route also needs Grove's core, which supplies the `grove` skill.

### Ravel

Add this record to the `skills.sources` list of the Ravel settings, beside the
record of Grove's core. Replace `<checkout>` with the absolute path of a
checkout of this repository:

```json
{ "id": "requirements", "type": "local", "path": "<checkout>/plugins/requirements", "roots": ["skills"] }
```

Ravel reads the checkout as a local directory. It does not fetch or update the
checkout. A tool outside Ravel holds the checkout at a published commit.

### Claude Code plugin

Install the plugin from Grove's marketplace:

```text
/plugin marketplace add Linkuistics/grove
/plugin install requirements@linkuistics
```

The manifest declares the dependency `grove`, so the installation also adds
Grove's core. The skills then have the namespace `requirements:`, for example
`requirements:grove-requirements`.

### Manual installation

Grove's manual installer links the skills of a pack:

```sh
<grove checkout>/plugins/install.sh --pack <requirements checkout>/plugins/requirements
```

Grove's marketplace entry for this pack arrives with the Grove release that
removes the requirements kinds from core. Until that release, Grove ships its
own copies of these kinds.

## Checks

Run every check of the pack:

```sh
task check
```

The checks use Grove's conformance runner from a plain Git checkout of Grove,
under `.cache/grove/`. The `Taskfile.yml` variable `GROVE_COMMIT` names that
Grove commit. The variable `GROVE_REPOSITORY` names the public Grove
repository.

- `task check:conformance` checks the pack alone, then the aggregate of
  Grove's core and this pack.
- `task check:plugin` checks the Claude Code plugin with
  `claude plugin validate`. It skips when Claude Code is not installed.

`GROVE_COMMIT` names Grove commit `302f85429e4b8f8ada02be594687155109dfbf6b`,
which removed Grove's own copies of these skills. The pack passes alone, and
the aggregate reads each requirements skill and each moved rule once.

## Provenance

The skills come from Grove commit `6cdab3a9883342e68fe51b3913d8c72ddb26e3bf`,
at these paths:

- `plugins/grove/skills/grove-requirements/`, with `SKILL.md` and
  `grilling.md`
- `plugins/grove/skills/grove-review-requirements/`
- `plugins/grove/skills/grove-integrate-review-requirements/`

Grove keeps the full history of these files. This repository imports none of
it. The move changed the opening paragraph of each kind skill and changed no
other text.

Grove commit `302f85429e4b8f8ada02be594687155109dfbf6b` removed Grove's copies
of these files. `GROVE_COMMIT` names that commit, so the aggregate check reads
each requirements skill once.

`grove-requirements` adapts and bundles passages of
[mattpocock/skills](https://github.com/mattpocock/skills), which uses the MIT
licence. `grove-requirements/LICENSES/mattpocock-skills.LICENSE` holds that
licence text. It is a copy of the file in Grove's spine, so the skill's
pointers resolve inside the skill.

## Licence

RequirementsSkills uses the Apache License 2.0, as Grove does. The `LICENSE`
file holds the full text.
