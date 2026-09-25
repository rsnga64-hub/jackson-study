# Study Site — Build & Publish Playbook

Read this in full before doing any work on the site. It records the conventions and hard-won lessons so they don't have to be rediscovered each session.

## What this is

A static study site: study guide + flashcards + self-graded quiz per unit, organized by subject. Served by GitHub Pages from `main`.

- Repo: https://github.com/rsnga64-hub/jackson-study
- Live site: https://rsnga64-hub.github.io/jackson-study/
- The repo is public, so cloning needs no auth: `git clone https://github.com/rsnga64-hub/jackson-study.git`

## Site structure

- `index.html` — top-level subject picker (cards: Geometry, Biology). The Geometry card is currently disabled (not a link); see **Geometry status** below.
- `geometry.html` — Geometry unit list (unit cards grouped under section labels)
- `biology.html` — Biology unit list
- `proofs_guide.html`, `unit2_flashcards.html`, `unit2_quiz.html` — Geometry Unit 2, Proofs
- `unit3_guide.html`, `unit3_flashcards.html`, `unit3_quiz.html` — Geometry Unit 3, Quadrilaterals (lessons 3.1–3.10)
- `bio_unit1_*`, `bio_unit2_*` — Biology units
- `bio_unit2_membrane_*` — Biology lesson 2.3, The Cell Membrane (a lesson-level set listed under Unit 2)

**Pattern for every future unit:** three files named `unitN_guide.html` / `unitN_flashcards.html` / `unitN_quiz.html`, linked from the relevant subject page under a new "Unit N — <title>" section-label block, and given a card on that subject page. Biology files use the `bio_` prefix (`bio_unitN_*`). When a single lesson gets its own set within a unit, name it `bio_unitN_<topic>_guide/flashcards/quiz.html` and give it its own section-label block on the subject page (e.g. "Unit 2 — Lesson 2.3: The Cell Membrane").

## Design system — copy verbatim, do not reinvent

- Fonts: Google Fonts **Space Grotesk** (headings) + **Inter** (body) via `<link>` tag in `<head>`.
- Every page defines the same CSS custom properties in `:root`: `--bg, --ink, --text, --text-soft, --blue/--blue-soft, --teal/--teal-soft, --coral/--coral-soft, --amber/--amber-soft, --violet/--violet-soft, --border, --card-bg, --gray, --gray-line, --grid-line`. Copy the exact hex values from an existing page's `<style>` block (e.g. `bio_unit2_membrane_flashcards.html`) rather than retyping from memory.
- Background: faint grid pattern via a repeating `linear-gradient` using `--grid-line`.
- **Guide pages** (`*_guide.html`): dark hero banner → tab bar (`.tab-btn` toggling `.panel` divs, one per lesson) → callout boxes (`.box.theorem`, `.box.postulate`, `.box.watch`, `.box.tip`) → `.def-grid` definition cards → `.proof` tables → `.numex` worked examples → `.foot-grid` at the bottom (chip bank + checklist).
- **Flashcard pages**: category tabs, 3D-flip card, progress bar, Prev/Shuffle/Next, Got it/Still learning tally. Diagrams are hand-built inline SVG via small JS helpers (`svg`, `lineEl`, `dot`, `txt`, `poly`, `chevron`, `tick`, `rightAngle`). Copy them from a live Biology page such as `bio_unit2_membrane_flashcards.html`. The Geometry pages are now short takedown notices, so they no longer contain the design system; the Geometry-only helpers (`chevron`, `tick`, `rightAngle`, `coordPlot(pts, labels, opts)`) survive only in git history (commit `153216e`). Never pull in an external diagram/chart library. Keep `.diagram-wrap svg { max-height: … }` (190px cards / 270px quiz) or tall diagrams cover the buttons at desktop width.
- **Quiz pages**: self-graded multiple choice, one question at a time, explanation after each answer, progress bar, final score + missed-question list, Retake. Unit 3's quiz shuffles answer order.
- Before publishing any new page: render it in a headless browser (e.g. Python Playwright + Chromium). Check the console, click every tab, flip every flashcard, answer every quiz question, and look at the screenshots. (Google Fonts may fail to load in a sandbox — that console error is expected.)

## Content principles

- Build **only** from the course's actual source materials. Never add correct-but-off-curriculum content or vocabulary.
- If only part of a unit's materials exist, say so and build only that part. Expect to rework (not just append) when more lessons arrive.
- Personal-interest themes are decorative flavor only — never part of the math/science content.
- **Copyright:** use ORIGINAL numbers/coordinates in quizzes, worked examples, and practice problems that test the same skills — don't reproduce worksheet problems. Some handouts are third-party commercial material (check for a copyright line); never copy those onto this site.
- Answer keys sometimes contain arithmetic slips. Verify every computed answer with a script; never copy key values blindly.
- Printable practice tests and answer keys are kept private and are **never** committed to this repo.
- Draft everything first, get the site owner's explicit approval, **then** push.

Source materials are kept outside this repo and are never committed to it.

## Publishing

The Claude GitHub app is installed on this repo, so a Claude session can push directly once the site owner has approved the update. In a cloud session, attach the repo with push access first; before that, pushes fail with a git-proxy 403 ("not in this session's authorized repository set"). If pushing still fails:

1. If the session offers a way to attach the repo with push access, try that once.
2. Otherwise, commit locally, then package `git format-patch origin/main --stdout > update.patch` plus a zip of the repo minus `.git`, and hand both to the site owner.
3. **Patch (preferred):** in a local clone on `main`: `git pull`, `git am update.patch`, `git push`.
   **Zip (fallback):** copy the changed `.html` files over the clone (never a `.docx`), then `git add` / `git commit` / `git push`.
4. Give step-by-step Git Bash instructions — don't assume git familiarity.

## Known issues

- Geometry Unit 2 quiz does not shuffle answer order; its correct answer is always A.
- Biology Unit 1 and Unit 2 quizzes don't shuffle either and are heavily weighted to A (Unit 1: 17 of 21; Unit 2: 17 of 28). Fix by shuffling answer order when those units are next touched. The 2.3 Cell Membrane quiz is already balanced.

## Current content

### Geometry status: removed at the teacher's request

All Geometry pages (`geometry.html`, `proofs_guide.html`, `unit2_*`, `unit3_*`) currently show "Removed per Teacher Request" notices, and the Geometry card on `index.html` is disabled. **Do not rebuild, restore, or re-link Geometry content unless the site owner explicitly says the issue is resolved.** A restore patch existing somewhere is not that confirmation.

### Biology (live)

- **Unit 1** (Bonding & Water, Macromolecules, Enzymes): guide with 4 tabs, 22 flashcards, 21-question quiz.
- **Unit 2** (Cells & Organelles, lessons 2.1–2.2): guide, 24 flashcards, 28-question quiz.
- **Unit 2, Lesson 2.3** (The Cell Membrane): guide with 4 tabs (Job & Fluid Mosaic, Lipids, Proteins & Carbs, What Gets Through), 20 flashcards in 3 categories, 22-question quiz. Built from the lecture recording and slides; follows the lecture's definition of peripheral proteins and flags that the 2.6 worksheet defines them differently.
- **Not yet built:** Unit 2 lessons 2.7–2.12 (passive transport, osmosis, surface area to volume, active transport). Source materials exist.
