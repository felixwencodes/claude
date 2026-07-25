# FelixHumanizerPro

My personal writing editor, packaged as an agent skill. It reads a piece of text, finds the habits that give away a language model, and rewrites them out so the result reads like a person actually typed it. The pattern list underneath is built on Wikipedia's "Signs of AI writing" guide, but I've bolted my own non-negotiables on top: no em or en dashes anywhere, tone that matches where the text is going, and a preference for a rougher draft over a shiny one. It's a single Markdown file, so it drops into any tool that understands skill instructions.

## Why I bothered

Most "humanizer" tools stop at deleting a few buzzwords and call it done. That is not enough. A rewrite can be free of "delve" and still smell like a bot because the rhythm is too even, every sentence lands like a quote, and there's a tidy summary at the end that no real person would write. So this skill does two things the plain approach skips. It runs a second pass that asks "what still reads as AI here" and fixes what the first draft missed, and it holds a few hard rules that never bend, the dash ban being the strictest. The point is not to pass a detector (nothing reliably does that). The point is that a human reader stops noticing the machine.

## Install

### Through the skills CLI

Global install, available in every project:

```bash
npx skills add felixwen/humanizer --global
```

Update it later:

```bash
npx skills update felixhumanizerpro --global
```

Push it into every agent harness you have configured:

```bash
npx skills add felixwen/humanizer --global --agent '*'
```

Or one specific harness:

```bash
npx skills add felixwen/humanizer --global --agent <agent-name>
```

Drop `--global` if you'd rather keep it local to one project and commit it alongside your code. Reload skills or start a fresh session afterward.

### As a Claude Code plugin

```
/plugin marketplace add felixwen/humanizer
/plugin install felixhumanizerpro@felixhumanizerpro
```

After that it answers to `/felixhumanizerpro:felixhumanizerpro`.

### Inside the Claude apps

To run it in claude.ai or Claude Desktop instead of a CLI:

1. Zip the skill folder (the archive needs `SKILL.md` at its root) and rename it with a `.skill` extension, or just have `SKILL.md` ready on its own.
2. Open **Settings → Capabilities → Skills** (Team and Enterprise plans may keep this under **Admin settings**), pick **Add / Upload skill**, and select the file.
3. Open a new chat and ask it to humanize something, or call it by name. Claude loads it on its own.

Whether this option shows up depends on your plan, and on Team or Enterprise, on whether an admin has turned skills on. Menu names drift over time, so if the path above is off, hunt for "Skills" or "Capabilities" in settings.

### By hand

The only thing that matters at runtime is `SKILL.md`, so any harness can use it directly. Clone it where your tool keeps skills:

```bash
git clone https://github.com/felixwen/humanizer.git /path/to/your/skills/felixhumanizerpro
```

Or copy the one file into an existing skill folder:

```bash
mkdir -p /path/to/your/skills/felixhumanizerpro
cp SKILL.md /path/to/your/skills/felixhumanizerpro/
```

## Running it

Call it the way your harness exposes skills, either a slash command or a plain request:

```
/felixhumanizerpro

[your text]
```

```
Please humanize this: [your text]
```

Point it at a file and it edits the prose in place, leaving code, frontmatter, and links alone:

```
Humanize the writing in docs/launch-post.md
```

### Teaching it your voice

Hand it a sample of your own writing and it copies your habits instead of producing generic clean prose:

```
/felixhumanizerpro

Here are a couple paragraphs I wrote, match this voice:
[2-3 of your own paragraphs]

Now rewrite this:
[the AI text]
```

It reads your sentence lengths, your word picks, and your quirks, then writes toward those. The one thing a sample can't override is the dash rule; that stays off no matter what your sample does.

## What it hunts for

The catalog splits into five buckets. Every row is a habit the skill flags and the direction it rewrites toward. The "before" snippets are made up to show the shape of the problem, not lifted from anything real.

### Padding the meaning

