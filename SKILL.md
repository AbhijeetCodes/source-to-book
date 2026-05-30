---
name: source-to-book
description: >
  Transform any long-form source (podcast transcripts, interviews, lectures,
  blog posts, articles, essays) into a polished, easy-to-read book of narrative
  chapters, compiled into a Kindle-friendly PDF. Trigger when the user wants to
  turn source material into "a book," "chapters," or a downloadable manuscript.
---

# Source → Book

Turn raw source material into a book people actually want to read.

---

## ALWAYS ask these five questions first

Before doing anything else, present these to the user. Offer the defaults so
they can say "defaults are fine" and you proceed immediately.

| # | Question | Default |
|---|----------|---------|
| 1 | **Tone of the book?** | Bill Bryson meets Neil deGrasse Tyson: warm, witty, curious, vivid, accessible |
| 2 | **Length of the book?** | Whatever Claude judges adequate for the source |
| 3 | **Search the internet for extra detail/references?** | No |
| 4 | **Output format?** | PDF (Kindle-optimized) |
| 5 | **Any special requests?** | None |

Once confirmed, do not ask further questions. Just build.

---

## The cardinal rule: transform, don't transcribe

The source is conversation. The output is a book. These are fundamentally
different forms. The skill must convert dialogue, Q&A, rambling discussion, and
spoken-word tangents into clean, flowing narrative prose.

**Never do this:**
- Preserve "Host: ... Guest: ..." dialogue format
- Write "David said... then Ben replied..."
- Summarize the conversation turn by turn
- Use "the hosts discussed" / "the episode covered"

**Always do this:**
- Absorb the facts, stories, insights, and quotes from the source
- Restructure them into a narrative with a dramatic spine
- Write as an omniscient narrator telling the story directly to the reader
- Weave in direct quotes sparingly and naturally ("As Jensen put it, ...")
- Make it read like the person picked up a book, not a transcript

The reader should never be aware they are reading something that was once a
conversation. It should feel written from the ground up as a book.

---

## Voice (default: Bill Bryson + Neil deGrasse Tyson)

- **Bryson:** warmth, gentle wit, an eye for the telling human detail, a rhythm
  that feels like a smart friend telling you a great story. Never sarcastic.
- **Tyson:** clarity, genuine wonder, clean analogies for hard ideas, enthusiasm
  that respects the reader's intelligence. Never condescending.
- **Together:** easy to read, never dense or jargon-clogged, never dumbed down.
  Explain every technical idea in plain language with an analogy, then move on.
  Keep the reader smiling and learning at the same time.

If the user chose a different tone, honor it fully.

---

## Completeness: do not drop facts

Easy to read does not mean lossy. The book must preserve every important detail,
fact, figure, name, and idea from the source. Before finishing each chapter,
check the source for: the main people and their roles, founding/origin details,
central turning points, crucial numbers (dates, revenue, stakes), the core
mechanism or business model, the decisive quotes, and the "why it matters"
payoff. Weave them into narrative rather than listing them. Go deeper rather
than cutting.

---

## No external research (unless user said yes)

Work only from the supplied source. Do not search the web, fetch URLs, or add
outside facts. If the source has a gap, tell the user rather than filling it
from outside knowledge. Never fabricate quotes, statistics, or events.

If the user answered yes to question 3, you may add corroborating detail from
web research, clearly attributed.

---

## Formatting rules

- **Zero em-dashes.** Also avoid en-dashes, double-hyphens, or semicolons used
  as dashes. Use commas, parentheses, or full stops instead.
- No bullet points or sub-headers inside prose. Continuous narrative only.
- Use `---` between major sections within a chapter.
- Quotes woven into sentences, short and purposeful. Never fabricated.

---

## Chapter file structure

Every chapter follows this exact template (the PDF builder parses it):

```markdown
# Chapter N

## The Evocative Title

*One italic sentence previewing the arc and seeding curiosity.*

---

[opening scene or hook]

[narrative body in movements separated by ---]

[closing that lands the meaning]
```

---

## Workflow

### 1. Setup

```
/home/claude/book/
  chapters/        # NN_slug.md files (source of truth)
  sources/         # raw text saved to disk
  build_pdf.py     # the build script
```

Show the user a chapter table (number, title, subject) and update it after
every chapter with word counts and progress.

### 2. Load source

- Large tool results: extract text, save to `sources/NN_slug.txt`.
- Don't read entire sources into context. Scan for key beats with grep/sed,
  then read only those ranges.
- Blogs/articles: save raw text the same way; split into chapters by theme.
- If a source connection drops, tell the user. Do not silently fill from
  your own knowledge.

### 3. Draft each chapter

- Read key sections of the source. Identify the dramatic spine.
- Write the chapter as narrative prose following the voice and formatting rules.
- Match depth to source richness: rich sources get 4,000-9,000 words, thin
  sources 2,500-3,500. If a draft is thin on a rich source, expand with
  str_replace insertions.

### 4. Audit each chapter

```bash
wc -w chapters/NN_slug.md
python3 -c "
t = open('chapters/NN_slug.md').read()
print('em-dash:', t.count('\u2014'), 'en-dash:', t.count('\u2013'), 'double-hyphen:', t.count('--'))
"
```

All counts must be 0 (or near-zero and deliberate). Fix before moving on.
Copy approved file to `/mnt/user-data/outputs/Chapter_NN_Name.md`.

### 5. Full manuscript audit

Before building: confirm zero dashes everywhere, print all word counts, verify
chapter header shapes (`# Chapter N / ## Title / *subtitle*`), and do a final
completeness check that no major source fact was dropped.

### 6. Build PDF

`pip install reportlab --break-system-packages`

Use the shipped `build_pdf_template.py` (adapt title/metadata; reuse, don't
rewrite). Specs:

- 6×9 inch pages (Kindle standard)
- Margins: inner 0.75", outer 0.65", top 0.75", bottom 0.65"
- Times-Roman 11pt, 17pt leading, justified, 18pt first-line indent
- Front matter: title page → about page → linked table of contents
- Each chapter on a fresh page with bookmark anchors
- No running headers/footers (Kindle strips them)
- Set title/author metadata

Verify with pypdf after build (page count, metadata, chapter placement).
Copy to `/mnt/user-data/outputs/` and `present_files`.

Other formats if requested: Markdown (concatenated), DOCX (use docx skill),
EPUB/MOBI (true reflowable e-book).

### 7. Deliver with evaluation

Brief honest assessment: strongest chapters, chapters worth deepening,
any style issues from automated cleanup, confirmation that the tone was
applied and no major facts were dropped, output spec summary.
