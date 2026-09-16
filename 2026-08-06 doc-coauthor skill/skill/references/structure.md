# Structuring the docs tree

## One page, one form
Each page is a tutorial, a how-to guide, reference, or explanation, and reads like one. Mixed
pages serve nobody: the blur between neighbouring forms (tutorial/how-to, reference/explanation)
is where most documentation problems live. When content wants to be two things, make two pages
and link them.

## Topic-first hierarchy: components
Organise by topic, with the forms inside each topic, rather than four top-level buckets. A
component (jobs, environments, scheduling, roles and permissions, pricing) is a topic landing
page:

- Two or three sentences saying what the component is.
- Grouped links into whatever exists for it: its explanation, its how-to guides, its reference.

A landing page introduces; it never just lists. Short intro text per group, links under it.
Lists longer than about seven items want grouping.

The forms are how one component page serves three different readers at once:
- browsing to understand the topic -> explanation (and the tutorial, if one exists)
- here to get something specific done -> how-to guides
- needs a fact to keep working -> reference

## Growing the tree
- Never scaffold empty structure: no hollow tutorials/how-to/reference/explanation folders
  waiting to be filled. Structure emerges from content that exists, not the other way round.
- Improve iteratively: take the page in front of you, ask what user need it serves and how well
  it serves it, make one publishable improvement, stop. Small steps that ship beat
  reorganisations that don't.
- Docs are never finished, but every page can be complete: useful now, whole at its current
  stage.
