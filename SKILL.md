---
name: draft-provisional
description: Draft a USPTO provisional patent application from a technical disclosure or IP analysis document. Generates all required sections in a single Markdown file ready for conversion to .docx.
---

# Draft Provisional Patent Application

Generate a complete, high-quality USPTO provisional patent application from an existing technical disclosure or IP analysis document.

## Usage

```
/draft-provisional <source>
```

Where `<source>` is one of:
- A **Notion page URL** containing a technical disclosure or IP analysis
- A **local file path** to a markdown or text document describing the invention
- A **URL to any web-accessible document** describing the invention (the skill will fetch it)

## What This Skill Does

1. **Reads the source document** — fetches the Notion page, web URL, or local file
2. **Extracts the invention** — identifies the core claims, prior art gaps, and technical details
3. **Searches prior art** — uses a Google Patents skill and/or patent MCP server to validate novelty
4. **Drafts all 9 patent sections** — following USPTO provisional application format
5. **Generates the output** — a single Markdown file in `docs/patents/` ready for review

## Instructions for Claude

When this skill is invoked:

### Step 1: Load Source Material

- If the source is a Notion URL, fetch it using a Notion MCP tool (e.g., `mcp__plugin_Notion_notion__notion-fetch` or equivalent in your environment)
- If the source is a generic web URL, use WebFetch
- If the source is a local file, read it directly
- If the source document references a related technical disclosure, fetch that as well to enrich the technical depth

### Step 2: Gather Inventor Information

Ask the user for:
- **Inventor name(s)** — full legal names of all inventors
- **Inventor address(es)** — street, city, state/province, postal code, country
- **Entity type** — Micro Entity, Small Entity, or Large Entity (default: Micro Entity)
- **Assignee** — company name if assigned (e.g., "Acme Corp.")

### Step 3: Prior Art Validation

Before drafting, run a quick prior art check to strengthen the application:
- Use a `google-patents-search` skill (or equivalent) to search for the closest prior art terms from the source doc
- Note the 3-5 closest prior art references found. These will be cited in the Background section
- Confirm the novelty gap identified in the IP analysis still holds

### Step 4: Draft the Application

Write the complete provisional application with these sections:

#### Cover Page
```
PROVISIONAL PATENT APPLICATION

Title: [Descriptive technical title, 10-15 words]
Inventor(s): [Names]
Assignee: [Company if applicable]
Filing Date: [Today's date]
Entity Status: [Micro/Small/Large]
```

#### 1. Title of Invention
- Descriptive, technical, 10-15 words
- Should capture the core innovation without marketing language
- Example: "Method and System for Adaptive Bandwidth Allocation in Distributed Networks"

#### 2. Cross-Reference to Related Applications
- "This application claims priority to no prior applications." (for first filing)
- Or reference any related provisionals if they exist

#### 3. Field of the Invention
- 2-3 sentences identifying the technical field
- Use CPC classification terms where possible (e.g., G06F21/56 for AI security, H04L for networking)

#### 4. Background of the Invention
- **Problem statement** — what technical problem exists (3-5 paragraphs)
- **Prior art limitations** — cite the specific prior art from the IP analysis doc and the prior art search, explaining what each fails to address
- **Do NOT say "the prior art is deficient"** — instead describe what is missing factually
- Keep under 500 words

#### 5. Summary of the Invention
- **Core invention** — 1-2 paragraphs describing the solution at a high level
- **Key advantages** — bullet list of 3-5 technical advantages over prior art
- **Embodiments preview** — "In one embodiment..." for 2-3 variations
- Keep under 400 words

#### 6. Brief Description of the Drawings
- List 3-7 figures that SHOULD accompany the application
- Format: "FIG. 1 is a [diagram type] illustrating [what it shows]."
- Common figures: system architecture, method flowchart, data flow, sequence diagram, component detail
- Note: actual figures can be added later before non-provisional conversion. The provisional just needs the descriptions.

#### 7. Detailed Description of Preferred Embodiments
- **This is the most important section** — it must be comprehensive enough to support all claims
- **Minimum 2,500 words**, target 4,000-6,000 words
- Structure:
  - Overall system architecture description (reference FIG. 1)
  - Detailed description of each novel component
  - Method steps (reference flowchart FIG.)
  - Data structures and formats where relevant
  - At least 2 alternative embodiments ("In another embodiment...")
  - Implementation details: protocols, algorithms, data flows
  - Performance characteristics where known (latency, throughput, accuracy)
- **Use reference numerals** — assign numbers to components (e.g., "controller 102", "policy engine 104") and use them consistently
- **Enable reproduction** — a skilled practitioner must be able to build it from this description (35 USC 112 requirement)
- **Avoid vague language** — replace "may" with "in one embodiment", replace "etc." with specific examples

