# Zettelkasten Assistant Plugin

A comprehensive Claude Code plugin for managing your Obsidian-based Zettelkasten knowledge base. This plugin provides intelligent capture, automated processing, and autonomous maintenance of your notes.

## Overview

This plugin helps you:
- **Capture** thoughts quickly with intelligent routing (inbox vs fleeting)
- **Process** notes systematically (daily inbox, weekly fleeting)
- **Organize** knowledge into Maps of Content
- **Connect** notes through meaningful links
- **Maintain** your knowledge base automatically

## Components

### Skills (2)

Skills provide deep domain knowledge that powers all other components:

1. **`zettelkasten-workflow`** - Deep knowledge of Zettelkasten methodology, best practices, and your vault structure
2. **`note-processing`** - Specialized guidance for converting fleeting → literature → permanent notes

**When to use**: Skills are invoked automatically by commands and agents, but you can also use them directly when asking methodological questions.

### Commands (5)

Quick, guided workflows for common tasks:

#### `/capture [text]`
Intelligent capture assistant that helps you decide between:
- Adding bullet to inbox.md (ultra-quick, < 30 sec)
- Creating fleeting note (developed thought, 1-2 min)
- Creating literature note (from external source)
- Creating permanent note (fully formed concept)

**Usage**:
```
/capture I think RAG could benefit from chain-of-thought reasoning
/capture  (will prompt for text)
```

#### `/process-inbox`
Guided daily workflow (5-10 min) to triage inbox bullets into proper notes.

**When to run**: Daily, or when inbox has 5+ bullets

**What it does**:
- Reads all inbox bullets
- Helps decide action for each (fleeting/literature/permanent/delete)
- Creates appropriate notes
- Clears inbox to zero

**Usage**:
```
/process-inbox
```

#### `/process-fleeting`
Weekly workflow (30 min) to synthesize fleeting notes into permanent knowledge.

**When to run**: Weekly, or when fleeting/ has 10+ notes

**What it does**:
- Reviews all fleeting notes
- Helps synthesize to permanent notes
- Enhances notes that need more work
- Deletes notes that aren't valuable
- Updates relevant MOCs

**Usage**:
```
/process-fleeting
```

#### `/create-moc [topic]`
Create new Map of Content with proper structure.

**When to use**: When you have 10+ notes on a topic

**What it does**:
- Searches for related notes
- Organizes into logical sections
- Creates learning path (beginner → advanced)
- Adds to maps-of-content.md directory

**Usage**:
```
/create-moc reinforcement learning
/create-moc  (will prompt for topic)
```

#### `/link-notes [note-path]`
Analyze a note and suggest meaningful connections.

**When to use**: After creating permanent note, or to enhance existing notes

**What it does**:
- Finds related concepts across vault
- Suggests foundational, related, and cross-domain connections
- Adds links with contextual integration
- Improves knowledge network density

**Usage**:
```
/link-notes permanent/ai-ml/transformer-architecture.md
/link-notes  (will prompt for path)
```

### Agents (3)

Autonomous agents for complex, batch operations:

#### `note-processor`
Autonomously processes inbox and fleeting notes into permanent knowledge.

**When to use**:
- Large backlog (10+ inbox bullets, 10+ fleeting notes)
- Want automated batch processing
- Need systematic cleanup

**What it does**:
- Processes all inbox bullets (fleeting/literature/permanent/delete)
- Synthesizes all fleeting notes (permanent/enhance/delete)
- Updates MOCs with new notes
- Generates comprehensive report

**How to invoke**:
```
Can you process all my notes?
Help me clean up my captures
Synthesize my fleeting notes
```

#### `link-suggester`
Deep analysis to discover and suggest meaningful connections.

**When to use**:
- Want to strengthen knowledge network
- Have many orphaned notes (no links)
- Want cross-domain connections discovered

**What it does**:
- Analyzes all permanent notes
- Finds orphaned and poorly linked notes
- Suggests meaningful connections (foundational, related, cross-domain)
- Adds links with contextual integration
- Identifies topic clusters for MOCs

**How to invoke**:
```
Find connections between my notes
Improve my note linking
Discover relationships in my vault
```

#### `moc-maintainer`
Maintains and updates Maps of Content automatically.

