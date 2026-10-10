# ISE-A2 | Domain Model and Testable Specification

**Course:** Intelligent Software Engineering  
**Project:** Assignment Deadline Change Tracking and Verification System  
**Due:** 12 October 2026, 16:30 (Asia/Shanghai)  
**Scope:** the student-owned deadline verification workflow defined in ISE-A1.

## Purpose

This repository formalizes the A1 normal, boundary, and abuse scenarios into domain vocabulary, a context map, an end-to-end process, a candidate state machine, a field-level data dictionary, five invariants, a nine-rule decision table, and an independent automated test suite. **This is a domain-specification exercise, not a deployed LMS integration.**

## What to read and submit

| Core file | Purpose |
|---|---|
| `ISE-A2-Final-Report.pdf` | Main five-page academic deliverable; glossary, diagrams, rules, governance and evidence summary |
| `ISE-A2-Final-Report.docx` | Editable report source |
| `Domain-Data-Dictionary-and-Rules.xlsx` | Four sheets: Glossary, Data Dictionary (31 fields), Rules (9 rows), Invariants (5) |
| `diagrams/context.svg` | Bounded context and information authority |
| `diagrams/process.svg` | Event/process flow with normal, blocked, invalid and rejected paths |
| `diagrams/state.svg` | Candidate states and permitted transitions |
| `src/domain.py` | Deterministic, in-memory reference implementation |
| `tests/test_domain.py` | Independently checkable positive/negative tests, including one expected failure |
| `data/synthetic-fixtures.json` | Frozen synthetic data only; no real student responses |
| `evidence/test-results.txt` | Actual output from the execution command below |
| `WALKTHROUGH.md` | Model walkthrough, observations, limitations, and reproduction |
| `PLATFORM-SUBMISSION.md` | 150–300-word result summary for learn.spaiq.ai |

No notebook is included because all verification is executable directly through Python's standard-library `unittest` framework. No screenshots or duplicate spreadsheet exports are needed.

## Reproduce

Run these commands from the repository's top-level directory:

```bash
python --version
python -m unittest discover -s tests -v
```

**Recorded environment:** CPython 3.13.5, Python standard library only, no pip dependencies, no network calls, no random inputs or seeds. The code is intended for Python 3.11+, but the saved verification run was performed using CPython 3.13.5. Operating-system-specific details do not affect the deterministic test fixtures. A seed is not applicable because no random sampling occurs.

**Dataset version:** `a2-synthetic-fixtures-v1`  
**Fixture SHA-256:** `49953c71f271df16c0d3f0085efebdecddec32caf120a5ddba0113005625de7a`

**Saved outcome:** 18 tests executed: 17 regular passing checks and 1 *expected failure*. The expected failure demonstrates that direct mutation of the public in-memory dictionary bypasses domain guards. It is deliberately retained as a counterexample, not counted as a successful security guarantee. The environment has no production authentication, persistent transactions, institutional-source verification or enacted deletion workflow.

## Source and privacy status

All example student, assignment and course IDs and all example notices are synthetic. The previous A1 investigation used simulated pilot profiles, which are **not** empirical interviews. No claim about real student behavior is inferred from these examples. `authority_verified: true` in the test fixture is a precondition assumed for a verified institutional source; this model does **not** independently establish source authenticity. Source SHA-256 is an integrity fingerprint, not proof of university origin. The tracked deadline is the student's personal record, never the university's official deadline.

For proposed retention and deletion rules, consult Section 7 of the report. They are design requirements rather than implemented services. Do not publish real LMS URLs, screenshots, emails or student identifiers to this repository.

## Stable platform submission

Commit the extracted repository to GitHub, create a release tag (for example `ise-a2-v1.0`), and submit a link to the **tag or commit** rather than an unpinned branch. Use `PLATFORM-SUBMISSION.md` as the 150–300-word result summary. No GitHub URL is fabricated here.
