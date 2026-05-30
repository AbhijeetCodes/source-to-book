# Source → Book

**Turn any podcast transcript, interview, lecture, or long-form article into a polished, Kindle-ready book — using Claude.**

> One prompt. One link. One book.

<img width="1448" height="1086" alt="image" src="https://github.com/user-attachments/assets/3fcb4d02-d2d0-4d6e-93bc-c68150f440d6" />


---

## What it does

Give Claude a URL (or paste raw text) and this skill transforms it into a proper narrative book:

- Converts dialogue / Q&A into flowing prose (no "the host asked…" or "he replied…")
- Structures the content into chapters with dramatic arcs
- Produces a **6×9" Kindle-optimized PDF** with title page, table of contents, and clean typography
- Preserves every important fact, quote, and insight from the source
- Writes in a warm, accessible voice (default: Bill Bryson meets Neil deGrasse Tyson)

---

## Quick start

### Option 1: Copy the prompt directly

1. Open [Claude](https://claude.ai)
2. Copy the entire contents of [`SKILL.md`](./SKILL.md)
3. Paste it as the **first message** in a new conversation (or add it to your project instructions)
4. Then type your request:

```
/source-to-book convert to book: https://lexfridman.com/jensen-huang-transcript/
```

### Option 2: Point Claude to this repo

1. Open [Claude](https://claude.ai)
2. Say:

```
Use the source-to-book skill from this GitHub repo:
https://github.com/AbhijeetCodes/source-to-book

Convert this to a book: https://lexfridman.com/jensen-huang-transcript/
```

Claude will fetch the `SKILL.md` and follow the instructions.

---

## How it works

```
You provide                          Claude produces
─────────────                        ───────────────
Podcast transcript          →        8-12 chapter narrative book
Interview URL               →        Kindle-ready 6×9" PDF
Lecture notes                →        Title page + TOC + chapters
Blog post / essay            →        ~10,000-50,000 words
```

### The 5-question setup

Before building, Claude asks you five quick questions (all have sensible defaults):

| # | Question | Default |
|---|----------|---------|
| 1 | **Tone?** | Bill Bryson meets Neil deGrasse Tyson |
| 2 | **Length?** | Claude judges based on source richness |
| 3 | **Add internet research?** | No (source-only) |
| 4 | **Output format?** | PDF (Kindle-optimized) |
| 5 | **Special requests?** | None |

Say "defaults are fine" and Claude starts building immediately.

---

## The cardinal rule

**Transform, don't transcribe.**

The reader should never know they're reading something that was once a conversation. The book reads as if it were written from scratch — an omniscient narrator telling a compelling story, with direct quotes woven in sparingly and naturally.

---

## Example

### Input

```
/source-to-book convert to book: https://lexfridman.com/jensen-huang-transcript/
```

**Source:** Lex Fridman Podcast #494 — Jensen Huang: NVIDIA, The $4 Trillion Company & the AI Revolution (2h 20m conversation)

### Output

**"The AI Factory: Jensen Huang, NVIDIA, and the Revolution That Changed Everything"**

📄 **47 pages** · 8 chapters · ~10,100 words · Kindle-optimized PDF

| Chapter | Title | Words |
|---------|-------|-------|
| 1 | The Bet That Nearly Killed Them | 1,433 |
| 2 | Sixty Reports and No One-on-Ones | 1,348 |
| 3 | The Four Laws of Intelligence | 1,306 |
| 4 | The Supply Chain Whisperer | 1,207 |
| 5 | Power, the Invisible Bottleneck | 1,250 |
| 6 | The Speed of Light | 1,135 |
| 7 | The Builder Nation | 1,088 |
| 8 | The Token Factories | 1,338 |

📥 **[Download the example PDF](./Jensen_Huang_NVIDIA_AI_Revolution.pdf)**

### Sample passage (Chapter 1)

> *There is a particular flavor of corporate gamble that looks, in retrospect, like genius, but in the moment feels a lot more like jumping off a cliff while still sewing the parachute. The decision by NVIDIA to embed CUDA into every GeForce graphics card it shipped was exactly that kind of gamble. It nearly destroyed the company. It also, as things turned out, laid the foundation for the most consequential computing platform of the twenty-first century.*

---

## What sources work best

| Source type | Quality | Notes |
|-------------|---------|-------|
| Podcast transcripts (1-3 hours) | Excellent | Rich, detailed — produces the best books |
| Long interviews | Excellent | Especially technical or biographical ones |
| Lecture series | Great | Each lecture maps naturally to a chapter |
| Long blog posts / essays | Good | Multiple posts can be combined |
| Short articles (<2000 words) | Fair | May produce only 1-2 chapters |
| Fiction / poetry | Not designed for this | Use a different approach |

---

## Customization

### Tone presets

You can ask for any tone. Some examples:

- **Default:** Bill Bryson + Neil deGrasse Tyson (warm, witty, curious)
- **Business:** Jim Collins style (analytical, case-study driven)
- **Technical:** IEEE longform (precise, detailed, academic)
- **Journalistic:** New Yorker profile style (literary, character-driven)
- **Casual:** Tim Urban / Wait But Why (conversational, fun, lots of analogies)

### Output formats

- **PDF** (default) — Kindle-optimized 6×9", Times Roman, justified
- **Markdown** — concatenated `.md` file
- **DOCX** — Word document (if Claude has the docx skill)
- **Individual chapters** — separate `.md` files

### Special requests

- "Add diagrams based on the conversation"
- "Include a glossary of technical terms"
- "Make it shorter / longer"
- "Focus only on the leadership and management parts"
- "Write it for a teenage audience"

---

## File structure

When Claude builds the book, it creates:

```
book/
├── chapters/
│   ├── 01_chapter_slug.md
│   ├── 02_chapter_slug.md
│   └── ...
├── sources/
│   └── raw_transcript.txt
├── build_pdf.py
└── Final_Book_Title.pdf
```

---

## Tips for best results

1. **Longer sources = better books.** A 2-hour podcast transcript gives Claude enough material for 8-12 rich chapters. A 500-word article won't produce much.

2. **Say "defaults are fine"** if you just want to get going fast. The defaults are well-tuned.

3. **Ask for specific things** in the "special requests" field: diagrams, focus areas, audience level, chapter count preferences.

4. **The transcript doesn't have to be perfect.** Claude handles messy, auto-generated transcripts well. Human-generated ones are better, but not required.

5. **You can iterate.** After the first build, ask Claude to expand thin chapters, add a new chapter on a missed topic, or adjust the tone.

---

## License

MIT — use it however you like.

---

## Credits

- Skill designed for use with [Claude](https://claude.ai) by Anthropic
- PDF generation uses [ReportLab](https://www.reportlab.com/)
- Example source: [Lex Fridman Podcast #494](https://lexfridman.com/jensen-huang/) with Jensen Huang
