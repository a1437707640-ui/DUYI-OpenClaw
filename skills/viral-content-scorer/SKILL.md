---
name: viral-content-scorer
description: >
  Score and diagnose WeChat article drafts for viral potential before publishing.
  Analyzes content across 29 sub-items (title, opening, trust signals, framework, emotion,
  punchlines, CTA) and outputs a score card with specific improvement suggestions.
  Use when user wants to: "给这篇文章打分", "分析这篇文章爆款潜力", "文章发布前审稿",
  "帮我看看这篇写得怎样", "诊断为什么这篇没爆", or evaluate any article draft.
---

# Viral Content Scorer

## Overview

[TODO: 1-2 sentences explaining what this skill enables]

## Overview

Read the article draft, score it across 7 content dimensions (29 sub-items), then output a score card with specific improvement suggestions targeting the lowest-scoring items.

## Workflow

### Step 1: Read the Article
Accept input as: pasted text, a file path, or a WeChat article URL (use agent-browser to fetch).

### Step 2: Score Content Layer (满分 145分)
Score each sub-item 1–5. Load `references/scoring_dimensions.md` for the full rubric.

**同时检查硬否决清单（H1-H5）**：位于 `references/scoring_dimensions.md` 末尾，不计入总分，但任一触发则强制 `revise`，优先级高于总分。

Quick dimension summary:
| # | 维度 | 子项数 | 满分 |
|---|---|---|---|
| D1 | 标题钩子力 | 4 | 20 |
| D2 | 开头前200字 | 5 | 25 |
| D3 | 信任建立层 | 5 | 25 |
| D4 | 核心框架力 | 4 | 20 |
| D5 | 情感曲线设计 | 5 | 25 |
| D6 | 金句密度 | 3 | 15 |
| D7 | CTA转化设计 | 3 | 15 |

### Step 3: Identify Top Issues
Find the 3 lowest-scoring sub-items. For each, give a **specific, actionable rewrite suggestion** — not generic advice.

Example of bad suggestion: "把标题写得更有吸引力"
Example of good suggestion: "标题缺乏具体人群定位，把「如何提升效率」改成「月入5千的打工人，做这件事3个月后辞职了」"

### Step 3.5: Apply judge-policy
Load `judge-policy.md` to determine final verdict：
- scorer ≥ 127 **且** 无 H1-H4 触发 **且** 无 judge-policy 必打回项触发 → `pass`
- 任一不满足 → `revise`（先列 H1-H4/必打回项触发原因，再补 scorer Top3，合并不超过 5 条）
- scorer < 73 且有 H/必打回项触发 → `drop`

### Step 4: Output Score Card

```
📊 文章爆款潜力诊断报告
========================
标题：[文章标题]
总分：XX / 145分  →  [等级]

维度得分：
  D1 标题钩子力   ████░░  16/20
  D2 开头前200字  ███░░░  13/25
  D3 信任建立     ████░░  18/25
  D4 核心框架     ████░░  16/20
  D5 情感曲线     ██░░░░  10/25
  D6 金句密度     ███░░░  09/15
  D7 CTA设计      ████░░  12/15

💡 Top 3 改进项（按优先级）：
1. [最低分子项] — 当前：X分 → 具体改法
2. [次低分子项] — 当前：X分 → 具体改法
3. [第三低子项] — 当前：X分 → 具体改法

🔮 预测表现：[基于历史基准的传播潜力描述]
```

## Grading Scale
| 分数 | 等级 | 含义 |
|---|---|---|
| ≥127 (87%+) | A 准爆款 | 具备爆款结构，发布后重点监控转发率 |
| 101–126 (70-87%) | B 爆款候选 | 补强最低2项后发布 |
| 73–100 (50-70%) | C 待优化 | 建议修改后再发 |
| <73 | D 需重做 | 核心框架或情感曲线存在根本问题 |

## Reference Files
- `references/scoring_dimensions.md` — Full 29-item rubric with 1/3/5 scoring criteria for each sub-item
