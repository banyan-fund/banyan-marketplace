# Speaker Identification

When a transcript uses generic labels ("Speaker 1", "Speaker 2", device names,
or room names), use this approach to infer who is speaking before asking the
user to confirm.

## Feature analysis

For each speaker, analyze:

| Feature | What it reveals |
|---------|----------------|
| **Word count / speaking time** | High = lead or presenter; low = observer |
| **Segment count** | Frequent turns = active participant |
| **Average segment length** | Long = presenter; short = responder |
| **Topic focus** | What areas they discuss most |
| **Interaction pattern** | Do others ask them questions? Do they assign tasks? |
| **Speaking style** | Formal/informal, technical depth, decision authority |

## Cross-reference with context

If the project's `CLAUDE.md` or vault notes contain information about expected
participants (roles, expertise areas, communication styles), match the feature
patterns against known team members.

Recent meeting notes in the same project folder often mention the same
participants — check these for names and roles.

## Present for confirmation

**Never silently assume speaker identity.** Present your analysis:

> Based on speaking patterns, I believe:
> - Speaker 1 → Alice (most technical content, assigns tasks)
> - Speaker 2 → Bob (asks product/requirements questions)
> - Speaker 3 → likely an external participant (unfamiliar terminology, asks
>   orientation questions)
>
> Can you confirm or correct these?

After confirmation, apply mappings consistently throughout the document.

## When identification isn't possible

If you can't confidently map speakers:
- Use the generic labels from the transcript
- Note the uncertainty in the meeting note
- Attribute substantive points to "one participant" rather than guessing
- The user can correct attribution in their review
