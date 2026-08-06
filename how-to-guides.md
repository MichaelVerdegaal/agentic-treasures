# Writing how-to guides

A how-to guide gives directions for a real-world task to an already-competent user. It serves
work, not study. It is defined by the user's goal, never by the machinery's operations: "To
deploy the configuration, select the options and press Deploy" describes controls, not guidance.
If basic competence makes a step obvious, cut it.

The title states exactly what the guide shows. "How to integrate application performance
monitoring" is good. "Application performance monitoring" is not: maybe it's about whether, or
what it is. If the page keeps wanting to discuss whether or why, it's explanation in the wrong
clothes.

## Principles
- Assume competence. Action and only action; no teaching, no digressions. If something matters,
  link to it.
- A how-to is not end-to-end complete. Start and end in reasonable places and let the reader
  join it to their own work.
- Address real-world complexity with conditional imperatives: "If you want x, do y. To achieve
  w, do z." Forks and branches are normal; a how-to is not always a straight procedure, and it
  can call on the reader's judgement.
- Order the sequence for flow: minimise context switches, don't make the reader hold a thought
  open for ten steps, don't send them back to redo earlier work.
- Omit reference detail: "Refer to the x reference for the full list of options."
- Put warnings inline where the risk lives, not collected at the top.

## Language
- "This guide shows you how to..."
- "If you want x, do y."
- "In the case of..., an alternative approach is to..."

## Co-writing
The user names the goal, and the goal must be something a user actually needs to get done, not a
tour of a feature: "How to configure frame profiling" passes; "How to use the settings page"
does not. The user confirms the sequence; you draft the steps and prose. Every command you write
gets executed in a terminal where possible; a step you could not run is labelled unverified so
the user knows to test it before publishing. Digressions you feel tempted to write become links
to explanation or reference.
