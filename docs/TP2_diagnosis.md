# TP2 diagnosis (from the pipeline log only)

| Defect | Gate that failed | Name in the log | One-line cause |
|---|---|---|---|
| Test 1 | tests | tests/test_api.py::test_submitting_for_an_unknown_team_is_not_found | submit uses team.id when the lookup returned None; unknown team is not handled (expected 404) |
| Test 2 | tests | tests/test_scoring.py::test_last_solve_time_is_the_most_recent_not_the_last_added | last_solve_time returns the last record of the list (12:05) instead of the latest time (12:30) |
| Credential | secret scan | gitleaks: leaks found: 2 (no rule or file shown in the log) | a secret-like value is committed in the repository |
| Dependency | dependency audit | idna 3.6: PYSEC-2024-60, PYSEC-2026-215 | vulnerable pinned dependency; fixed in 3.7 and 3.15 |

How each test failed:

- Test 1: by exception (AttributeError: 'NoneType' object has no attribute 'id').
- Test 2: by assertion (AssertionError: 12:05 is not equal to 12:30).