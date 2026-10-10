# Model Walkthrough Record

**Exercise:** ISE-A2. **Date:** 10 October 2026 (Asia/Shanghai).  
**Environment observed:** CPython 3.13.5. **Verification method:** standard-library `unittest`.  
**Input provenance:** dataset `a2-synthetic-fixtures-v1`, SHA-256 recorded in `README.md`. No live student data, interviews, university credentials or live platform connections.

## Model inspection decisions

The group of personally tracked assignment fields is the aggregate root `Assignment`. `EvidenceSource` links a candidate to a source reference, kind, a SHA-256 integrity value and an **externally established** `authority_verified` boolean. The source kind may be `OFFICIAL_LMS`, `INSTRUCTOR_EMAIL`, or an untrusted message. The model must not infer trust from a message's contents. `DeadlineValue` combines an ISO timestamp with explicit offset and an IANA time-zone identifier. A candidate is associated with its observed `base_version` so that two changes cannot silently overwrite one another.

## Executed scenario walkthrough

| ID | Input and action | Observation / independent oracle | Test evidence |
|---|---|---|---|
| W1 Normal | Start at 2026-10-10 16:30 +08:00, propose 2026-10-17 16:30 +08:00 from a synthetic verified LMS source, then owner confirms. | Candidate becomes `APPLIED`; one `HistoryEntry` records old and new values; assignment version is exactly 2. | `test_I3_versioned_history_and_single_current_deadline` |
| W2 Boundary | Propose two different dates based on version 1; apply the first before confirming the second. | Second confirmation raises `VERSION_CONFLICT`, and the first update remains current. | `test_I3_stale_candidate_rejected` |
| W3 Abuse | An untrusted chat source proposes a new deadline, even if source metadata tries to claim trust. | Confirmation raises `SOURCE_NOT_AUTHORIZED`, preserving the original deadline. | `test_I2_untrusted_kind_even_with_bad_verified_flag_is_blocked` |
| W4 Rejection | Owner chooses `REJECT` on a pending candidate. | Candidate becomes `REJECTED`; deadline/version/history remain unchanged; later confirmation is illegal. | `test_I4_rejected_candidate_no_mutation` and `test_I4_rejected_cannot_confirm_later` |
| W5 Temporal fault | Propose a missing-UTC-offset value or a UTC offset inconsistent with Asia/Shanghai. | `NAIVE_DATETIME` or `OFFSET_MISMATCH` is raised; no revision occurs. | `test_naive_datetime_rejected`, `test_time_zone_offset_mismatch_rejected` |
| W6 Deliberate challenge | Mutate `DeadlineStore.assignments` directly instead of using `review()`. | Invariant protection is bypassed; an **expected failing assertion** records the reference implementation's lack of encapsulated access control. | `test_documented_limitation_public_store_mutation` |

## Test result retained

Run `python -m unittest discover -s tests -v` and compare with `evidence/test-results.txt`.

```text
Ran 18 tests
OK (expected failures=1)
```

This means 17 regular tests passed and one designated `@unittest.expectedFailure` exhibited the documented limitation. It **does not** mean a security vulnerability was corrected or that tests establish production correctness. No negative results were discarded.

## Walkthrough review findings

1. **Authority boundary:** A student confirming a candidate changes only the *personal tracked record*. Authentication of an official university source is deliberately outside this reference model.
2. **State consistency:** Invalid same-value candidates become `INVALID`; malformed timestamps are rejected at validation; a failed confirmation leaves a candidate pending and the deadline unchanged.
3. **Version consistency:** A successful guarded update produces a single new version and history entry. Repeated confirmation of the already applied candidate is idempotent.
4. **Privacy:** Only source references, metadata and hashes are modeled. Raw academic documents and communication text are not stored in `EvidenceSource`. Real deployments must restrict sensitive source URLs.
5. **Residual limitation:** Direct dictionary access, verified-source flag spoofing upstream, concurrent persistent writes, deletion, and real authentication are not solved. These become follow-up implementation requirements rather than unreported guarantees.

## Governance walk-through

**Source:** authorized instructor/LMS notice; student-supplied document is only a candidate input until its authority is independently established. **Quality:** identifiers and source links must exist, deadlines must have consistent zone/offset, and the candidate must not equal the current deadline. **Privacy:** student ownership is required for review; fixture data are anonymous synthetic records. **Retention proposal:** source/candidate metadata until course end plus 30 days; revision history until course end plus 90 days, subject to institutional approval. **Deletion:** entire student workspace and derived entries must be removed through a deployed deletion workflow; none is implemented by this in-memory model, which disappears when the process ends.

The resulting decision is to continue to a bounded implementation only after adding protected persistence and a credible academic-source verification procedure. The A2 submission itself does not claim those operations already exist.
