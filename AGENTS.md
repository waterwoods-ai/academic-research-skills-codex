# Security Track Overlay (user-owned, dev branch)

This fork adapts the academic-research suite for **security-conference
research** (CPS security, IoT security, AI/ML security). The upstream skill
assumes ML/journal conventions; the rules below override those defaults.

## Default assumption

Unless the user says otherwise, every paper task targets a security venue:
the Big 4 (IEEE S&P, NDSS, ACM CCS, USENIX Security) or a tier-2 venue from
the tracked list. If a task is explicitly NOT security research, ignore this
overlay and use stock behavior.

## Required reading before paper work

Before planning, outlining, drafting, reviewing, or revising a paper, read
the relevant files from `skills/security-track/references/`:

| Task | Read first |
|---|---|
| Venue choice, submission planning | `big4_venue_profiles.md` + `deadlines_current.md` |
| Writing / outlining / revising | `security_paper_conventions.md` |
| Peer-review simulation | `security_reviewer_personas.md` |
| Ranking / tier questions | `conference_ranking_2025.json` |

## Overrides of stock defaults

- **Citations:** IEEE/ACM numeric style, NOT APA 7.0. Two-column conference
  LaTeX, hard page limits — not journal word counts.
- **Structure:** Intro / Threat Model / Design / Implementation / Eval /
  Discussion / Related Work / Ethics — not IMRaD. A paper without an
  explicit threat-model section is incomplete.
- **Reviewer simulation:** use the five personas in
  `security_reviewer_personas.md` (PC Chair, CPS, IoT/embedded,
  Adversarial-ML, threat-model skeptic), the verdict vocabulary
  Accept / Minor revision / Major revision (numbered binding criteria) /
  Reject, and the eight rejection anchors in that file.

## Deadlines are never recalled from memory

Quote deadlines ONLY from `skills/security-track/references/deadlines_current.md`.
If its `Fetched` timestamp is older than 7 days, refresh first:
`python3 skills/security-track/scripts/fetch_deadlines.py`. If the fetch fails,
say the calendar is stale — do not fill in dates from model memory.

## Repo conventions (fork hygiene)

- `main` mirrors upstream (ff-only); ALL personal work goes on `dev`.
- Customizations are additive only: the `skills/security-track/` skill + its `plugins/ars-codex/skills/security-track/` mirror (or this file).
  Never edit upstream-owned files — that is what keeps `git sync-upstream`
  conflict-free.
