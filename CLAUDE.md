# CLAUDE.md
c
This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this vault is

This is a personal Obsidian knowledge vault used for work at Eli Lilly, focused on **Supply Chain Resiliency** and related strategic initiatives. Notes capture meeting summaries, working data documentation, and project context — not code.

## Vault structure

```
Work/
  Supply Chain Resiliency/
    Everstream/       — Everstream risk platform notes and contacts
    SCRM/             — Supply Chain Risk Management council notes
    Supplier of Focus/ — Supplier scoring data sources and logic
    Riverlogic/       — River Logic optimization alignment notes
    Optimus/          — Optimus tool data working sessions
  AI Tools/           — Notes on internal AI tools (OFG/OFG, Steven's tool)
  Insights/           — Personal development and leadership notes
```

## Key domain context

- **Everstream**: Third-party geopolitical/supplier risk platform. Managed by this user; Brooke to oversee post-training. Altana is a separate tool under Vivi.
- **Supplier of Focus (SoF)**: Annual supplier risk scoring exercise. Data sources include SAP ZS163n (raw export, no filters), ICRG geopolitical scores, USCIRF scores, Finance spend data (invoiced, 2023, pharma only), and Material Group Criticality from John Hamilton.
- **Optimus**: Internal supply chain optimization tool. Demand data currently lacks strength/count splits — using actual sales as a proxy.
- **River Logic**: Enterprise long-term optimization tool (target: June timeframe). Arturo Garcia is the key contact for data alignment.
- **AI tools**: Pablo's OFD/OFG AI tool (strategic, 3+ year horizon, distributed as GSX artifacts) vs. Steven's internal tool (node-level throughput, execution-adjacent). These are positioned as complementary, not replacements for Kinaxis/River Logic/Maestro.

## Installed plugins

Key plugins that affect how notes are written and queried:

- **Tasks** (`obsidian-tasks-plugin`): Emoji format. Custom statuses: `[ ]` Todo, `[x]` Done, `[/]` In Progress, `[-]` Cancelled. Done/cancelled dates auto-set.
- **Templater**: For note templates.
- **Dataview**: For dynamic queries across notes.
- **Excalidraw**: For diagrams embedded in notes.
- **Obsidian Git**: Vault is version-controlled via Git.

## Working with this vault via CLI

Use the `obsidian-cli` skill (not the `Read` tool) to read vault files. Common patterns:

```bash
obsidian read file="Note Name"
obsidian search query="search term"
obsidian tasks daily todo
obsidian append file="Note Name" content="- New line"
obsidian property:set name="status" value="done" file="Note Name"
```