**When to use**:
- After creating many permanent notes
- MOCs feel outdated
- Want comprehensive MOC updates

**What it does**:
- Updates all MOCs with new notes
- Finds and fixes orphaned notes
- Suggests new MOCs for emerging topics
- Improves MOC quality (descriptions, learning paths)
- Creates sub-MOCs for overgrown hubs

**How to invoke**:
```
Update my MOCs
Maintain my maps of content
Organize my navigation hubs
```

## Typical Workflows

### Daily Routine (5-10 min)

1. **Throughout the day**: Quick captures to inbox.md
   ```
   /capture [quick thought]
   ```

2. **End of day**: Process inbox
   ```
   /process-inbox
   ```

### Weekly Review (30-45 min)

1. **Process fleeting notes**:
   ```
   /process-fleeting
   ```

2. **Link new permanent notes**:
   ```
   /link-notes permanent/topic/new-note.md
   ```

3. **Update MOCs** (if many notes created):
   ```
   Can you update my MOCs?  (invokes moc-maintainer agent)
   ```

### Monthly Deep Dive (1-2 hours)

1. **Comprehensive note processing**:
   ```
   Process all my notes  (invokes note-processor agent)
   ```

2. **Network strengthening**:
   ```
   Find connections between my notes  (invokes link-suggester agent)
   ```

3. **MOC maintenance**:
   ```
   Maintain my maps of content  (invokes moc-maintainer agent)
   ```

4. **Create MOCs for emerging topics**:
   ```
   /create-moc [new topic]
   ```

## Best Practices

### Capture Philosophy
- **Ultra-quick thoughts** → inbox.md bullets
- **Developed thoughts** → fleeting notes
- **External sources** → literature notes
- **Fully formed** → permanent notes

### Processing Discipline
- **Daily**: Clear inbox to zero (5-10 min)
- **Weekly**: Process fleeting notes (30 min)
- **Monthly**: Comprehensive maintenance (1-2 hours)

### Quality Standards
- **Atomic**: One concept per permanent note
- **Own words**: Never copy-paste, always explain
- **Examples**: Include concrete examples
- **Linked**: 3-5 meaningful connections per note
- **MOCs**: Every permanent note in at least one

### Deletion Mindset
- Delete 40-60% of captures (healthy filtering)
- If fleeting note sits 3+ weeks, delete or synthesize
- Better to delete than to keep clutter

## Customization

You can customize the plugin by editing the component files:

- **Skills**: Adjust methodology to match your preferences
- **Commands**: Modify workflows and templates
- **Agents**: Change automation behavior and thresholds

## Tips for Success

1. **Start small**: Use commands first, agents when comfortable
2. **Process regularly**: Daily inbox, weekly fleeting - consistency matters
3. **Trust deletion**: 50% deletion rate is healthy, not wasteful
4. **Link generously**: Connections create value, categories don't
5. **Let structure emerge**: Don't over-organize upfront
6. **Use MOCs**: They're workstations for thinking, not just indexes
7. **Ask questions**: Skills and agents can explain methodology

## Troubleshooting

**Inbox growing too large**:
- Run /process-inbox more frequently
- Be more selective in captures
- Delete more ruthlessly

**Fleeting notes accumulating**:
- Run /process-fleeting weekly
- Lower your standards - "good enough" beats perfect
- Delete notes that haven't progressed in 2-3 weeks

**Notes feel disconnected**:
- Run link-suggester agent
- Use /link-notes on key permanent notes
- Aim for 3-5 links per note

**Can't find notes**:
- Update MOCs with moc-maintainer agent
- Create MOCs for major topics with /create-moc
- Improve linking with /link-notes

## Advanced Usage

### Combining Commands
```
# Morning: Capture throughout day
/capture [thought 1]
/capture [thought 2]

# Evening: Process and organize
/process-inbox
/link-notes permanent/ai-ml/new-concept.md
```

### Agent Automation
```
# Weekly batch job
Process all my notes and update MOCs
(Runs note-processor + moc-maintainer sequentially)

# Network optimization
Find connections and update MOCs
(Runs link-suggester + moc-maintainer sequentially)
```

### Methodological Questions
```
# The skills will be invoked automatically
What's the best way to process literature notes?
How should I organize my MOCs?
When should I create a new permanent note vs enhancing fleeting?
```


