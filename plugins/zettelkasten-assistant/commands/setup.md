---
name: setup
description: Initialize a new Zettelkasten vault with standard directory structure and templates
---

# Setup Command

You are helping the user set up a fresh Obsidian vault for Zettelkasten knowledge management.

## Context

This command creates the complete vault structure needed for the Zettelkasten workflow:
- Core directories for the progressive refinement pipeline
- Template files for consistent note creation
- An inbox file for quick captures

## Your Task

Set up the vault in the current working directory. Follow these steps exactly:

### Step 1: Validate No Existing Structure

First, check if any Zettelkasten structure already exists. Use the following checks:

1. Use Glob to check for `inbox.md` file
2. Use Glob to check for any files matching `fleeting/**` pattern (indicates fleeting directory with content)
3. Use Glob to check for any files matching `permanent/**` pattern (indicates permanent directory with content)
4. Use Glob to check for any files matching `maps-of-content/**` pattern (indicates MOC directory with content)
5. Use Bash `ls` to check if `fleeting/`, `permanent/`, or `maps-of-content/` directories exist (even if empty)

If **ANY** of these checks find existing structure, **STOP** and inform the user:

```
Vault Already Initialized

Found existing Zettelkasten structure:
- [list what was found]

Setup cannot proceed to protect your existing notes.

Options:
1. Use your existing vault - it's ready for /capture
2. Choose a different directory for a new vault
3. Back up and remove existing structure to reinitialize
```

If **NONE** exist, proceed to creation.

### Step 2: Create Directory Structure

Create these directories (all will be empty initially):

1. `fleeting/`
2. `literature/`
3. `permanent/`
4. `maps-of-content/`
5. `projects/`
6. `templates/`

### Step 3: Create inbox.md

Create `inbox.md` with this content:

```markdown
# Inbox

Quick captures go here. Each bullet should take < 30 seconds to write. Process daily with `/process-inbox`.

## Today's Captures

-

## Older Captures

<!-- Move bullets here if not processed same day -->
```

### Step 4: Create Templates

Create these template files in `templates/`:

#### templates/fleeting-template.md

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

#### templates/literature-template.md

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

#### templates/permanent-template.md

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

#### templates/moc-template.md

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

### Step 5: Confirm Success

After creating everything, display this summary:

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

## Important Notes

- Do NOT create any topic subfolders under `permanent/` - user will create these organically
- Do NOT create sample notes - only templates
- Do NOT use emojis in any file content
- All directories should be created empty (no `.gitkeep` files needed)
