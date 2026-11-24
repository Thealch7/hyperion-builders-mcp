# Install: Hyperion Builders MCP (HBMeasure)

This is a hosted (remote) MCP server. There is nothing to clone, build or run locally, and no API key, token or environment variable is needed. Do not clone this repository to install it; the only thing to do is register the remote URL.

- Transport: streamable HTTP
- URL: `https://measure.hyperion.builders/mcp`
- Authentication: none

## 1. Add the server

Add this entry to `cline_mcp_settings.json` (merge it into the existing `mcpServers` object; keep other servers as they are):

```json
{
  "mcpServers": {
    "hyperion-builders": {
      "type": "streamableHttp",
      "url": "https://measure.hyperion.builders/mcp",
      "disabled": false
    }
  }
}
```

No `command`, `args` or `env` fields are used for this server.

## 2. Check that it works

The server answers plain JSON-RPC over HTTP POST. Both calls below are free, read-only requests.

```bash
curl -s -X POST https://measure.hyperion.builders/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-03-26","capabilities":{},"clientInfo":{"name":"install-check","version":"1.0"}}}'
```

Expected: HTTP 200 and a JSON body whose `result.serverInfo.name` is `hyperion-builders`.

```bash
curl -s -X POST https://measure.hyperion.builders/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":2,"method":"tools/list","params":{}}'
```

Expected: a `result.tools` array with the 11 tools below.

## 3. Tools

Every tool is read-only (`readOnlyHint: true`). None takes payment, creates an order or needs a key.

| Tool | What it returns |
|---|---|
| `explain_hbmeasure` | What HBMeasure is for agents: purchase rules, prices, terms of sale, discovery URLs. Start here. |
| `get_agent_report_catalog` | Machine catalog for buying an HBMeasure report: price, checkout URL, terms of sale. Read-only; creates no checkout. |
| `get_x402_payment_requirements` | x402 payment requirements for the report, the coverage check and the building code lookup. Takes and forwards no payment. |
| `get_sample_report` | Metadata and download links for redacted sample reports. |
| `get_entity` | Stable facts about Hyperion Builders for citation: legal name, license, contact, hours, service area. |
| `explain_services` | Hyperion Builders services and the $200 Inspection & Assessment fee. |
| `explain_free_vs_paid` | Why the inspection is paid rather than free. |
| `explain_denied_claim` | What Hyperion does when a roof insurance claim is denied or partially paid (Greenville / Upstate SC). Not legal advice. |
| `explain_real_estate_insurance` | Roof age, insurance and home sales. |
| `list_slots` | Currently bookable Inspection & Assessment start times. |
| `create_booking_handoff` | Takes a `slotId` from `list_slots` and returns a signed booking URL. The customer completes payment on the website; the tool does not pay or book. |

Call `explain_hbmeasure` first. The rest of the HBMeasure flow (coverage check, report order, payment) happens over HTTP at the URLs those tools return, not through MCP tool calls.

## Prices and terms (as stated on https://hyperion.builders)

- Human-readable PDF report: $7.52
- Agent report: $1.34 by card, or $1.00 over x402 (USDC on Base)
- Coverage check: $0.05 over x402
- All sales are final; there are no refunds. Each figure in a report is graded Verified, Measured or Estimated.

Terms: https://hyperion.builders/terms. Agent instructions: https://hyperion.builders/llms.txt. Contact: office@hyperion.builders
