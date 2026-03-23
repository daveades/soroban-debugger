# GitHub Issue Drafts

These are ready to turn into GitHub issues for `Timi16/soroban-debugger` after `gh auth login`.

Current implemented baseline:
- `repl`, `symbolic`, `analyze`, and `scenario` now run
- VS Code function-entry breakpoints work
- true source-line debugging is still not implemented

## 1. True source-line breakpoints from DWARF/source maps
**Title**: Implement exact source-line breakpoints in the debugger backend
**Labels**: enhancement, debugger, vscode
**Summary**: Replace function-entry breakpoint mapping with exact executable source-line stops.
**Acceptance criteria**:
- Clicking an executable Rust line pauses on that exact line.
- Non-executable lines are reported as unverified with a reason.

## 2. Source-level `next`
**Title**: Implement source-level step over
**Labels**: enhancement, debugger, vscode
**Summary**: `next` should move to the next source line instead of resuming whole-function execution.
**Acceptance criteria**:
- `next` skips over nested calls at the current frame.
- VS Code receives updated stack/source location data.

## 3. Source-level `step in`
**Title**: Implement source-level step in
**Labels**: enhancement, debugger, vscode
**Summary**: Add true line-based step-in behavior for contract execution.
**Acceptance criteria**:
- `step in` enters nested calls when a call site is stepped.
- The reported frame location changes to the callee source line.

## 4. Source-level `step out`
**Title**: Implement source-level step out
**Labels**: enhancement, debugger, vscode
**Summary**: Add frame-aware step-out behavior in the backend and adapter.
**Acceptance criteria**:
- `step out` resumes to the caller line after the current frame returns.
- The adapter updates the active frame correctly.

## 5. Stack frame source locations
**Title**: Expose real source locations in stack frames
**Labels**: enhancement, debugger, vscode
**Summary**: Populate frames with file/line metadata instead of generic function names only.
**Acceptance criteria**:
- Stack frames include source path and line for each paused frame.
- Frames are stable across continue/step operations.

## 6. Variable inspection at pause points
**Title**: Add locals and arguments inspection for paused executions
**Labels**: enhancement, debugger, vscode
**Summary**: Surface variables/arguments in the Debug Variables pane.
**Acceptance criteria**:
- Arguments are visible when paused.
- Locals are shown where recoverable from debug info.

## 7. Evaluate expressions in the debug console
**Title**: Support debug-console expression evaluation
**Labels**: enhancement, debugger, vscode
**Summary**: Let users inspect simple variables and contract state expressions from VS Code.
**Acceptance criteria**:
- Debug console requests return evaluated values or clear unsupported errors.
- Evaluation works while paused.

## 8. Conditional breakpoints
**Title**: Implement conditional breakpoint expressions
**Labels**: enhancement, debugger
**Summary**: The breakpoint manager currently reports conditional breakpoints as unsupported.
**Acceptance criteria**:
- Users can attach a condition string to a breakpoint.
- Unsupported conditions are rejected with explicit errors.

## 9. Hit-count breakpoints
**Title**: Add hit-count breakpoint support
**Labels**: enhancement, debugger, vscode
**Summary**: Support pausing only after a breakpoint has been reached N times.
**Acceptance criteria**:
- Breakpoints can be configured with hit counts.
- Counters reset predictably on a new session.

## 10. Logpoints
**Title**: Support logpoints without pausing execution
**Labels**: enhancement, debugger, vscode
**Summary**: Let VS Code emit messages at locations without stopping the program.
**Acceptance criteria**:
- Logpoints print formatted messages during execution.
- Execution does not pause when a logpoint is hit.

## 11. Persist breakpoint resolution cache
**Title**: Cache source-to-function breakpoint resolution in the extension
**Labels**: enhancement, vscode
**Summary**: Avoid reparsing source files on every breakpoint update.
**Acceptance criteria**:
- Repeated breakpoint changes reuse cached function ranges when files are unchanged.
- Cache invalidates on file modifications.

## 12. Load network snapshots inside the REPL
**Title**: Apply `--network-snapshot` in REPL sessions
**Labels**: enhancement, repl
**Summary**: `ReplConfig` accepts snapshots, but the REPL executor does not apply them yet.
**Acceptance criteria**:
- REPL sessions can preload snapshot state.
- Snapshot load failures are surfaced before the prompt starts.

## 13. REPL export/function discovery command
**Title**: Add `functions` command to the REPL
**Labels**: enhancement, repl
**Summary**: Expose contract exports/signatures directly from the REPL.
**Acceptance criteria**:
- `functions` lists callable exports and parameter types.
- Output is readable with and without color.

## 14. REPL tab completion
**Title**: Add function and command completion in the REPL
**Labels**: enhancement, repl
**Summary**: Improve usability with completion for built-in commands and contract functions.
**Acceptance criteria**:
- Tab completes `call`, `storage`, `history`, and known contract functions.
- Completion does not break non-interactive input.

## 15. REPL typed argument hints
**Title**: Show typed argument hints in REPL help and errors
**Labels**: enhancement, repl
**Summary**: Surface expected parameter types when a REPL call fails or help is requested.
**Acceptance criteria**:
- Help includes function signatures.
- Invalid REPL calls mention expected argument shapes when known.

