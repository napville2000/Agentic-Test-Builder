# ⚡ StudyPulse — AI-Powered Quiz Generator

A lightweight, single-file web app that turns any study material into a randomized multiple-choice quiz using Claude AI. Built for students who learn best through practice testing.

**No installation. No login. No app download. Just open the link and study.**

---

## 🎯 What It Does

Paste in any study content — class notes, textbook excerpts, slide text, or copied articles — and StudyPulse uses Claude AI to instantly generate a custom multiple-choice quiz from that material.

Each quiz features:
- Randomized questions pulled directly from your content
- Four answer choices per question with carefully crafted distractors (wrong answers that are plausible, not obviously silly)
- Immediate right/wrong feedback after each answer
- A full review screen at the end showing every question with correct and incorrect answers marked
- A **"New Questions, Same Topic"** button that sends a fresh request to Claude — different questions, different distractors, same material — so students can keep drilling until it clicks

---

## 🚀 How to Use It

### Step 1 — Get Your API Key

StudyPulse uses the Anthropic API to generate questions. You need a free API key:

1. Go to **[console.anthropic.com](https://console.anthropic.com)**
2. Create a free account (this is separate from your Claude.ai account)
3. Click **API Keys** in the left sidebar
4. Click **Create Key**, give it a name like `StudyPulse`
5. **Copy the key immediately** — it starts with `sk-ant-...` and is only shown once

> 💡 **Cost:** Generating a 10-question quiz costs roughly $0.01–0.02 (one to two cents). New accounts receive free credits to get started.

---

### Step 2 — Open the App

Visit the live app at:

**[https://napville2000.github.io/Agentic-Test-Builder/](https://napville2000.github.io/Agentic-Test-Builder/)**

Works on desktop and mobile browsers — no install needed.

---

### Step 3 — Generate a Quiz

1. Paste your **Anthropic API key** into the key field at the top
2. Paste your **study content** into the large text box (notes, slides, articles, etc.)
3. Choose how many questions you want (**3–25**) and a difficulty level
4. Hit **Generate Quiz ⚡**
5. Claude reads your content and builds the quiz in a few seconds

---

### Step 4 — Take the Quiz

- Answer each question by tapping or clicking a choice
- Get instant feedback — green for correct, red for wrong with the right answer revealed
- Work through all questions, then review your results
- Hit **New Questions, Same Topic** to generate a completely fresh quiz from the same material

---

## 📋 Tips for Best Results

**Preparing your content:**
- Plain text works best — paste directly from notes, Google Docs, or slide text
- PDFs need to be converted to text first (copy/paste the text content)
- More content = better, more varied questions. Aim for at least a few paragraphs
- Content is trimmed to ~12,000 characters if very long — break large topics into sections

**Getting good questions:**
- Use **Standard** difficulty for general studying
- Use **Challenging** for exam prep — distractors will be much closer to the correct answer
- Use **Easy** when first encountering new material
- Run **New Questions** multiple times — each pass generates entirely different questions from the same content

---

## 🔐 Privacy & Security

- Your API key is held **only in memory** during your session — it is never stored, logged, or transmitted anywhere other than directly to Anthropic's API
- Refreshing the page clears the key — you'll need to paste it again each session
- No accounts, no data storage, no cookies, no tracking

---

## 🛠 Technical Details

- Single self-contained HTML file — no framework, no build step, no dependencies
- Calls the Anthropic Messages API directly from the browser using `claude-sonnet-4-20250514`
- All quiz state is held in memory — nothing is persisted between sessions
- Works on any modern browser on desktop or mobile

---

## 📁 Repository Structure

```
index.html   — The entire application (HTML + CSS + JavaScript)
README.md    — This file
```

---

## 🔄 Updating the App

To deploy a new version:
1. Edit or replace `index.html` in this repository
2. GitHub Pages automatically rebuilds and redeploys within about 60 seconds

---

*Built with Claude AI by Anthropic · Hosted free on GitHub Pages*
