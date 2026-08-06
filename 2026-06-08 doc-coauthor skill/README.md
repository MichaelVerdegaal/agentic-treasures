# README
This folder contains the `doc-coauthor` skill, used to help you write documentation 
*together* with LLM's, with the [Diátaxis framework](https://diataxis.fr/) in mind.


## Motivation
I think writing documentation is important, especially so in this current age. However, 
i also think that letting AI fully write your documentation just sucks; it tends to 
super verbose, hallucinates and just misses the bigger picture. On the other hand, 
while i think writing manually drastically increases the quality, it also drastically increases the time it takes to *write* said documentation.

To attempt to solve this problem i decided to implement an agent skill that helps you 
write documentation, with a focus on you making the important decisions on the text. The flow 
goes roughly like this:

1. You supply the initial outline of the text and important references to the agent, and activate the skill.
2. the agent skill will generate a draft, fetching links and asking questions and whatnot to help it do so.
3. You will then review the draft, and make decisions on what to keep, what to change, and what to remove. You can then decide to make the edits yourself or let the agent revise it, up to you!

Second, the [Diátaxis framework](https://diataxis.fr/) is used to make the documentation 
produced more structured, and purposeful. It will help the agent categorize your 
intent for the documentation, and write it in a manner that is more suitable 
for your intended audience.

## Recommendations

- Create a template markdown file, have the agent produce according to your template.
- Use a markdown formatter like [mdformat](https://mdformat.readthedocs.io/en/stable/users/style.html) to keep your markdown files clean and consistent.
- Use a link checker like [lychee](https://github.com/lycheeverse/lychee) to ensure your links are not dead.
- I found [this website to help you clean up code snippets](https://trevorfox.com/tools/developer/claude-code-paste-cleaner/) helpful when i copy-paste directly from Claude.