#### 8. Claims
Draft **15-25 claims** organized as:

**Independent Claims (3-4):**
- Claim 1: Method claim. "A method for [core innovation], comprising: [steps]"
- Claim N: System claim. "A system for [core innovation], comprising: [components]"
- Claim M: Computer-readable medium claim. "A non-transitory computer-readable medium storing instructions that, when executed, cause a processor to: [steps]"

**Dependent Claims (12-20):**
- Each narrows an independent claim with specific details
- Cover: specific implementations, parameters, thresholds, alternatives, components
- Use "The method of claim 1, further comprising..." or "The method of claim 1, wherein..."

**Claims drafting rules:**
- Each claim is ONE sentence (use semicolons and "wherein" to connect)
- Use consistent terminology matching the Detailed Description
- Independent claims should be broad enough to be hard to design around
- Dependent claims should add specific implementation details that strengthen enforceability
- Include at least one claim covering the specific prior art gap identified in the IP analysis

#### 9. Abstract
- **Exactly 150 words or fewer**
- Single paragraph summarizing the invention
- Must match the broadest independent claim
- No legal language, no "the present invention"

### Step 5: Save Output

Save the complete application as:
```
docs/patents/provisional_patent_<short_name>_<YYYYMMDD>.md
```

Example: `docs/patents/provisional_patent_bandwidth_allocation_20260329.md`

### Step 6: Quality Check

After drafting, self-evaluate against this checklist:
- [ ] Title is descriptive and technical (not marketing)
- [ ] Background cites specific prior art with factual gap descriptions
- [ ] Detailed Description is 2,500+ words with reference numerals
- [ ] At least 2 alternative embodiments described
- [ ] 15+ claims with 3+ independent claims (method, system, CRM)
- [ ] Claims use consistent terminology from Detailed Description
- [ ] Abstract is under 150 words
- [ ] No use of "the present invention" anywhere
- [ ] No vague terms: "etc.", "and the like", "such as" without specific examples
- [ ] A skilled practitioner could build it from the description alone

Report the checklist results and any items that need attention.

## Patent Sections Generated

| # | Section | Target Length | Priority |
|---|---------|--------------|----------|
| 1 | Title | 10-15 words | Required |
| 2 | Cross-Reference | 1 sentence | Required |
| 3 | Field of Invention | 2-3 sentences | Required |
| 4 | Background | 300-500 words | Required |
| 5 | Summary | 200-400 words | Required |
| 6 | Brief Description of Drawings | 3-7 figure descriptions | Required |
| 7 | Detailed Description | 2,500-6,000 words | **Critical** |
| 8 | Claims | 15-25 claims | **Critical** |
| 9 | Abstract | Max 150 words | Required |

## Quality Targets

- **Detailed Description** must enable reproduction by a skilled practitioner (35 USC 112)
- **Claims** must be supported by the Detailed Description. Every claim element must appear in the description.
- **Prior art** must be cited factually in the Background. No disparaging language.
- **Terminology** must be consistent throughout. Create a glossary if needed.
- **Reference numerals** must be used in Detailed Description and match figure descriptions.

## Notes

- This generates a **provisional** application. It establishes a priority date but does not require formal claims or drawings.
- However, including well-drafted claims and figure descriptions NOW makes the non-provisional conversion (within 12 months) much easier and cheaper.
- The provisional must contain enough detail to support all claims in the eventual non-provisional.
- After drafting, run a patent-filing QC skill (e.g., [Claude-Skill-for-Patent-Filing-Quality-Control](https://github.com/LeonardHope/Claude-Skill-for-Patent-Filing-Quality-Control)) to run quality checks before filing.

## Recommended Companion Skills

- **Prior-art search:** [Google-Patents-Natural-Language-API-Search](https://github.com/LeonardHope/Google-Patents-Natural-Language-API-Search) — full-text claims and description search across 166M+ patents in 17+ jurisdictions, backed by Google's BigQuery patents dataset.
- **Filing QC:** [Claude-Skill-for-Patent-Filing-Quality-Control](https://github.com/LeonardHope/Claude-Skill-for-Patent-Filing-Quality-Control) — 70+ automated checks across the spec, ADS, declaration, and drawings before submission.
- **Diagrams:** [drawio MCP server](https://www.npmjs.com/package/@drawio/mcp) — generate the figures referenced in the Brief Description of Drawings.

## Disclaimer

This skill helps you produce a provisional patent application that meets USPTO formal requirements. It does **not** replace patent counsel. For high-stakes filings, novel art areas, or when claim scope is critical, have an IP attorney review before submission.
