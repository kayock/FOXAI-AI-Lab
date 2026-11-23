# FOXAI AI Lab Command Center

Station-local companion for **Direct Chat**, **Compare Models**, and QR tooling
against discovered LAN inference endpoints (Ollama, llama-server, and
OpenAI-compatible servers).

AI Lab is **not** Fleet Monitor. It discovers servers via Fleet Monitor metrics,
verifies LAN reachability, and never auto-loads or unloads models.

| Item | Value |
| --- | --- |
| Version | **1.3.1** |
| Default URL | `http://127.0.0.1:3010/` |
| Companion | [Kayock Fleet Monitor](https://github.com/kayock/Kayock-Fleet-Monitor) (diagnostics ingest) |

## Features (v1.3.1)

- **Streaming Direct Chat** — Ollama NDJSON / OpenAI SSE with live partial text
- **Timeout tiers** — separate connect, first-token, stream-inactivity, and optional total budgets (no silent fixed 90s kill on active streams)
- **Configurable timeouts** — UI fields + env (`AI_LAB_*_TIMEOUT`) + per-request `timeouts`
- **Maximum output tokens** — model/client default or custom (no hard-coded 96/64 caps)
- **Diagnostics bridge** — metadata-only posts to Fleet Monitor; deep links to `#mcc?diag=<id>`
- **Cancel** — AbortController terminates the stream and preserves partial replies
- **Compare Models** — independent non-stream requests with configurable non-stream total

## Install (Ubuntu Station)

```bash
git clone https://github.com/kayock/FOXAI-AI-Lab.git ~/Applications/FoxaiAiLab
cd ~/Applications/FoxaiAiLab
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
chmod +x start.sh
```

User systemd unit example: [`deploy/foxai-ai-lab.service`](deploy/foxai-ai-lab.service).

```bash
systemctl --user enable --now foxai-ai-lab.service
curl -sS http://127.0.0.1:3010/api/health
```

## Timeout configuration

| Setting | Default | Meaning |
| --- | --- | --- |
| `AI_LAB_CONNECT_TIMEOUT` | 10s | TCP/HTTP connect |
| `AI_LAB_FIRST_TOKEN_TIMEOUT` | 90s | Time before first stream chunk |
| `AI_LAB_READ_INACTIVITY_TIMEOUT` | 120s | Max idle between stream chunks |
| `AI_LAB_TOTAL_TIMEOUT` | none | Optional overall streaming wall-clock |
| `AI_LAB_NONSTREAM_TOTAL_TIMEOUT` | 90s | Non-stream / compare total |

Streaming Direct Chat uses inactivity between chunks so a slow but actively
generating model (for example DeepSeek Q8) is not terminated solely because
wall-clock time exceeds 90 seconds.

## Safety

- Does not start, stop, load, unload, or reconfigure inference servers
- Does not store prompts or responses in Fleet Monitor diagnostics
- Prefers loaded models only; unloaded selections return an error

## License

See repository license / project conventions shared with Kayock Fleet Monitor.
