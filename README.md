# 🧠 Smart Quiz \& Performance Analyzer System

A menu-driven, console-based quiz application built entirely in Python using only loops, conditions, and basic data structures — no external libraries required. Designed to run inside a Jupyter Notebook.

\---

## 📋 Table of Contents

* [Overview](#overview)
* [Features](#features)
* [Custom Features](#custom-features)
* [Project Structure](#project-structure)
* [How to Run](#how-to-run)
* [Menu Options](#menu-options)
* [Question Bank](#question-bank)
* [Scoring System](#scoring-system)
* [Performance Classification](#performance-classification)
* [Technical Constraints](#technical-constraints)
* [Bonus Features](#bonus-features)

\---

## Overview

This system conducts a multiple-choice quiz, tracks your performance across multiple attempts, identifies your strengths and weaknesses by topic, and provides personalised feedback rather than just a raw score. It is entirely menu-driven and runs interactively inside a Jupyter Notebook kernel.

\---

## Features

|Feature|Description|
|-|-|
|Menu-driven interface|6-option main menu navigated via numbered input|
|MCQ quiz engine|Displays questions one at a time with 4 options each|
|Input validation|All inputs loop until a valid value is entered|
|Score tracking|Marks awarded per question based on difficulty|
|Topic breakdown|Per-topic accuracy shown after every quiz|
|Performance classification|Beginner / Intermediate / Advanced / Needs Work|
|Personalised feedback|Specific written feedback based on 5 different signals|
|Multi-attempt history|All sessions stored in memory for the kernel lifetime|
|Trend detection|Detects improving / declining / stable across last 3 attempts|
|Personal leaderboard|Top 5 attempts ranked by percentage|

\---

## Custom Features

### 1\. ⏱ Timer Per Question

Every question has a time limit based on its difficulty level:

|Difficulty|Time Limit|Marks|
|-|-|-|
|Easy|20 seconds|1 mark|
|Medium|30 seconds|2 marks|
|Hard|45 seconds|3 marks|

The timer starts the moment the question is displayed and stops when you press Enter. If you exceed the limit, the answer is marked as a timeout and no marks are awarded (no penalty either).

### 2\. ➖ Negative Marking

Toggled on/off during quiz setup before each attempt. When enabled:

* A correct answer earns the full marks for that question
* A wrong answer deducts half the question's marks (minimum 1)
* A timed-out answer is always 0 (no deduction)

\---

## Project Structure

The notebook is divided into **11 cells**, each with a single responsibility. All cells must be run top-to-bottom once to load functions into memory. Only **Cell 11** needs to be re-run to start a new quiz session.

```
Cell 1  — Markdown title and usage instructions
Cell 2  — Imports (random, time) and global state
Cell 3  — Question bank (16 MCQs across 5 topics)
Cell 4  — Input validation helpers
Cell 5  — Display utilities (headers, ASCII progress bars)
Cell 6  — Quiz engine (timer, answer checking, result list)
Cell 7  — Scoring engine (metrics, topic breakdown)
Cell 8  — Performance analyser (classification, feedback)
Cell 9  — History and leaderboard
Cell 10 — Quiz setup wizard (difficulty, count, negative marking)
Cell 11 — Main menu (ENTRY POINT — re-run this to play)
```

\---

## How to Run

### Requirements

* Python 3.x
* Jupyter Notebook or JupyterLab
* No external packages needed

### Steps

1. Open the notebook in Jupyter:

```
   jupyter notebook smart\_quiz\_system.ipynb
   ```

2. Run all cells from top to bottom using:
`Kernel → Restart \& Run All`
3. The quiz menu will launch automatically at Cell 11.
4. To play again without losing your history, **re-run only Cell 11**.
5. To reset all scores and start fresh, **re-run Cell 2** (resets `scores\_history = \[]`), then re-run Cell 11.

> \*\*Note:\*\* All quiz history is stored in memory only. Restarting the kernel clears all attempt history.

\---

## Menu Options

```
1. Start Quiz              → Opens the setup wizard, then runs the quiz
2. View Last Score         → Shows the full score card for your most recent attempt
3. Performance Analysis    → Shows detailed analysis and feedback for the last attempt
4. View All Attempts       → Lists every attempt with timestamp, score, and level
5. Leaderboard             → Shows your top 5 attempts ranked by percentage
6. Exit                    → Prints a summary and ends the session
```

### Quiz Setup (triggered by Option 1)

When you start a quiz, you are asked three questions:

1. **Difficulty** — Easy / Medium / Hard / Mixed (random from all)
2. **Number of questions** — Between 1 and however many are available for that difficulty
3. **Negative marking** — Yes or No

\---

## Question Bank

16 questions across 5 topics and 3 difficulty levels:

|Topic|Easy|Medium|Hard|
|-|-|-|-|
|Python|3|2|1|
|Data Structures|1|1|1|
|OOP|1|2|0|
|Algorithms|0|1|1|
|Databases|0|1|1|

Each question dict contains: `id`, `question`, `options` (list of 4), `answer` (1–4), `topic`, `difficulty`, `marks`.

\---

## Scoring System

```
raw\_score  = sum of marks\_earned across all questions
max\_score  = sum of max\_marks across all questions
percentage = (raw\_score / max\_score) × 100   \[clamped to 0 minimum]
accuracy   = (correct\_count / total\_questions) × 100
avg\_time   = mean seconds taken per question
```

Each attempt is stored as a session dict and appended to `scores\_history`, which persists for the entire kernel session.

\---

## Performance Classification

|Percentage|Level|Icon|
|-|-|-|
|80% and above|Advanced|🏆|
|55% – 79%|Intermediate|⚡|
|30% – 54%|Beginner|📘|
|Below 30%|Needs Work|🔄|

### Personalised Feedback Signals

The analyser generates specific written feedback based on five independent signals:

1. **Overall level** — what to do next based on your classification
2. **Weak topics** — the 1–2 topics where your accuracy was lowest
3. **Speed** — whether you answered too fast, too slow, or at a good pace
4. **Timeouts** — if you ran out of time, actionable advice is given
5. **Trend** — whether your scores are improving, declining, or stable across attempts

\---

## Technical Constraints

This project uses **only**:

* `random` (Python standard library) — for `random.sample()` to shuffle questions
* `time` (Python standard library) — for `time.time()` and `time.strftime()`
* Python built-in types: `list`, `dict`, `str`, `int`, `float`, `bool`
* Control flow: `while`, `for`, `if/elif/else`, `break`, `continue`
* I/O: `input()`, `print()`

No pandas, numpy, or any third-party library is used anywhere.

\---

## Bonus Features

|Bonus|Implementation|
|-|-|
|Multi-attempt storage|`scores\_history` list accumulates all session dicts in memory|
|Progress tracking|`detect\_trend()` compares last 3 attempts and returns improving/declining/stable|
|Personal leaderboard|`view\_leaderboard()` sorts `scores\_history` by percentage and prints top 5 with medals|

\---

## Author Notes

* The `get\_valid\_input()` and `get\_int\_input()` helper functions are the foundation of all user interaction — they ensure no invalid input ever reaches the quiz logic.
* Mixed difficulty mode attaches a `\_time\_limit` key to each question dict so the engine can apply per-question time limits without a global override.
* Re-running Cell 11 replays the full menu without losing any history from previous attempts in the same session.

