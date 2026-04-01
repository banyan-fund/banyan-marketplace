# Output Guide

## File structure

The meeting note is a single markdown file with YAML frontmatter, then — in
this order — a key takeaway, action items, topic sections, and open questions.

The key takeaway and action items come first because they are what the user
most often needs when returning to a meeting note. The detailed topic sections
provide supporting context below.

### Frontmatter

Generate frontmatter following vault conventions. Extract metadata from the
transcript where possible. Ask the user to confirm anything you can't determine.

```yaml
---
date: 2026-02-03
attendees:
  - "[[Alex Chen]]"
  - "[[Maria Santos]]"
company: "[[Horizons Education]]"
type: partner
status: complete
transcript: transcripts/20260203 Horizons update.txt
---
```

Fields:
- `date` — meeting date in ISO format
- `attendees` — every participant as a `[[Full Name]]` wiki-link
- `company` — the external counterparty organization, if applicable. Omit for
  internal meetings.
- `type` — one of: `partner`, `investor`, `internal`, `advisory`, `intro`
- `status` — set to `complete`
- `transcript` — relative path to the transcript file

Use full names for wiki-links. If the transcript only has first names, use
context (CLAUDE.md key entities, recent meeting notes) to resolve. If ambiguous,
ask the user.

---

### Key takeaway

Immediately after the frontmatter, write a **1-3 sentence opinionated summary**
of the meeting's most important outcome. This is not a neutral recap — it should
capture the "so what?" for the user. Think of it as what you'd say if someone
asked "how did the meeting go?" in the hallway.

**Good example:**

> Maria signaled strong interest and strategic fit — the Foundation has
> appetite for impact investing in Sub-Saharan Africa and is willing to take
> a first-loss or mezzanine position to attract larger institutional investors.
> Follow-up with CIO David and development lead Nina in ~1 month.

**Bad example (neutral, not opinionated):**

> Alex and Dana presented the fund to Maria. Several topics were discussed
> including the pilot vehicle and the Foundation's investment approach.

### Action items (at the top)

Place action items immediately after the key takeaway, before topic sections.
Use markdown checkboxes with wiki-linked owners:

```markdown
## Action items

- [ ] **[[Alex Chen]]** — Send key terms summary and impact data to Maria
- [ ] **[[Maria Santos]]** — Set up follow-up with David and Nina (~1 month)
```

Each item must pass the three-part test (see below). The `[[wiki-link]]` format
enables Obsidian Dataview queries to find outstanding action items across all
meeting notes.

---

## Writing guidance for topic sections

Each topic section should read as a self-contained briefing. Structure around
what was discussed, not when it was discussed.

Use the format that best serves the content: prose for connective tissue (who
proposed what, what was decided, what depends on what), bullets for discrete
facts and trade-offs that benefit from scanning. Most topics benefit from a mix.

**Good pattern:**

> ## Partner Onboarding Timeline
>
> Kenji walked through Meridian's current onboarding process:
> - **6-8 weeks** from initial MOU to first student disbursement
> - Main bottleneck is the Peruvian compliance review, which requires three
>   separate sign-offs
>
> Alex proposed running the New Zealand and Peruvian compliance processes in
> parallel to shorten the timeline. Kenji confirmed this was possible but
> flagged trade-offs:
> - Would require engaging Meridian's legal counsel earlier
> - Estimated additional cost of **US$3,000-5,000** in legal fees
> - Could cut the timeline to **4 weeks**
>
> The group agreed to try the parallel approach for the first cohort as a
> pilot. Dana noted that Oakwood's board would need to approve the additional
> legal spend before this could proceed.

**Bad pattern (chronological, unattributed, lossy):**

> ## Partner Onboarding
>
> The team discussed the onboarding timeline for partners. Several approaches
> were considered, including parallel processing of compliance requirements.
> It was agreed to try a faster approach for the first cohort.

Notice what the bad version loses:
- Who proposed what
- The specific numbers (6-8 weeks, 4 weeks, US$3,000-5,000)
- The contingency (board approval needed)
- The reasoning (earlier legal engagement = more cost but less time)

### Handling uncertainty in attribution

If the transcript has unclear speaker labels:
- Use "one participant" or "another attendee" rather than guessing
- If you can infer from context (e.g., only one person would know about the
  Peruvian legal process), tentatively attribute but flag it:
  "Kenji (likely) mentioned that..."
- Ask the user to correct attribution in their review

### Handling tangents and asides

If a substantive point was made during an otherwise off-topic tangent, capture
the point under the relevant topic section. The goal is thematic coherence.

If a tangent produced a decision or action item, it belongs in those sections
regardless of how off-topic the surrounding conversation was.

---

## Action items: The three-part test

Action items are placed at the top of the note (after the key takeaway) using
checkbox format with `[[wiki-linked]]` owners. See the format above.

An action item must pass ALL THREE tests to be included:

### Test 1: Specific person committed

Someone identifiable said they would do it — not "the team should" or "we need to."

- "I'll send that over by Friday" — Lin committed. Passes.
- "Someone should look into this" — no owner. Fails.

### Test 2: Concrete task described

Specific enough that the person could actually do it.

- "Send the updated financial model to Kenji" — passes
- "Think about our pricing strategy" — too vague. Fails.

### Test 3: Stated as a commitment, not a suggestion

Clear intent to do it — not "should," not a recommendation, not aspirational.

- "I'll do that this week" — passes
- "It would be great if someone could..." — wish, not commitment. Fails.

### Items that fail the test

They go in **Open Questions & Follow-ups**:

- Things raised as important but not assigned
- Suggestions needing further discussion
- FYI items about external events
- "We should probably..." statements
- Questions asked but not answered

Frame these so they can become agenda items for the next meeting. Example:

> - **Regulatory timeline uncertainty:** Alex noted that the regulatory body's
>   processing times have been unpredictable (2 weeks to 3 months). No one
>   committed to following up, but this affects the partner onboarding timeline.

---

## Formatting conventions

- Use **bold** for names on first mention within a section when there are many
  participants
- Use specific numbers, dates, and dollar amounts as stated — don't round
- Quote memorable or precise phrasing when it matters (e.g., "this is a
  dealbreaker for us")
- Keep paragraphs to 3-5 sentences
- If a topic has clear sub-themes, use ### subheadings within the ## topic section
