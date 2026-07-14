# Unslop

A Claude skill that removes signs of AI writing from English text. Not just the word-level tells ("delve", "it's important to note") but the structural ones: over-explained conclusions, template paragraphs, invented specifics. Calibrates to your personal voice.

> GitHub About line:
> "English text humanizer for Claude: typography, vocabulary, structure. Calibrates to your voice. Built on the UMD / Google DeepMind study and Wikipedia's Signs of AI writing."

## Why word-swapping doesn't work

Every humanizer promises to make AI text "sound human", and most of them swap vocabulary: "delve" out, "explore" in. There are two problems with that.

First, the wordlist decays. "Delve" peaked in 2023 and collapsed in 2025. GPT-5.1 suppresses em dashes. Wikipedia's [Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) now dates its vocabulary lists by model era, because each generation retires the previous tells.

Second, and worse: the words were never the real signal. In 2026, the University of Maryland and Google DeepMind compared 61,608 texts written by humans and five models. When they ran AI texts through surface editing (clichés, purple prose, redundant exposition removed), a classifier looking only at structure barely noticed: detection dropped from 95.5% to 93.9%. The text gives itself away by how it's built. AI states the moral of every paragraph, runs single-track claim-support-takeaway structure, renders emotion through body metaphors, avoids naming real things, and rounds every ending to a tidy point.

A humanizer that only touches vocabulary fixes the layer that was already fixing itself, and leaves the durable signal intact. This skill works on three levels at once.

## How it's built

**Level 1: typography and mechanics.** Em dashes, mixed quotation marks, Title Case Headings, bold on every "key term", inline-header bullet lists, "Conclusion" sections, markdown debris like `utm_source=chatgpt.com`. Cheap fixes; partially grep-checkable, and the skill greps itself.

**Level 2: vocabulary and rhetoric.** 18 categories of constructions with stop lists and fixes: inflated significance ("stands as a testament"), negative parallelisms ("it's not X, it's Y"), the rule of three, superficial -ing analysis ("...highlighting the importance of"), copula avoidance ("serves as" for "is"), synonym cycling, vague attributions, and more. Every category ships with a before/after. The AI wordlist is dated by model era, because it expires.

**Level 3: structure and epistemics.** The part that survives model updates. Don't chew conclusions. Break paragraph symmetry. Use real specifics, and only real ones: the skill forbids inventing numbers, examples, and thresholds for "liveliness", because invented specifics are worse than clichés: a cliché reads as filler, an invented fact reads as fact. Hedge once per limitation, not once per sentence. And a section most humanizers skip: what human writing is allowed to do. Plain "is" and "has". Plain verbs: wrote, used, died. Superlatives when true. Hedges when honest. These are the constructions AI avoids and people use freely; the skill protects them instead of sanding them off.

There's also a rule against replacement tics: any substitute phrase repeated across three texts becomes a new marker. The cure for a cliché is usually not another phrase but its absence.

## Voice calibration

The default "human" tone is still someone else's. So the skill can become yours:

```
calibrate to my style
```

Claude asks for a few texts you wrote yourself, extracts a profile (em dash tolerance, pet connectives, sentence rhythm, the personal tics that must never be cleaned out, per-genre registers), and saves it to `references/style-profile.md`: plain markdown you can edit by hand. From then on, every edit lands on top of your voice, not on top of an averaged "good style".

Updates are incremental: "learn from this text too" appends to the profile instead of rewriting it.

What calibration will not do: enable invented facts, switch off the epistemics rules, or imitate another named author.

## Install

**Claude.ai / Claude Desktop:** Settings, Capabilities, Skills, upload `unslop.skill` (or the repo as a ZIP).

**Claude Code:**

```bash
git clone https://github.com/asavvin-pixel/unslop ~/.claude/skills/unslop
```

## Use

The skill triggers on "humanize this", "unslop", "de-AI", "make it sound human", and when writing English text from scratch. Typical requests:

```
unslop this: [...]
write a post about [...] that doesn't read as generated
rewrite this section, keep the length and structure
calibrate to my style
```

Three edit modes: free (filler gets deleted, text may shrink), careful (structure and 80–110% of length survive, for fixed formats), minimal. The genre always survives: a post stays a post, an email stays an email.

## Repo structure

```
unslop/
├── SKILL.md                              # the rules, three levels
└── references/
    ├── blacklist.md                      # 18 stop-list categories with fixes
    └── style-profile-template.md         # calibration profile template
```

## Sources

- Russell J. et al. StoryScope: Investigating idiosyncrasies in AI fiction. UMD / Google DeepMind, 2026. [arXiv:2604.03136](https://arxiv.org/abs/2604.03136)
- [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing)
- Format inspired by [blader/humanizer](https://github.com/blader/humanizer)

## What it won't do

It doesn't try to beat AI detectors, and that's deliberate: detectors err in both directions, and text written to fool a detector is bad in its own way. The goal is text you're not embarrassed to show a person. It also won't replace the author: if the source has no facts in it, the output is an honest short text plus a list of what's missing, not a convincing imitation.

## Russian version

For Russian text, use the sister skill [ochelovech](https://github.com/asavvin-pixel/ochelovech): Russian AI text has its own lexicon (bureaucratic calques, «является»-constructions, the letter «ё» as a tell), so wordlists don't translate. The structural level is shared; each skill adapts it to its language.
