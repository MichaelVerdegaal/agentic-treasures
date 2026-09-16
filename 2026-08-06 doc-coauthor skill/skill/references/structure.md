# Structuring the docs tree

## One page, one form
Each content page is a tutorial, a how-to guide, reference, or explanation, and reads like one. Mixed
pages serve nobody: the blur between neighbouring forms (tutorial/how-to, reference/explanation)
is where most documentation problems live. When content wants to be two things, make two pages
and link them.

Every documentation page you create or edit has the frontmatter required by the
[page metadata rules](../SKILL.md#page-metadata): `tutorial`, `guide`, `reference`, `explanation`,
or the navigation-only custom type `index`. Metadata describes the page, not its folder.

## Minimal starting structure
Use the existing docs location, naming, and navigation conventions when there are any. Do not
reorganise a working tree just to match a preferred layout.

If there is no docs structure, propose this small default with the page outline:
- `docs/README.md`, with `diataxis-type: index`, linking to the page being written.
- The actual content page beside it, with a descriptive filename and its own `diataxis-type`.
- A link from the repository README to `docs/README.md`, if that README exists.

Create the index and approved content together, not an empty index in advance. If the existing
README already serves as the docs entry index, reuse it instead of adding a second one. If the
requested deliverable is itself a standalone index, do not create another index to contain it.
Keep a small collection flat; add topic folders and local indexes only when actual content
benefits from grouping. Do not create four empty form folders, placeholder pages, or empty
headings for types that have no content yet.

## Topic-first hierarchy: components
When grouping becomes useful and no local convention exists, prefer topics with the forms inside
each topic, rather than four top-level buckets. A component (jobs, environments, scheduling,
roles and permissions, pricing) can have a topic landing page with `diataxis-type: index`:

- A brief orientation to the linked material, if needed.
- Grouped links into whatever exists for it: its explanation, its how-to guides, its reference.

An index helps readers choose a destination. A table of contents can simply list links; add short
descriptions only where they help navigation. Lists longer than about seven items want grouping.
Keep substantial explanation, instructions, and reference detail in linked content pages, not in
the index. Do not label a substantive overview `index` merely because it also contains links.

The forms are how one component page serves three different readers at once:
- browsing to understand the topic -> explanation (and the tutorial, if one exists)
- here to get something specific done -> how-to guides
- needs a fact to keep working -> reference

## Navigation pass: connect both directions
Do this for every new or edited documentation page, not only when asked to organise docs.
An outgoing link leaves the page; an incoming link lets a reader find it from somewhere else.

1. Read the docs entry point, nearest topic index, and relevant neighbouring pages. Search for
  the topic and for existing references to the page's path or title. Choose links from content
  you have inspected, not from filenames alone.
2. Add outgoing links for what the reader needs before, during, or after this page: prerequisites,
  related reference, supporting explanation, or a useful next task. Link at the point of need;
  keep optional reading at the end when it would interrupt a tutorial or guide. A link to the
  parent index is enough when no related content exists. Do not force a tour of all four forms.
3. Ensure an incoming route from the docs entry point. Add the page to the appropriate index,
  table of contents, or site navigation if missing. The entry point may lead through a topic
  index; every page need not appear at the root. Check that new topic indexes are reachable too.
4. Where an existing page mentions this topic or would naturally send its reader here, add a
  contextual link there as well. Make the edit, rather than just suggesting it in the handover.
  Do not add reciprocal links everywhere: each link must help the reader.
5. If a page moves, is renamed, or changes headings, find and repair affected incoming links and
  navigation entries. Edit navigation sources rather than generated output. Preserve existing
  metadata and add or correct `diataxis-type` on documentation pages touched by these edits.
6. Check relative paths from each linking file and verify target headings for fragment links.
  Use the repository's link checker or docs build if available; otherwise inspect local targets
  and anchors directly and report what could not be verified.

The top-level entry page does not need an incoming link when there is no higher entry point.
If the user explicitly limits edits to one file, respect that limit and report the exact linking
edit needed elsewhere. If there is no useful outgoing destination, say so; never fabricate one.
Report incoming and outgoing links added or checked, and any unresolved navigation gap.

## Growing the tree
- Never scaffold empty structure: no hollow tutorials/how-to/reference/explanation folders
  waiting to be filled. Structure emerges from content that exists, not the other way round.
- Improve iteratively: take the page in front of you, ask what user need it serves and how well
  it serves it, make one publishable improvement, stop. Small steps that ship beat
  reorganisations that don't.
- Docs are never finished, but every page can be complete: useful now, whole at its current
  stage.
