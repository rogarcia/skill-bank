---
name: zettelkasten-workflow
description: Deep knowledge of Zettelkasten methodology, best practices, and vault structure. Use when user asks about Zettelkasten principles, workflow optimization, or how to organize their knowledge base.
---

# Zettelkasten Workflow Skill

You are an expert in Zettelkasten methodology and this user's specific knowledge management system. This skill provides comprehensive guidance on building and maintaining an effective Zettelkasten in Obsidian.

## User's Vault Structure

This vault follows a **research-backed Zettelkasten approach** with progressive refinement:

```
vault-name/
├── inbox.md           # Ultra-quick capture (< 30 sec bullets)
├── fleeting/          # Developed thoughts (1-2 min, timestamped)
├── literature/        # Source summaries (papers, books, articles)
├── permanent/         # Atomic concepts (organized by topic)
│   ├── foo/
│   ├── bar/
│   ├── baz/
├── maps-of-content/   # Topic navigation hubs
├── projects/          # Active work
└── templates/         # Reusable formats
```

## Core Zettelkasten Principles

### 1. Atomicity - One Idea Per Note
- Each permanent note contains a single, well-defined concept
- Atomic notes are easier to link precisely and reusable in multiple contexts
- Typically 200-500 words, but length varies naturally
- Must be understandable independently

**Anti-pattern**: Kitchen sink notes covering "everything about X"

### 2. Connectivity - Links Over Categories
> "The value of a Zettelkasten increases exponentially with the number of connections, not the quality of categorization."

- **Direct links are primary**: Link generously between related concepts
- Each new note should link to at least 2-3 existing notes
- Add context to links: `[[transformer-architecture|transformers]]`
- Create bidirectional connections naturally

**Supporting organization**:
- MOCs for navigation and thinking
- Tags for flexible grouping (use sparingly)
- Folders for workflow stages, not topics

### 3. Your Own Words - Rewrite, Don't Copy
All permanent notes should be written in the user's own language.

**Why**:
- Forces understanding (can't explain what you don't understand)
- Creates unique connections based on their mental model
- Makes notes searchable in their vocabulary

**Process**:
1. Read source material
2. Close it or put it aside
3. Explain the concept as if teaching someone
4. Add source attribution afterward

### 4. Progressive Refinement - Two-Stage Workflow

**Stage 1: Capture** (Throughout the day)
- Ultra-quick bullets → `inbox.md` (< 30 sec)
- Developed thoughts → `fleeting/` via Unique Note Creator plugin (1-2 min)
- Source material → `literature/`

**Stage 2: Process** (Scheduled)
- **Daily** (5-10 min): Triage inbox → proper notes
- **Weekly** (30 min): Convert fleeting/literature → permanent
- **Ongoing**: Link and organize via MOCs

**Critical**: Unprocessed captures become clutter. Regular maintenance is essential.

## User's Specific Workflows

### Daily Inbox Processing (5-10 min)

For each inbox bullet, decide:

1. **Convert to Fleeting Note**: Idea worth developing but needs more thought
   - Use Unique Note Creator plugin (creates timestamped note in `fleeting/`)
   - Expand into 2-3 sentences with context

2. **Convert to Literature Note**: Bullet from external source
   - Create in `literature/` with proper citation
   - Summarize key points + personal analysis

3. **Convert to Permanent Note**: Concept already clear and well-formed
   - Create atomic note in `permanent/`
   - Write in own words, link to related concepts

4. **Delete**: Not valuable after all
   - Delete without guilt (50%+ deletion rate is healthy!)

5. **Defer**: Can't process yet (waiting on something)
   - Add date and reason, but don't let sit > 1 week

**Goal**: Clear inbox to zero daily

### Weekly Fleeting Processing (30 min)

Review notes in `fleeting/` folder (1-7 days old):

- **Ready to synthesize?** → Create permanent note, delete fleeting
- **Needs more work?** → Enhance fleeting note, keep for now
- **Not valuable?** → Delete without guilt

Also:
- Update MOCs with new permanent notes
- Identify orphaned notes (no links in/out)
- Extract concepts from literature notes → permanent

### Monthly Deep Dive

- Audit permanent notes for quality
- Suggest MOC reorganization
- Find emerging topics for new MOCs
- Identify gaps in knowledge

## Metadata Philosophy

Use **minimal frontmatter** to reduce friction:

**Core fields (all notes)**:
```yaml
---
type: fleeting | literature | permanent | moc | project
tags: []
created: YYYY-MM-DD
---
```

**Additional fields by type**:
- Literature: `source`, `authors`, `year`
- Project: `status` (planning/active/paused/completed)

**What we DON'T track** (reduces maintenance burden):
- `modified` dates
- `status` on permanent notes
- `last-review` / `next-review`
- `aliases` by default (only when genuinely useful)

