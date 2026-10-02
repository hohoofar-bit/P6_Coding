# EVMbench full pass — coding results (scheme v1.1)

Output of `notebook/evmbench_full_pass.ipynb`. All 117 tasks were coded in skeleton order (i = 0 … 116). The corpus was pinned to
`openai/frontier-evals@51052cede8cc608f95bb00346635e03759013e5a`.

## Files

| file | contents |
|---|---|
| `coding_log_full.csv` | the log: 117 first-look rows and 82 second-look rows (199 rows) |
| `evmbench_coding_skeleton.csv` | the cell 3 skeleton (117 rows, coder columns empty) |
| `cell8_progress.txt` | cell 8 output on the final log |

## Summary (cell 8)

- coded: 117 / 117
- levels: 1 = 40, 2 = 66, 3 = 8, U = 3. **U = 3 is under the threshold of 12**, so the scheme does not need revision.
- low confidence: 81
- second-look changes: 0 of 82

## Assumptions to review

The scheme v1.1 document was not available, only the prompts inside the notebook. These points are therefore my readings and should be checked against the scheme:

- **Step 3 gate (level 1).** I answered "yes" only when the write-up points to an expression that contradicts something the code itself establishes: a sibling path, a nearby variable or unit, a type, or a callee parameter name. Code comments never counted. An omitted check or state update counted as level 1 only when the write-up names a sibling path that does it (for example task 50, where `borrow` lacks the solvency check that `repay` has). Otherwise it went to step 4.
- **Level 3.** I assigned level 3 only when the write-up quotes a project document with a location: an audit README invariant, the project docs, or the whitepaper. Wardens' paraphrases and judges' references to unquoted docs did not qualify. Documentation of a dependency did not qualify either, with one exception: in task 15 the Seaport docs state a requirement on zone contracts, and the audited `Create` contract is such a zone.
- **Rules invoked.** The notebook names only B3, B4, B5, B6 and B8. I applied them as follows:
  - B3: overflow, rounding, precision or decimals;
  - B4: access control;
  - B5: oracle, price, TVL or NAV valuation;
  - B8: a shared root cause within the same audit, which also sets `root_cause_group`.

  B6 is the procedure itself, so it is not logged per task. Every task that invokes a B rule is set to `confidence = low`, as the cell 6 prompt requires. That is why low confidence is high (81).
- **Second-look rows.** Cell 7 shows a second look for every task in an audit that has `patch/` or `exploit/`, even when that audit has no diff for the task in question. Those rows say so in `coding_notes`.
- **Root-cause group added late.** `2024-12-secondswap::H-01` shares a root cause with H-02. I only noticed after H-01 had been logged. Its first row was left unedited, so the group is recorded on H-02 only, with a note.
