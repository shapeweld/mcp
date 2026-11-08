# Shapeweld MCP server

A 3D converter your agent can call. [Shapeweld](https://shapeweld.com)'s free converters are
also a remote MCP server: point an agent at one URL and it can ask what a conversion keeps,
hand over a file, wait for it and give you the download link.

This repository holds the server's registry manifest (`server.json`) and this guide. The
server itself runs at shapeweld.com; there is nothing to install.

## The endpoint

```
https://shapeweld.com/mcp
```

Streamable HTTP, no sign-in, no key. The server is the same converter the pages use, with the
same limits and the same 3-day retention.

## Connecting

**Claude**

1. Open Settings → Connectors.
2. Choose Add custom connector.
3. Paste `https://shapeweld.com/mcp` and save.
4. Ask Claude to convert a file you attached or linked.

**ChatGPT**

ChatGPT lists MCP servers through its directory; Shapeweld is submitted and will appear there
once accepted. Until then the server works in any client that takes a URL.

**Cursor and other clients**

Cursor, Windsurf, VS Code and most agent frameworks take a remote MCP server by URL in their
MCP settings: add `https://shapeweld.com/mcp` as a `streamable-http` server. No command, no
environment variables.

```json
{
  "mcpServers": {
    "shapeweld": { "type": "streamable-http", "url": "https://shapeweld.com/mcp" }
  }
}
```

## The five tools

| Tool | What it does | Input |
|---|---|---|
| `list_tools` | Lists the conversions on offer with the accepted extensions and size caps. | none |
| `conversion_notes` | Says what a pair keeps and what it drops before you upload anything. | `from`, `to` |
| `convert_file` | Downloads your file from a link, or takes the file you attached in ChatGPT, and starts the conversion. | `to`, `file_url` or `file`, `from` for a zip, `options` |
| `job_status` | Queued, processing, ready, failed, or refused with the reason. | `jobId` |
| `result_link` | The download link, the file name, the expiry date and a viewer link. | `jobId` |

Formats: GLB, FBX, OBJ, Blender (.blend), STL, PLY, DAE, USDZ and 3MF, in the pairs listed by
`list_tools`; plus the mesh tools (`compress-glb`, `mesh-decimate`, `mesh-lod`, `mesh-repair`,
`mesh-unwrap`, `svg-to-3d`), addressed by their kind in `to`.

## Limits

Per session: 10 conversions an hour and 40 a day. Across all agents: 200 a day. Files follow
the page's cap: 100 MB for the GLB, FBX, OBJ and Blender pairs and the compressor, 50 MB for
the rest, 5 MB for an SVG. Only https links are fetched.

When a limit is reached the tool says so and gives the page URL, where the same conversion
runs in the browser.

## What we receive and keep

We receive the file the agent fetched for you and the agent's name (Claude, ChatGPT, Cursor or
other). We keep the upload and the result for 3 days, then delete both. We never receive your
conversation. Results are handed out through a signed link that works without a cookie for
those 3 days. Privacy: https://shapeweld.com/privacy

## Domain verification

When the server is submitted to the ChatGPT directory, the challenge file is served from the
site at `https://shapeweld.com/.well-known/openai-apps-challenge`; it is not part of this
repository.

## Questions

support@shapeweld.com · https://shapeweld.com/developers

MIT licensed.
