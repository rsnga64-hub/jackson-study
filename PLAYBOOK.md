# Study Site — Build & Publish Playbook

Read this in full before doing any work on the site. It records the conventions and hard-won lessons so they don't have to be rediscovered each session.

## What this is

A static study site: study guide + flashcards + self-graded quiz per unit, organized by subject. Served by GitHub Pages from `main`.

- Repo: https://github.com/rsnga64-hub/jackson-study
- Live site: https://rsnga64-hub.github.io/jackson-study/
- The repo is public, so cloning needs no auth: `git clone https://github.com/rsnga64-hub/jackson-study.git`

## Site structure

- `index.html` — top-level subject picker (cards: Geometry, Biology)
- `geometry.html` — Geometry unit list (unit cards grouped under section labels)
- `biology.html` — Biology unit list
- `unit1_guide.html`, `unit1_flashcards.html`, `unit1_quiz.html` — Geometry Unit 1, Polynomials & Geometry Basics
- `proofs_guide.html`, `unit2_flashcards.html`, `unit2_quiz.html` — Geometry Unit 2, Proofs (plus 2.13 coordinate formulas)
- `unit3_guide.html`, `unit3_flashcards.html`, `unit3_quiz.html` — Geometry Unit 3, Quadrilaterals (lessons 3.1–3.10)
- `bio_unit1_*`, `bio_unit2_*` — Biology units

**Pattern for every future unit:** three files named `unitN_guide.html` / `unitN_flashcards.html` / `unitN_quiz.html`, linked from the relevant subject page under a new "Unit N — <title>" section-label block, and given a card on that subject page. Biology files use the `bio_` prefix (`bio_unitN_*`).

**Group by unit, never by lesson (site owner's rule).** Every lesson numbered N.x belongs in Unit N's guide/flashcards/quiz, as a tab in the guide and a category or section in the flashcards and quiz. Never give a single lesson its own section or files. Group by the lesson number even when the source file is filed in a different unit's folder (e.g. Geometry 2.13 was filed under Unit 3 but lives in Unit 2). Long unit quizzes should offer a per-lesson picker (see `bio_unit2_quiz.html`).

**Exception: Biology prefix/suffix vocabulary.** The class word-part list (`Biology/Prefix Suffix Words.pdf`, 12 groups) has its own section on `biology.html`, separate from the units: `bio_prefix_suffix_flashcards.html` and `bio_prefix_suffix_quiz.html`. Never mix it into unit flashcards. Jackson is responsible for **groups 1–6 only**; add later groups only when the site owner says so. Keep each word part and meaning exactly as the list writes it (e.g. it shows Algia-, Cide-, Emia- with a trailing hyphen even though they're usually word endings).

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

- Biology Unit 1 quiz doesn't shuffle and is heavily weighted to A (17 of 21). Fix by shuffling answer order when that unit is next touched. (Geometry Unit 2 and Biology Unit 2 were fixed; Geometry Units 1 and 3 shuffle at runtime.)

## Current content

### Geometry (live)

The Geometry teacher approved the content going back up (site owner confirmed, Sept 2026). Keep following the copyright rules above.

- **Unit 1** (Polynomials & Geometry Basics, 1.1–1.7): guide with 7 tabs, 34 flashcards in 4 categories, 39-question quiz (runtime shuffle). All worked examples and quiz problems use original numbers, checked by script. 1.8 is a review sheet; its geometric proofs are tested on Unit 2.
- **Unit 2** (Proofs, 2.1–2.6 plus 2.13 coordinate formulas): guide with 7 tabs, 32 flashcards, 31-question quiz. 2.7 and 2.8 are test reviews with no new content.
- **Unit 3** (Quadrilaterals, 3.1–3.10; no 3.5/3.8 materials): guide with 6 tabs, 34 flashcards in 5 categories, 31-question quiz.

### Biology (live)

- **Unit 1** (Bonding & Water, Macromolecules, Enzymes): guide with 4 tabs, 22 flashcards, 21-question quiz.
- **Unit 2** (Cells, the Membrane & Transport, 2.1–2.12): guide with 9 tabs (2.1 ×2, 2.2, 2.3 ×2, 2.7, 2.7–2.8, 2.10, 2.11–2.12), 63 flashcards in 6 categories, 75-question quiz with a per-lesson picker. No materials exist for 2.4, 2.5, 2.9; 2.6 is the same membrane content as 2.3.
- **Prefix & Suffix Vocabulary** (own section, groups 1–6): 60 flashcards in 6 group tabs, 60-question quiz with a group picker. Wrong answers never include a look-alike meaning (Two / Two, twice; the two "Same" and two "Blood" entries).
- **Source conflicts flagged on the pages:** peripheral proteins (2.3 lecture vs. 2.6 worksheet key); origin of the cotransport gradient (2.11 slide vs. 2.12 worksheet key).
- **Third-party handouts in Unit 2 (scope only, never copied):** Bethany Lau worksheets (2.6, 2.7, 2.11/2.12 keys), NCCSTS "Osmosis Is Serious Business!" case (2.8), 3D Molecular Designs modeling kit (2.12).
