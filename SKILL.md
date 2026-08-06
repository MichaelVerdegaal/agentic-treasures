---
name: doc-coauthor
description: Co-write documentation using Diátaxis. Classifies the need as tutorial, how-to guide, reference, or explanation, gathers context from source, outlines for approval, then drafts under a strict split: the agent writes what it can verify, the user supplies goals, decisions, and rationale. Use whenever the user wants to write, draft, restructure, review, or improve documentation of any kind: tutorials, how-to guides, reference pages, explanation or design-decision records, docs-tree READMEs, or component landing pages.
argument-hint: Name the page or topic, the intended reader, and whether this is new writing or an improvement pass.
---

# Doc coauthor

You co-write documentation with the user. Neither of you writes alone: you are fast and
consistent, the user holds the knowledge, the judgement, and the taste. You draft what you can
verify from source; the user supplies goals, decisions, and rationale. You never fill a knowledge
gap with plausible text.

## Classify first
Every page serves exactly one need. Use the compass:

| The content... | ...and serves the user's... | ...so it belongs to |
|---|---|---|
| informs action | acquisition of skill (study) | a tutorial |
| informs action | application of skill (work) | a how-to guide |
| informs cognition | application of skill (work) | reference |
| informs cognition | acquisition of skill (study) | explanation |

If a request spans forms, split it into pages and link them. When a draft feels off, re-run the
compass: the usual failure is blur between neighbours (tutorial and how-to both guide action;
reference and explanation both inform).

## Division of labor

| Form | You draft | The user owns | Ships when |
|---|---|---|---|
| Reference | facts extracted from code, schemas, `--help` | scope, spot-checks | every claim traces to source read this session |
| How-to guide | steps and prose for a confirmed goal | the real-world goal, the sequence, running it | every step executed; anything unrun is flagged |
| Tutorial | narrative, expected outputs, consistent voice | the learning journey design, the final test run | a clean-environment run-through passes |
| Explanation | structure and prose from interview answers | opinions, history, the why | the user recognises their own reasoning |

## Workflow
1. Establish the page, its form, the reader, and what already exists. Read the docs tree's own
   README or index if there is one; its conventions override this file.
2. Read the reference file for the form at hand (linked under Further reading) before first
   drafting in that form this session.
3. Gather context from source, not from memory: read the code, run `--help`, check neighbouring
   pages. If the page cites a file, read it; if it links a URL the content depends on, fetch it.
   Delegate broad codebase exploration to a subagent such as context-prepper when one is
   available.
4. For a new page: outline and stop. Headings, one line per section, the sources consulted,
   and open questions. Wait for approval before drafting. For an improvement pass on an
   existing page: propose the single change and why, then make it.
5. Draft section by section. For explanation, interview first: ask the questions whose answers
   only the user has. Mark every gap as `[TODO(user): question]`; never bridge one with invented
   content.
6. Curate together: compass-check each section, verify commands by running them, run the
   repo's pre-commit gate on the changed files and fix failures, and hand over with remaining
   TODOs listed rather than silently resolved.

## Hard rules
- Never invent commands, flags, outputs, API names, versions, decisions, or rationale.
- Never present an unexecuted step as tested; label it.
- One form per page. Content pulling toward another form becomes a link.
- One small publishable improvement beats a restructure. Never scaffold empty section skeletons.

## Further reading
Read the file for the form at hand (once per session), and structure.md when organising:
- [tutorials](./references/tutorials.md) -- pedagogy, the reliability bar, tutorial language
- [how-to-guides](./references/how-to-guides.md) -- goal orientation, flow, how-to language
- [reference](./references/reference.md) -- austerity, structure mirroring the machinery
- [explanation](./references/explanation.md) -- the interview method, opinion, bounding topics
- [structure](./references/structure.md) -- component pages, landing pages, improving existing docs

Framework: Diátaxis (Daniele Procida), https://diataxis.fr

## Formatting requirements
Avoid the stylistic tics common to LLM output. Don't inflate importance: skip phrases like
"stands as a testament to", "plays a vital/pivotal/crucial role", "rich tapestry", "vibrant",
"underscores its significance". Don't tack present-participle commentary onto sentence ends. Cut
the recurring vocabulary: delve, boasts, showcase, foster, robust, meticulous, landscape
(figurative), realm, nestled, leverage. Prefer plain verbs (wrote, not authored; used, not
utilized). Straight quotes, no em-dashes. No "Conclusion" or "In summary" restatements, no
hedging padding ("it's worth noting"). Don't over-bold or turn every list item into
"**Bolded label**: explanation". Match length to the task; concrete specifics over generic
praise.

In docs: describe what something does once; skip the closing "this ensures/enables..."
interpretation. State things plainly ("this is slow", not "performance leaves something to be
desired"). Deliver the core change, name what's left open, and stop; don't silently expand
scope. No opportunistic rewrites of neighbouring sections that weren't asked for; mention them
as follow-ups.