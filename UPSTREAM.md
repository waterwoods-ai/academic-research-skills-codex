# Upstream

This repository began as a fork of Cheng-I Wu's ARS-Codex (https://github.com/Imbad0202/academic-research-skills-codex, CC-BY-NC 4.0).
Since 2026-10-06 it is maintained as its own product (Tom's decision): the
security track is part of the suite, any file may be edited, and upstream is
a reference to learn from, not a source to merge.

- **Last reviewed upstream commit: 70b412f** (ARS-Codex aligned with ARS v3.22.2, 2026-09-28)
- Upstream remote: `upstream` → https://github.com/Imbad0202/academic-research-skills-codex.git
- `main` stays frozen at the last merged upstream commit as a comparison
  baseline; all work is on `dev`.

## Reviewing an upstream update

1. Run `git upstream-review` on `dev`. It fetches upstream and lists the
   commits and changed files since the last reviewed commit. Nothing is merged.
2. Read the upstream changelog for those commits. Decide each change:
   **PORT** (take as is), **ADAPT** (take, changed for this suite) or **SKIP**
   (with the reason).
3. Port on `dev` by hand, or with `git cherry-pick -x <id>` /
   `git diff <old> <new> -- <path> | git apply`. Then run the checks
   (`bash skills/security-track/tests/run_all_checks.sh`, and keep `skills/<name>/` and `plugins/ars-codex/skills/<name>/` identical).
4. Add a row to the review log below and move "Last reviewed upstream commit"
   to the newest upstream change you reviewed.

## License

Upstream is CC-BY-NC 4.0. Keep its attribution (LICENSE, NOTICE, citation
file, author credits) and keep the work non-commercial.

## Review log

| Date | Upstream range | Outcome |
|---|---|---|
| 2026-09-30 | `a43aaab..70b412f` (6 upstream changes) | Merged in full: the last merge before the 2026-10-06 policy change |
