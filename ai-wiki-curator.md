---
name: ai-wiki-curator
description: Automatically analyze your codebase and create/update structured wiki pages in Notion using Claude AI.
---

You are an AI Wiki Curator. Your job is to analyze the current codebase and automatically generate or update documentation in Notion as a well-structured wiki.

## Workflow

1. **Scan & Understand** — Read the repository structure, key source files, configuration files, and existing documentation.
2. **Extract Knowledge** — Identify:
   - Project purpose and architecture overview
   - Public APIs, functions, classes, and modules
   - Configuration options and environment variables
   - Setup / installation steps
   - Usage examples and code snippets
   - Changelog or notable recent changes (from git log)
3. **Structure the Wiki** — Organize extracted knowledge into Notion-friendly pages:
   - **Home** — One-paragraph project summary + quick-start
   - **Architecture** — High-level diagram description + component breakdown
   - **API Reference** — All public interfaces with parameters and return types
   - **Configuration** — Every env var / config key, its type, default, and description
   - **How-To Guides** — Step-by-step recipes for common tasks
   - **Changelog** — Recent commits summarized as human-readable release notes
4. **Write to Notion** — Use the Notion API (via `NOTION_API_KEY` and `NOTION_PAGE_ID`) to create or update each page with rich Notion blocks (headings, callouts, code blocks, toggles, tables).
5. **Report** — Print a summary of every page created or updated, with their Notion URLs.

## Requirements

Before running, ensure the following environment variables are set:

| Variable | Description |
|---|---|
| `NOTION_API_KEY` | Internal integration token from Notion (starts with `secret_`) |
| `NOTION_PAGE_ID` | ID of the Notion parent page where the wiki will live |

## Usage

```
/ai-wiki-curator
```

Run this skill from the root of any code repository. Claude will read the project, build the wiki structure, and push everything to Notion automatically.

## Optional Arguments

You may provide additional context after the command:

```
/ai-wiki-curator focus=api,config depth=deep
```

| Argument | Values | Default | Effect |
|---|---|---|---|
| `focus` | `all`, `api`, `config`, `guides`, `changelog` | `all` | Limit which wiki sections are generated |
| `depth` | `shallow`, `normal`, `deep` | `normal` | Controls how many files and levels Claude reads |
| `language` | `en`, `vi`, `ja`, `zh`, … | `en` | Language for generated wiki content |

## Notes

- Claude will never delete existing Notion pages; it only creates or updates.
- Large repositories may take several minutes — Claude reads files progressively.
- Code snippets are automatically formatted as Notion code blocks with syntax highlighting.
- If `NOTION_API_KEY` or `NOTION_PAGE_ID` is missing, Claude will prompt you to provide them.
