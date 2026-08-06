# Writing explanation

Explanation is discursive treatment of a topic, read for understanding, away from the work: the
only kind of documentation that makes sense to read in the bath. It answers "Can you tell me
about...?" and "Why...?". Its scope is a topic, and every title should tolerate an implicit
"About" in front of it: "About user authentication", "About the scheduling model". If "About"
doesn't fit, the page is probably reference or a how-to.

Opinion belongs here. Design decisions, history, constraints, alternatives that lost, trade-offs
that were accepted: this is the one place they get discussed rather than hidden.

## Principles
- Make connections, including to things outside the immediate topic.
- Give reasons: "The reason for x is that historically, y."
- Weigh alternatives openly: "Some prefer w, because z. This can be a good approach, but..."
- Take a perspective and admit it. Neutrality is for reference.
- Keep the topic bounded. Instruction and technical description creep in easily; move them to
  their own homes and link.

## Co-writing
The substance of explanation lives in the user's head and nowhere in the repo. You cannot know
why a decision was made, so you never write a rationale you were not given. The method is an
interview:

1. Ask the questions only the user can answer: why this over the alternatives, what constraint
   forced it, what would break otherwise, what came before, what still bothers them about it.
2. Let the answers be rough; spoken-style fragments are fine.
3. Compose the piece from the answers, taking the user's positions, not yours.
4. You may propose a candidate rationale only as a question ("was this because of x?"), never as
   drafted text.

The test at the end: the user reads it and recognises their own reasoning. If a paragraph
surprises them, it was invented; cut it or ask.

Decision records for a platform (why this SKU, why managed identities over keys, why these role
assignments) are explanation. Capturing them at decision time, while the reasoning is fresh, is
cheap; reconstructing them later is archaeology.
