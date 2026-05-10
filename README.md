# Claude Skill: Draft Provisional Patent

A Claude Code skill that drafts a complete USPTO provisional patent application from a technical disclosure or IP analysis document. Generates all 9 required sections (cover, title, field, background, summary, drawings description, detailed description, claims, abstract) in a single Markdown file ready for conversion to `.docx`.

## What it produces

- 9 USPTO-format sections following provisional application requirements
- 15-25 claims (3-4 independent, 12-20 dependent)
- 2,500-6,000 word detailed description with consistent reference numerals
- Abstract under 150 words
- Self-QC checklist run after drafting

## Installation

### As a project skill

```bash
mkdir -p .claude/skills/draft-provisional
curl -o .claude/skills/draft-provisional/SKILL.md \
  https://raw.githubusercontent.com/bradthebeeble/Claude-Skill-Draft-Provisional/main/SKILL.md
```

### As a user skill (available in all projects)

```bash
mkdir -p ~/.claude/skills/draft-provisional
curl -o ~/.claude/skills/draft-provisional/SKILL.md \
  https://raw.githubusercontent.com/bradthebeeble/Claude-Skill-Draft-Provisional/main/SKILL.md
```

## Usage

```
/draft-provisional <source>
```

Where `<source>` is one of:
- A Notion page URL (requires the Notion MCP plugin)
- A local file path to a markdown or text technical disclosure
- A web URL to any document describing the invention

Example:

```
/draft-provisional ./docs/technical-disclosure.md
```

The skill will:
1. Read the source document
2. Ask you for inventor info (names, addresses, entity type, assignee)
3. Run prior-art search (if a companion skill is available)
4. Draft all 9 sections
5. Save to `docs/patents/provisional_patent_<short_name>_<YYYYMMDD>.md`
6. Run a self-QC checklist

## Recommended companion skills

This skill works well alongside:

- [Google-Patents-Natural-Language-API-Search](https://github.com/LeonardHope/Google-Patents-Natural-Language-API-Search) — prior-art search across 166M+ patents in 17+ jurisdictions, backed by BigQuery.
- [Claude-Skill-for-Patent-Filing-Quality-Control](https://github.com/LeonardHope/Claude-Skill-for-Patent-Filing-Quality-Control) — 70+ pre-filing QC checks across spec, ADS, declaration, and drawings.
- [@drawio/mcp](https://www.npmjs.com/package/@drawio/mcp) — generate the figures referenced in the Brief Description of Drawings.

## Disclaimer

This skill helps produce a provisional patent application that meets USPTO formal requirements. It does **not** replace patent counsel. For high-stakes filings, novel art areas, or when claim scope is critical, have an IP attorney review before submission.

## License

MIT. See [LICENSE](LICENSE).
