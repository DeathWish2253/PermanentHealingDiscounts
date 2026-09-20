# Codex Execution Contract Standard

This document is a mandatory compliance standard for all ISARN Codex tasks in this repository.

It applies in addition to:
- the repository-root `AGENTS.md`;
- the active GitHub issue/task contract;
- project-level ISARN development and Release Gate requirements;
- any plugin-specific checklist or authoritative baseline requirements.

If any instruction conflicts, stop and report **BLOCKED** rather than guessing or silently weakening a stricter requirement.

## 1. Core execution rule

Codex is a scarce local execution resource. ChatGPT is the primary analyst, researcher, planner, reviewer, and Release Gate authority.

Codex should primarily:
- edit local source;
- run Gradle/build/tests;
- collect local compiler/NMS/runtime evidence unavailable to ChatGPT;
- generate and verify artifacts;
- perform authorized Git operations.

Codex should not spend substantial context on broad investigations that ChatGPT can perform first.

## 2. One issue per chat

- One GitHub issue = one fresh Codex chat.
- Target a maximum of **250,000 context tokens per issue/chat**.
- Do not continue unrelated issues in the same chat.
- If the task cannot be completed safely within the scoped issue/context ceiling, stop and report:
  **BLOCKED / NEEDS NEW ISSUE**.

## 3. Mandatory startup verification

Before editing, building, testing, packaging, or committing:

1. Read the active GitHub issue in full.
2. Read repository-root `AGENTS.md`.
3. Read this `Codex-Execution-Contract-Standard.md`.
4. Verify authenticated GitHub access.
5. Verify actual Git root.
6. Verify authoritative branch.
7. Verify local HEAD.
8. Verify `origin` remotes.
9. Run `git fetch origin`.
10. Verify current remote authoritative HEAD.
11. Check `git status --short`.
12. Confirm the authoritative baseline required by the issue.

Live repository state overrides stale issue SHAs, stale documentation, and old local assumptions unless the issue explicitly states otherwise.

If a required baseline fact cannot be verified, report **BLOCKED**.

## 4. No guessing

Never guess:
- APIs;
- dependency behavior;
- compatibility;
- source state;
- build state;
- artifact state;
- runtime results;
- test results;
- Git state.

Verify each required fact or mark it **BLOCKED**.

## 5. Preserve authoritative work

Do not:
- reset;
- rebase;
- force-push;
- discard authoritative work;
- overwrite newer remote work;
- modify upstream;
- deploy;
- alter secrets;
- bypass a failed state;
- revert to a prior build solely to evade a discovered failure,

unless explicitly authorized.

A failed build/correction state becomes the current state to fix forward.

## 6. Development efficiency loop

Use the smallest useful execution loop:

1. Capture a broad failure set once when needed.
2. Cluster failures by root cause.
3. Fix shared root causes, not symptoms one-by-one unless independent.
4. Use incremental builds and targeted tests during correction.
5. Run broader affected-subsystem tests after a cluster passes.
6. Avoid repeated clean/full builds during intermediate fixes unless necessary for diagnosis.
7. Do not re-audit unchanged baseline facts.
8. Prefer machine-readable evidence for hashes, manifests, Git state, failure summaries, artifact inspection, and metadata.
9. Keep interim reports compact.

Efficiency never weakens final Release Gate requirements.

## 7. Build and audit sequence

Required development sequence:

`BASELINE → IMPLEMENT/FIX → BUILD → AUDIT → FIX → REBUILD → RE-AUDIT until clean`

Targeted checks are allowed between fixes.

Final audit must cover all affected areas, including where applicable:
- regressions;
- stale/duplicate/dead/conflicting code;
- imports and compile pathways;
- dependency conflicts;
- metadata/version;
- API/runtime compatibility;
- config/UI/behavior;
- functionality preservation;
- security;
- plugin-specific integration requirements.

CI success does not equal Release Gate approval.

## 8. Final committed-source gate

Before task completion, final committed source must receive all required final verification.

Where the issue requires an artifact, verify:
- final clean build from final committed source;
- all discovered issues fixed or explicitly BLOCKED;
- expected JAR;
- JAR contents/integrity;
- metadata/version;
- SHA-256 checksum;
- artifact ↔ final source/build correspondence;
- exact artifact path/name;
- release ZIP and ZIP integrity if required.

Only package the exact JAR from the final successful build of final committed source.

Do not claim a build PASS from a pre-fix, pre-commit, or stale artifact.

## 9. Git / cross-computer handoff

Every issue/task must end in a remotely reproducible handoff state.

Codex must:
- commit all task-relevant source/tests/config/scripts/docs/evidence;
- push all task-relevant commits to the authorized remote branch;
- fetch remote state after push;
- verify local HEAD == remote authoritative HEAD;
- record final branch and remote HEAD SHA;
- leave no task-relevant local-only uncommitted/untracked work;
- confirm another authorized computer can fetch/pull and continue exactly from that state.

Reproducible caches, build directories, and IDE state do not need to be uploaded unless required as evidence.

If any task-relevant work cannot be preserved remotely, report **BLOCKED**.

## 10. Evidence/reporting

Every required gate item must be classified as:
- **PASS**
- **N/A**
- **BLOCKED**

Any unresolved required item means the task is not complete.

The final Codex report must be compact and include, as applicable:
- actual starting branch/SHA;
- files changed;
- targeted validation results;
- final build result;
- artifact path/hash/size;
- metadata/integrity result;
- final branch;
- final local HEAD;
- final remote HEAD;
- final `git status --short`;
- remaining manual/runtime steps;
- every unresolved blocker.

Codex must not claim overall ISARN Release Gate approval. ChatGPT independently reviews the pushed source, evidence, build/artifact correspondence, and handoff.

## 11. Stop conditions

Stop and report **BLOCKED** rather than guessing when:
- GitHub authentication/access is unavailable;
- repository/root/branch/baseline cannot be verified;
- required source or authoritative asset is missing;
- required API behavior cannot be verified;
- implementation cannot preserve required behavior;
- targeted/final build fails and cannot be resolved within scope;
- final artifact verification fails;
- push fails;
- local and remote HEAD do not match;
- task-relevant work remains local-only;
- continuing would exceed the issue scope or the 250k-token chat ceiling.

## 12. ChatGPT handoff model

Default workflow:

`ChatGPT diagnose/research → scope smallest useful GitHub issue → fresh Codex chat → Codex edit/build/test → push all relevant work → ChatGPT independently review → repeat only as needed → final Release Gate`

This document is binding for every Codex assignment, correction cycle, validation cycle, evidence report, and cross-computer handoff in this repository.
