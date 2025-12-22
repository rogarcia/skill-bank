---
name: create-moc
description: Create new Map of Content with proper structure and organization
arguments:
  topic:
    type: string
    description: The topic or domain for the new MOC (optional - will prompt if not provided)
    required: false
---

# Create MOC Command

You are helping the user create a new Map of Content (MOC) for their knowledge base. MOCs are navigation hubs that organize related concepts and provide structure for thinking about a domain.

## Context

**What is a MOC?**
- Navigation hub that organizes related permanent notes
- More than a static index - a "workstation" for exploring connections
- Provides learning paths and context for a topic
- Created when 10+ notes exist on a topic

**User's MOC structure**:
- Stored in `maps-of-content/` folder
- Named with `-moc` suffix: `topic-moc.md`
- Linked from main `maps-of-content.md` directory
- Uses minimal frontmatter

## Your Task

{{#if topic}}
The user wants to create a MOC for: "{{topic}}"
{{else}}
The user wants to create a MOC but hasn't specified the topic yet.

First, ask: "What topic or domain would you like to create a MOC for?"

Wait for their response before proceeding.
{{/if}}

## Workflow

### Step 1: Validate MOC Creation

Before creating, check if a MOC is warranted:

1. **Search for existing notes** on this topic:
   - Use Glob/Grep to find related permanent notes
   - Count how many exist
   - Check if MOC already exists

2. **Assess readiness**:

**If < 10 notes exist**:
```
I found only [N] notes related to "[topic]".

MOCs work best with 10+ notes. With fewer notes, consider:

Options:
A) Create it anyway (you're about to add more notes)
B) Wait until you have more notes (I'll remind you when you hit 10)
C) Add this topic as a section in an existing MOC

What would you prefer?
```

**If MOC already exists**:
```
Found existing MOC: [[maps-of-content/topic-moc]]

Would you like to:
A) Update/enhance the existing MOC
B) Create a new sub-MOC for a specific aspect
C) Cancel (use existing MOC)

What would you prefer?
```

**If 10+ notes exist** (good to proceed):
```
Perfect! I found [N] notes related to "[topic]".

This is a good size for creating a MOC. Let's build a navigation
hub for your knowledge in this domain.
```

### Step 2: Gather Related Notes

1. Search vault for notes related to the topic
2. Organize findings by relevance and subtopic
3. Present to user:

```
Found [N] notes related to "[topic]":

Core Concepts ([X] notes):
- [[note-1]] - [Brief description from content]
- [[note-2]] - [Brief description from content]

Applications ([Y] notes):
- [[note-3]] - [Brief description from content]
- [[note-4]] - [Brief description from content]

Related Topics ([Z] notes):
- [[note-5]] - [Brief description from content]

I'll organize these into a MOC structure. Any notes I missed or
categories you'd like to add?
```

### Step 3: Create MOC Structure

Create the MOC file in `maps-of-content/` with this structure:

```yaml
---
type: moc
tags: [topic, keywords]
created: YYYY-MM-DD
---

# [Topic] - Map of Content

> [One-sentence overview of what this topic encompasses]

## Overview

[2-3 paragraphs explaining the domain, why it matters, and what this
MOC helps you navigate. Written in user's own words.]

## Core Concepts

The fundamental building blocks of [topic]:

- [[concept-1]] - [What it is and why it matters]
- [[concept-2]] - [What it is and why it matters]
- [[concept-3]] - [What it is and why it matters]

## [Custom Section 2]

[Organize notes into logical sections based on the topic.
Common patterns: Applications, Techniques, History, People,
Companies, Case Studies, Tools, etc.]

- [[note-1]] - [Brief description]
- [[note-2]] - [Brief description]

## [Custom Section 3]

- [[note-1]] - [Brief description]
- [[note-2]] - [Brief description]

## Learning Path

**Beginner**: Start here if you're new to [topic]
1. [[intro-note]] - Foundational concept
2. [[basic-note]] - Basic application
3. [[example-note]] - Concrete example

**Intermediate**: Once you understand the basics
1. [[advanced-concept]] - Deeper theory
2. [[practical-application]] - Real-world use
3. [[comparison-note]] - Trade-offs and choices

**Advanced**: Deep dives and cutting edge
1. [[complex-topic]] - Advanced technique
2. [[research-note]] - Current research
3. [[open-questions]] - Unsolved problems

## Key Questions

Questions I'm exploring in this domain:

- Question 1 that guides your thinking?
- Question 2 you're investigating?
- Question 3 without clear answers yet?

## Related MOCs

- [[related-moc-1]] - How it connects
- [[related-moc-2]] - How it connects

## Gaps in Knowledge

Areas I want to develop further:

- [ ] Gap 1 to fill
- [ ] Gap 2 to explore
- [ ] Gap 3 to understand

---

**Statistics**: [N] notes • Created [date] • Last major update [date]
```

### Step 4: Update MOC Directory

1. Read `maps-of-content.md`
2. Add new MOC to appropriate section:

```markdown
## [Category]

- [[topic-moc]] - [One-sentence description of what this MOC covers]
```

3. Confirm update

### Step 5: Present and Confirm

```
Created: maps-of-content/[topic-moc].md

Structure:
- Overview and context
- [N] notes organized into [X] sections
- Learning path (beginner → advanced)
- Key questions and knowledge gaps
- Links to [Y] related MOCs

Also updated: maps-of-content.md (added to directory)

Your new MOC is ready! You can:
- Add more notes as you create them
- Refine sections as understanding grows
- Use it as a writing workstation

Would you like me to suggest any improvements or additional sections?
```

## MOC Design Principles

### Organization Patterns

Choose appropriate sections based on the topic:

**For Concepts/Theories**:
- Core Concepts
- Key Principles
- Applications
- Historical Development
- Criticisms and Limitations

**For Technologies/Tools**:
- Fundamentals
- Core Features
- Use Cases
- Implementations
- Comparisons
- Resources

**For Practices/Methods**:
- Principles
- Techniques
- Case Studies
- Common Pitfalls
- Best Practices

**For Domains/Fields**:
- Key Concepts
- Major Thinkers/Contributors
- Subfields
- Applications
- Current Research
- Open Questions

### Learning Path Guidelines

**Good learning paths**:
- 3-5 notes per level (beginner/intermediate/advanced)
- Build progressively (each level assumes previous)
- Mix theory and practice
- Include concrete examples

**Avoid**:
- Listing every note (overwhelming)
- Random order (confusing)
- Only theory or only practice (incomplete)

### MOC Quality Checklist

Before finalizing, verify:

- [ ] Contains 10+ notes (or user accepted fewer)
- [ ] Notes organized into logical sections
- [ ] Each note has brief description
- [ ] Learning path provides progressive structure
- [ ] Key questions reflect active thinking
- [ ] Related MOCs linked (if applicable)
- [ ] Added to maps-of-content.md directory
- [ ] Written in user's own voice

## Edge Cases

### User wants sub-MOC

If the topic is a subset of an existing MOC:

```
"[subtopic]" seems like a subset of [[existing-moc]].

Options:
1. Create sub-MOC: maps-of-content/[subtopic-moc].md
   - Link from [[existing-moc]] as a sub-section
   - Focused on just this aspect

2. Add as section in [[existing-moc]]
   - Simpler, keeps related content together
   - Good if only 3-5 notes on subtopic

Which approach fits better?
```

### Very large MOC (50+ notes)

```
You have [N] notes on this topic - that's a lot!

Consider:
1. Create main MOC with sub-sections
2. Create sub-MOCs for major areas
3. Link sub-MOCs from main MOC

This creates a hierarchical structure that's easier to navigate.

Shall we break this down into sub-topics?
```

### MOC for emerging topic (5-9 notes)

```
You have [N] notes - close to the 10-note threshold.

I can create the MOC now and you'll fill it out as you learn more,
or we can wait until you have a few more notes.

Creating now has benefits:
- Provides structure for future notes
- Shows knowledge gaps clearly
- Acts as learning workstation

What would you prefer?
```

## Integration with Other Commands

After creating MOC:
- Suggest: "Want to find more connections between these notes? Try /link-notes"
- If user is actively researching: "Capture new ideas with /capture"
- For existing fleeting notes: "Process fleeting notes with /process-fleeting to add to this MOC"

## Important Reminders

- **MOCs are living documents**: They evolve as understanding grows
- **No emojis**: Keep filenames and headings clean
- **Minimal metadata**: Only essential frontmatter fields
- **User's voice**: Write overview and descriptions in their language
- **Navigation focus**: MOCs help find and connect, not just list
- **Questions matter**: Include active thinking, not just organization
