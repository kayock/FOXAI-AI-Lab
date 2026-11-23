# Changelog

## [v1.3.1] — 2026-11-23

Stable streaming-timeout and diagnostics-bridge release.

### Added

- Separate timeout tiers: connection, first-token, stream inactivity, optional total
- Direct Chat streaming UI with cancel, partial-response preservation, timeout kind labels
- `/api/settings/timeouts` and health payload timeout defaults
- Per-request `timeouts` overrides and env-based defaults

### Fixed

- Direct Chat no longer uses a hard non-stream 90s wall-clock timeout for generation
- Slow active streams are distinguished from stalled inactivity timeouts
- Timeout category recorded in Fleet Monitor diagnostics (`timeout_kind`)
- Partial text retained when a stream times out or is cancelled

### Verified

- DeepSeek Q8 extended stream: 895 tokens, 255.6s, 3.53 tok/s, normal completion
- Diagnostic record `0baa669fdd5b` retrieved in Fleet Monitor

## [v1.3.0] — 2026-11-23

- Removed hard-coded Direct Chat `max_tokens: 96` and Compare `max_tokens: 64`
- Model-default vs custom Maximum Output Tokens controls
- Metadata-only diagnostics emit to Fleet Monitor ingest
