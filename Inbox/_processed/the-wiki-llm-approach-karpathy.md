---
title: The wiki-LLM approach
source: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
author: Andrej Karpathy
clipped: 2026-01-15
type: clip
---

# The wiki-LLM approach

> Fictional example capture. This is what a clipped article looks like before `/ingest` turns it into a `Library/` page. It has already been processed (that's why it lives in `_processed/`), producing [[The wiki-LLM approach - Andrej Karpathy]].

Obsidian is the IDE, the LLM is the programmer, the wiki is the codebase.

The idea: keep your knowledge as plain markdown files and point an LLM agent at the folder. The agent reads the files, writes new ones, links them together, and keeps the whole thing organised. You capture raw material; the agent does the distillation and the cross-referencing.

The wiki grows to hundreds of thousands of words, most of it written and maintained by the agent. You rarely touch files by hand — you steer, review, and feed it new sources.

The payoff isn't storage. It's that connections you'd never draw by hand emerge automatically, and the knowledge compounds: every new source is compared against everything already there.
