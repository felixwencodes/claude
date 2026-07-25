# humanizer

Remove signs of AI-generated writing from text, making it sound more natural and human. Based on Wikipedia's "Signs of AI writing" guide, maintained by WikiProject AI Cleanup.

## What it does

This is a portable agent skill (`SKILL.md`) for use with Claude Code, Cowork, and other agent harnesses that support Markdown-based skills. It detects and fixes patterns commonly seen in AI-generated text, including:

- Inflated significance and legacy claims
- Promotional, advertisement-like language
- Vague attributions and weasel words
- Overused "AI vocabulary" (crucial, delve, tapestry, landscape, etc.)
- Copula avoidance ("serves as" instead of "is")
- Em dash and en dash overuse
- Rule-of-three overuse
- Passive voice and subjectless fragments
- Filler phrases, hedging, and generic conclusions
- Chatbot artifacts ("I hope this helps!", "Let me know if...")

33 patterns total, each with a before/after example.

## Installation

### As a standalone skill

Drop `SKILL.md` into your agent's skills directory (for Claude Code: `~/.claude/skills/humanizer/SKILL.md`, or your project's `.claude/skills/`).

### As a Claude Code plugin

```
/plugin marketplace add felixwen/humanizer
/plugin install humanizer
```

## Usage

Invoke the skill directly, or ask your agent to "humanize this text" / "make this sound less like AI." You can also point it at a file to rewrite in place, or provide a writing sample of your own so it matches your voice instead of a generic "natural" tone.

## License

MIT
