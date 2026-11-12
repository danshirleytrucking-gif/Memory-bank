# Mind Vault — Project Plan
**Status:** Concept and requirements documented; implementation not started.
**Updated:** 2026-11-12
**Repository:** Memory-bank
**Product name:** Mind Vault

## 1. Purpose and product principles
Mind Vault is a voice-first personal memory and notes application, designed initially for Android phones (especially Samsung) and headset use. It should let the user capture, organize, retrieve, review, and act on memories naturally without needing to type. The design must also support quiet, touch-only review.

Priorities:
- Natural conversational commands, not rigid keywords.
- Low-distraction voice operation; avoid requiring screen interaction while driving. Never encourage looking at or touching the phone while driving.
- Fast capture of short facts as well as complex long-term notes.
- User control over retention, deletion, and classification.
- Offline-first note storage and retrieval where feasible; research offline speech recognition and speech synthesis separately.
- Preserve exact original note content and timestamps; AI organization should not silently alter source facts.
- Simple, uncluttered portrait interface.

## 2. Approved visual direction
Portrait layout with an upper and lower half:
- Top: title **Mind Vault**, a large metallic chrome/steel vault-door microphone with a gold ship-wheel-like handle and glowing blue rim. Separate speaker on/off toggle near the top.
- Bottom: **one large swipeable memory card** at a time, not a crowded grid of cards.
- Default card: **Short-Term Memory**, showing the newest note (example: “Remember door 57”).
- Card displays category, exact note, timestamp, and a counter such as “Note 1 of 3.”
- Swipe up/down to move through notes within a category.
- Swipe left/right to switch categories such as Today, Tomorrow, Short-Term Memory, Long-Term Memory, Trucking, and Thrifty 2 U.
- Short-term card actions: **Delete** and **Commit to Long-Term**; both require confirmation. Appropriate actions for other categories to be designed.
- The user approved this simple portrait concept. Visual mockup screenshots may exist on the user's phone, but are **not assumed to be in this repository**. Obtain images explicitly before treating them as assets.

## 3. Microphone and speaker interaction
The microphone and speaker are **independent controls**:
- Tap microphone once to start continuous listening. Listening continues across natural pauses until the user stops, mutes, or finishes a note; the system must distinguish silence from completion.
- While listening, vault wheel gently rotates or rocks. When a note is successfully saved, wheel stops/locks, optionally with a vault-clunk sound.
- Tap mic again, or say “Save note” or “Confirm,” to end dictation. App briefly summarizes the captured note and suggested category, asks for confirmation, then saves and turns microphone off.
- Support “cancel,” corrections before confirmation, and incomplete notes without accidentally saving.
- Explicit interaction states: idle, listening, processing, awaiting confirmation, saved, and error/retry.
- Speaker ON: spoken confirmations, answers, briefings, and optional sound effects.
- Speaker OFF: no spoken output or effects; visual confirmations and silent touch/swipe review remain available. Speaker OFF does not necessarily disable microphone input.
- Adjustable confirmation verbosity and sound settings.

## 4. Short-Term Memory
Examples of separate notes:
- “Remember door 57.”
- “Remember this phone number 88268.”
- “Remember to feed the chickens before we go.”

Requirements:
- Each utterance creates a distinct note, retaining exact content, timestamps, and identifiers.
- Natural retrieval: “What was the door number?” should locate the relevant note and answer with source context where ambiguity exists.
- Configurable review age (e.g., 7 or 14 days). **Never automatically delete notes** merely because they are old; flag them for review.
- Morning briefing can say, “You have three Short-Term Memory items needing attention.”
- Review choices: **Delete**, **Keep another week**, **Commit to Long-Term**. Confirm destructive or moving actions.
- Committing to long-term moves the same underlying record (not a duplicate), preserving original text and creation time, and may suggest a long-term category/subject for user approval.

## 5. Long-Term Memory organization
- Hierarchy: **Categories → Subjects → Note Cards**.
- Create, rename, and manage categories and subjects by voice or touch.
- AI may suggest categories and tags; user can override.
- A note can be associated with multiple subjects/tags **without duplicating the underlying record**.
- Voice retrieval, search, filtering, and topic summaries should work across categories.
- Example project-related categories: Trucking and Thrifty 2 U, with room for custom topics.
- Distinguish exact original notes from AI-generated summaries.

## 6. Today, Tomorrow, reminders, and tasks
- Today and Tomorrow are useful views of date-related notes, reminders, tasks, and appointments across categories, rather than necessarily separate copies.
- Interpret dates carefully and ask when missing or ambiguous; do not invent deadlines.
- Support rollover, overdue visibility, and completion status without losing history.
- Morning briefing can include due tasks, reminders, and short-term notes needing review.
- Reminder scheduling and notification delivery need explicit Android platform design and permissions.

