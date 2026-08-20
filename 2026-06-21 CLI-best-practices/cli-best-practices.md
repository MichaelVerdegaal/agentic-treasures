# CLI Usability Standards

Instructions for improving a CLI tool's user experience. Apply these to the existing command surface; prefer fixing violations over adding features.

## 1. Command grammar

- Actions are top-level verbs (`get`, `list`, `status`, `run`). Managed things are noun subgroups with a standard verb set (`<noun> add/list/remove/update`). Never mix the two styles for the same concept.
- If commands operating on one concept are scattered across the top level, consolidate them into one noun group.
- Never give one word two meanings. If a `list` command exists and you need to list a subset, add a filter flag (`list --source X`), not a second list-like command.
- Command names should match user intent, not internal mechanics (prefer `update` over `resync-index`, `get` over `process-url`).
- Provide one command that runs the whole common workflow end to end. Keep individual stages available as separate commands for debugging, but the happy path must be a single invocation.
- A bare noun-group invocation (`tool source`) should perform the obvious read-only action (usually list), not error.

## 2. stdout is data, stderr is everything else

This is the most important rule. Enforce it strictly:

- Only payload data goes to stdout. Logs, progress, warnings, hints, and "nothing found" messages all go to stderr - including friendly diagnostics like "No items yet. Add one with: ...". An empty result must produce empty stdout.
- Route all stdout writes through a single small output module (`emit()` / `emit_json()`), so the invariant has one enforcement point instead of a convention.
- Add a regression test asserting logs never appear on stdout and that piping data commands yields only data.
- Success should be quiet. Reports after a run must cover only that run, never re-dump historical state or old failures.

## 3. Composition with pipes and standard tools

- Accept `-` as an argument meaning stdin, consistently across commands. If stdin is a TTY when `-` is given, fail with a usage error that shows an example pipe.
- Tabular output: tab-separated columns, one record per line, so it feeds grep/cut/awk. Offer `--null`/`-0` (NUL separators) where filenames or URLs may contain odd characters.
- `--json` global flag: streaming commands emit JSONL (one object per line, works with head/jq); one-shot commands emit a single object. Mutating commands should emit one consistent summary object (counts of what happened this run) so scripts can assert outcomes without a follow-up status call.
- Do not reimplement Unix tools. No built-in filtering/grepping/paging when a pipe does it. Prefer a `path`-style command that prints a file location (composing with cat/less/grep) over a `cat`-style command that prints content.
- Design commands so output of one feeds input of another (`tool extract file | grep x | tool get -`).

## 4. Conventional surface (table stakes)

Verify all of these exist; add any that are missing:

- `--version` (eager, works without a subcommand), `--help` on every command with real descriptions.
- Shell tab completion for commands and flags.
- `-v`/`-vv` (debug/trace), `-q` (errors only, wins over verbose), `--color auto|always|never`.
- Global options should work both before and after the subcommand - users type `tool status --json` naturally; make it valid.
- Follow existing flag conventions: `-n`/`--limit`, `--force`, `--all`, `--dry-run`. Fix any nonconforming flags (e.g. a bare `--n`).
- Meaningful exit codes (sysexits: 64 usage error, 66 missing input; framework parse errors keep 2) so scripts can branch.
- No-args invocation of the root command shows help, not an error or silent nothing.

## 5. Destructive operations

- Every deletion asks for confirmation, with `--force` to skip. The prompt states specifically what will be destroyed, including a count from a dry-run selection ("Delete 14 page(s) (all failed entries) and their files?").
- Defaults are the least destructive option. Extra destruction is always an explicit opt-in flag (`--remove-files`), never a side effect.
- Deletion of a parent must not destroy children still referenced elsewhere.

## 6. Errors that name the next step

- Every error message should tell the user what to do next, ideally the exact command: "X is still pending. Run: tool fetch".
- Validation rejections must carry their evidence and threshold: "too short (23 words < 50, from 5 KB input)" - not just "rejected".
- Clean one-line errors to stderr with nonzero exit; no tracebacks for expected failures.
- Mutually exclusive arguments fail fast with a message listing the valid combinations.

## 7. Introspection commands

Every piece of hidden state deserves a read-only command that answers the question a user would otherwise ask the docs:

- A `status` command: state counts plus a summary of recent failures.
- A command that prints where data lives on disk.
- If the tool has rules/config that transform input: a `list` command showing effective config in application order, and a `test <input>` dry-run that prints the verdict and names the specific rule that fired.

## 8. Configuration

- Config that users edit is data (a TOML/config file), not CRUD subcommands mutating a database. User config loads ahead of packaged defaults so it wins.
- Tunables read from namespaced env vars with sane fallbacks; malformed values warn and fall back rather than crash.
- Remove knobs nobody can meaningfully tune. Every flag and env var is surface the user must remember; when in doubt, hardcode a good value and document it.

## 9. Documentation and help

- Per-command `--help` is the authoritative reference; the README is a guided tour with one runnable example per command, each with a short trailing comment.
- Document the conceptual distinctions between similar commands explicitly ("X is one-shot and tracks nothing; Y is registered and re-syncable").
- Keep a plan file with "Parking lot" and "Rejected" sections recording what was deliberately not built and why, so scope stays small on purpose.

## 10. Process rules

- Test the UX invariants, not just the logic: stdout/stderr split, JSON shapes, NUL separation, stdin handling, exit codes, confirmation prompts. A convention without a test disappears in the next refactor.
- Build for what exists, not what might exist: no speculative flags, actions, or abstraction until a real case needs them.
- Apply the 2am test: could a tired maintainer understand and debug this? Immutable structural logic belongs in code; editable policy belongs in data.
- Prefer deleting features over polishing them: dead commands, unused options, and secondary interfaces that split iteration budget should go.

When applying this document: audit the repo against each section first, list the violations, then fix them in order of section number - the stdout/stderr rule and command grammar changes are the highest-leverage and most breaking, so do them before adding table-stakes flags or docs.