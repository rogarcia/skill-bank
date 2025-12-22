---
name: vault-setup
description: Knowledge of vault structure, templates, and setup process. Use when setting up a new vault, validating vault structure, or referencing standard templates for notes.
---

# Vault Setup Knowledge

This skill provides the canonical vault structure, file templates, and setup procedures for the Zettelkasten assistant.

## Canonical Directory Structure

A properly configured vault has the following structure:

```
vault-root/
├── inbox.md                    # Quick capture (< 30 sec bullets)
├── fleeting/                   # Developed thoughts (timestamped)
├── literature/                 # Source summaries
├── permanent/                  # Atomic knowledge (user creates topic folders)
├── maps-of-content/            # Navigation hubs (MOCs)
├── projects/                   # Active work
└── templates/                  # Note templates
    ├── fleeting-template.md
    ├── literature-template.md
    ├── permanent-template.md
    └── moc-template.md
```

### Directory Purposes

- **inbox.md**: Single file for ultra-quick captures. Bullets only, processed daily.
- **fleeting/**: Timestamped notes for developed thoughts. Created via Unique Note Creator plugin.
- **literature/**: Summaries of external sources with citations.
- **permanent/**: Atomic concept notes organized by topic subfolders (user creates as needed).
- **maps-of-content/**: Topic navigation hubs linking related permanent notes.
- **projects/**: Active work and project-specific notes.
- **templates/**: Reusable note structures for consistency.

## Validation Rules

To determine if a vault already has Zettelkasten structure, check for ANY of:

1. `inbox.md` file exists (use Glob)
2. `fleeting/` directory exists (use Bash `ls` since Glob won't detect empty directories)
3. `permanent/` directory exists (use Bash `ls`)
4. `maps-of-content/` directory exists (use Bash `ls`)
5. Files exist in `fleeting/**`, `permanent/**`, or `maps-of-content/**` (use Glob)

If **ANY** of these exist, the vault is considered already initialized. Setup should **fail** to prevent accidental overwrites.

**Note**: Glob only finds files, not empty directories. Use Bash `ls` to detect directories that exist but contain no files.

## Setup Principles

1. **Fail-safe**: Never overwrite existing structure
2. **Minimal friction**: Create everything in one step
3. **Ready to use**: Templates included, user can start immediately
4. **Empty permanent/**: User creates topic folders as they write notes
5. **No sample content**: Templates only, no fake notes to confuse

## Template Library

All templates follow these conventions:
- Minimal frontmatter: `type`, `tags`, `created`
- No emojis in filenames or content
- Lowercase with hyphens for filenames
- YYYY-MM-DD date format

### inbox.md Content

```markdown
# Inbox

Quick captures go here. Each bullet should take < 30 seconds to write. Process daily with `/process-inbox`.

## Today's Captures

-

## Older Captures

<!-- Move bullets here if not processed same day -->
```

### templates/fleeting-template.md

```markdown
---
type: fleeting
tags: []
created: YYYY-MM-DD
---

# [Descriptive Title]

[2-3 sentences explaining the thought with context]

## Context

[Why this matters, where the idea came from]

## Related Concepts

- [[potential-link-1]]
- [[potential-link-2]]

## Questions to Explore

- Question 1?
- Question 2?
```

### templates/literature-template.md

```markdown
---
type: literature
source: [URL or citation]
authors: [Author names]
year: YYYY
tags: []
created: YYYY-MM-DD
---

# [Source Title]

## Summary

[1-2 paragraphs in your own words - not copy-paste]

## Key Insights

- Insight 1
- Insight 2
- Insight 3

## Critical Analysis

**Strengths:**
-

**Limitations:**
-

**Questions Raised:**
-

## Concepts for Permanent Notes

- [[concept-1]] - Brief description
- [[concept-2]] - Brief description

## Related Notes

- [[existing-note-1]]
- [[existing-note-2]]
```

### templates/permanent-template.md

```markdown
---
type: permanent
tags: []
created: YYYY-MM-DD
---

# [Concept Name]

> One-sentence summary of the concept

## Core Explanation

[Explain as if teaching someone. Use clear language, examples, and analogies. 200-500 words typical.]

## Examples

[1-2 concrete examples that illustrate the concept]

## Why It Matters

[Brief context on importance, applications, or implications]

## Related Concepts

- [[related-note-1]] - How they connect
- [[related-note-2]] - How they connect
- [[related-note-3]] - How they connect

## Questions for Future Exploration

- Unanswered question 1?
- Unanswered question 2?

## Sources

- [[literature-note-1]]
```

### templates/moc-template.md

```markdown
---
type: moc
tags: []
created: YYYY-MM-DD
---

# [Topic] - Map of Content

> One-sentence overview of what this topic encompasses

## Overview

[2-3 paragraphs explaining the domain, why it matters, and what this MOC helps navigate]

## Core Concepts

The fundamental building blocks:

- [[concept-1]] - What it is and why it matters
- [[concept-2]] - What it is and why it matters
- [[concept-3]] - What it is and why it matters

## [Section Name]

Organize notes into logical sections:

- [[note-1]] - Brief description
- [[note-2]] - Brief description

## Learning Path

**Beginner** - Start here:
1. [[intro-concept]]
2. [[basic-application]]
3. [[example-note]]

**Intermediate** - Build on basics:
1. [[advanced-concept]]
2. [[practical-application]]

**Advanced** - Deep dives:
1. [[complex-topic]]
2. [[research-note]]

## Key Questions

Questions being explored in this domain:

- Question 1?
- Question 2?

## Related MOCs

- [[related-moc-1]] - How it connects

## Gaps to Fill

Areas to develop further:

- [ ] Gap 1
- [ ] Gap 2
```

## Error Messages

### Structure Already Exists

When validation finds existing structure:

```
Vault Already Initialized

Found existing Zettelkasten structure:
[list which items were found]

Setup cannot proceed to protect your existing notes.

Options:
1. Use your existing vault - it's ready for /capture
2. Choose a different directory for a new vault
3. Back up and remove existing structure to reinitialize

Need help? Ask about migrating or restructuring your vault.
```

### Successful Setup

After successful creation:

```
Vault Setup Complete

Created structure:
- inbox.md (quick capture file)
- fleeting/ (for developed thoughts)
- literature/ (for source notes)
- permanent/ (for atomic concepts)
- maps-of-content/ (for navigation)
- projects/ (for active work)
- templates/ (4 note templates)

Next Steps:
1. Try /capture to add your first thought
2. Run /process-inbox daily to triage captures
3. Run /process-fleeting weekly to synthesize notes

Your Zettelkasten is ready. Start capturing!
```
