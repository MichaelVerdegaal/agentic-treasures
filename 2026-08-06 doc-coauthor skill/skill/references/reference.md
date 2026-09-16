# Writing reference

Reference describes the machinery: austere, authoritative, consulted rather than read. It serves
the working user who needs facts as a firm platform to stand on. It is led by the product, not by
the reader's tasks: a map of the territory. Like a map, its structure mirrors the structure of
the thing it describes; if a method belongs to a class in a module, the documentation shows the
same relationship.

## Principles
- Describe and only describe. The natural urges (explain, instruct, opine) all belong elsewhere;
  link to them instead of giving in.
- Use standard patterns consistently. Readers use reference by knowing where things will be and
  what format they come in. This is not the place for varied prose.
- Examples illustrate without sliding into instruction or explanation. One usage example after a
  command's description is ideal.
- State warnings as facts about the machine: "You must not apply b unless c. Never d."
- Lists and tables of commands, options, errors, and limits are the natural form. Boring is
  correct here.

## Language
- State facts: "Django's default logging configuration inherits Python's defaults. It's
  available as django.utils.log.DEFAULT_LOGGING."
- Enumerate: "Sub-commands are: a, b, c, d, e, f."

## Co-writing
This is your home turf; draft freely, but only from source read this session: the code, schemas,
`--help` output, configuration files. Model memory is not a source; an API surface you remember
is an API surface to verify. The user sets the scope and spot-checks; you keep every claim
traceable, and anything you could not confirm becomes a `[TODO(user): ...]` rather than a guess.

Where tooling can generate a section (docstring-driven API reference, `--help` dumps), prefer
generating over writing: generated reference cannot drift from the code.
