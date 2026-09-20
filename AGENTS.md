# ISARN Codex Rules

These rules are the complete standing Codex instructions for this repository. The active GitHub issue supplies task-specific requirements.

## Scope and authority
- One GitHub issue = one fresh Codex chat. Work only that issue.
- ChatGPT performs broad research, diagnosis, architecture decisions, and Release Gate review. Codex performs the exact local edits, commands, builds, tests, evidence collection, and authorized Git operations requested by the issue.
- Do not broaden the task, invent additional audits, or investigate unrelated problems.
- Live remote repository state overrides stale local state, old ZIPs, old SHAs, and stale documentation.

## Startup
Before editing: read this file and the active issue; run `git fetch origin`; verify Git root, branch, local HEAD, remote HEAD, remotes, and `git status --short`. If the required baseline cannot be safely reconciled forward, report BLOCKED.

## Platform
- Java 25.
- Paper 26.3 Build #25.
- Paper API `26.3.build.25-alpha`.
- Never guess APIs, dependencies, compatibility, behavior, build results, runtime results, or artifact state. Verify or report BLOCKED.

## Efficient execution
- Follow the issue's exact files, behavior, commands, and stopping conditions.
- Prefer the smallest targeted compile/test first. Do not repeatedly run clean/full builds during correction work.
- **No open-ended debug/fix/retry loops.** On an unexpected failure not already covered by a deterministic issue instruction, capture the exact command, working directory, exit code, and relevant raw output once, then STOP and return it to ChatGPT for diagnosis.
- Do not perform speculative dependency changes, refactors, architecture changes, or alternative implementations to “see if they work.”
- If a configured local Minecraft test server is available, use it only for runtime scenarios explicitly requested by the issue; record exact Paper/plugin versions and relevant logs. Do not equate local runtime evidence with production-only validation.
- At meaningful checkpoints, report completed scope plus any usage/context meter values that are actually visible. Never invent or estimate unavailable usage.
- Target under 250k context tokens per issue/chat; if scope would materially exceed that, report BLOCKED / NEEDS NEW ISSUE.

## Validation and release
Every modification is a new release iteration. Use targeted validation during implementation. Once source is ready, perform the issue-required final clean build and final affected-subsystem audit exactly once unless a verified correction requires another.
For release work verify the exact final JAR, contents, metadata/version, integrity, SHA-256, and source/build correspondence; verify release ZIP when required.
Report required gate items as PASS / N/A / BLOCKED. Any unresolved required item blocks completion. Codex does not grant final ISARN Release Gate approval; ChatGPT independently reviews the pushed evidence.

## Git and handoff
Do not reset, rebase, force-push, discard authoritative work, modify upstream, deploy, alter secrets, or bypass gates unless explicitly authorized.
Keep unrelated changes out. At completion, commit/push all task-relevant source/tests/config/scripts/docs/evidence to the authorized branch, fetch remote state, verify local HEAD == remote HEAD, record the final SHA, and leave no task-relevant local-only work.

## IDE, runtime, and completion hardening
- Use available IntelliJ plugins/MCP/integration resources when they materially improve structured navigation, symbol search/usages, inspections, Gradle execution, debugger/runtime evidence, or test-server work. If an expected IntelliJ/MCP capability is unavailable or misconfigured, report the exact limitation instead of using blind/manual workarounds that broaden scope.
- Runtime-sensitive changes require issue-defined runtime validation when compile/static evidence cannot prove behavior. When local runtime validation is authorized, prefer the configured IntelliJ-integrated Minecraft test server when practical and record exact Java/Paper/plugin versions plus relevant evidence.
- Runtime Release Gate evidence must exercise the exact final artifact produced from the final committed build HEAD, not an intermediate or substitute artifact.
- If an exact gated artifact later fails an authorized runtime test, treat the failure as authoritative for the affected behavior: the prior PASS is superseded, preserve its historical evidence, and handle the correction as a new release iteration.
- After posting and verifying the detailed GitHub issue completion report, keep the final Codex chat response compact: final SHA, build result, runtime result when applicable, artifact path/size/SHA-256, GitHub report URL/comment ID, and BLOCKED status.
