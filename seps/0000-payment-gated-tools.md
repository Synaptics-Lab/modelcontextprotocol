# SEP-0000: Payment-Gated Tool Execution & Challenge Protocol Extension (MCP-402)

- **Status**: Draft
- **Type**: Extensions Track
- **Created**: 2026-09-26
- **Author(s)**: Abdul Shabazz (@veritasvaultone, Synaptics Lab) <veritasvaultone@gmail.com>
- **Sponsor**: None
- **PR**: https://github.com/modelcontextprotocol/modelcontextprotocol/pull/0000
- **Related Specs**: IETF draft-shabazz-http-x402-tswp-00; Solana SIMD PR #671; Zenodo DOI 10.5281/zenodo.22979715

---

## Abstract

This Specification Enhancement Proposal defines **MCP-402**, an Extensions Track standard for the Model Context Protocol (MCP) that introduces native payment capability negotiation, payment challenge error handling (JSON-RPC error code `402`), and cryptographic proof-of-settlement validation for machine-to-machine (M2M) tool execution. 

MCP-402 enables autonomous AI agents to discover commercial tool pricing, negotiate settlement terms across public multi-rail ledgers (including Solana and XRPL) or HTTP 402 endpoints, and deliver verified settlement receipts without relying on out-of-band subscription keys or centralized payment aggregators.

---

## Motivation

The Model Context Protocol currently standardizes JSON-RPC transport for Prompts, Resources, and Tools. However, as autonomous AI agents interact with external commercial services—such as high-value computational simulations, financial data streams, or institutional execution gateways—the absence of a native payment negotiation layer creates severe operational limitations:

1. **Out-of-Band Key Management:** Existing MCP tools require human operators to manually provision and configure proprietary API keys, breaking end-to-end agent autonomy.
2. **Absence of In-Band Metering:** Servers have no protocol-compliant mechanism to signal credit exhaustion, per-invocation pricing, or replenishment challenges within the standard JSON-RPC exchange.
3. **No Verifiable Settlement Grammar:** Clients lack a standardized channel to provide cryptographic evidence (such as on-chain transaction hashes with challenge preimages) that an invocation fee has been fulfilled.

MCP-402 resolves these deficiencies by embedding the typed wire protocol of IETF `X402-TSWP` directly into the MCP lifecycle.

---

## Specification

### 1. Capability Negotiation (`initialize`)

During the `initialize` handshake, clients and servers declare payment negotiation capabilities under `capabilities.payments`.

#### Client Initialization Request:
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "initialize",
  "params": {
    "protocolVersion": "2024-11-05",
    "capabilities": {
      "roots": { "listChanged": true },
      "sampling": {},
      "payments": {
        "schemes": ["x402"],
        "supportedRails": ["solana-devnet", "solana-mainnet", "xrpl-altnet", "xrpl-mainnet"]
      }
    },
    "clientInfo": {
      "name": "SynapticTraderAgent",
      "version": "1.0.0"
    }
  }
}
