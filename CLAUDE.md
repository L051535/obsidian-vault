# CLAUDE.md

This file provides guidance to Claude Code when working with this repository.

## What this vault is

A personal Obsidian knowledge vault used for work at Eli Lilly, focused on **Supply Chain Resiliency** and related strategic initiatives. Notes capture meeting summaries, project context, working data documentation, and long-term knowledge — not code.

## Vault structure

```
Inbox/                              — Capture zone; triage regularly into Projects/ or Zettelkasten/
Projects/
  Supply Chain Resiliency/
    Everstream/                     — Everstream risk platform notes and contacts
    Supplier of Focus/              — Supplier risk scoring data sources and logic
    Optimus/                        — Optimus tool data working sessions
    SCRM/                           — Supply Chain Risk Management council notes
    Foundayo Theft Risk/            — Foundayo theft risk project
Areas/
  Daily Notes/                      — Daily notes
  Personal Development/             — Leadership and growth notes
  Obsidian Dashboards/              — Dashboard notes
  Obsidian Vault Setup/             — Vault configuration and system notes
Resources/
  Notes Framework/                  — PARA, Zettelkasten, and system reference notes
  Technology Setup/                 — Step-by-step configuration guides (SAP filters, tool setups)
  AI Tools/                         — Notes on internal AI tools
  Templates/                        — Note templates (in progress)
Zettelkasten/
  Literature Notes/                 — Source material processed in your own words
  Permanent Notes/                  — Atomic, evergreen concepts that outlive projects
Archive/                            — Closed projects and inactive notes
Attachments/                        — Images, PDFs, and file attachments
```

## Note-taking system

This vault uses a **PARA + Zettelkasten hybrid**:
- **PARA** provides the folder skeleton and project lifecycle management
- **Zettelkasten** provides the long-term knowledge layer inside `Zettelkasten/`
- **Inbox** serves as the fleeting notes layer — no separate Fleeting Notes folder
- Meeting notes go in `Projects/` if tied to an active initiative, `Areas/` if recurring with no end date
- Insights worth keeping beyond a project get distilled into `Zettelkasten/Permanent Notes/`

## Key domain context

- **Everstream**: Third-party geopolitical/supplier risk platform. Managed by this user; Brooke to oversee post-training. Altana is a separate tool under Viviane Miyazato (Vivi).
- **Supplier of Focus (SoF)**: Annual supplier risk scoring exercise. Data sources include SAP ZS163n (raw export, no filters), ICRG geopolitical scores, USCIRF scores, Finance spend data (invoiced, 2023, pharma only), and Material Group Criticality from John Hamilton.
- **Optimus**: Internal supply chain optimization tool. Demand data currently lacks strength/count splits — using actual sales as a proxy.
- **SCRM**: Supply Chain Risk Management council.
- **River Logic**: Enterprise long-term optimization tool. Arturo Garcia is the key contact for data alignment.
- **AI tools**: Pablo's OFG AI tool (strategic, 3+ year horizon) vs. Steven's internal tool (node-level throughput, execution-adjacent). Positioned as complementary, not replacements for Kinaxis/River Logic/Maestro.

## Installed plugins

- **Tasks** (`obsidian-tasks-plugin`): Emoji format. Custom statuses: `[ ]` Todo, `[x]` Done, `[/]` In Progress, `[-]` Cancelled. Done/cancelled dates auto-set.
- **Templater**: Note templates.
- **Dataview**: Dynamic queries across notes.
- **Excalidraw**: Diagrams embedded in notes.
- **Obsidian Git**: Vault is version-controlled via Git.

## Working with this vault via CLI

Always use the `obsidian-cli` skill — not the `Read` tool — to read vault files. Use `obsidian folders` to list all folders including empty ones (Glob misses empty directories).

```bash
obsidian read file="Note Name"
obsidian search query="search term"
obsidian folders vault="Eli Lilly Vault"
obsidian append file="Note Name" content="- New line"
obsidian tasks daily todo
obsidian property:set name="status" value="done" file="Note Name"
```
