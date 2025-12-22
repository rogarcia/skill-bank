---
name: link-notes
description: Analyze a note and suggest relevant connections across the vault
arguments:
  note-path:
    type: string
    description: Path to the note to analyze (optional - will prompt if not provided)
    required: false
---

# Link Notes Command

You are helping the user discover and create meaningful connections between notes in their Zettelkasten. Good links are the foundation of knowledge building.

## Context

**Why linking matters**:
- Zettelkasten value increases exponentially with connections
- Links reveal unexpected relationships
- Well-linked notes are more useful for thinking and writing
- Each permanent note should link to 3-5 related concepts

**User's linking philosophy**:
- Direct links are primary (not categories)
- Link when there's meaningful relationship
- Quality > quantity (avoid link spam)
- Use contextual link text: `[[note|display text]]`

## Your Task

{{#if note-path}}
The user wants to find connections for: "{{note-path}}"
{{else}}
The user wants to find connections for a note but hasn't specified which one.

First, ask: "Which note would you like to analyze for connections?"

Wait for their response before proceeding.
{{/if}}

## Workflow

### Step 1: Read and Understand the Target Note

1. Read the specified note
2. Identify:
   - Main concept(s) covered
   - Current outgoing links (if any)
   - Note type (fleeting, literature, permanent)
   - Topic domain

3. Present summary:

```
Analyzing: [[note-name]]

Type: [fleeting/literature/permanent]
Main concept: [1-sentence summary]

Current links:
- Outgoing: [N] links to other notes
- Backlinks: [M] notes linking to this one

I'll search for meaningful connections across your vault.
```

### Step 2: Search for Related Notes

Use multiple search strategies:

1. **Semantic relationships**:
   - Search for notes on related concepts
   - Look for cause-effect relationships
   - Find complementary or contrasting ideas
   - Identify examples or applications

2. **Topic overlap**:
   - Notes in same domain (e.g., ai-ml, startups)
   - Notes with similar tags
   - Notes mentioned in same MOC

3. **Conceptual dependencies**:
   - Notes this concept builds upon
   - Notes that build upon this concept
   - Notes that explain prerequisites

4. **Cross-domain connections**:
   - Unexpected connections across different topics
   - Analogies from other fields
   - Shared patterns or principles

### Step 3: Evaluate and Rank Connections

For each potential connection, assess:

**Meaningful relationships** (suggest these):
- Concept A explains Concept B
- Concept A contradicts Concept B
- Concept A is an example of Concept B
- Concept A and B are complementary
- Concept A depends on Concept B
- Concept A and B share a pattern

**Weak connections** (skip these):
- Vaguely related topics
- Same word used differently
- Only tangentially connected
- Already indirectly connected through other notes

### Step 4: Present Suggestions

Organize suggestions by relationship type:

```
Found [N] meaningful connections for [[note-name]]:

## Strong Connections (Add These)

**Foundational Concepts** (this note builds on):
1. [[concept-1]] - [1-sentence: how it relates]
   Suggested link text: "This extends [[concept-1|concept 1]]"

2. [[concept-2]] - [1-sentence: how it relates]
   Suggested link text: "Unlike [[concept-2]], this approach..."

**Related Concepts** (complementary ideas):
3. [[concept-3]] - [1-sentence: how it relates]
   Suggested link text: "Combines well with [[concept-3]]"

4. [[concept-4]] - [1-sentence: how it relates]
   Suggested link text: "See also [[concept-4]] for alternative approach"

**Applications/Examples**:
5. [[example-1]] - [1-sentence: how it relates]
   Suggested link text: "Applied in [[example-1]]"

## Interesting Connections (Consider These)

**Cross-domain patterns**:
6. [[pattern-1]] - [1-sentence: shared pattern or principle]

7. [[analogical-concept]] - [1-sentence: analogous idea from different domain]

## Already Connected (Good!)

These notes already link to [[note-name]]:
- [[existing-1]] - via backlink
- [[existing-2]] - via outgoing link

Would you like me to add any of these connections?
```

### Step 5: Implement Links

Based on user selection:

1. **Add outgoing links**:
   - Update the target note
   - Insert links in appropriate context
   - Use suggested link text for clarity

2. **Consider bidirectional links**:
   - Ask: "Should I also add links FROM these notes TO [[note-name]]?"
   - If yes, update related notes to link back

3. **Update MOCs** (if applicable):
   - If note isn't in a relevant MOC, suggest adding it
   - If multiple related notes in same MOC, highlight cluster

### Step 6: Confirm and Summary

```
Updated [[note-name]]:

Added [N] new links:
- [[link-1]] - Foundational concept
- [[link-2]] - Related concept
- [[link-3]] - Application

Total links now: [N] outgoing, [M] backlinks

The note is now better connected! These links will help:
- Understand the concept in context
- Find related ideas while reading
- Build on this knowledge in future notes

Would you like to analyze another note or review the updated content?
```

## Linking Strategies

### Types of Meaningful Relationships

**1. Hierarchical** (is-a, part-of):
- "[[transformers]] are a type of [[neural-network-architecture]]"
- "[[attention-mechanism]] is part of [[transformer-architecture]]"

**2. Causal** (causes, enables):
- "[[product-market-fit]] enables [[startup-scaling]]"
- "[[overfitting]] is prevented by [[regularization]]"

**3. Temporal** (precedes, follows):
- "[[rnns]] preceded [[transformers]] historically"
- "[[data-collection]] comes before [[model-training]]"

**4. Comparative** (similar-to, different-from):
- "Unlike [[supervised-learning]], [[rl]] learns from rewards"
- "Similar to [[human-attention]], [[attention-mechanism]] focuses selectively"

**5. Functional** (uses, applies-to):
- "[[rag]] uses [[retrieval]] to enhance [[generation]]"
- "[[agile]] applies to [[software-development]]"

**6. Evidential** (supports, contradicts):
- "[[paper-x]] supports [[hypothesis-y]]"
- "[[finding-a]] contradicts [[assumption-b]]"

### Link Text Best Practices

**Good link text** (adds context):
```
"The [[attention-mechanism]] enables parallel processing"
"Unlike [[rnns]], transformers can process sequences in parallel"
"This builds on [[gradient-descent|basic gradient descent]]"
```

**Poor link text** (just keywords):
```
"The attention-mechanism"
"transformers"
"gradient descent"
```

**Guideline**: Imagine reading the sentence aloud - does it flow naturally?

### Avoiding Link Spam

**Don't link**:
- Every mention of a keyword
- Vaguely related topics
- Already indirectly connected notes (A→B→C, don't need A→C)
- Same concept multiple times in one note (link once)

**Do link**:
- Meaningful relationships
- First mention of important related concepts
- When you'd want to follow the link while reading

## Edge Cases

### Note already well-linked (5+ links)

```
[[note-name]] already has [N] links - nicely connected!

Current connections:
- [List existing links]

I found [M] additional potential connections:
- [[suggestion-1]]
- [[suggestion-2]]

These could add value, but the note is already well-integrated.
Add more links, or keep it as is?
```

### Orphaned note (no links)

```
[[note-name]] has no links yet - let's fix that!

This note is "orphaned" - isolated from your knowledge network.
Strong connections will make it much more valuable.

I found [N] essential connections. I recommend adding at least 3-5
to integrate this note into your Zettelkasten.

Suggested priority connections:
[List top 5 with clear relationships]

Shall I add these?
```

### Note needs restructuring

```
While analyzing [[note-name]], I noticed it covers [N] distinct concepts.

Following the atomic note principle, consider splitting into:
1. [[concept-a]] - [What it would cover]
2. [[concept-b]] - [What it would cover]
3. [[concept-c]] - [What it would cover]

This would make linking more precise and notes more reusable.

Would you like help splitting this note?
```

### Circular or excessive connections

```
Warning: [[note-a]] and [[note-b]] might be too similar.

They cover: [similar concepts]
Current links: A→B, B→A, both to C, D, E

Consider:
1. Merging them into one stronger note
2. Clarifying what makes each distinct
3. Reducing redundant links

What would work best?
```

## Link Quality Metrics

After adding links, evaluate:

**Good indicators**:
- Note has 3-7 outgoing links (sweet spot)
- Links span multiple relationship types
- Mix of in-topic and cross-topic connections
- Contextual link text used
- Bidirectional connections exist

**Warning signs**:
- 0-1 links (too isolated)
- 10+ links (might be link spam)
- All links to same topic (not diverse enough)
- Plain keyword links (no context)
- One-way connections only

## Integration with Other Commands

After linking:
- If note is now well-connected: "Consider adding to a MOC with /create-moc"
- If discovered new concepts to explore: "Capture these ideas with /capture"
- If found gaps: "Create new notes to fill the connections"

## Important Reminders

- **Quality > quantity**: 3-5 meaningful links beat 20 weak ones
- **Contextual linking**: Add context to link text when helpful
- **Bidirectional thinking**: Consider both directions (A→B and B→A)
- **Cross-pollination**: Look for unexpected cross-domain connections
- **No link spam**: Don't link every keyword mention
- **Update MOCs**: When adding links, keep MOCs in sync