| Habit | Looks like | Becomes |
|-------|-----------|---------|
| Inflated importance | "This update marks a defining moment in the product's journey." | "This update adds dark mode." |
| Notability stuffing | "Recognized across the industry with a strong and growing following." | Name the one real mention, or drop it. |
| Fake-depth -ing tails | "The endpoint returns JSON, empowering teams and driving adoption." | "The endpoint returns JSON." |
| Brochure voice | "a vibrant space nestled in the heart of the district" | "an office downtown" |
| Borrowed authority | "Many experts agree this is the right call." | Say who, or own the claim yourself. |
| Challenges-then-hope arc | "Despite the hurdles, the team continues to thrive." | Keep the actual problem, cut the pep talk. |

### Word and sentence habits

| Habit | Looks like | Becomes |
|-------|-----------|---------|
| Model vocabulary | "We leveraged a robust setup to delve into the landscape." | "We used the setup to study the data." |
| Dodging "is" | "The tool serves as a bridge between the two teams." | "The tool connects the two teams." |
| Fake contrast | "This isn't just a script, it's a system." | "This script schedules the backups." |
| Forced groups of three | "faster, cleaner, and smarter" | "faster and easier to read" |
| Synonym shuffling | "the script... the utility... the helper... the tool" | Pick one word and repeat it. |
| Meaningless range | "everything from setup to scaling" | "setup and scaling" |
| Missing subject | "No install needed." | "You don't have to install anything." |

### Formatting tics

| Habit | Looks like | Becomes |
|-------|-----------|---------|
| Bold sprinkled around | "It uses **OKRs**, **KPIs**, and a **scorecard**." | "It uses OKRs, KPIs, and a scorecard." |
| Bold-label bullet lists | "**Speed:** it got faster." | Written as a sentence. |
| Title Case Headings | "## Getting Started With The Tool" | "## Getting started with the tool" |
| Decorative emoji | "🚀 Launch, 💡 Insight, ✅ Done" | The words, no icons. |
| Curly quotes | `“the project”` (from a chatbot paste) | `"the project"` |
| Hyphen everything | "the report is high-quality and data-driven" | "the report is high quality and data driven" |

### Chatbot residue

| Habit | Looks like | Becomes |
|-------|-----------|---------|
| Assistant chatter | "Here's a summary! Hope this helps, let me know." | Gone. Start at the content. |
| Cutoff hedging | "As of my last update, details are limited." | State what's known, or cut it. |
| Flattery | "Great question! You're absolutely right." | Skip it and answer. |

### Filler and hedging

| Habit | Looks like | Becomes |
|-------|-----------|---------|
| Wordy phrases | "in order to", "due to the fact that" | "to", "because" |
| Stacked hedges | "could potentially possibly help" | "may help" |
| Upbeat wrap-ups | "The future is bright." | The last real fact, then stop. |

### The ones I added

These aren't in the base guide. I put them in because they're the tells I personally trip over most.

| Habit | Looks like | Becomes |
|-------|-----------|---------|
| Fake-profound framing | "At its core, what really matters is speed." | "Speed is the main thing here." |
| Announcing the obvious | "Let's dive in. Here's what you need to know." | Just say the thing. |
| Empty line under a header | "## Performance" then "Speed matters." | Let the header stand and get to the point. |
| Narrating the diff | "This function was added to replace the old loop." | Describe what it does now. |
| Manufactured drama | "No prior. No bias. No nostalgia." | One clear sentence instead of a stack of fragments. |
| Aphorism mode | "Symmetry is the language of trust." | The plain claim it's dressing up. |
| Fake-candid opener | "Honestly? It depends." | "It depends on how often you use it." |

## The one rule that never bends: no dashes

Every other rule can flex to match your voice. This one can't. The final text carries zero em dashes, zero en dashes, and none of the spaced or double-hyphen stand-ins people reach for in their place. To most readers a stray long dash is the single loudest AI tell, and I don't want one in anything with my name on it. Wherever a draft uses a dash to punctuate, the skill swaps it for a period, a comma, a colon, or parentheses, whichever fits. An aside that a model would bracket with dashes gets commas instead: "It's promoted by institutions, not the people, and it sticks." A dash used to bolt a punchline onto a sentence becomes a full stop: "One node runs the whole thing. Wild." Real hyphens inside compound words (self-hosted, human-in-the-loop) are safe, since this rule is only about dashes doing a comma or period's job. Before it hands anything back, the skill scans for all four of those dash shapes and treats any hit as an unfinished draft.

## A full pass

