# ISARN Development Rules

These rules apply to all Codex work in this repository.

## Authority and baseline
- Verify the live authoritative branch/state and inspect the actual baseline before modification.
- Treat the current authoritative working state and descendants as development state; never silently revert to an older ZIP/commit/artifact.
- Existing ISARN documentation, checklists, and repository-specific requirements are binding.

## Platform and verification
- Java 25; Paper 26.3 Build #8 `26.3-8-dev/26.3@c5c8f6c`; API `26.3.build.8-alpha`.
- Never guess APIs, dependencies, compatibility, configuration, behavior, or runtime results. Verify source/docs/config/build/runtime evidence or report unresolved.

## Required workflow
Every modification is a new release iteration:
`BASELINE -> STRUCTURAL/TARGET AUDIT -> BUILD -> FULL AUDIT -> FIX -> BUILD -> RE-AUDIT -> REPEAT UNTIL CLEAN -> FINAL COMMITTED-SOURCE BUILD -> FINAL AUDIT -> PACKAGE`.
A failed iteration remains current development state; do not bypass it by reverting.
Audit source/build files, imports/compile pathways, dependencies, Java/Paper compatibility, metadata, configuration, requested/affected systems, stale/duplicate/dead/conflicting code, regressions, runtime/API risks, version consistency, and security where applicable.

## Release gate
- BUILD VERIFIED requires an actual successful required build; CI alone is insufficient.
- Final committed source must receive the final clean build and full audit.
- Verify expected final JAR contents/integrity/metadata/version; package that exact JAR; verify ZIP contents/integrity/checksums where applicable and artifact-to-source correspondence.
- Report applicable gate items as PASS/N/A/BLOCKED. Any unresolved required item = GATE FAILED.
- Automated evidence does not replace required human gameplay/runtime validation.

## Git safety and autonomy
Codex may autonomously edit and run builds/tests/audits for an explicitly assigned task. Inspect status/diff and exclude unrelated changes. Do not merge, rebase, reset, force-push, discard authoritative work, modify upstream, deploy production, bypass gates, or push without explicit task/controller authorization.

## ChatGPT synchronization
When using the ISARN ChatGPT <-> Codex channel, follow the scoped task and report findings, build/audit evidence, changed files, final Git state, artifact identity, unresolved issues, and human validation requirements. Codex PASS remains subject to independent ChatGPT Release Gate verification.
