# Hyperion Builders MCP (HBMeasure)

Roof measurement reports and building code lookups for a single street address, for people and for software agents.

This repository is a pointer. The service is hosted; there is no source code here.

- Website: https://hyperion.builders
- MCP endpoint (streamable HTTP): https://measure.hyperion.builders/mcp
- Agent instructions: https://hyperion.builders/llms.txt
- Discovery manifest: https://hyperion.builders/.well-known/hbmeasure.json
- Terms: https://hyperion.builders/terms
- Contact: office@hyperion.builders

## What an agent can do

Orders and payment use the HTTP and x402 endpoints described in https://hyperion.builders/llms.txt and the discovery manifest, not the MCP tools.

- Read the public information tools (services, sample reports, entity facts) through the MCP endpoint. These are read-only and need no key.
- Check coverage for an address ($0.05 over x402).
- Order a roof measurement report ($1.00 over x402 in USDC, or $1.34 by card). Every figure in a report carries a grade: Verified, Measured or Estimated.
- Order a building code lookup for an address ($0.25 over x402, or $2.37 by card).

All sales are final. Each report is computed when it is ordered, so there are no refunds.
Measurements are estimates graded Verified / Measured / Estimated; check them before ordering materials or relying on them for a claim or contract.

## Connect

Add `https://measure.hyperion.builders/mcp` as a remote MCP server (streamable HTTP). Read https://hyperion.builders/llms.txt first.

## License

The files in this repository are released under the MIT License (see LICENSE). The hosted service is governed by the terms linked above.
