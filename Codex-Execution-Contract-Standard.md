# ISARN ChatGPT → Codex Execution Contract

This document is binding on ChatGPT when preparing, reviewing, correcting, validating, or handing work to Codex for this repository. It is **not a standing Codex prompt**. Codex should normally read repository-root `AGENTS.md` plus the active GitHub issue.

## 1. Role split
ChatGPT is the primary analyst, researcher, planner, failure diagnostician, architecture owner, and ISARN Release Gate authority.

Codex is the local execution resource for:
- exact source edits;
- exact build/test/runtime commands;
- local compiler/NMS/runtime evidence unavailable to ChatGPT;
- artifact generation/inspection;
- authorized Git operations.

Do not outsource broad research, architecture selection, root-cause diagnosis, or open-ended investigation to Codex when ChatGPT can perform it first.

## 2. Before assigning Codex
ChatGPT must, as applicable:
- inspect the live GitHub branch/HEAD, relevant source/diffs/issues/evidence;
- consult binding repository/project requirements;
- verify APIs/dependencies/compatibility from authoritative sources;
- reuse verified unchanged facts rather than make Codex rediscover them;
- cluster known failures by root cause;
- define the smallest useful deterministic scope;
- identify exact files/methods/behavior, exact validation commands, runtime scenarios where required, and explicit stop conditions.

Redundant project steps may be omitted only when they repeat already-verified unchanged facts and the omission cannot weaken correctness or the Release Gate.

## 3. Task construction and Codex chat size
Put the detailed implementation contract in the GitHub issue whenever practical. Keep the Codex chat prompt short and operational: identify the private repository, issue number, required GitHub CLI access, repository-root `AGENTS.md`, and tell Codex to execute the issue exactly.

Do not duplicate a large GitHub issue body into Codex chat.

Before execution, tell Codex to:
1. verify GitHub CLI authentication with `gh auth status`;
2. use `gh repo clone OWNER/REPO` only if the private repository is not already present; otherwise verify `origin` and fetch;
3. read repository-root `AGENTS.md` and the active GitHub issue;
4. use available IntelliJ plugins/MCP/integration resources when they materially improve structured navigation, symbol search/usages, inspections, Gradle execution, debugger/runtime evidence, or test-server work;
5. if an expected IntelliJ/MCP capability is unavailable or misconfigured, report the exact limitation rather than burning time on blind/manual workarounds;
6. verify only the minimum live state needed for safe execution;
7. execute the exact issue scope and stopping conditions.

Prefer issue instructions that identify exact edits, preserved invariants, targeted checks, final checks, failure stopping conditions, and Git handoff.

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

A second attempt must be based on a concrete verified hypothesis, not another guess.

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

## 8. Runtime-gate rules
Runtime-sensitive changes require an explicit runtime gate whenever compile/static evidence cannot prove the requested behavior. This includes, where applicable, event behavior, scheduling, persistence/serialization, NMS/Paper runtime behavior, packet behavior, world/entity interactions, UI interaction behavior, and other mechanics whose correctness depends on server execution.

When an issue authorizes local runtime validation:
- prefer the configured IntelliJ-integrated Minecraft test server and available IntelliJ/MCP resources when practical, unless the issue specifies another environment;
- record exact Java/Paper/plugin versions and relevant logs/evidence;
- runtime Release Gate evidence must use the **exact final artifact produced from the final committed build HEAD**, never an intermediate or substitute build;
- verify the artifact hash/identity before or as part of runtime evidence.

Local runtime PASS is evidence for the tested environment; it does not automatically prove production-only integration when the production environment materially differs.

If an exact gated artifact later fails an authorized runtime test, that failure is authoritative. The previous PASS is superseded for the affected behavior, the artifact must not remain the current production-approved release for that behavior, and the correction is a **new release iteration**. Preserve the prior evidence/history; do not rewrite it as though it never passed the earlier gate.

## 9. Git / handoff
Codex may edit/build/test/audit/commit/push only within task authorization.

Never authorize merge of main, rebase, reset, force-push, discard of authoritative work, upstream modification, deployment, secret changes, or gate bypass unless the user explicitly authorizes it.

Every task must end in a remotely reproducible handoff:
- all task-relevant work committed/pushed;
- remote branch contains final commits;
- final branch and remote HEAD recorded;
- no task-relevant local-only work;
- another authorized computer can fetch and continue exactly.

## 10. GitHub reporting
Required gate items are PASS / N/A / BLOCKED. Any unresolved required item means GATE FAILED.

