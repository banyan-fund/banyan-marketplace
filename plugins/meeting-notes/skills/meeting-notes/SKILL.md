---
name: meeting-notes
description: >
  You MUST use this skill to prepare meeting notes from transcripts, recordings, or
  call notes. This skill produces thematically organized notes with opinionated key
  takeaways, attributed discussion, checkbox action items with [[wiki-linked]] owners,
  and open questions. Use this skill whenever the user: shares or mentions a meeting
  transcript (.txt, .vtt, .srt, .md); asks to summarize, recap, or write up a meeting
  or call; mentions "meeting notes", "meeting summary", "action items from a meeting";
  references transcripts in ~/Documents/Meetings/ or similar locations; pastes
  transcript text inline; asks to process or check for new transcripts; or wants to
  capture what happened in a meeting from their own rough notes. Use this even for
  short standups, 1:1 syncs, and informal calls.
model: opus
context: fork
permissionMode: acceptEdits
mcpServers:
  - obsidian
tools:
  - Read
  - Glob
  - Grep
  - Bash
  - Write
  - mcp__obsidian__write_note
  - mcp__obsidian__read_note
  - mcp__obsidian__patch_note
  - mcp__obsidian__update_frontmatter
  - mcp__obsidian__search_notes
  - mcp__obsidian__list_directory
---

# Meeting Notes

Turn meeting transcripts into well-organized, insightful, and actionable notes.

## Why this skill exists

Most AI-generated meeting summaries fail in predictable ways. This skill exists to prevent four specific failure modes:

**Chronological retelling.** A good summary reads like a briefing document, not
a play-by-play. People think about meetings as *topics*, not sequences.
Discussions revisit earlier points, clarify, and reach new conclusions — a
minute-by-minute retelling buries substance.

**Hallucinated action items.** Someone says "we should probably look into X" and
the AI lists it as an assigned action item. This destroys trust. If nobody
explicitly said "I will do X", it is not an action item.

**Lossy compression.** A single "summarize this" pass produces ~500 words of
watered-down generalities. The specific arguments, disagreements, and reasoning
behind decisions — all lost. These notes often become source material for later
work. Rich detail matters.

**Summarizing the user's own material.** The user already knows what they said.
When they pitched, presented updates, or shared material they authored, they
don't need it played back. The value is in what the *other party* did: what they
asked, what they reacted to, what they volunteered unprompted, where they
engaged deeply, and where they didn't.

---

## Hard gate

Do NOT write any meeting notes until you have:

1. Identified the project the meeting relates to
1. Read relevant project context (vault conventions, recent meetings, key
   people)
1. Identified the thematic structure of the meeting

Only after confirming topics with the user should you begin writing.

---

## Workflow

### Step 0: Secure the transcript

If the user provided a transcript path outside the current working directory
(e.g., `~/Downloads/`, `~/Desktop/`), **immediately copy it** to a temp
location before doing anything else:

```
cp <user-provided-path> /tmp/<filename>
```

Read from the `/tmp/` copy for all subsequent steps. This avoids repeated
permission prompts for external paths.

### Step 1: Build context

- Explore the Obsidian vault to locate meeting note folders (typically
  `Meetings/`)
- Read the project's `Meetings/CLAUDE.md` if one exists — it contains
  conventions, transcript locations, frontmatter schema, and key entities
- **Build a name/entity lookup.** Scan recent meeting notes and vault pages for
  the correct spelling of people, companies, and key terms. ASR transcripts
  consistently mangle proper nouns (e.g., "Ostara" for "Astra", "meridean" for
  "Meridian"). When you encounter unfamiliar spellings in the transcript, check
  against your lookup before writing them into the notes.
- Check for existing meeting notes on the same date to avoid duplicates
- If an existing note covers this meeting (e.g., preparation notes), plan to
  merge into it rather than creating a new file

### Step 2: Read and analyze the transcript

Read the **full transcript**. Do not skim. Then:

1. **Extract the meeting date.** Check the transcript metadata (headers like
   "Meeting started: DD/MM/YYYY"), the filename (often contains a date like
   `2026-02-12`), or timestamps within the transcript. Do not default to today's
   date — the meeting may have happened days or weeks ago.

1. **Identify participants.** If the transcript uses device names or generic
   labels ("Speaker 1", "Conference Room B"), flag these for the user. When
   speakers are anonymous, use feature analysis to infer identity — see
   [references/speaker-identification.md](references/speaker-identification.md).

