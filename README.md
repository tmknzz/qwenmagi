# QwenMAGI — Council of Three Sages

A derivative of [MAGI](https://github.com/tmknzz/MAGI) with pi, local Qwen and per-seat Formations. Install alongside the original `magi`; invoke this skill explicitly as `qwenmagi`.

A skill for Claude Code and Codex that hones a prompt, themed on the **MAGI** of Evangelion. Just as Dr. Naoko Akagi transplanted the three facets of her own personality into three supercomputers, three personas — **the scientist, the mother, the woman** (MELCHIOR / BALTHASAR / CASPER) — score any given prompt independently. They auto-iterate until all three reach 80, then return the version the council has passed. Any genre is fair game — planning, naming, copy, explanation, strategy, analysis — anything worth honing. It runs straight through to a verdict without asking the user questions mid-deliberation.

**Host-neutral:** the same skill runs under Claude Code, Codex, and pi. Seats can also delegate to local models such as Qwen through pi.

**Per-seat Formations:** assign an executor, model, and Thinking level to each council member. Omitted seats use the AI that invoked MAGI (`primary`). The Real MELCHIOR shortcut remains available.

## How it works

There is no external moderator. The three are not critics but **authors** — they write the proposals, sharpen each other, and merge the result themselves.

1. The three units each open with a **proposal** from their own angle, then combine them into a working draft.
2. Each unit **scores independently**, attaching its rejection reasons and "how to raise this next time" (it scores on its own axis alone and never compromises).
3. Each unit **writes the next version itself** from its own angle (the actual content, not abstract instructions).
4. The three drafts are merged into the next version, and this **repeats** until all three reach 80.
5. At all-80 the proposal **passes**; the finished version is presented and the loop ends.

Scores are never fabricated. When the iteration cap is reached and not all three reach 80, MAGI does not stage an 80 — it honestly reports where things stand and **why the council is split** (an honest deadlock report).

## A run, condensed

An example of what a MAGI deliberation looks like (illustrative):

```text
Prompt: "Name a focus timer that never nags."

━━━ MAGI / Opening ━━━
  MELCHIOR (logic)     "Pomo-Quiet" — descriptive; says exactly what it does.
  BALTHASAR (empathy)  "Hush" — calm, and it never scolds the user.
  CASPER (edge)        "'Pomo-Quiet' is a spec sheet, not a name. Make it felt."

━━━ MAGI / Round 1 ━━━
  MELCHIOR   78  reject — "Hush" is memorable but says nothing about focus.
  BALTHASAR  84  pass
  CASPER     63  reject — safe, therefore forgettable. Where is the pull?

━━━ MAGI / Passed (Round 2) ━━━  M:83  B:86  C:82
  Final: "Lull" — a quiet that pulls you under into focus, and never nags.
```

Scores are honest, not scripted: the council passes only at all-80, and reports an honest deadlock if it cannot get there.

## Install

### As a plugin (recommended)

Inside Claude Code, run:

```text
/plugin marketplace add tmknzz/qwenmagi
/plugin install qwenmagi@qwenmagi
```

This registers the skill automatically.

### Manual install (Claude Code)

Copy the skill folder into Claude Code's skills directory:

```bash
git clone https://github.com/tmknzz/qwenmagi.git
cd qwenmagi
mkdir -p ~/.claude/skills && cp -R skills/qwenmagi ~/.claude/skills/qwenmagi
```

### Manual install (Codex)

Copy the skill folder into Codex's skills directory:

```bash
git clone https://github.com/tmknzz/qwenmagi.git
cd qwenmagi
mkdir -p ~/.agents/skills && cp -R skills/qwenmagi ~/.agents/skills/qwenmagi
```

Codex reads user-level skills from `~/.agents/skills` (a repo-local `.agents/skills` also works for per-project use).

### pi

```sh
pi --skill /absolute/path/qwenmagi/skills/qwenmagi/SKILL.md
```

Invoke `/skill:qwenmagi` inside pi. Installing to `~/.agents/skills/qwenmagi` also works.

## Selecting a Formation

Create `~/.config/qwenmagi/formations/local.conf`, then ask qwenmagi to use the `local` Formation:

```text
MAGI-M: pi qwen38-local/Qwen3.8-27B-abliterated-MLX-4bit low
MAGI-B: primary
MAGI-C: codex gpt-6-astra high
```

Use model IDs and Thinking levels supported by your provider. Omitted seats use the calling AI. Without a selected MAGI Formation, MAGI inherits VDGG's council seats, or uses primary for all seats if none are assigned. Standalone use does not require VDGG.

See the [Formation guide](skills/qwenmagi/references/formations.md) for selection precedence, CLI execution, pi configuration, and failure handling. The helper requires Python 3.

## Usage

Trigger it with any of:

- Type `/qwenmagi`
- Ask for it by name, e.g. "qwenmagiで練って" / "qwenmagiで審議して"
- Name "qwenmagi" explicitly

Once it receives a prompt, MAGI **runs autonomously without inserting questions**. It shows the deliberation log (each round's scores and critiques) but never stops to ask "what should I do?" — it runs straight through to a verdict.

## Working with VDGG

When used alongside [VibesDeGoGo! for Claude Code](https://github.com/tmknzz/VibesDeGoGo-for-Claude-Code) or [VibesDeGoGo! for Codex](https://github.com/tmknzz/VibesDeGoGo-for-Codex), explicitly select `qwenmagi` for these two roles. The default MAGI integration continues to use the original `magi`:

- **(a) Step 0 requirements council** — the three personas pressure-test the requirements draft and hand the user the material to decide on.
- **(b) Review gate for subjective artifacts** — passing or rejecting copy, docs, design, naming, and the like.

The trigger conditions live in the user's global instructions (CLAUDE.md for Claude Code, AGENTS.md for Codex). Only a passing review gate is recorded through the calling host’s current VDGG review mechanism (`vdgg_review_run` on Codex).

In these cases it runs a trimmed-down **lightweight deliberation** (3 rounds by default; extended to at most 5 only when convergence is clear). MAGI is the **guardian of desirability, not the guardian of correctness** — it does not deliberate on whether code is correct (does it run, is it bug-free). That belongs to tests and external code review.

## Homage & non-affiliation

MAGI is an independent **fan homage** to *Neon Genesis Evangelion*. The MAGI supercomputer and the names MELCHIOR, BALTHASAR, and CASPER originate from that work and belong to their respective rights holders (Khara, Inc. / GAINAX). This project is **not affiliated with, endorsed by, or sponsored by** Khara or GAINAX, and claims no rights to those names or concepts — they are used purely in tribute. The MIT License below covers only this repository's own original code and text, not the referenced trademarks or characters.

## License

MIT License. Copyright (c) 2026 tmknzz. See [LICENSE](LICENSE) for details.
