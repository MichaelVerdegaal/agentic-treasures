# CLI Usability Standards

Instructions for improving a CLI tool's user experience. Apply these to the existing command surface; prefer fixing violations over adding features.

## 0. Pick the shape first

Two archetypes need different subsets of this document:

- **Data tool**: commands emit records meant to be piped, filtered, or read. Sections 3 and 5 are where most of the value is.
- **Operational tool**: one or a few long-running jobs driving hardware, models, or external services. Section 4 carries most of the weight; section 5 largely does not apply.

The shape is per command, not per project. A capture tool still has a `status` that must obey section 3, and a data tool still has flag dependencies. Decide per command, then hold every command to the same conventions. The most common failure in an otherwise good CLI is drift between the primary command and the ones added later (`train`, `eval`, `convert`, `export`): they skip the output discipline, invent their own error style, and return the wrong exit codes.

## 1. Command grammar

- Actions are top-level verbs (`get`, `list`, `status`, `run`). Managed things are noun subgroups with a standard verb set (`<noun> add/list/remove/update`). Never mix the two styles for the same concept.
- If commands operating on one concept are scattered across the top level, consolidate them into one noun group.
- Never give one word two meanings. If a `list` command exists and you need to list a subset, add a filter flag (`list --source X`), not a second list-like command.
- Provide one command that runs the whole common workflow end to end. Keep individual stages available as separate commands for debugging, but the happy path must be a single invocation.
- A bare noun-group invocation (`tool source`) should perform the obvious read-only action (usually list), not error.

## 2. Names the user has to type

Command names are typed dozens of times a day and are the entire interface until someone reads `--help`. Treat them as the primary UX surface, not a labelling step.

- Use the word the user would say, not the word the code uses: `update`, not `resync-index`; `get`, not `process-url`; `prune`, not `cleanup-orphaned-records`.
- One lowercase word where possible. If a name needs two words it is usually a noun group plus a verb (`source add`), not a hyphenated top-level command (`add-source`).
- Prefer the shortest word that stays unambiguous in the domain: `get`, `run`, `list`, `path`, `prune`, `status`. Not `initialize`, `synchronize`, `configuration`.
- The tool name itself is the most-typed token of all. Three to five characters (`aic`, `gh`, `uv`).
- Abbreviate only where the abbreviation is already more common than the full word in the domain (`db`, `url`, `img`). Never invent one the user has to learn (`gen-cfg`, `prc`).
- Do not ship two commands whose names are near-synonyms in English (`get`/`fetch`/`pull`, `list`/`show`). If both must exist because they do different things, state the distinction in one line in each `--help` and in the README: "`get` is one-shot and tracks nothing; `source` is registered and re-syncable".
- Flag names describe the effect, not the internal field: `--save-video`, `--no-detector`, `--dry-run`.
- Short flags are scarce. Spend them on the flags used every run; leave rare ones long-form only.
- Resolve values from an obvious location so the common invocation stays short: `--model best.pt` should look in the models directory before treating the value as a path, while still accepting a full path.
- Never make the user type what the tool can derive. One positional for the required thing, flags for the rest, defaults for everything else.

## 3. stdout is data, stderr is everything else

This is the most important rule. Enforce it strictly, in every command:

- Only payload data goes to stdout. Logs, progress, warnings, hints, and "nothing found" messages all go to stderr, including friendly diagnostics like "No items yet. Add one with: ...". An empty result must produce empty stdout.
- Route all stdout writes through a single small output module (`emit()` / `emit_json()`), so the invariant has one enforcement point instead of a convention.
- Add a regression test asserting logs never appear on stdout and that piping data commands yields only data.
- Success should be quiet. Reports after a run must cover only that run, never re-dump historical state or old failures.
- Configure logging once, at the root callback, for every subcommand. A command that sets up its own logging will drift.

## 4. Flags, defaults, and prerequisites

The CLI layer parses intent and validates combinations; workflow code does the work. Keeping validation in the CLI is what makes the next three rules cheap.

- The no-flag invocation should be the most common real use, not a demo or a safe no-op.
- Encode prerequisites in code, not in the README. The user should never have to learn a dependency chart to build a valid command.
- Infer rather than demand. When one flag implies another, turn the implication on and offer an explicit opt-out (`--track` enables the expected detector; `--no-detector` overrides). Only error when the tool genuinely cannot choose.
- Validate every combination before anything expensive starts: opening cameras, loading models, hitting the network, creating files, touching the database. A bad combination must fail in milliseconds with a message naming the flags to add or drop.
- Optional capabilities degrade, required ones fail. If an auxiliary sink (a stream, a notification) cannot start, warn on stderr and keep going, because the primary work is still useful. If the primary output cannot be written, stop.
- Every flag is permanent surface the user must remember. Before adding one, check it cannot be a default, an inference from another flag, or a config value.

## 5. Composition with pipes and standard tools

For data-shaped commands:

- Accept `-` as an argument meaning stdin, consistently across commands. If stdin is a TTY when `-` is given, fail with a usage error that shows an example pipe.
- Tabular output: tab-separated columns, one record per line, so it feeds grep/cut/awk. Offer `--null`/`-0` (NUL separators) where filenames or URLs may contain odd characters.
- `--json` global flag: streaming commands emit JSONL (one object per line, works with head/jq); one-shot commands emit a single object. Mutating commands emit one consistent summary object (counts of what happened this run) so scripts can assert outcomes without a follow-up status call.
- Do not reimplement Unix tools. No built-in filtering, grepping, or paging when a pipe does it. Prefer a `path`-style command that prints a file location (composing with cat/less/grep) over a `cat`-style command that prints content.
- Design commands so the output of one feeds the input of another (`tool extract file | grep x | tool get -`).

## 6. Conventional surface (table stakes)

Verify all of these exist; add any that are missing:

- `--version` (eager, works without a subcommand), `--help` on every command with real descriptions.
- `--help` and `--version` must be instant. Import models, drivers, and database layers inside the command body, not at module import time, so the help path stays lightweight.
- Root `--help` is the landing page: order commands by lifecycle or frequency, not alphabetically, and group them if there are more than about seven. Features that depend on an optional extra stay listed, with a one-line note on how to enable them, rather than being hidden or pretending to be installed.
- Shell tab completion for commands and flags.
- `-v`/`-vv` (debug/trace), `-q` (errors only, wins over verbose), `--color auto|always|never`.
- Global options should work both before and after the subcommand: users type `tool status --json` naturally, so make it valid. If the framework needs custom parser surgery to allow this, keep that code in one small documented place with tests covering both orderings. It is the kind of cleverness that breaks silently on a framework upgrade.
- Follow existing flag conventions: `-n`/`--limit`, `--force`, `--all`, `--dry-run`. Fix any nonconforming flags (for example a bare `--n`).
- Meaningful exit codes (sysexits: 64 usage error, 66 missing input; framework parse errors keep 2) so scripts can branch.
- No-args invocation of the root command shows help, not an error or silent nothing.

## 7. Destructive operations

- Every deletion asks for confirmation, with `--force` to skip. The prompt states specifically what will be destroyed, including a count from a dry-run selection ("Delete 14 page(s) (all failed entries) and their files?").
- Defaults are the least destructive option. Extra destruction is always an explicit opt-in flag (`--remove-files`), never a side effect.
- Deletion of a parent must not destroy children still referenced elsewhere.

## 8. Errors that name the next step

- Every error message tells the user what to do next, ideally the exact command: "X is still pending. Run: tool fetch".
- Validation rejections carry their evidence and threshold: "too short (23 words < 50, from 5 KB input)", not just "rejected".
- Clean one-line errors to stderr with nonzero exit; no tracebacks for expected failures.
- Mutually exclusive arguments fail fast with a message listing the valid combinations.

## 9. Introspection commands

Every piece of hidden state deserves a read-only command that answers the question a user would otherwise ask the docs:

- A `status` command: state counts plus a summary of recent failures.
- A command that prints where data lives on disk.
- If the tool has rules or config that transform input: a `list` command showing effective config in application order, and a `test <input>` dry-run that prints the verdict and names the specific rule that fired.

## 10. Configuration

- Config that users edit is data (a TOML or similar file), not CRUD subcommands mutating a database. User config loads ahead of packaged defaults so it wins.
- Tunables read from namespaced env vars with sane fallbacks; malformed values warn and fall back rather than crash.
- Remove knobs nobody can meaningfully tune. When in doubt, hardcode a good value and document it.

## 11. Documentation and help

- Per-command `--help` is the authoritative reference; the README is a guided tour with one runnable example per command, each with a short trailing comment.
- Document the shape of the tool once, up front: which commands compose and which are jobs. It tells the reader which half of the interface they are in.

## 12. Process rules

- Test the UX invariants, not just the logic: stdout/stderr split, JSON shapes, NUL separation, stdin handling, exit codes, confirmation prompts, invalid flag combinations. A convention without a test disappears in the next refactor.
- Build for what exists, not what might exist: no speculative flags, actions, or abstraction until a real case needs them. Prefer deleting surface over polishing it. Dead commands, unused options, and secondary interfaces that split the iteration budget should go.
- Keep a plan file with "Parking lot" and "Rejected" sections recording what was deliberately not built and why, so scope stays small on purpose.
- Apply the 2am test: could a tired maintainer understand and debug this? Immutable structural logic belongs in code; editable policy belongs in data.

When applying this document: audit the repo against each section first, list the violations, then fix them in order of section number. Command grammar, naming, and the stdout/stderr rule are the highest-leverage and most breaking, so do them before adding table-stakes flags or docs. Where a section does not apply to a command's shape (section 0), say so explicitly in the audit rather than skipping it silently.
