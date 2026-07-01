# Source: https://github.com/qShipyard/narratorlog/blob/main/docs/01-problem-statement.md

### Uh oh!

There was an error while loading. [Please reload this page]().

[qShipyard](https://github.com/qShipyard) / **[narratorlog](https://github.com/qShipyard/narratorlog)** Public

- [Notifications](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog) You must be signed in to change notification settings
- [Fork 0](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog)
- [Star 0](https://github.com/login?return_to=%2FqShipyard%2Fnarratorlog)
 

 

## FilesExpand file tree

 main

/

# 01-problem-statement.md

Copy path

Blame

More file actions

Blame

More file actions

## Latest commit

## History

[History](https://github.com/qShipyard/narratorlog/commits/main/docs/01-problem-statement.md)

History

69 lines (43 loc) · 4.58 KB

## FilesExpand file tree

 main

/

# 01-problem-statement.md

Copy path

Top

## File metadata and controls

- Preview
 
- Code
 
- Blame
 

69 lines (43 loc) · 4.58 KB

[Raw](https://github.com/qShipyard/narratorlog/raw/refs/heads/main/docs/01-problem-statement.md)

Copy raw file

Download raw file

Outline

Edit and raw actions

# narratorlog — Problem Statement

## The Problem

Engineering teams ship code every day. But the story of what they built rarely reaches the people who need to hear it — and when it does, it arrives too late, in the wrong language, or not at all.

This creates a communication gap that costs teams in real ways:

- **Product Managers** chase engineers every Friday asking "what shipped this week?" and get a Slack wall of commit hashes or a vague summary that took 30 minutes to write.
- **Marketing teams** find out about new features weeks after launch — too late to write copy, prepare announcements, or capitalise on momentum.
- **Engineering Managers** have no clear picture of shipping velocity, what areas of the codebase are moving, or whether last sprint was mostly features or maintenance.
- **Founders and executives** read raw changelogs they cannot interpret, or rely on secondhand summaries that lose important detail.
- **New engineers joining the team** have zero context on why the codebase looks the way it does, how it evolved, or what the team has been focused on.
- **The engineers themselves** spend time writing summaries, release notes, and changelogs that duplicate effort already captured in their commits and pull requests.

---

## Why Existing Tools Fall Short

Tools exist on both ends of the spectrum, but neither end solves the full problem.

**Conventional changelog tools** (git-cliff, conventional-changelog, auto-changelog) work well when commit conventions are strictly followed. Most teams do not follow them strictly. These tools also produce output in a single format and voice — useful for developers, unreadable for everyone else.

**Manual processes** (weekly Slack updates, Notion pages, Confluence wikis) produce good output but at high cost. Someone has to write them. That person is usually an engineer who would rather be building. The process is inconsistent, skipped when the team is busy, and impossible to scale.

**Existing AI changelog tools** are either one-off scripts, repo-specific implementations, or SaaS products that require sending your codebase to their servers. None of them handle the full workflow: understanding what shipped in context, translating it for multiple audiences, routing it through a human approval step, and delivering it to where each audience actually lives.

---

## The Root Cause

The gap is not a tooling problem at its core. It is a **translation problem**.

Engineering work is captured in the language of code — commits, diffs, pull requests, linked issues. The rest of the organisation operates in different languages — features, benefits, decisions, velocity signals. No automatic translation layer exists between them.

Commit messages describe _what changed_. Diffs show _how it changed_. Neither communicates _why it matters_, _who it affects_, or _what to say about it publicly_.

The information exists. It is sitting in your git history, your pull request descriptions, your linked issues, and your codebase itself. What is missing is the layer that reads all of that, understands it in context, and tells the right story to the right person.

---

## The People Affected

| Person | Their pain | What they actually need |
| --- | --- | --- |
| Engineers | Writing summaries that duplicate commit work | Their commits understood and communicated automatically |
| Engineering Manager | No visibility into shipping patterns | Weekly signal on velocity, areas of activity, team contribution |
| Product Manager | Chasing engineers for weekly updates | Plain-English summary of what moved, what is ready to demo |
| Marketing | Learning about features weeks late | Benefit-focused output ready to use in copy and announcements |
| Founders / Execs | Unreadable raw changelogs | High-level narrative of what the team is building and shipping |
| New engineers | No historical context on the codebase | Understandable timeline of how the product evolved |

---

## What Success Looks Like

A team sets up narratorlog once. Every week — without any manual effort — the right summary reaches every person in the right language, in the place they already work, after a brief human review to ensure nothing surprising gets published.

Engineers keep writing code. Everyone else stays informed. The communication gap closes.

---

## What narratorlog Is Not

- It is not a project management tool
- It is not a code review tool
- It is not a SaaS product — teams own their deployment and their data
- It is not a replacement for human judgment — it surfaces information and drafts; humans approve before anything is published
- It is not locked to any AI provider — teams bring their own