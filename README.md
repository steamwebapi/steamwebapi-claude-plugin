# SteamWebAPI for Claude

An integration skill and the official remote SteamWebAPI MCP connection, bundled
as a Claude plugin. Discover endpoints, understand item and price data, and build
inventory, marketplace and CS2 release-discovery integrations.

## Install

In Claude clients that support custom plugin uploads, open Plugins, choose the
upload option and select `steamwebapi-claude.zip`.

For Claude Code, extract the ZIP into a directory named `steamwebapi`, then run:

```sh
claude --plugin-dir /absolute/path/to/steamwebapi
```

Ask Claude to use the SteamWebAPI skill, or invoke `/steamwebapi:steamwebapi`.
Try: "Use SteamWebAPI to build a CS2 inventory valuation endpoint. Check the live
endpoint schema and explain how missing prices should be handled."

## API access

Endpoint discovery and documentation work without an API key. The bundled MCP
connection contains no credentials. Authenticated API requests require your own
SteamWebAPI key and the appropriate endpoint access.

For authenticated MCP reads in Claude Code, configure a user-scoped connection
to the same server using Claude's MCP settings and an `X-Api-Key` header. A
user-scoped connection to the same endpoint takes precedence over the plugin
connection. Store the key in private client configuration or an environment
variable; do not add it to this distributable plugin or put it in chat.

For other Claude clients, use their supported connector authentication settings.
If the client cannot configure the header, use the plugin for documentation and
keep authenticated application calls in your own backend.

## Data and capabilities

The MCP executor permits only its supported read-only routes. Installing the
plugin does not grant account access or permission to trade. Live endpoint
documentation is authoritative; the bundled skill is a versioned guide, not a
promise that every described feature has reached the deployed API.

## Publishing

The extracted directory is self-contained and can be placed in a separate public
plugin repository. It contains no SteamWebAPI backend source. Validate it with
`claude plugin validate /absolute/path/to/steamwebapi` before submitting it to
the Claude plugin directory. Creating this package does not submit or publish it.

Documentation: https://www.steamwebapi.com/mcp

## Install from GitHub in Claude Code

```text
/plugin marketplace add steamwebapi/steamwebapi-claude-plugin
/plugin install steamwebapi@steamwebapi
```

The repository is the plugin package and its small marketplace catalog.
