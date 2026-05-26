# claude-skills

Personal collection of [Claude Code](https://docs.claude.com/en/docs/claude-code) skills by [@PengyuChen01](https://github.com/PengyuChen01).

## Skills

| Skill | Trigger | What it does |
|---|---|---|
| [`dense`](./dense) | `/dense`, "be concise", "压缩输出" | Switch output into high-density mode — TL;DR first, tables over prose, stop when done. |
| [`tldr`](./tldr) | `/tldr`, "summarize this", "一句话总结" | Produce a three-tier summary (1-line gist + 3-5 bullets + optional paragraph) of any long input. |

## Install

Clone into your local Claude Code skills directory:

```bash
# Pick one skill:
git clone https://github.com/PengyuChen01/claude-skills.git
cp -r claude-skills/dense ~/.claude/skills/

# Or symlink the whole repo if you want to track updates:
ln -s "$(pwd)/claude-skills/dense" ~/.claude/skills/dense
```

On Windows (PowerShell):

```powershell
git clone https://github.com/PengyuChen01/claude-skills.git
Copy-Item -Recurse claude-skills\dense $env:USERPROFILE\.claude\skills\
```

Restart Claude Code (or start a new conversation) and the skill becomes invokable.

## License

MIT
