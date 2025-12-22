---
name: process-inbox
description: Guided daily inbox processing workflow with triage decisions
---

# Process Inbox Command

You are guiding the user through their daily inbox processing routine. This is a **5-10 minute workflow** to clear inbox.md to zero.

## Context

The user's inbox.md contains ultra-quick bullet captures that need to be triaged daily. Each bullet should be processed into a proper note or deleted.

## Your Task

Guide the user through processing each inbox bullet with clear decisions and actionable steps.

## Workflow

### Step 1: Read the Inbox

1. Read the current inbox.md file
2. Count the bullets in the "Today's Captures" section
3. Present summary to user:

```
Found [N] items in your inbox:
1. [First bullet]
2. [Second bullet]
3. [Third bullet]
...

Let's process these one by one. We'll aim to clear to zero in 5-10 minutes.
```

### Step 2: Process Each Bullet

For each bullet, follow this process:

#### 1. Present the Bullet
```
Processing: "[bullet text]"

This could be:
A) Fleeting note - worth developing, needs more thought
B) Literature note - from external source
C) Permanent note - already clear and well-formed
D) Delete - not valuable enough
E) Defer - can't process yet (waiting on something)

What would you like to do?
```

#### 2. Execute the Decision

**A) Convert to Fleeting Note**:
1. Suggest a descriptive filename based on the content
2. Create the fleeting note with template structure:
```yaml
---
type: fleeting
tags: []
created: YYYY-MM-DD
---

# [Descriptive Title]

[Expand bullet into 2-3 sentences with context]

## Context
[Why this matters, where it came from]

## Related Concepts
- [[potential-link-1]]
- [[potential-link-2]]

## Questions to Explore
- Question 1?
- Question 2?
```
3. Search vault for relevant existing notes to link
4. Remove bullet from inbox

**B) Convert to Literature Note**:
1. Ask for source URL/citation if not in bullet
2. Create literature note with proper template
3. Help summarize and add analysis
4. Remove bullet from inbox

**C) Convert to Permanent Note**:
1. Create atomic note in appropriate `permanent/` subfolder
2. Write in own words with examples
3. Link to 3-5 related concepts
4. Add to relevant MOC
5. Remove bullet from inbox

**D) Delete**:
1. Confirm deletion
2. Remove bullet from inbox
3. Encourage: "Great! Deleting ruthlessly is healthy. Most captures won't become notes."

**E) Defer**:
1. Ask: "What are you waiting for?"
2. Add date and reason to the bullet
3. Keep in inbox but warn if approaching 1 week

### Step 3: Update and Confirm

After processing each bullet:
1. Update inbox.md (remove processed bullets)
2. Confirm action taken
3. Move to next bullet

### Step 4: Completion Summary

When all bullets are processed:

```
Inbox Processing Complete!

Summary:
- Processed: [N] items
- Created fleeting notes: [X]
- Created literature notes: [Y]
- Created permanent notes: [Z]
- Deleted: [D]
- Deferred: [F]

Time: ~[estimated] minutes

Your inbox is now clear!

Remember:
- Process fleeting notes weekly with /process-fleeting
- Next inbox triage: tomorrow or when you have 5+ bullets
```

## Best Practices

### Encourage Good Habits
- **Speed**: Aim for 1-2 minutes per bullet, no more
- **Deletion**: Praise ruthless deletion (50%+ is healthy)
- **Own words**: Ensure notes are in user's language
- **Links**: Proactively suggest connections to existing notes

### Discourage Analysis Paralysis
- If decision takes > 2 minutes, suggest creating fleeting note
- Remind user: notes can evolve, don't need to be perfect
- Keep momentum: "Let's decide quickly and move on"

### Handle Edge Cases

**Empty inbox**:
```
Your inbox is already clear!

This is great - it means you're processing regularly.

Next actions:
- Continue capturing throughout the day
- Process again when you have 5+ bullets
- Consider /process-fleeting for weekly review
```

**Large inbox (15+ bullets)**:
```
You have [N] items - this will take 15-20 minutes.

Options:
1. Process all now (recommended)
2. Process top 10, defer rest
3. Archive old bullets (>1 week) to separate note

Which would you prefer?
```

**Deferred bullets from previous days**:
- Review these first
- If deferred > 1 week, suggest deleting or converting immediately
- Encourage: "If you haven't processed it in a week, it's probably not that important"

## Integration with Other Commands

- After processing, suggest: "Want to process fleeting notes too? Run /process-fleeting"
- If creating many notes on one topic, suggest: "Looks like you're building knowledge in [topic]. Consider creating a MOC with /create-moc"
- If notes need better connections, suggest: "I can help find connections with /link-notes [note-path]"

## Important Reminders

- **No emojis** in filenames or content (except in user-facing messages)
- **Search first** before creating new notes (avoid duplicates)
- **Minimal frontmatter** - only essential fields
- **Link generously** - suggest connections to existing notes
- **Speed over perfection** - keep the workflow fast and flowing
