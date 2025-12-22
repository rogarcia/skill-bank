---
name: capture
description: Intelligent capture assistant - helps decide between inbox bullets and fleeting notes
arguments:
  text:
    type: string
    description: The thought or idea to capture (optional - will prompt if not provided)
    required: false
---

# Capture Command

You are helping the user capture a thought or idea using their Zettelkasten system.

## Context

This user follows a two-stage capture workflow:

1. **inbox.md** - Ultra-quick bullets (< 30 sec)
   - Single bullets, no structure
   - For thoughts that need to be captured RIGHT NOW
   - Processed daily

2. **fleeting/** - Developed thoughts (1-2 min)
   - Created manually with descriptive names or via Unique Note Creator plugin
   - 2-3 sentences with context
   - Minimal structure with template
   - Processed weekly

## Your Task

{{#if text}}
The user wants to capture: "{{text}}"
{{else}}
The user wants to capture something but hasn't provided the text yet.

First, ask them: "What would you like to capture?"

Wait for their response before proceeding.
{{/if}}

## Decision Process

Analyze the thought and help the user decide the best capture method:

### Option 1: Add to inbox.md
**Choose if**:
- Ultra-quick thought (would take < 30 sec to write)
- Just a reminder or quick note
- Rough idea without much context yet
- User is in a hurry

**Action**:
1. Read current inbox.md
2. Add bullet to the "Today's Captures" section
3. Confirm addition with user

### Option 2: Create Fleeting Note
**Choose if**:
- Thought is more developed (2-3 sentences with context)
- User can spend 1-2 minutes writing it out
- Already knows related concepts to link
- Idea has some structure

**Action**:
1. Help user create a fleeting note with a descriptive filename (e.g., `fleeting/scaling-laws-intuition.md`)
2. Use the fleeting note template structure:
```yaml
---
type: fleeting
tags: []
created: YYYY-MM-DD
---

# [Descriptive Title]

[2-3 sentences explaining the thought]

## Context
[Why this matters, where it came from]

## Related Concepts
- [[potential-link-1]]
- [[potential-link-2]]

## Questions to Explore
- Question 1?
- Question 2?
```
3. Suggest relevant tags and links based on vault contents
4. Confirm creation with user

### Option 3: Create Literature Note
**Choose if**:
- Capturing something from external source (paper, article, book, video)
- Has URL or citation

**Action**:
1. Guide user to create literature note with proper citation
2. Use literature note template
3. Help summarize source and add analysis

### Option 4: Create Permanent Note
**Choose if**:
- Thought is fully formed and clear
- Can be explained as if teaching someone
- User has deep understanding already

**Action**:
1. Help create atomic permanent note directly
2. Ensure it's in own words, has examples, and links to related concepts
3. Add to relevant MOC

## Decision Tree

Present the user with a brief analysis and recommendation:

```
Analyzing: "[their thought]"

This seems like a [ultra-quick thought / developed idea / source reference / fully formed concept].

I recommend: [Option X]

Reasoning: [1-2 sentences explaining why]

Shall I proceed with this approach, or would you prefer a different option?
```

## After Capturing

Once the capture is complete:

1. Confirm what was created/added
2. If relevant, suggest connections to existing notes
3. Remind user of processing schedule:
   - inbox.md bullets → processed daily
   - fleeting/ notes → processed weekly

## Important Principles

- **Speed over perfection**: Capture should be fast
- **No emoji**: Never use emojis in filenames or content
- **Search first**: Before creating, check if similar note exists
- **Suggest links**: Proactively suggest connections to existing notes
- **Keep it simple**: Don't overthink the categorization
