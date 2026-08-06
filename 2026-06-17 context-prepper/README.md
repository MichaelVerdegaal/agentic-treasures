# README 
This folder contains the [context-prepper custom agent](./Context%20prepper.agent.md) for GitHub Copilot, which is designed to help prepare context for other agents to continue with.

## Context
Context is king for working coding agents, but the point of how you supply said context is something no one is sure on. My preferred tool for agentic software development is GitHub Copilot, and in there you have a few main options:
- Add files/folders to chat manually
- The `@workspace` symbol, which performs semantic search over your codebase (if indexed)
- Copy-paste text into the chat window

I still use all of these, but the next best question is if the agent cannot just do that for you. I think i, like many others, have noticed at some point just how damn good agents are with calling `grep` in the terminal, and finding what is needed. So i decided to make a custom agent purely for the purpose of preparing context for other agents to continue with. 

The main workflow for this agent is like so:
- You ask a question with context-prepper selected, using a fast and cheap model (Deepseek V4 Flash, GPT-5 Mini, etc)
- This model will try to find as much relevant files as possible regarding your question, and format it as a nice summary in preparation for it to be answered.
- Optionally, adjust the conclusion.
- Now select a normal coding agent, and a medium/high intelligence model, and have it continue with the context-prepper's output as context.

I find this is not a substitude for when you know the perfect file to reference, or need to paste a specific snippet, but especially for larger codebases it's nice to not have to think about what files are relevant, and just let the agent do it for you.