1. **Check for user notes.** If the user provides their own notes alongside the
   transcript, treat these as **higher-signal than the transcript**. The user's
   notes reflect what *they* found important in the moment.

1. **Determine the meeting type.** This affects how you organize topics:

   - **Pitch / intro** — The user's pitch is one compressed topic (1-2
     sentences). Remaining topics focus on the counterparty: questions asked,
     engagement signals, what they volunteered unprompted, reactions that reveal
     priorities.

   - **Update / check-in** — Both sides' updates share a single brief topic.
     Detail goes to what the other party told the user, questions they asked,
     and which updates drew particular engagement.

   - **Internal / advisory** — Asymmetric attention is less relevant. Organize
     by substance discussed.

### Step 3: Identify topics

Extract 3-6 major topics — what was *actually talked about*, not agenda items.
Merge closely related threads. Ignore small talk and logistics unless they
contained a decision or commitment.

**Asymmetric attention.** Organize topics around what the *other party*
contributed: reactions, questions, insights, volunteered ideas, and signals of
engagement or disengagement. The user's own material should be compressed.

**Map the overlap, not the pitch.** Topics should surface:

- Where interests naturally align (common ground)
- What the counterparty volunteered unprompted (what they genuinely care about)
- Where they pushed back, hedged, or didn't engage (friction or limits)

### Step 4: Confirm topics with user

Present the proposed topic list and explicitly ask for validation:

> Here are the key topics I identified:
>
> 1. [Topic A] — one-line description
> 1. [Topic B] — one-line description ...
>
> Before I write the full summary:
>
> - Are these the right topics? Should any be merged, split, or dropped?
> - Is anything missing that you remember being important?
> - Any topic you want covered in more depth?

**Do not skip this step** unless running in batch mode. The user just came out
of the meeting and has context you don't.

### Step 5: Write the summary

Read [references/output-guide.md](references/output-guide.md) before writing.
Then write the summary **section by section**, not in one shot. Expanding each
topic individually produces richer, more faithful detail.

For each topic:

- Re-read the relevant transcript portions
- Attribute specific points to the people who made them
- Preserve disagreements and reasoning — don't flatten nuance into false
  consensus
- Spend more words on topics that had more substantive discussion

After all topics, write action items (applying the strict three-part test from
the output guide) and open questions.

### Step 6: Self-review

Before presenting to the user, review against
[references/completeness-checklist.md](references/completeness-checklist.md).
Fix any gaps silently.

### Step 7: Save and report

Save the note to the project's meeting folder following vault conventions, or
update the existing note if one already exists. When merging into an existing
note, preserve the user's content (higher-signal) and integrate the transcript
summary.

Report back:

- **Note path** — where the note was saved
- **Topics** — count and names
- **Action items** — list with owners
- **Open questions** — anything unresolved

---

## Batch mode

When invoked in batch mode (e.g., processing multiple transcripts
automatically):

- Skip Step 4 (topic confirmation) — use your best judgment
- If speaker attribution is ambiguous, note uncertainty inline rather than
  blocking
- If meeting type is unclear, default to `internal`
- Apply the same quality bar for writing — batch mode skips confirmation, not
  depth

---

## Handling long or complex meetings

For meetings over 60 minutes or with many participants, a single pass through
the transcript risks losing content. In these cases, consider making multiple
passes:

1. **First pass:** Read the full transcript, identify topics, write initial
   sections
1. **Second pass:** Re-read the transcript looking specifically for details,
   quotes, and commitments you may have missed
1. **Reconcile:** Merge any new findings into the draft (union, never remove)

The goal is completeness, not redundancy. Two careful passes catch more than
one.

---

## Important principles

**Attribution matters.** Don't write "the team discussed X." Write "Lin proposed
X, Alex raised concerns about Y, and the group agreed to Z." If you can't tell
who said something, say "one participant noted..." rather than guessing.

**Preserve tension.** If people disagreed, capture both sides. Don't resolve
disagreements the meeting didn't resolve.

**No editorializing.** Reflect what was said, not what you think should have
been said. No recommendations unless the user asks.

**Proportional depth.** A five-minute status update gets a paragraph. A
thirty-minute strategy debate gets detailed treatment with attributed positions.

**Fidelity over brevity.** A 2,000-word summary of a 60-minute meeting is fine.
500 words is almost certainly too short.
