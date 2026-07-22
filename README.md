# 🧪 Chemistry Vocabulary Glossary

An interactive, shared vocabulary glossary for Chinese chemistry students learning
English chemistry vocabulary. The whole class shares one word list; each student
tracks their own learning against it.

## Features

- **Shared glossary** — one common list for the whole class, divided into four
  categories: **Chemicals**, **Processes**, **Apparatus**, and **Other**.
  It ships pre-loaded with 10 starter terms in each category, every one with a
  definition and an example sentence in context.
- **Anyone can add words** — any student can add a term, definition, example
  sentence and notes. New words instantly appear for every other student, with
  the contributor's name shown.
- **Shared notes** — students can attach memory-aid notes to any word, visible
  to the whole class.
- **Personal study lists** — each student marks words as *"I've learned this"*
  or *"Study this"*. These selections are private to each student (identified by
  the name they enter) and appear under **My Words**.
- **Quiz area** with three modes, run on the student's study list, learned
  words, a category, or the whole glossary:
  - **Multiple choice** — alternates between *term → meaning* and
    *meaning → term* questions.
  - **Flashcards** — flip to reveal meaning, example and notes; self-mark
    "knew it" or "study more".
  - **Fill in the blank** — the example sentence appears with the word blanked
    out; the student types the missing word.
  - After every quiz, missed words can be added to the study list in one click.
- **Smart review (spaced repetition)** — every quiz answer is recorded. The
  default quiz source, *Smart review*, uses a Leitner-style schedule: a word's
  streak of consecutive correct answers decides how soon it comes back
  (immediately, then 1, 3, 7, 14 and 30 days). Recently-missed words come first.
- **Pronunciation** — a 🔊 button on every word (in the glossary and in
  quizzes) speaks the term using the browser's built-in speech synthesis; no
  internet service required.
- **Flagging** — students can flag an entry that contains a mistake, with a
  reason; flagged entries show a 🚩 marker until the teacher dismisses the flag.
- **Class dashboard (📊 Class tab)** — visible to everyone:
  - *Words the class wants to study* — the terms most students marked "study
    this", a ready-made revision list for the teacher.
  - *Flagged words* — open flags with reasons, dismissible once handled.
  - *Leaderboard* — per student: words added (5 pts), words learned (2 pts) and
    correct quiz answers (1 pt).

## How it works

The app is a single `index.html` file (no build step, no framework). All shared
data lives in a [Supabase](https://supabase.com) Postgres database
(project `chem-vocab`), accessed through its REST API with a public
(publishable) key:

| Table | Purpose |
|---|---|
| `vocab_items` | The shared glossary: term, category, definition, example, notes, added_by |
| `study_selections` | Per-student selections: one row per (student, word) with status `learned` or `study` |
| `quiz_results` | One row per quiz answer (student, word, correct?) — powers spaced repetition and the leaderboard |
| `flags` | Open reports of mistakes in entries, until dismissed from the Class tab |

Row Level Security is enabled. The public key can read, add, and update entries
and manage study selections, but **cannot delete glossary words** — removing a
word is a teacher action done from the Supabase dashboard.

Students identify themselves by simply typing their name (remembered on their
device) — no passwords, suitable for a trusted classroom.

## Running it

Open `index.html` in any browser — that's it. To give the class one link,
enable **GitHub Pages** for this repository (Settings → Pages → deploy from
branch), and share the resulting URL.

## Teacher administration

- To edit or delete a word: open the Supabase dashboard → Table Editor →
  `vocab_items`.
- To see class-wide activity: query `study_selections` (e.g. which words most
  students have marked "study" — a good signal for revision lessons).