*(The rewrite keeps every claim from the before and just strips the tells. The invented specifics here, the month and the error code, stand in for details a real author would supply. In an actual session the skill asks for those rather than inventing them.)*

**Before (AI-sounding):**

> I recently invested in a robot vacuum, and let me tell you, this little machine has completely transformed my home. From the very first run, I knew my cleaning routine would never be the same.
>
> Nestled quietly in the corner of my living room, this smart device stands as a testament to how far home automation has come, seamlessly blending cutting-edge technology with everyday convenience. Yes, the initial setup was a touch tricky (the app took some getting used to!) but every scheduled clean rewards you with spotless floors that make it all worthwhile.
>
> No modern home would be complete without embracing smart cleaning. And the results? Simply flawless. Watching it glide effortlessly across the room, navigating around furniture with ease, was a moment of pure satisfaction I won't soon forget.
>
> But what truly sets it apart isn't just the convenience, it's the peace of mind. Whether you're at the office or relaxing on the couch, you can trust it to keep your space pristine, giving you back precious time for the things that matter most.
>
> Would I recommend it? Absolutely. This isn't just a gadget, it's an investment in a cleaner, smarter, more effortless lifestyle. If you're dreaming of reclaiming your weekends, this is one purchase that promises to change your home forever. ✨

**After (humanized):**

> I bought a robot vacuum in March and my honest verdict is that it's great about eighty percent of the time and mildly infuriating the rest.
>
> The good part is real. It runs while I'm at work and the floors are cleaner than when the job depended on me remembering to do it every couple of weeks, which was the actual baseline. Dog hair especially. It gets the stuff along the baseboards I always skipped.
>
> Setup was more of a hassle than the box lets on. The app made me create an account and update the firmware before it would move an inch, and then it spent its first run bumping the same chair leg like it had a grudge. It mapped the place on the second try and has mostly behaved since.
>
> Where it loses me is the edge cases. It swallows phone cables if I leave them on the floor. It wedged itself under the couch once and just sat there announcing "error 4" to an empty apartment until I got home. And it has its own upkeep, emptying the bin, pulling hair off the roller, rinsing the filter, so "set it and forget it" is a stretch.
>
> I still run it almost daily. Would I buy it again? Probably, but going in I'd know it's a tool that buys me time, not a robot butler. It doesn't replace cleaning so much as change what the word means.

## A quick detector check

I ran a fresh paragraph written to these rules through QuillBot's AI detector. It came back 100% human-written, 0% AI.

![QuillBot AI detector showing 0% AI generated and 100% human-written](screenshots/Screenshot%202026-07-25%20173438.png)

One detector on one sample isn't proof of anything. Detectors disagree with each other and none of them are reliable, so a clean score here is a sanity check, not a guarantee. The goal was never to beat a detector anyway. It's that a person reading the text stops noticing the machine.

## How the rewrite actually works

Three steps, every time. It reads the text and marks each pattern it finds. It writes a first draft aimed at plain verbs, varied sentence length, and the right register for where the text is headed. Then it stops and asks itself two questions: what here still reads as machine-written, and did I slip in any fact, name, or number that wasn't in the source. It fixes both and does a final dash sweep before handing anything over. If you tell it the result still sounds like a bot, it believes you and cuts harder rather than defending the draft.

## Voice by platform

The same text gets a different treatment depending on where it lands. A Reddit comment stays short and casual with bare links and no headers. An email is a greeting, the point, a short close, and none of the "I hope this finds you well." A README keeps its structure but drops the puffery. A GitHub issue reads like a telegram: what broke, why it matters, how to reproduce. LinkedIn gets first person and one idea per post, no motivational three-act build.

## Credit

The base pattern set comes from [Wikipedia's "Signs of AI writing"](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing), kept up by WikiProject AI Cleanup, which distilled it from thousands of real examples of machine text. Their one-line summary is the whole problem in a sentence:

> "LLMs use statistical algorithms to guess what should come next. The result tends toward the most statistically likely result that applies to the widest variety of cases."

The hard rules layered on top, the dash ban, the platform register, the bias toward rougher drafts, and the extra patterns in the last table, are mine.

## Version

3.0.0. Full Wikipedia-based pattern set, my own hard rules on top, the no-fabrication guard, and the two-pass audit-then-rewrite loop.

## License

MIT.
