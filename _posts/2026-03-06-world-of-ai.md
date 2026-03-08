---
layout: post
title:  "SDLC in the Age of AI"
date:   2026-03-08
categories: ai software SDLC 
---

## Short Notes

- You really want the tests to be deterministic so that when the AI runs them it's able to use them as a tool and not start hallucinating because of them. In the best scenario the AI will notice that they are not deterministic and try to fix them, but even so, you will be spending money on yet another thing to fix.
- Should the memory of the agent be shared among development teams? This way you get to reuse the information and not have to re-generate it for each of the team members.
- How should we on-board people in the age of AI?
- Can you get production information from your agent? Maybe integrate your observability platform thru MCP with your AI agent in order to see trends for example, or peaks in traffic, errors in logs etc.
- Will AI lead to ease of using multiple programming languages and does this mean that the the cost of a polyglot company goes down?
- Now it's easy to keep track of architectural decisions (ADRs): just ask the AI to write one when you decide. Even better, you can ask to specifically say that the new arch decision is overriding a previous one.
- AI rules (CLAUDE.md, AGENTS.md, etc) are code: write with care, open PRs for it, fix it when necessary, refactor it etc. Learnings on how to interact with the AI, must be treated as code and written down (think infrastructure as code)
- checkout [methodologies](https://www.youtube.com/watch?v=LOHgRw43fFk) for monitoring the productivity gains of AI adoption (end of the movie)
- how will this affect the cognitive load of developers? As you progress with dev of more and more features at a faster pace, will the devs have the same capacity to judge the complexity of a feature? Or is that capacity even needed anymore?

## Tools

- llmfit - easy to use tool in order to select the models that are best fit for the computer at your disposal, for running locally
- openspec - just spec it, don't write the code anymore
- tessl - under review manager for skills
