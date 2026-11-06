<p align="center">
  <a href="https://www.bulkpublish.com"><img src="assets/og-default.jpg" alt="BulkPublish" width="720"></a>
</p>

# BulkPublish for Hermes Agent

**Homepage:** [https://www.bulkpublish.com](https://www.bulkpublish.com)

The official [BulkPublish](https://www.bulkpublish.com) plugin for [Hermes Agent](https://github.com/NousResearch/hermes-agent). Draft, schedule and publish social media posts across 15 networks, manage your media library, channels and approval queue, and read engagement analytics, all by asking Hermes in plain language.

The plugin is a portable Agent Plugins v1 package (`plugin.json` + `mcp.json` + `skills/`). It contains configuration and skills only: no code runs on your machine. Hermes connects to BulkPublish's hosted MCP server at `https://mcp.bulkpublish.com/mcp/full`.

## What you get

- **80 MCP tools** from the hosted BulkPublish server: posts, scheduling and the queue, bulk actions, stories, media uploads (including large files in parts), channels and channel health, analytics and per-post metrics, labels, templates, hashtag groups, calendar notes, repeating schedules, RSS autopost, team approvals, client review and connect links, the Inbox and post comments, and plan quota.
- **33 skills** that teach Hermes how to use them well: platform rules and character limits for every network, safe scheduling, bulk publishing, fixing failed posts, approvals, recurring posts, analytics reports, content planning and repurposing. See [`skills/README.md`](skills/README.md).
- **Approval prompts for anything that changes your account.** The plugin marks the server `untrusted`, so Hermes asks you before every tool call that the server does not annotate as read-only (creating, publishing, deleting, replying). Read-only tools such as listing channels or reading analytics run without a prompt.

**Supported networks:** Facebook, Instagram, X/Twitter, TikTok, YouTube, Threads, Bluesky, Pinterest, Google Business Profile, LinkedIn, Mastodon, Discord, Telegram, Tumblr and Snapchat.

## Requirements

- Hermes Agent 0.21.6 or newer.
- A BulkPublish account ([sign up free](https://www.bulkpublish.com)) with at least one connected social account.
- A BulkPublish API key, created at [app.bulkpublish.com/developer](https://app.bulkpublish.com/developer). Keys start with `bp_`.

## Install

1. Add your API key to your Hermes profile's `.env` file (for the default profile, `~/.hermes/.env`):

   ```bash
   echo 'BULKPUBLISH_API_KEY=bp_your_key_here' >> ~/.hermes/.env
   ```

2. Install and enable the plugin:

   ```bash
   hermes plugins install bulkpublish          # from the Hermes plugin catalog
   # or straight from this repository:
   # hermes plugins install bulkpublish/hermes-bulkpublish
   hermes plugins enable bulkpublish
   ```

3. Check the plugin is enabled, then start a new Hermes session:

   ```bash
   hermes plugins list
   ```

   Ask Hermes something like *"Which social accounts do I have connected in BulkPublish?"* It answers with the `mcp__bulkpublish__list_channels` tool.

Hermes reads `BULKPUBLISH_API_KEY` from the active profile and sends it to `mcp.bulkpublish.com` as an `Authorization: Bearer` header. The key never goes in `mcp.json` or anywhere in this repository. If the variable is not set, Hermes does not connect the server and tells you which variable is missing.

## Example requests

- "Draft a LinkedIn and X post announcing our new pricing page, and show me both before scheduling."
- "Schedule this video to TikTok, Instagram Reels and YouTube Shorts for Friday at 10am."
- "Why did yesterday's Instagram post fail? Retry it if it's safe."
- "Which of last month's posts got the most engagement on each network?"
- "Put every post scheduled for next week into the approval queue."
- "How much of my plan's monthly post quota have I used?"

Hermes shows drafts before anything is scheduled and never publishes unless you ask. Every write still goes through Hermes's approval prompt.

## What this plugin does on your machine and on the network

So you know before installing:

- **Network:** every tool call, with its arguments, goes to BulkPublish's hosted server at `mcp.bulkpublish.com`, authenticated with your API key, and acts on your BulkPublish workspace with the permissions of that key.
- **Public effects:** publishing tools send posts to your connected social accounts, and Inbox and comment tools send replies to real people. Neither can be undone from Hermes. These tools are not marked read-only, so Hermes asks before each one, and refuses them in unattended runs (cron, gateway) unless you trust the server yourself.
- **Credentials:** the API key is read from your Hermes profile's environment. The plugin stores nothing else and has no local code, background processes, shell commands or telemetry.
- **Tool set:** the tools are served by the remote server at runtime, so they can change without a new version of this repository. The skills and manifests here are what the pinned catalog commit covers.

## Troubleshooting

| Symptom | Fix |
|---|---|
| Hermes has no BulkPublish tools | Check `hermes plugins list` shows it enabled, and that `BULKPUBLISH_API_KEY` is set in the active profile's `.env`. Start a new session afterwards. |
| Tools answer `Unauthorized` | The key is wrong, revoked or has stray whitespace. Create a new one at [app.bulkpublish.com/developer](https://app.bulkpublish.com/developer). |
| `403 FEATURE_DISABLED` | That feature is not on your plan. Ask Hermes to check your quota, or see [pricing](https://www.bulkpublish.com/pricing/). |
| `403 APPROVAL_REQUIRED` | Your key's role cannot publish directly. The post waits for an owner, admin or approver. |
| You want writes without a prompt | Add a `bulkpublish` entry to `mcp_servers` in your Hermes `config.yaml` with the same `url` and `Authorization` header plus `trust: full`. A same-name entry in `config.yaml` replaces the plugin's entry entirely. |

## Other ways to use BulkPublish

- [BulkPublish AI Toolkit](https://github.com/bulkpublish/bulkpublish-ai-toolkit): plugins and skills for other AI tools, and the local `@bulkpublish/mcp-server` package.
- [BulkPublish API](https://www.bulkpublish.com): REST API for your own integrations.

## Repository layout

| Path | What it is |
|---|---|
| `plugin.json` | Agent Plugins v1 manifest, with the Hermes `trust: untrusted` extension for the server |
| `mcp.json` | The hosted BulkPublish MCP server (Streamable HTTP) |
| `skills/` | 33 Agent Skills, loaded by Hermes as `bulkpublish:<skill>` |
| `assets/` | Official BulkPublish logo (`logo.svg`) and the same image used on https://www.bulkpublish.com/social-media-api/ (`og-default.jpg`) |

## Support

- Help and account questions: [support@bulkpublish.com](mailto:support@bulkpublish.com)
- Partnerships and the plugin itself: [azeem@bulkpublish.com](mailto:azeem@bulkpublish.com)
- Bugs in this plugin: [open an issue](https://github.com/bulkpublish/hermes-bulkpublish/issues)

## License

[MIT](LICENSE) © BulkPublish
