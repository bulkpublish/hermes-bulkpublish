# Changelog

## 1.0.2 — 2026-11-24

- `schedule-post` and `using-bulkpublish`: a thread part can carry a poll (X and Threads on any part, Mastodon on the first), and in a non-thread post `platformThreadParts` plus a `"thread"` post type sends one network a thread while the others post normally.

## 1.0.1 — 2026-11-23

- `platform-reference`: an Instagram `feed_video` publishes as a Reel, so `trialReel` (and show-in-feed) now take effect on it too instead of being ignored.

## 1.0.0 — 2026-11-06

- First release of the BulkPublish plugin for Hermes Agent: the hosted BulkPublish MCP server (`https://mcp.bulkpublish.com/mcp/full`, 80 tools) authenticated with `BULKPUBLISH_API_KEY`, marked `untrusted` so Hermes asks before every write, plus 33 BulkPublish skills.
