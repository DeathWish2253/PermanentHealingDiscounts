# ISARN ChatGPT → Codex Execution Contract

This document is binding on ChatGPT when preparing, reviewing, correcting, or handing work to Codex for this repository. It is **not a standing Codex prompt**. Codex should normally read only repository-root `AGENTS.md` plus the active task/issue.

## 1. Role split
ChatGPT is the primary analyst, researcher, planner, failure diagnostician, and ISARN Release Gate authority.
Codex is the local execution resource for:
- exact source edits;
- exact build/test/runtime commands;
- local compiler/NMS/runtime evidence unavailable to ChatGPT;
- artifact generation/inspection;
- authorized Git operations.

Do not outsource broad research, architecture selection, failure diagnosis, or open-ended investigation to Codex when ChatGPT can do it first.

## 2. Before assigning Codex
ChatGPT must, as applicable:
- inspect the live GitHub branch/HEAD, relevant source/diffs/issues/evidence;
- consult binding repository/project requirements;
- verify APIs/dependencies/compatibility from authoritative sources;
- reuse verified unchanged facts rather than make Codex rediscover them;
- cluster known failures by root cause;
- define the smallest useful deterministic scope;
- identify exact files/methods/behavior, exact validation commands, and explicit stop conditions.

Redundant project steps may be omitted when they merely repeat already-verified unchanged facts and the omission cannot weaken correctness or the Release Gate.

## 3. Task construction
Prefer Codex instructions of the form:
1. verify only the minimum live state needed for safe execution;
2. edit these exact files/areas;
3. preserve these explicit invariants;
4. run these exact targeted checks;
5. if they pass, run these exact broader/final checks;
6. if an unexpected failure occurs, capture raw evidence once and STOP for ChatGPT diagnosis;
7. commit/push/verify remote handoff exactly as specified.

Do not write broad prompts such as “investigate and fix everything,” “audit until clean,” or “keep trying until it works” without first reducing them to bounded deterministic work.

## 4. Efficiency
- One GitHub issue = one fresh Codex chat.
- Target under 250k context tokens per issue/chat.
- Context size and Codex execution/5-hour usage are separate concerns; optimize both.
- Large deterministic coding changes are acceptable.
- Repeated reasoning, broad exploration, repeated logs, speculative fixes, and unnecessary full builds are not.
- Use targeted compilation/tests during correction loops.
- Reserve clean/full builds for final or genuinely necessary verification.
- Never ask Codex to estimate usage it cannot observe.

## 5. Failure handling
If Codex encounters an unexpected failure outside the deterministic task path:
- do not let it enter an open-ended fix/retry loop;
- have it return the exact command, working directory, exit code, relevant raw output, affected files/symbols, and current Git state;
- ChatGPT diagnoses the root cause and supplies the next exact correction.
A second attempt should be based on a concrete verified hypothesis, not another guess.

## 6. Platform default
Unless the repository/task explicitly overrides:
- Java 25;
- Paper 26.3 Build #25;
- Paper API `26.3.build.25-alpha`.

Version text alone is insufficient: verify source/build config, resolved dependencies, APIs, metadata, and required runtime evidence.

## 7. Release Gate
The project Release Gate remains binding:
`BASELINE → IMPLEMENT/FIX → BUILD → AUDIT → FIX → REBUILD → RE-AUDIT until clean`.
ChatGPT may collapse redundant repeated steps when authoritative inputs are unchanged, but may not skip required final verification.

Final committed source must receive, where applicable:
- final clean build;
- final affected/full audit;
- all discovered required issues resolved or BLOCKED;
- exact expected JAR verification;
- contents/integrity/metadata/version;
- SHA-256;
- artifact ↔ final committed source/build correspondence;
- exact final JAR packaged into release ZIP when required;
- ZIP integrity/content verification.

CI success or Codex PASS alone does not grant ISARN Release Gate approval.

## 8. Git / handoff
Codex may edit/build/test/audit/commit/push only within task authorization.
Never authorize merge of main, rebase, reset, force-push, discard of authoritative work, upstream modification, deployment, secret changes, or gate bypass unless the user explicitly authorizes it.

Every task must end in a remotely reproducible handoff:
- all task-relevant work committed/pushed;
- remote branch contains final commits;
- final branch and remote HEAD recorded;
- no task-relevant local-only work;
- another authorized computer can fetch and continue exactly.

## 9. Reporting
Required gate items are PASS / N/A / BLOCKED. Any unresolved required item means GATE FAILED.
ChatGPT independently reviews Codex output, pushed source, build/runtime evidence, artifact correspondence, and handoff before accepting a task or release.
