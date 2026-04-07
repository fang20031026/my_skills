---
name: "paper-reading-translator"
description: "Read and explain academic papers section by section in Chinese. Use when the user wants help reading a paper, PDF, preprint, or article with a workflow like: first translate each section into Chinese, then add useful explanations, clarify terminology, answer side questions, and adapt the pacing chapter by chapter instead of dumping the whole paper at once."
---

# Paper Reading Translator

## Overview

Help the user read a paper in a paced, chapter-by-chapter way.

Default output style:
- translate the current section into Chinese first
- then explain the important ideas in plain Chinese
- separate direct translation from commentary so the user can tell which is which
- adapt depth to the user's reading pace

## Workflow

### 1. Build the reading scaffold first

Before deep explanation:
- identify the paper title and source file or link
- extract or inspect the section structure
- confirm the paper version if relevant
- tell the user you will proceed section by section

If the user already specifies a section, start there.
If the user says to go one chapter at a time, do not dump the whole paper.

### 2. Translate faithfully before interpreting

For each section:
- provide a faithful Chinese translation first
- preserve the paper's technical meaning and logical order
- keep model names, dataset names, algorithm names, and benchmark names in their original forms when useful
- translate surrounding prose naturally into Chinese

Do not let explanation replace translation.
The user should be able to recover the original meaning from your translation alone.

### 3. Add explanation after the translation

After the translation, add a clearly separated explanation block.
Use it to do only the helpful work that the paper does not already do well:
- explain the purpose of the section
- define technical terms in plain language
- connect formulas or methodology to intuition
- point out what is important versus background framing
- highlight what the reader should remember

Prefer explanations such as:
- “这一段在回答什么问题”
- “这一步为什么重要”
- “这和前文的关系是什么”
- “作者真正想证明什么”

### 4. Keep the granularity user-controlled

Default to one section at a time.
If the user asks for more detail:
- slow down
- expand the section-level explanation
- unpack formulas, terms, and comparisons more carefully

If the user asks for less detail:
- compress to translation plus a short takeaway

When the user asks an intermediate question, answer it, then return to the paper flow.
If the paper itself answers the user's question later, say so explicitly and point to the relevant section.

### 5. Distinguish translation, explanation, and takeaway

A good structure is:
- section title
- 原文翻译
- 这一节在说什么 / 解释
- 关键点 / 读后该记住什么

Use Chinese throughout unless the user asks otherwise.
Keep the writing clear and instructional rather than ornamental.

## Explanation Rules

### Translate terminology carefully

Keep the English term when it matters for future reading, especially for:
- model names
- algorithms
- benchmarks
- datasets
- optimization methods
- evaluation settings

A good pattern is:
- “监督微调（Supervised Fine-Tuning, SFT）”
- “部分可观测马尔可夫决策过程（POMDP）”

### Explain at the right level

Prefer concrete intuition over textbook definitions.
For example:
- explain compounding errors as “一步偏了，后面越来越偏”
- explain PPO as “边和环境交互边更新，并限制每次策略变化不要太大”

Do not over-explain universally known basics unless the user signals they need it.
Do explain terms the user has already said they do not know well.

### Preserve the paper's claims versus your interpretation

Be explicit about which statements are:
- the paper's own claim
- the paper's reported evidence
- your interpretation to help the user understand

Useful labels include:
- “原文翻译”
- “作者的意思是”
- “补充解释”
- “更直白地说”

## Interaction Pattern

When the user is reading continuously:
- keep momentum
- continue to the next requested section without re-planning everything

When the user asks “为什么” or “这是什么意思”:
- answer directly
- if later sections contain the stronger answer, say where
- do not force the user to wait if a short answer is already helpful

When summarizing a section, prefer conclusions like:
- what problem this section solves
- what evidence it gives
- what the reader should remember

## Quality Bar

A strong response for this skill should make the user feel:
- the paper became readable in Chinese
- the important ideas were separated from the noise
- technical terms became easier to carry into later sections
- they can interrupt anywhere and still keep the reading thread coherent

Avoid:
- turning every section into an unstructured wall of prose
- replacing translation with pure paraphrase
- giving only surface-level summaries when the user asked for detail
- drifting away from the paper into unrelated background
