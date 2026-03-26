# ⚡ StudyPulse — AI Quiz & Flashcard Generator

A single-file, browser-based study app powered by the Anthropic Claude API. No backend, no install, no account — just paste your API key, load your study material, and go.

**Live app:** [napville2000.github.io/Agentic-Test-Builder](https://napville2000.github.io/Agentic-Test-Builder/)

---

## What's New in v2

Version 2 is a major update focused on matching real test formats and simplifying the parent/student workflow.

### New Question Types

**Multi-Select ("Choose exactly N")**
The app now generates questions that require selecting multiple correct answers — the same format used on many standardized and classroom tests. Partial credit is tracked and displayed (e.g. "2 of 3 correct"). Wrong answer explanations are shown automatically so the student understands *why* they missed it.

**Paired Fill-in-the-Blank**
Two linked blanks in a single sentence that must both be correct for full credit. Example: "The karyotype shows the ___ number of chromosomes found in ___ of a human female." Both blanks are graded individually with partial credit.

**Improved Distractors**
Wrong answers now represent common student misconceptions rather than random options. A brief explanation is shown after each wrong answer explaining the most common error.

---

### File-Based Workflow

StudyPulse v2 uses a portable `.json` study file as the source of truth. This replaces the manual paste-and-send workflow.

**How it works:**

1. **Admin (parent/teacher)** creates a study file from the Admin tab — paste the unit name, answer key, and study notes. Click Generate. The app runs a scope check and exports a `.json` file.
2. **Send the file once** to the student.
3. **Student loads the file** from the Quiz tab. Her notes appear in the Notes tab for editing.
4. **She appends notes** during the week — auto-saved in memory, downloadable anytime.
5. **She generates quizzes** directly from the file. No re-pasting required.

The admin never needs to touch the file again unless the unit changes.

---

### Notes Editor with Auto-Save

The Notes tab shows the raw markdown notes from the loaded study file in an editable textarea.

- **Auto-saves** 2 seconds after the student stops typing
- Shows a live status indicator: *Saving…* → *Saved ✓*
- Manual **Save Notes** button as a fallback
- **Download File** button exports the updated `.json`
- **Copy JSON** button as an iOS Safari fallback (paste into a `.json` file)
- Browser warns before closing if there are unsaved changes

---

### Scope Check & Gap Fill

When a study file is loaded, a **Full Scope Mode** toggle appears in Quiz Settings and the Settings tab.

When enabled:
1. Claude analyzes the notes against standard curriculum for the unit
2. A modal shows which concepts are covered and which are missing
3. Missing concepts can be added individually with **[+ Add]** or all at once with **Supplement All**
4. Added concepts are included in question generation, tagged with a 🔵 badge on each question card
5. A coverage score (0–100%) is shown so the student can see how complete her notes are

Scope check only runs once per file load. It does not re-run unless notes have changed.

---

### Bottom Tab Navigation

The app is now organized into four tabs, accessible from a fixed bottom navigation bar — optimized for mobile thumb reach.

| Tab | Purpose |
|-----|---------|
| ⚡ Quiz | Generate and take quizzes or flashcard sessions |
| 📝 Notes | View and edit study notes from the loaded file |
| ⚙️ Settings | Configure scope mode, question type mix |
| 🔧 Admin | Create and export new study files |

---

## Study File Format

The `.json` study file has two layers:

**Structured layer** (managed by the app):
```json
{
  "version": 2,
  "unit": "Cell Division and Cell Differentiation",
  "created": "2026-03-25",
  "last_modified": "2026-03-27",
  "last_compiled": "2026-03-27",
  "answer_key": "Q1: A, C, D\nQ2: B...",
  "scope_check": {
    "covered_concepts": ["Mitosis", "Karyotype reading"],
    "missing_concepts": [
      { "concept": "Inversion vs translocation", "reason": "commonly tested chromosome mutation type" }
    ],
    "coverage_score": 62
  },
  "questions": []
}
```

**Raw notes layer** (human-editable markdown string):
```json
{
  "notes_md": "# Cell Division\n\n## Mitosis\nProduces two identical daughter cells...\n\n---\n*Added 3/27*\nInversion = segment reverses within same chromosome"
}
```

The `notes_md` field is a plain string. The student edits it directly in the Notes tab. When she generates a quiz, the full string is sent to Claude as-is. No parsing required.

---

## Question Prompt Architecture

### Scope Check Prompt
Runs once when Full Scope Mode is toggled on. Returns JSON only — covered concepts, missing concepts, and a coverage score. Zero question generation tokens spent until the student actually generates a quiz.

### Quiz Generation Prompt
Supports three question types in a single API call. The question mix is controlled by the Settings tab toggles. Default mix with all types enabled:

- ~40% single-answer MCQ
- ~35% multi-select (choose exactly 3)
- ~20% fill-in-the-blank paired

Each question in the response includes:
- `type`: `single_select` | `multi_select` | `fill_blank`
- `source`: `from_notes` | `supplemented`
- `distractor_reasoning`: explains the most common wrong answer

### Token Efficiency
- Scope check: ~800 tokens (runs once per file load, only if toggled on)
- Quiz generation: ~4,000–5,000 tokens
- Study guide: ~2,000 tokens (only generated on demand for missed topics)
- Most sessions cost zero Claude calls — the student retakes questions from the compiled set

---

## Mobile & Browser Compatibility

StudyPulse v2 is designed for Android and iOS mobile browsers — not just desktop.

- Uses `dvh` units instead of `vh` to handle mobile browser address bar behavior
- Fixed bottom tab bar with safe area inset support (iPhone notch/home bar)
- All tap targets minimum 48px
- No hover states — everything works on tap
- Font size 16px+ on all inputs to prevent iOS auto-zoom
- `-webkit-tap-highlight-color: transparent` on interactive elements
- Markdown editor: plain textarea with auto-resize (no heavy editor libraries)
- iOS Safari file handling: use **Copy JSON** button as fallback if file download fails

---

## Settings

| Setting | Default | Description |
|---------|---------|-------------|
| Full Scope Mode | Off | Supplement missing curriculum concepts |
| Multi-Select Questions | On | Include "choose N" questions |
| Fill-in-the-Blank | On | Include paired sentence completion |
| Difficulty | Standard | Easy / Standard / Challenging |

---

## Getting Started

### Student
1. Load the `.json` study file from the Quiz tab
2. Optionally add notes in the Notes tab
3. Tap **Generate Quiz ⚡** or **Generate Flashcards 🃏**
4. Review missed topics with the Study Guide button after completing a quiz

### Parent / Teacher (Admin)
1. Open the Admin tab
2. Enter your Anthropic API key, unit name, answer key, and study notes
3. Tap **Generate Study File**
4. Send the downloaded `.json` to the student
5. Done — no further action needed unless the unit changes

### API Key
You need an [Anthropic API key](https://console.anthropic.com/) (starts with `sk-ant`). The key is held in browser memory only — it is never saved to disk or transmitted anywhere except directly to the Anthropic API.

---

## Architecture

Single HTML file. No build step, no npm, no framework. Deploy by dropping `index.html` anywhere — GitHub Pages, Netlify, a USB drive, a text message.

- **Claude model:** `claude-sonnet-4-20250514`
- **API call:** direct browser fetch to `https://api.anthropic.com/v1/messages`
- **Storage:** in-memory only during session; file download for persistence
- **Dependencies:** Google Fonts (Syne + DM Sans) via CDN — loads fine offline after first visit

---

## Legacy File Compatibility

Study files and session files saved with v1 still load correctly. The app detects the format automatically and handles both:
- v1 quiz sessions (type: "quiz") → loads directly into quiz mode
- v1 flashcard sessions (type: "flashcard") → loads directly into flashcard mode
- v2 study files (has `notes_md` or `unit` field) → activates full file workflow

---

## Changelog

### v2.0 (2026-03-25)
- Added bottom tab navigation (Quiz / Notes / Settings / Admin)
- Added multi-select question type with partial credit scoring
- Added fill-in-the-blank paired question type
- Added Admin tab for study file creation
- Added Notes tab with auto-save and live status indicator
- Added Scope Check modal with gap analysis and per-concept supplementation
- Added Full Scope Mode toggle with coverage score bar
- Added 🔵 supplemented question badge
- Added distractor reasoning shown on wrong answers
- Improved prompt architecture for question type mix and source tagging
- Mobile-first layout with dvh, safe-area-inset, and 48px tap targets
- Added Copy JSON fallback for iOS Safari
- Added beforeunload warning for unsaved notes changes
- v1 session file backward compatibility preserved

### v1.0
- Initial release: single-answer MCQ quiz and flashcard generation
- Study guide generation for missed topics
- Session save/load
