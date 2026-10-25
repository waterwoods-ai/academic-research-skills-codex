# Security Track — Installation and Usage (Codex edition)

> Day-to-day manual (quick reference for every stage, plus the research loop S0–S8): [`HOWTO.md`](../../HOWTO.md)

> This fork adds a second skill, `security-track`, to the upstream ARS-Codex suite
> (`academic-research-suite`): an overlay for research aimed at top security
> conferences (the Big 4 and tier-2), calibrated for CPS, IoT and AI security.
> Since 2026-10-06 this repository is maintained as its own product: any file may be
> edited, and nothing is merged from upstream any more; upstream updates are reviewed
> and ported by hand (see `UPSTREAM.md` at the repository root).
> All work happens on the `dev` branch (the GitHub default branch is `dev`).

## What this skill contains

| File | Contents |
|---|---|
| `SKILL.md` | Triggers, and the rules that override the stock suite's defaults (citation style, paper structure, review simulation, double-blind, the deadline rule) |
| `references/big4_venue_profiles.md` | Big-4 venue profiles (checked item by item against the official CFPs, 2023–2027 editions) |
| `references/security_paper_conventions.md` | Security-paper writing conventions: the Threat Model section, the evaluation bar, responsible disclosure, the anonymization checklist |
| `references/security_reviewer_personas.md` | The five-person security reviewer panel and the standard rejection anchors |
| `references/major_revision_playbook.md` | Field manual for multi-round review at the Big 4: rebuttal / revision / re-review, mapped to the suite's modes |
| `references/perspective_retrieval_protocol.md` | Perspective-driven retrieval protocol (an adaptation of STORM's retrieval-side mechanism; opt-in) |
| `references/conference_ranking_2025.json` | Snapshot of the 22-conference CIF ranking (updated yearly) |
| `references/deadlines_current.md` | Deadline calendar (**generated file, do not edit by hand**) |
| `scripts/fetch_deadlines.py` | Fetches the deadline calendar (re-fetches automatically when it is older than 7 days, then syncs it to the plugins/ mirror) |
| `contracts/reviewer/security_full.json` | Sprint contract for security venues (blind pre-commitment rubric; validates against Schema 13.2) |
| `references/research_loop_protocol.md` | Research loop protocol S0–S8: gap → topic → method (novelty + contribution) → falsification experiments → execution → bounded improvement → paper |

This skill has two copies in the repository: `skills/security-track/` (the source,
used by the direct install) and `plugins/ars-codex/skills/security-track/` (the
plugin's materialized mirror). After editing the source, `cp` it to the mirror by
hand (the calendar file too).

## Installation (Codex)

As a plugin (recommended; installs the main suite and security-track together):

```bash
# If you installed the upstream ars-codex, remove it first
codex plugin marketplace add waterwoods-ai/academic-research-skills-codex --ref dev
codex plugin add ars-codex@ars-codex
```

In the Codex Desktop app: Plugins → add the repository
`https://github.com/waterwoods-ai/academic-research-skills-codex.git`
(the default branch is `dev`) → install **ARS-Codex**.

As direct skill installs (note that there are two to install):

```bash
python3 "$HOME/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py" \
  --repo waterwoods-ai/academic-research-skills-codex --ref dev \
  --path skills/academic-research-suite --method git

python3 "$HOME/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py" \
  --repo waterwoods-ai/academic-research-skills-codex --ref dev \
  --path skills/security-track --method git
```

Start a **new Codex session** after installing. To update: `codex plugin marketplace
upgrade ars-codex` (if the content has not changed, clear the cache to force it:
`rm -rf ~/.codex/plugins/cache/ars-codex`, then reinstall — the upstream version
number is not bumped, so as not to break conflict-free syncing), then run
`codex plugin add` again. For the direct installs, `rm -rf` the old directories
first, then run the installer again.

To verify, ask "What is the NDSS Major Revision process?": the agent should read
`major_revision_playbook.md` rather than answer from memory.

## Usage

> **About slash commands:** Codex CLI 0.147+ removed the custom slash-command channel
> (the custom prompts mechanism is deprecated and no longer loaded, and the plugin
> ships no commands); `/ars-*` is a Claude Code-only form. In Codex, use either of
> these two equivalent ways:
>
> 1. `$academic-research-suite ars-reviewer <paper>` — type `$` to get autocompletion
> 2. Send the bare alias text: `ars-reviewer <paper>` (the skill's trigger description
>    registers every ars-* alias; no slash and no command pop-up, the simplest way)

**Automatic activation:** it triggers on any paper task that involves a security
conference (conference names, threat model, CPS/IoT/AI security and the other
triggers). You can also call `$security-track` explicitly. When the user works in
security research, every paper task targets a security conference by default; say
explicitly that the work is not security research to fall back to the stock behaviour.

| What you want to do | What the overlay adds |
|---|---|
| Choose a venue / plan the submission | venue profiles + the live deadline calendar |
| Literature review (say "make sure the coverage is comprehensive" to trigger the perspective retrieval protocol) | six-lens query expansion + follow-up on unused searches |
| Writing / outline | security-paper structure, numeric citations, the double-blind checklist |
| Simulated review | the five-person security panel + the target venue's exact decision vocabulary |
| Responding to reviews / checking a rebuttal | a roadmap, response-package structure and hard-rule audit for the venue and decision tier |

**Deadline rule:** quote dates only from `references/deadlines_current.md`. If it is
older than 7 days, first run `python3 skills/security-track/scripts/fetch_deadlines.py`;
if the fetch fails, say clearly that the calendar is stale, and never give dates from
model memory.

## Maintenance (fork workflow)

```bash
git upstream-review   # fetch upstream and list the changes and files since the last review (no merge)
```

Upstream changes are reviewed one by one and ported by hand; the outcome is logged in
`UPSTREAM.md`, and changes are synced to the plugins/ mirror. The venue profiles pin
structural facts; page limits and the number of review rounds drift, so check the
current year's CFP close to submission.
