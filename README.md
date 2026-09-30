# JoyStream plugin for Claude Code and Codex

Build, ship and debug [JoyStream](https://joystream.ai) agents from your coding agent. The
plugin connects Claude Code or Codex to the JoyStream MCP server and adds skills that write
an agent spec, take it live with a pilot run first, and explain why a run failed.

## What you get

- **`joystream-agent-author`**: turns a request into a valid agent spec, using your
  connected services, catalog skills and connector actions.
- **`joystream-agent-ship`**: builds the agent, runs a pilot, and takes it live.
- **`joystream-agent-debug`**: finds the cause of a failed run and proposes a spec change,
  or says the cause is unclear.
- **The JoyStream MCP server** at `https://api.joystream.ai/mcp`, connected when you install
  the plugin.
- **Confirmation for risky actions.** Running an agent, taking it live, deploying,
  re-pinning a version and connector actions that change something do nothing until you
  approve the summary the server returns.

## Requirements

A JoyStream account. Sign up at [joystream.ai](https://joystream.ai).

## Install

### Claude Code

```
/plugin marketplace add joystream-ai/joystream-plugin
/plugin install joystream@joystream
```

Or from the terminal:

```
claude plugin marketplace add joystream-ai/joystream-plugin
claude plugin install joystream@joystream
```

### Codex

```
codex plugin marketplace add joystream-ai/joystream-plugin
```

Then run `codex`, type `/plugins`, open the **JoyStream** tab and install **joystream**. In
the ChatGPT desktop app, open the **Plugins** tab, find **joystream** and select the plus
button. Start a new session to use the plugin.

## Sign in

The first time the plugin uses the MCP server, your browser opens JoyStream's sign-in and
consent page. Sign in and choose **Approve**. Your coding agent then acts with your
JoyStream account's access, and nothing is stored in a config file.

- **Claude Code:** run `/mcp`, select **joystream** and choose **Authenticate**.
- **Codex:** approve the sign-in when Codex asks during install. If it doesn't, run
  `codex mcp login joystream`.

To revoke access, open **Settings → Connected apps** in JoyStream and select **Revoke**.

## Example prompts

- "Build a JoyStream agent that summarizes the latest release of acme/api on GitHub and
  posts it to #releases in Slack."
- "Why did my last run of release-digest fail?"
- "Ship release-digest: build it, run a pilot, and take it live if the pilot passes."
- "Add a weekday 9am schedule to release-digest."
- "Which of my agents failed this week, and why?"

## Use the MCP server without the plugin

Skills come only with the plugin, which supports Claude Code and Codex. Any MCP client can
connect to the server itself.

**Claude Code**

```
claude mcp add --transport http joystream https://api.joystream.ai/mcp
```

Then run `/mcp`, select **joystream** and authenticate.

**Codex:** add this to `~/.codex/config.toml`:

```toml
[mcp_servers.joystream]
url = "https://api.joystream.ai/mcp"
```

Then sign in:

```
codex mcp login joystream
```

**Claude Desktop and claude.ai:** open **Settings → Connectors → Add custom connector** and
enter `https://api.joystream.ai/mcp`.

**Other MCP clients:** add `https://api.joystream.ai/mcp` as a remote MCP server. In Grok,
for example, open [grok.com/connectors](https://grok.com/connectors), choose
**New Connector → Custom** and enter the URL.

## What the MCP server can do

| Area | Can | Needs your confirmation |
|---|---|---|
| Read | Workspaces, agents, versions, specs, runs, tickets, trigger events, credit balance, connectors and their actions | No |
| Build | Create and update agents, build them, stage them to `dry_run` or `pilot` | No |
| Run and go live | Run an agent, take it live, deploy, re-pin a version, run connector actions that change something | Yes |
| Can't | Manage members, sharing, vaults, billing or settings | |

It works only with what your JoyStream account can see and do. Viewers can read but not
change anything.

## Troubleshooting

**"Endpoint not found" or 404.** The server URL is wrong. It must be exactly
`https://api.joystream.ai/mcp`.

**Sign-in keeps coming back.** Sign in to JoyStream in the same browser first, then
authenticate again from your client. The consent page sends you to the JoyStream login when
you are not signed in; after you sign in, you return to it.

**"Needs authentication" or 401.** The client was never signed in, or its access was
revoked. Authenticate again: `/mcp` in Claude Code, `codex mcp login joystream` in Codex.

**Start over.** Revoke the app in **Settings → Connected apps**, then authenticate again
from your client.

## Links

- [JoyStream](https://joystream.ai)
- Support: [support@joystream.ai](mailto:support@joystream.ai)
- License: [MIT](./LICENSE)
