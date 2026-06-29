<div align="center">
  <h1>⚔️ Adversarial Verify · 对抗验证</h1>

  <p><strong>Pack your project. Let an adversarial AI find the bottlenecks you're blind to.</strong></p>

  <p>
    <a href="https://github.com/Arykozhang/adversarial-verify/stargazers">
      <img src="https://img.shields.io/github/stars/Arykozhang/adversarial-verify?style=for-the-badge&color=FFD700&labelColor=050b1f&logo=github" alt="Stars" />
    </a>
    <a href="LICENSE">
      <img src="https://img.shields.io/github/license/Arykozhang/adversarial-verify?style=for-the-badge&color=blue&labelColor=050b1f" alt="MIT License" />
    </a>
    <img src="https://img.shields.io/badge/Claude%20Code-skill-D97757?style=for-the-badge&labelColor=050b1f" alt="Claude Code Skill" />
    <img src="https://img.shields.io/github/last-commit/Arykozhang/adversarial-verify?style=for-the-badge&color=22c55e&labelColor=050b1f&logo=git&logoColor=white" alt="Last commit" />
  </p>

  <p>
    <a href="README_CN.md">中文文档</a>
  </p>
</div>

---

## What is this?

A **Claude Code Skill** that packs your project into structured materials and sends them to an independent adversarial AI for deep diagnosis.

This is not "ask AI for a few suggestions." It's a systematic audit: you package your entire project — history, iterations, failed attempts, source code, outputs — into a flat 20-file brief, and another AI attacks it from 4 directions to find the design choices you've normalized into blindness.

## Who is this for?

You're deep in something — code, strategy, research, product — and you *know* there are blind spots, but you can't see them yourself. This skill gives you a structured way to get an external adversarial perspective.

| Role | Use case |
|---|---|
| **Engineers** | Pack your system architecture + source code → find regex bugs, logic gaps, edge cases your test suite misses |
| **Founders / Strategists** | Pack your business plan + market analysis → stress-test assumptions before they cost you money |
| **Product Managers** | Pack your PRD + competitor teardown → find UX contradictions and missing edge cases |
| **Researchers** | Pack your methodology + dataset → audit your reasoning chain for gaps and unstated assumptions |
| **Security Engineers** | Pack system design + threat model → let adversarial AI hunt for attack surfaces you normalized |
| **Investors / Analysts** | Pack your investment thesis → get a devil's advocate that actually read your source materials |

The common thread: **you have a system or argument that feels ~80% there, and you need someone (or something) to tell you where the last 20% is hiding.**

## Why adversarial?

| Ordinary approach | Adversarial Verify |
|---|---|
| Describe the problem → AI gives tips | Package the *whole project* → AI runs a structured audit |
| AI doesn't know what you've tried | You tell it: "AST approach was tried and abandoned" (so it won't suggest it again) |
| You get generic advice | You get P0/P1/P2-graded findings with file references and fix specs |
| You implement alone | Each fix comes with acceptance criteria — you know when it's done |

## How it works

```
  ╔══════════════════════════════════════════════════════╗
  ║                  ADVERSARIAL VERIFY                  ║
  ╠══════════════════════════════════════════════════════╣
  ║                                                      ║
  ║  Step 1        Step 2        Step 3–4      Step 5    ║
  ║  ┌──────┐     ┌──────┐     ┌──────────┐   ┌──────┐   ║
  ║  │SCOPE │ ──▶ │PACK  │ ──▶ │ GENERATE │──▶│SEND  │   ║
  ║  │check │     │20-50 │     │ tasks +  │   │to    │   ║
  ║  │level │     │files │     │ message  │   │adv.AI│   ║
  ║  └──────┘     └──────┘     └──────────┘   └──────┘   ║
  ║                                      │               ║
  ║                                      ▼               ║
  ║  Step 6                  Step 7          ┌──────┐    ║
  ║  ┌──────────────┐      ┌──────────┐     │adv.AI│    ║
  ║  │P0 fix / P1   │ ◀─── │CLEANUP   │ ◀── │reply │    ║
  ║  │schedule / P2 │      │stale     │     │with  │    ║
  ║  │log           │      │artifacts │     │audit │    ║
  ║  └──────────────┘      └──────────┘     └──────┘    ║
  ║                                                      ║
  ╚══════════════════════════════════════════════════════╝
```

**Step 1** — Confirm scope: current metrics, target benchmark, known bottlenecks, iteration history.

**Step 2** — Assemble the 7-category material pack using `templates/package-structure.md`: background docs, representative samples, core source code, system outputs, methodology notes, full dataset, and the mission brief. All flat, ≤50 files.

**Step 3** — Generate a mission document (4 attack directions with specific deliverables) using `templates/task-document.md`.

**Step 4** — Generate a ready-to-paste message for the adversarial AI using `templates/message-template.md`.

**Step 5** — The user uploads the files and sends the message.

**Step 6** — Process the adversarial AI's reply using `templates/execution-command-template.md`: P0 (blocking) → fix immediately, P1 (important) → schedule, P2 (optimization) → log.

**Step 7** — **Interference cleanup**: after deploying fixes, purge stale artifacts (old agent scripts, expired SystemPrompts, `__pycache__`, obsolete CSVs) that could silently break the new system.

## Quick install

```bash
git clone https://github.com/Arykozhang/adversarial-verify.git \
  ~/.claude/skills/adversarial-verify
```

Then in Claude Code:

```
/adversarial-verify
```

Trigger words: `对抗验证` · `找AI评审` · `四轮验证` · `adversarial verify` · `魔鬼代言人`

## Real-world pattern

> Fictional example illustrating the complete workflow. All data and names are invented.

Xiao Zhang built a "Smart Code Review Agent" — it audits GitHub PRs and suggests fixes. After three iterations he hit a wall: 78% recall, 35% false positives. An open-source project claimed 92% / 12%.

He ran `/adversarial-verify`, packed 29 files across 7 categories (iteration history, abandoned AST approach, known generic-type blind spots, 200-test-PR dataset, current prompt templates, system behavior traces for 10 key cases), and sent them with a 4-axis mission brief.

The adversarial AI returned 17 findings. **P0-1**: the regex `<T extends \w+>` matched single-layer generics but missed nested `<T extends Comparable<T>>` — causing ~6% missed detections. **P0-3**: the prompt asked the LLM to check for null pointers but never fed it variable type information — the LLM was guessing blind.

After fixes and Step 7 cleanup (deleted the old `ast_reviewer.py`, purged `__pycache__`): recall jumped from 78% to 87%.

## Project layout

```
adversarial-verify/
├── SKILL.md                              # Core instructions (Claude Code loads this)
├── README.md                             # You are here
├── README_CN.md                          # 完整中文文档
├── LICENSE                               # MIT
└── templates/                            # 4 reusable templates
    ├── package-structure.md              # 7-category packing checklist
    ├── task-document.md                  # Adversarial AI mission brief
    ├── message-template.md               # Ready-to-paste communication script
    └── execution-command-template.md     # P0/P1/P2 reply processor
```

## License

MIT — see [LICENSE](LICENSE).