## 7. Voice commands and flows
Examples (illustrative, not a closed keyword list):
- “Remember door 57.”
- “What was the door number?”
- “Add a note for Thrifty 2 U: …”
- “Show me my notes for Thrifty 2 U.”
- “What do I need to remember today?”
- “Move this to long-term memory.”
- “Keep that another week.”
- “Delete that note.” → request confirmation.
- “Change that note to say …”
- “Save note.” / “Confirm.” / “Cancel.”
- “Mute speaker.” / “Turn speaker on.”

For every voice flow, handle recognition uncertainty, interruptions, duplicates, ambiguous matches, offline state, and accidental activation. Prefer brief spoken questions when information is missing.

## 8. Data model — initial proposal
A single durable note record with:
- id; original_text; current_text (if user edits); created_at; updated_at
- memory_tier (short_term / long_term), status (active / completed / archived / deleted as appropriate)
- category_ids; subject_ids; tags
- due_at and reminder fields (optional, timezone-aware)
- review_due_at, review_interval, last_reviewed_at
- source/input method, edit history where practical
- optional metadata for project association and retrieval

Views such as Today/Tomorrow are derived from dates, not copies of notes. Commit to Long-Term changes classification while preserving identity and original creation timestamp. Deletion should be confirmed; consider recoverable trash and export/restore policies.

## 9. Offline, privacy, and backup
- Offline-first local note database; notes should remain accessible without a network connection.
- Explore on-device speech recognition, natural-language processing, and text-to-speech. **Do not promise fully offline continuous voice operation until validated on target Android hardware.**
- Design export/backup and restore with clear confirmation and data-integrity checks. Avoid silent data loss.
- User-controlled privacy, microphone permissions, and storage permissions.
- Background or locked-screen continuous listening is **not yet validated**; research Android microphone/background service restrictions, battery management, and safety before committing to this capability.
- Minimize unnecessary cloud dependence and recurring costs.

## 10. Settings and customization
- Speaker on/off; voice confirmation verbosity; sound effects.
- Short-term review interval (e.g., 7/14 days) and morning briefing settings.
- Category/subject management.
- Reminder/notification preferences.
- Backup/restore and local data management.
- Accessibility, readable card text, large touch targets, and silent mode.

## 11. Engineering decisions still open
- Native Android (likely needed for robust microphone/background integration) versus PWA or hybrid architecture.
- Exact speech-to-text, TTS, and on-device NLP technology and offline behavior.
- Continuous listening, wake behavior, locked-screen and headset control, battery and Android policy compliance.
- Data schema, search indexing, encryption, backup format, and migration strategy.
- Notifications and background jobs.
- Visual assets and animations based on approved screenshots when provided.
- Whether multi-device sync is needed; not approved as an initial requirement.

## 12. Proposed phased delivery
1. **Requirements and prototype:** Preserve this plan, collect approved UI reference screenshots, validate navigation and voice flow diagrams.
2. **Local notes MVP:** Android portrait interface, add/read/edit/delete notes, one-card swiping, short/long-term categorization, local persistence.
3. **Voice MVP:** Microphone states, dictation, confirmation, natural retrieval, independent speaker toggle.
4. **Memory intelligence:** AI-assisted classification, categories/subjects, review workflow, Today/Tomorrow, briefings.
5. **Reliability and offline:** Backup/restore, tests, on-device speech feasibility, interruptions, permissions, headset support.
6. **Advanced capabilities:** Reminders, background operation where supported, and optional future integrations.

## 13. Acceptance criteria
- Saying “Remember door 57” and confirming creates one persistent note with the exact number.
- Asking “What was the door number?” returns 57 from the saved note, not a guess.
- Multiple separate memories remain distinct and can be navigated on one card.
- Swipe vertically changes note; swipe horizontally changes category.
- Speaker can be muted while microphone remains usable.
- Listening animation reflects actual state; a saved note receives a clear confirmation.
- Short-term review never silently deletes stale notes.
- Committing a note to long-term preserves its original content, timestamp, and identity.
- A note tagged to multiple subjects is stored once.
- Notes remain accessible offline after app restart.
- All deletion and restore actions require explicit confirmation.
- Voice and touch flows are tested on the intended Android device.

## 14. Scope and change control
This is a **design and planning document**, not a claim that the app or features are implemented. Do not deploy or modify the separate **Thrifty 2 U** project as part of Mind Vault work. Record future approved design decisions and open questions in this repository.