**Rationale**: Less metadata = faster note creation. Obsidian's search and links handle organization.

## Maps of Content (MOCs)

### When to Create
- **Create when**: 10+ notes exist on a topic, navigation is difficult
- **Don't create when**: Only 2-3 notes exist (too early)

### MOC Structure
**Start with**:
- Brief overview of the domain
- Organized sections (Core Concepts, Applications, etc.)
- Short descriptions for each linked note
- Cross-references to related MOCs

**Evolve to include**:
- Questions being explored
- Gaps in knowledge
- Learning paths (beginner → advanced)
- Synthesis of how ideas connect

**Remember**: MOCs are living documents, not static indexes.

## Naming Conventions

- Lowercase with hyphens: `transformer-architecture.md`
- Type prefixes when useful: `paper-`, `book-`, `project-`
- Fleeting notes: Use **Unique Note Creator** plugin (format: `YYYYMMDDHHmm.md`)
- Do **NOT** use emojis in filenames or headings
- No spaces or underscores in filenames

## Obsidian-Specific Features

### Auto-Updating Links
- Obsidian updates all links when renaming files
- Eliminates need for unique IDs in permanent notes
- Use descriptive filenames instead

### Graph View
- Discover unexpected connections
- Identify orphaned notes (no links in/out)
- Spot clusters that might need MOCs

### Search
- Powerful search - use it liberally
- Don't over-rely on folder organization
- Search by content, not just filename

## Common Pitfalls to Avoid

1. **Over-Organizing**: Spending more time organizing than writing
   - Solution: Default to writing, let structure emerge

2. **Premature Structure**: Creating detailed taxonomies before having notes
   - Solution: Wait until ~10 notes before creating a MOC

3. **Copy-Paste Syndrome**: Notes are just excerpts from sources
   - Solution: Close source and explain in own words

4. **Link Hoarding**: Linking every keyword, making links meaningless
   - Solution: Link only meaningful relationships, quality > quantity

5. **Fleeting Note Debt**: Hundreds of unprocessed fleeting notes
   - Solution: Process weekly, delete ruthlessly (50%+ deletion is good)

## Decision Tree for Capturing

```
User wants to capture something:
  ├─ Ultra-quick (one sentence, no context)?
  │  → Add bullet to inbox.md
  │
  ├─ Slightly developed (2-3 sentences with context)?
  │  → Create fleeting note via Unique Note Creator plugin
  │
  ├─ From external source (paper/article)?
  │  → Create literature note
  │
  └─ Fully formed concept?
     → Create permanent note directly
```

## Quality Indicators

**Good permanent note**:
- Atomic (one idea)
- In own words
- Linked to 2-5 related concepts
- Understandable standalone
- Includes examples or analogies

**Unnecessary** (don't let these block note creation):
- Perfect prose
- Comprehensive coverage
- Citation format
- Publication-ready

**Remember**: Notes evolve. Better to capture imperfectly than wait for perfect.

## Success Metrics

**Good signs**:
- Creating new notes feels easy
- Finding old notes is quick
- Unexpected connections appear
- Writing projects flow from notes
- Notes referenced regularly

**Warning signs** (fix by simplifying, not adding complexity):
- Avoiding note creation (too much friction)
- Can't find notes you know exist
- Notes feel isolated/disconnected
- Fleeting notes pile up unprocessed
- System feels like a burden

## When Assisting Users

### Creating Literature Notes
1. Summarize source in user's own words (not copy-paste)
2. Extract key insights and contributions
3. Add critical analysis (strengths, limitations, questions)
4. Identify concepts for potential permanent notes
5. Link to related existing notes

### Creating Permanent Notes
1. Explain concept as if teaching someone
2. Use clear examples and analogies
3. Add mermaid diagrams or tables for clarity
4. Connect to 3-5 related concepts via wikilinks
5. Keep atomic (one concept per note)

### Maintaining MOCs
1. Add new permanent notes with brief descriptions
2. Organize into logical sections/hierarchies
3. Create sub-MOCs when exceeding ~50 items
4. Include learning paths (beginner → advanced)
5. Cross-reference related MOCs

### Visual Representations
- Prefer **mermaid diagrams** for flowcharts and concept relationships
- Use **tables** for structured comparisons
- Keep visuals simple and focused

## Important Reminders

- Do **NOT** use emojis in filenames or headings
- Always check if concept already has a note before creating new one
- Suggest connections between new and existing notes
- Maintain atomic note principle (one concept per note)
- Keep permanent notes timeless (avoid "recently" or dated references)
- Use **minimal frontmatter** - only fields with clear purpose
- Link generously - connections > categories
- Let structure emerge from content, don't force it upfront

## Core Philosophy

This vault prioritizes **thinking and connections over perfect organization**. The system should feel natural, not bureaucratic. When in doubt, simplify.
