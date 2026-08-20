# README 
This folder contains the [cli-best-practices](cli-best-practices.md) prompt/instructions, 
which is a collection of instructions to improve the quality of a CLI tool.

## Context
This prompt came about because i noticed at some point while agents are really 
good at writing CLI tools, that's no guarantee that the CLI feels nice to use. 

I had a CLI built by a coding agent for [Arciv](https://github.com/MichaelVerdegaal/Arciv), 
and noticed that while everything worked.....it was insanely clunky to use. Around 
this time i also was very heavily working with Linux, and the contrast of Arciv's 
CLI against all of the super streamlined linux commands (`ls`, `grep`, `cat`) became glaring.

These commands tend to have some patterns in common:
- A `--help` command is available for everything, even subcommands
- A `-v`/`--verbose` flag to make the command output more verbose
- Can be easily piped into the next command
- Print output in a consistent manner

And more. I collected a few of these, with the great [Command Line Interface Guidelines](https://clig.dev) 
as reference. 

I primarily use this solely as prompt, not as custom agent or whatever. The reasoning 
for this is that once you have had a coding agent restructure/build a CLI in a certain 
manner, they're very good at extending it the next time you ask something, even 
without having to paste this again. 

Dumber models can sometimes try to do *everything* in the prompt, but that's not 
always desired. Think for example a `--json` flag when you mainly just use the CLI 
as an interface to run scripts, or adding regression tests when you only have like 2-3 
commands. 