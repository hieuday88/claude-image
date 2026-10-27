---
name: claude-image
description: Generate images from a text prompt by delegating to the Antigravity CLI (agy), which calls Google's Gemini image model. Use whenever the user asks to create, generate, draw, or make an image, picture, illustration, logo, poster, icon, wallpaper, or artwork — including Vietnamese requests like "tạo ảnh", "vẽ ảnh", "tạo hình", "làm logo/poster". Output is saved as an image file and its path is reported.
allowed-tools:
  - Bash
  - Read
---

# claude-image — Generate images via Antigravity CLI (agy)

Delegate image generation to `agy` (Antigravity CLI), which renders with Google's Gemini
image model using the account already logged in on this machine. No paid API key required.

## Binary

- Use `agy` from PATH. If it does not resolve, locate it first:
  ```bash
  command -v agy || ls ~/AppData/Local/agy/bin/agy.exe /usr/local/bin/agy ~/.local/bin/agy 2>/dev/null
  ```
  then call that full path.
- Active account: whatever is set in `~/.gemini/google_accounts.json`.

## Command

Run this single call, substituting the user's description and an output filename:

```bash
agy -p "Generate an image: <DESCRIPTION>. Save it to a file named <OUTFILE> in the current directory. When done, report the file path." --dangerously-skip-permissions --print-timeout 3m0s 2>&1
```

- `<DESCRIPTION>`: the user's prompt, enriched with style/lighting/composition details when the request is vague.
- `<OUTFILE>`: a short kebab-case name ending in `.png` (the data is usually JPEG despite the extension; that is fine).
- Run from the directory where the image should land (default: current working directory).

## Steps

1. Build the prompt. If the user gave only a short idea, expand it with concrete visual details (subject, style, lighting, composition) before sending.
2. Run the command above. It blocks until the image is ready (up to 3 minutes).
3. Verify the file exists (`ls -la <OUTFILE>`). The agent reports the path in its output.
4. Send the file to the user and state the absolute path. On the desktop app, surface it with the file-send tool.

## Notes

- `--dangerously-skip-permissions` lets `agy` write the file without prompting. Safe here because the task only creates an image; do not reuse this flag for tasks that touch important files.
- Each generation consumes the Gemini account's quota. There is no local counter; if `agy` reports a quota/rate-limit error, wait for the reset.
- For multiple variations, run the command several times with different output filenames.
- If `agy` is missing or not authenticated, tell the user to open Antigravity and sign in, rather than falling back to another provider.
