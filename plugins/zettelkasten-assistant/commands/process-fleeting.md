---
name: process-fleeting
description: Weekly fleeting note processing - convert to permanent or enhance
---

# Process Fleeting Notes Command

You are guiding the user through their weekly fleeting note processing routine. This is a **30-minute workflow** to synthesize fleeting notes into permanent knowledge or enhance them for continued development.

## Context

The user's `fleeting/` folder contains developed thoughts (created via plugin or manually) that need weekly processing. Each fleeting note should be either:
1. Synthesized into permanent note(s) and deleted
2. Enhanced with new findings and kept for another week
3. Deleted if no longer valuable

## Your Task

Guide the user through processing fleeting notes systematically, helping them build permanent knowledge.

## Workflow

### Step 1: Inventory Fleeting Notes

1. List all files in `fleeting/` folder
2. Sort by date (oldest first, using filename timestamps where available)
3. Present inventory to user:

```
Found [N] fleeting notes to process:

📝 Older (>7 days):
1. [note-name.md] - Created [date] ([age] days ago)
2. [note-name.md] - Created [date] ([age] days ago)

📝 This week (1-7 days):
3. [note-name.md] - Created [date] ([age] days ago)
4. [note-name.md] - Created [date] ([age] days ago)

Let's process these one by one, starting with the oldest.
Estimated time: ~[N * 2] minutes
```

### Step 2: Process Each Fleeting Note

For each fleeting note, follow this process:

#### 1. Read and Summarize
1. Read the fleeting note
2. Summarize for user:
```
Processing: "[note title]"
Created: [date] ([age] days ago)

Content summary:
[1-2 sentence summary of what the note is about]

Current state:
- Has [X] related concepts linked
- [Has/Doesn't have] clear questions to explore
- [Seems ready / Needs more development]

Options:
A) Synthesize to permanent note(s) - ready to formalize
B) Enhance and keep - needs more work
C) Delete - not valuable enough

What would you like to do?
```

#### 2. Execute the Decision

**A) Synthesize to Permanent Note(s)**:

1. **Identify atomic concepts**:
   - If fleeting note contains multiple ideas, help split into separate permanent notes
   - Each permanent note = one atomic concept

2. **For each permanent note**:
   - Suggest appropriate subfolder in `permanent/` (ai-ml, agents, startups, etc.)
   - Create note with this structure:

```yaml
---
type: permanent
tags: [relevant, tags]
created: YYYY-MM-DD
---

# [Concept Name]

> One-sentence summary

## Core Explanation

[Write in teaching style - explain as if teaching someone.
Include examples, analogies, visual representations.]

## Examples

[1-2 concrete examples]

## Why It Matters

[Context on importance or applications]

## Related Concepts

- [[related-note-1]] - How they connect
- [[related-note-2]] - How they connect
- [[related-note-3]] - How they connect

## Questions for Future Exploration

- Unanswered question 1?
- Unanswered question 2?

## Sources

- Based on fleeting note: [[fleeting/original-note]]
- [Any other sources]
```

3. **Update MOC**:
   - Identify relevant MOC in `maps-of-content/`
   - Add new permanent note(s) with brief description
   - Organize into appropriate section

4. **Delete fleeting note**:
   - Confirm deletion
   - Explain: "The fleeting note has served its purpose - the knowledge is now permanent!"

**B) Enhance and Keep**:

1. **Ask what's missing**:
   - "What have you learned about this since creating the note?"
   - "What questions remain?"
   - "What concepts should this link to?"

2. **Enhance the note**:
   - Add new findings or insights
   - Suggest additional links to existing notes
   - Refine questions to explore
   - Update created date if substantial changes

3. **Set expectation**:
   - "Let's revisit this next week in /process-fleeting"
   - Warn if note has been enhanced 3+ times without becoming permanent

**C) Delete**:

1. Confirm deletion
2. Encourage: "Great! Ideas that seemed good often aren't upon reflection. Deleting keeps your system valuable."
3. Remove the file

### Step 3: Post-Processing

After processing all fleeting notes:

#### Summary Report
```
Weekly Fleeting Processing Complete!

Summary:
- Processed: [N] fleeting notes
- Synthesized to permanent: [X] notes
- Enhanced and kept: [Y] notes
- Deleted: [Z] notes

New permanent notes created:
1. [[permanent/topic/note-1]] - [Brief description]
2. [[permanent/topic/note-2]] - [Brief description]

MOCs updated:
- [[maps-of-content/topic-moc]] - Added [N] notes

Time: ~[estimated] minutes

Remaining fleeting notes: [N] (process next week)
```

#### Suggested Next Steps
- If created 5+ permanent notes on similar topic: "Consider creating/updating a MOC with /create-moc [topic]"
- If notes need better linking: "Want to find more connections? Try /link-notes [note-path]"
- Remind about next processing: "Next weekly review: 7 days from now"

## Best Practices

### Encourage Quality Synthesis

**Good permanent note indicators**:
- Explains concept in own words (not copied)
- Includes examples or analogies
- Has visual representation (table or mermaid diagram)
- Links to 3-5 related concepts
- Can be understood standalone

**Red flags to address**:
- Note is copied from source (help rewrite)
- Too broad (split into multiple atomic notes)
- No examples (suggest adding concrete examples)
- Orphaned (no links - suggest connections)

### Handle Common Situations

**Fleeting note contains multiple concepts**:
```
I notice this fleeting note actually covers [N] distinct concepts:
1. [Concept A]
2. [Concept B]
3. [Concept C]

Let's create separate permanent notes for each - this follows the
atomic note principle. Shall we start with [Concept A]?
```

**Fleeting note has been enhanced 3+ times**:
```
This note has been in fleeting for [N] weeks now. A few possibilities:

1. It's actually ready - let's synthesize to permanent
2. It's too broad - let's split into focused atomic notes
3. It's not valuable enough - let's delete it

What do you think is the case here?
```

**Many fleeting notes on same topic**:
```
I notice you have [N] fleeting notes all related to [topic].

This suggests emerging knowledge in this area! Options:

1. Synthesize all to permanent notes, create/update MOC
2. Combine related fleeting notes, then synthesize
3. Create MOC structure first to guide synthesis

Which approach feels right?
```

**No fleeting notes to process**:
```
Your fleeting folder is empty!

This could mean:
✅ You're processing regularly (great!)
✅ You're using inbox.md more (also fine!)
❓ You're not capturing enough (might be missing ideas)

Workflow check:
- Capturing quick thoughts to inbox.md? (daily processing)
- Creating fleeting notes for developed ideas? (weekly processing)
- Converting fleeting to permanent regularly? (happening now)

Everything on track, or would you like to adjust your workflow?
```

## Integration with Skills

When processing notes, leverage the skills:

**For methodology questions**:
- Invoke `zettelkasten-workflow` skill if user asks about best practices
- Use skill guidance on atomicity, linking, MOC structure

**For synthesis guidance**:
- Invoke `note-processing` skill for detailed help on creating permanent notes
- Use skill templates and quality checklists

## Important Reminders

- **Atomic notes**: One concept per permanent note
- **Own words**: Always rewrite in user's language, never copy-paste
- **Links matter**: Each permanent note should link to 3-5 related concepts
- **Delete freely**: 40-60% deletion rate is healthy
- **No emojis**: Except in user-facing messages
- **Search first**: Check if concept already has a permanent note
- **Update MOCs**: Add new permanent notes to relevant hubs