## 16. Persist REPL address aliases
**Title**: Persist generated REPL address aliases across sessions
**Labels**: enhancement, repl
**Summary**: Generated aliases currently exist only for one REPL session.
**Acceptance criteria**:
- Aliases can be saved and reused across sessions.
- Users can clear persisted aliases explicitly.

## 17. REPL storage mutation helpers
**Title**: Add storage set/delete helpers to the REPL
**Labels**: enhancement, repl
**Summary**: Add controlled commands to seed or modify storage during exploration.
**Acceptance criteria**:
- Users can set and delete storage keys from the REPL.
- Mutations are visible through the existing storage inspection flow.

## 18. Type-aware symbolic input generation
**Title**: Make symbolic input generation type-aware
**Labels**: enhancement, symbolic
**Summary**: The symbolic analyzer currently generates generic numeric combinations.
**Acceptance criteria**:
- Address, string, symbol, option, and tuple inputs get meaningful generated values.
- Generated inputs match exported function signatures.

## 19. Symbolic support for functions with 3+ arguments
**Title**: Expand symbolic exploration beyond two-argument functions
**Labels**: enhancement, symbolic
**Summary**: Current fallback behavior for 3+ args is too limited.
**Acceptance criteria**:
- Symbolic analysis explores bounded combinations for 3+ args.
- Path explosion is limited by documented caps.

## 20. Symbolic path deduplication improvements
**Title**: Improve symbolic path uniqueness and panic grouping
**Labels**: enhancement, symbolic
**Summary**: Unique outcomes are currently deduplicated by raw result strings only.
**Acceptance criteria**:
- Equivalent paths are grouped more intelligently.
- Panic categories are summarized cleanly.

## 21. Symbolic JSON output mode
**Title**: Add machine-readable JSON output for symbolic reports
**Labels**: enhancement, symbolic, cli
**Summary**: Symbolic output is currently text plus optional generated TOML.
**Acceptance criteria**:
- CLI supports JSON output for symbolic reports.
- Output is stable enough for tests and downstream tooling.

## 22. Symbolic counterexample fixtures
**Title**: Emit replayable scenario fixtures from symbolic findings
**Labels**: enhancement, symbolic, scenario
**Summary**: Turn symbolic paths into ready-to-run scenario files or fixtures.
**Acceptance criteria**:
- Generated files can be consumed by the `scenario` command.
- Panic and success paths are both representable.

## 23. Dynamic trace normalization for `analyze`
**Title**: Normalize diagnostic events into structured dynamic-analysis traces
**Labels**: enhancement, analyze
**Summary**: Dynamic analysis currently uses stringified diagnostic events.
**Acceptance criteria**:
- Dynamic rules operate on structured trace objects.
- Trace extraction is deterministic across runs.

## 24. Expand security rule coverage
**Title**: Add more static and dynamic security analysis rules
**Labels**: enhancement, analyze, security
**Summary**: The current analyzer has a small initial rule set.
**Acceptance criteria**:
- Add rules for auth misuse, unsafe upgrades, dangerous storage patterns, and external-call ordering.
- Each rule has tests with at least one positive and one negative case.

## 25. Severity summary in analyze text output
**Title**: Add severity totals and summary section to `analyze`
**Labels**: enhancement, analyze, cli
**Summary**: Text output should summarize high/medium/low findings before detailed entries.
**Acceptance criteria**:
- Text output includes severity counts.
- Output remains stable under `--format text`.

## 26. SARIF output for security analysis
**Title**: Support SARIF export from `analyze`
**Labels**: enhancement, analyze, security
**Summary**: Enable CI/security tooling integration with SARIF output.
**Acceptance criteria**:
- CLI can emit SARIF.
- SARIF includes rule IDs, severity, and locations where available.

## 27. Scenario expected-failure assertions
**Title**: Support expected errors and panics in scenario steps
**Labels**: enhancement, scenario
**Summary**: Scenario steps can only assert successful returns today.
**Acceptance criteria**:
- Scenario TOML can declare expected failures.
- A matching failure marks the step as passed.

## 28. Scenario per-step setup hooks
**Title**: Add per-step storage and auth setup in scenarios
**Labels**: enhancement, scenario
**Summary**: Scenarios need more control over step-local environment setup.
**Acceptance criteria**:
- Steps can override or seed storage before execution.
- Auth mocking/setup can be configured per step.

## 29. Scenario event and budget assertions
**Title**: Support event and resource-budget assertions in scenarios
**Labels**: enhancement, scenario
**Summary**: Scenarios currently validate return values and selected storage only.
**Acceptance criteria**:
- Steps can assert emitted events.
- Steps can assert CPU or memory budget ceilings.

## 30. GitHub issue bootstrap automation
**Title**: Add a scripted workflow to publish local issue drafts with `gh`
**Labels**: tooling, github
**Summary**: Issue drafts exist locally, but publishing them is manual and auth-dependent.
**Acceptance criteria**:
- A documented script can create issues from the local draft backlog.
- The workflow fails clearly when `gh auth status` is not authenticated.
