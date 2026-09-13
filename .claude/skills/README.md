# Installed skills

These skill directories are vendored (copied in, not submodules) from their
upstream repos so Claude Code can use them directly on this project. Each
still carries its own `SKILL.md`, license, and docs — treat upstream as the
source of truth and re-sync here when it changes.

## video-use

- Source: https://github.com/browser-use/video-use
- Vendored at commit: `9575612f066aa517354790a645fd90f9f95a743b`
- Conversation-driven video editing (transcribe, cut, color grade, subtitle,
  overlay animations) via ffmpeg + ElevenLabs Scribe. See
  `video-use/SKILL.md` for the workflow and `video-use/install.md` for the
  original setup contract.

**Runtime requirements** (not vendored, install per-environment):
- `ffmpeg` / `ffprobe` on `$PATH`
- Python deps from `video-use/pyproject.toml` (`uv sync` or `pip install -e .`
  run from inside `video-use/`)
- An `ELEVENLABS_API_KEY` in `video-use/.env` (copy `.env.example`) — required
  for transcription. Never commit this file; it's already git-ignored.
- Optional: `yt-dlp` for pulling source footage from URLs.

## hyperframes (+ its 19 sub-skills)

- Source: https://github.com/heygen-com/hyperframes
- Vendored at commit: `1acc65dbf813d5750c241c7e6e15b48329271ffb`
- HTML-to-video rendering framework. `hyperframes/SKILL.md` is the router —
  it reads the project state and pulls in the relevant sub-skill:
  `hyperframes-core`, `hyperframes-animation`, `hyperframes-audio`,
  `hyperframes-cli`, `hyperframes-creative`, `hyperframes-keyframes`,
  `hyperframes-registry`, `embedded-captions`, `faceless-explainer`,
  `figma`, `general-video`, `media-use`, `motion-graphics`,
  `music-to-video`, `pr-to-video`, `product-launch-video`,
  `remotion-to-hyperframes`, `slideshow`, `talking-head-recut`.

**Runtime requirements**:
- Node.js 22+ (present in this environment)
- `ffmpeg`
- `npx hyperframes ...` fetches the CLI on demand; no local install needed.

## Updating

Re-pull from upstream and copy the relevant directories back in, e.g.:

```bash
git clone --depth 1 https://github.com/browser-use/video-use /tmp/video-use
cp -a /tmp/video-use/. .claude/skills/video-use/ && rm -rf .claude/skills/video-use/.git

git clone --depth 1 https://github.com/heygen-com/hyperframes /tmp/hyperframes
for d in /tmp/hyperframes/skills/*/; do
  n=$(basename "$d"); cp -a "$d." ".claude/skills/$n/"
done
```

Then update the commit hashes above.
