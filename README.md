# AgentUIManifest

**Declarative UI schema for MCP agents.**

AgentUIManifest enables MCP agents to declare their human-facing UI through a simple, validated JSON schema. Instead of LLMs interpreting raw `inputSchema`, users interact with beautiful, type-safe forms.

---

## The Problem

MCP tools expose `inputSchema` designed for LLMs:

```json
{ "repo": { "type": "string" } }
```

But humans need guidance to use them. AgentUIManifest solves this:

```json
{
  "label": "GitHub Repository",
  "description": "Paste a GitHub URL",
  "fieldType": "url",
  "placeholder": "https://github.com/owner/repo"
}
```

---

## Quick Start

### Define Your Manifest

```json
{
  "version": "1",
  "displayMode": "form",
  "primaryTool": "analyze_repo",
  "fields": [
    {
      "id": "repo",
      "label": "GitHub Repository",
      "fieldType": "url",
      "validation": {
        "required": true,
        "pattern": "^https://github\\.com/.+"
      }
    }
  ],
  "resultFormat": "markdown"
}
```

### Display Modes

| Mode | Description | Use Case |
|------|-------------|----------|
| **form** | Single-page form | 1-5 inputs, straightforward tools |
| **chat** | Multi-turn streaming | Agents that clarify intent |
| **wizard** | Multi-step forms | Complex workflows with decisions |
| **viewer** | Read-only display | Dashboards, results |

### Field Types

`text` · `textarea` · `url` · `number` · `select` · `toggle`

---

## Examples

See [`examples/`](examples/) for complete manifests:

- [`scout-form.json`](examples/scout-form.json) — GitHub security scanner
- [`wizard-example.json`](examples/wizard-example.json) — Multi-step workflow
- [`chat-example.json`](examples/chat-example.json) — Conversational agent

---

## Specification

The full specification is in [`SPEC.md`](SPEC.md), covering:

- Schema reference
- Display modes
- Field types & validation
- Governance model

---

## Implementations

- [Zigzag](https://zigzag.ai) — Agent marketplace with AgentUIManifest support
- [Scout](https://github.com/zigzag-ai/scout) — GitHub security scanner
- [Ziggy](https://github.com/htafolla/ziggy-mcp) — Tweet generator MCP server

---

## Governance

Simple and open:

- @htafolla is the current maintainer
- Submit RFCs as GitHub issues
- No CLA required (DCO via PRs)
- Community governance added when there are 10+ active contributors

See [`docs/governance/CHARTER.md`](docs/governance/CHARTER.md) for details.

---

## Contributing

1. Read the [specification](SPEC.md)
2. Open a GitHub issue describing your change
3. Submit a PR
4. Maintainer reviews and merges

---

## License

Apache 2.0 — see [LICENSE](LICENSE)