# Writing tutorials

A tutorial is a lesson: the learner does something meaningful under your guidance and learns by
doing. It serves study, not work. Success is measured by what the learner acquires, not by what
they produce. Responsibility sits almost entirely with the teacher; the learner's only job is to
follow directions.

The exercise must be meaningful (a sense of achievement), successful (completable by everyone),
logical (the path makes sense), and usefully complete (it touches every action, concept, and tool
the learner needs to meet).

## Principles
- Tell the learner at the start what they will achieve: "In this tutorial we will create and
  deploy x." Not "you will learn..."; that's a poor pattern.
- Deliver a visible result early and often. Every step produces something the learner can see.
- Keep a narrative of the expected: "You will notice...", "After a few moments, the server
  responds with...". Show actual output, ideally exact. Flag likely failure signs: "If the
  output doesn't show x, you probably forgot y." Warn before surprises ("this returns several
  hundred lines of logs").
- Point out what to notice in passing (a changed prompt, a new file). Learners are too busy
  doing to observe unprompted.
- Make steps repeatable where possible. Learners repeat what works, and repetition is how skill
  settles in.
- Ruthlessly minimise explanation. "We use HTTPS because it's safer" plus a link is enough;
  explanation mid-tutorial breaks the learner's focus.
- Stay concrete: this command, this file, this result. General patterns emerge from concrete
  examples on their own.
- No options or alternatives. One path to the conclusion.

## Language
- "In this tutorial we will..." First-person plural throughout: tutor and learner together.
- "First, do x. Now, do y. Now that you have done y, do z." No room for ambiguity.
- "The output should look something like..."
- "Notice that... Remember that... Let's check..."
- Close by describing, and mildly admiring, what the learner built.

## Co-writing
The user designs the learning journey: what is to be learned, which encounters deliver it, in
what order. You draft the narrative, keep the voice consistent, and check that every step has a
visible result with an expected output. Expected outputs are captured from a real run, never
composed.

The reliability bar is absolute: a learner who follows the directions and doesn't get the
promised result loses confidence in the tutorial, the product, and themselves. A tutorial ships
only after a full run-through in a clean environment, and gets re-run after any edit: tutorial
changes cascade, because the end-to-end journey has to keep making sense.
