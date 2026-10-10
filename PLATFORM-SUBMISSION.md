# ISE-A2 Platform Result Summary

This assignment extends the ISE-A1 investigation of university assignment deadline changes into an explicit domain model and independently checkable specification. The modeled workflow begins with a student-maintained assignment deadline, records an attributable source, creates a change candidate, and permits a versioned update only after authorized student confirmation.

The submission defines the principal entities and value objects, a bounded context map, an event/process flow, a candidate state machine, and a field-level data dictionary containing 31 fields. Five invariants specify owner authorization, evidence provenance, version consistency, rejection safety, and unambiguous time-zone handling. A nine-row decision table formalizes the conditions required to apply, reject, block, or safely repeat a change.

A deterministic Python reference model and synthetic fixture dataset were checked with the standard-library test framework. The recorded run executed 18 tests, including 17 passing checks and one documented expected failure. The latter demonstrates that direct mutation of the public in-memory store bypasses model guards and remains a limitation. The walkthrough also covers normal confirmation, conflicting versions, untrusted sources, rejection, and invalid timestamps.

The report explicitly specifies authority, quality, privacy, retention, and deletion boundaries. The fixtures are synthetic; there is no live LMS integration, institutional authentication, persistent storage, or deployed deletion mechanism. The versioned repository contains the editable report, PDF, diagrams, workbook, source code, test fixtures, and reproducible evidence.
