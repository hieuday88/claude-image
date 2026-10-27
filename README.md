# 🎨 claude-image

A **Claude Code** skill that generates images by delegating to the **Antigravity CLI (`agy`)** —
rendered with Google's **Gemini** image model, using the account already logged in on your
machine. No paid API key required.

```
You ──► Claude Code ──► /claude-image "prompt" ──► agy -p "..." ──► image file ──► You
```

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed.
- **Antigravity CLI (`agy`)** installed and authenticated (open Antigravity IDE, sign in with your Google/Gemini account).
- Quick check:
  ```bash
  agy --version
  cat ~/.gemini/google_accounts.json   # shows the active account
  ```

## Install

### Option 1 — skills.sh CLI (recommended)

```bash
npx skills add hieuday88/claude-image
```

### Option 2 — git clone

```bash
git clone https://github.com/hieuday88/claude-image.git ~/.claude/skills/claude-image
```

### Option 3 — manual

Drop the whole folder into `~/.claude/skills/claude-image/`.

Claude Code auto-discovers skills in `~/.claude/skills/` on startup — restart your session
after installing.

### `agy` binary path

The skill uses `agy` from PATH. If it is not on PATH, locate it and call the full path:

```bash
command -v agy || ls ~/AppData/Local/agy/bin/agy.exe /usr/local/bin/agy ~/.local/bin/agy 2>/dev/null
```

## Usage

In Claude Code, just ask for an image in plain language — the skill activates automatically:

```
> generate an image of an orange cat in an astronaut suit, digital art
> draw a minimalist logo for a coffee shop called "May"
> make an event poster for a game night with the text "GAME NIGHT"
```

Or call it directly: `/claude-image <prompt>`.

Claude runs:

```bash
agy -p "Generate an image: <prompt>. Save it to a file named <name>.png in the current directory..." \
    --dangerously-skip-permissions --print-timeout 3m0s 2>&1
```

then reports the file path and sends the image back to you.

## Notes

- Files end in `.png` but the data is usually JPEG — they open normally.
- Each image consumes that **Gemini account's quota**; there is no local counter. On a *quota/rate-limit* error, wait for the reset (a few hours up to a day).
- `--dangerously-skip-permissions` lets `agy` write the file; only safe because the sole action is creating an image.
- **Quality:** Gemini is strong at complex prompts and text inside images. For photorealism or fine control, consider ComfyUI + SDXL running locally (RTX 5060, 8GB).

## Credits

- Delegation idea adapted from [SafeMantella/claude-code-agy-CLI-skill](https://github.com/SafeMantella/claude-code-agy-CLI-skill) — a general skill that delegates many task types to `agy`. `claude-image` trims it down to a single job: image generation.

## License

MIT.