Every Codex task must end with a GitHub issue completion comment unless the issue explicitly says otherwise. ChatGPT must provide the exact `gh issue comment ISSUE --repo OWNER/REPO --body-file REPORT_FILE` command and required report fields in the issue.

The completion report should include, as applicable:
- starting SHA;
- files changed;
- targeted validation results;
- runtime validation results when required;
- final committed build HEAD;
- final clean-build result;
- artifact path/size/SHA-256;
- artifact ↔ committed HEAD confirmation;
- final local HEAD;
- final remote HEAD;
- final `git status --short`;
- `BLOCKED: none` or exact blockers.

Codex must verify the comment was actually posted, for example with `gh issue view ISSUE --repo OWNER/REPO --comments`.

After the detailed GitHub report is posted, Codex's final chat response should stay compact and normally contain only the final SHA, build result, runtime result when applicable, artifact identity/hash, GitHub report URL/comment ID, and BLOCKED status.

ChatGPT independently reviews the GitHub report, pushed source, build/runtime evidence, artifact correspondence, and handoff before accepting a task or release.

## 11. Full-state cross-PC handoff
For ISARN, **Cross-PC Handoff means a remotely recoverable snapshot of all meaningful current local repository state**, not merely completed source code.

Before ChatGPT or Codex may report Cross-PC Handoff PASS, Codex must inventory and reconcile the local repository against the authoritative remote, including:
- committed local commits not yet pushed;
- tracked modifications;
- untracked files;
- ignored files that may contain meaningful project state;
- generated artifacts such as JARs/ZIPs when they currently exist locally and may be needed to reproduce or continue the exact current state;
- incomplete/WIP implementation;
- local tests, scripts, configs, documentation, evidence, notes, patches, migration work, and task-related temporary outputs that contain meaningful state;
- IDE/project metadata when it carries project configuration, run/debug/test-server setup, or other state needed to continue on another authorized computer;
- local files that also exist remotely when the local copy is a newer or otherwise distinct iteration.

Do **not** assume a file is disposable because it is generated, ignored, machine-local, IDE-created, incomplete, or normally excluded from Git. Every such item must be explicitly classified.

Allowed classifications are:
- **REMOTE-PRESERVED** — committed/pushed or otherwise uploaded to the authoritative remote in a form another authorized computer can retrieve;
- **N/A-DISPOSABLE** — verified reproducible/cache-only/nonessential state whose absence cannot lose work or impede exact continuation;
- **BLOCKED-SENSITIVE** — secrets, credentials, tokens, private keys, or other sensitive machine/user data that must not be propagated; record the category and required secure re-provisioning path without exposing the secret;
- **BLOCKED** — meaningful state that cannot yet be preserved remotely.

A handoff may overwrite, replace, or create a new remote iteration for existing files when necessary to preserve the current local state. Completed status is irrelevant: unfinished/WIP work must also be preserved if it exists locally and is meaningful.

The handoff audit must inspect at minimum:
- `git status --short --ignored`;
- local branch/HEAD and remote tracking state;
- commits ahead/behind remote;
- tracked diff;
- untracked files;
- ignored files with an explicit meaningful-vs-disposable classification;
- locally present build/release artifacts and their remote availability when meaningful;
- IDE/project metadata relevant to continuation.

The practical acceptance test is:

> If the originating PC were wiped immediately after handoff, could another authorized PC sync/clone the authoritative remote and recover every meaningful piece of the current local plugin-development state needed to continue from exactly where work stopped?

If the answer is no, Cross-PC Handoff is **FAIL/BLOCKED**.

This rule is distinct from release-artifact publication. A downloadable public release asset may be a separate release requirement, but if a locally existing artifact is meaningful to the current working state, its preservation status must still be explicitly accounted for in the cross-PC handoff.

### Preservation default
Codex may not self-classify an existing local repository item as disposable merely because it is generated, reproducible, cached, ignored, IDE-created, or rebuildable. Every non-sensitive file or directory currently existing under the local repository root requires **REMOTE-PRESERVED** status unless the user explicitly authorizes exclusion of that specific item or class of items.

This includes current Gradle `build/` output, JARs, ZIPs, reports, generated resources, logs/evidence, IDE/project files, WIP, and incomplete iterations. Rebuildability is not a valid reason to omit an existing local artifact from the handoff.

`N/A-DISPOSABLE` may be used only when the user has explicitly authorized exclusion of that specific local item or class of items. Codex does not have authority to make that exclusion decision on its own.

