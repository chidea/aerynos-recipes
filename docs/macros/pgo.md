# pgo macros

Profile-guided / BOLT optimization.

| Macro            | Description                                          |
| ---------------- | ---------------------------------------------------- |
| `%llvm_merge_s1` | Merge LLVM profile data (to `combined.profdata` if no stage2) |
| `%llvm_merge_s2` | Merge LLVM profile data after stage2 PGO workload    |
| `%bolt_instr`    | Instrument a file with llvm-bolt                     |
| `%bolt_merge`    | Instrument a file with llvm-bolt                     |
| `%bolt_opt`      | Apply a BOLT profile                                 |
| `%bolt_perf`     | Collect perf data suitable for the bolt macros       |
| `%bolt_perf2bolt`| Convert perf data into BOLT equivalent               |
