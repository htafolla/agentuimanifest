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
| **viewer** | Read-only display | Dashboards, auto-running agents |

### Field Types

`text` · `textarea` · `url` · `number` · `select` · `multiselect` · `toggle` · `readonly`

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
- Adoption strategy

---

## Implementations

- [Zigzag](https://zigzag.ai) — Agent marketplace with AgentUIManifest support
- [Scout](https://github.com/zigzag-ai/scout) — GitHub security scanner

---

## Governance

AgentUIManifest follows a 3-phase governance model:

1. **Phase V1 (0-12mo)** — Benevolent dictatorship (current)
2. **Phase V2 (12-24mo)** — Technical steering committee
3. **Phase V3 (24mo+)** — Formal foundation

See [`docs/governance/CHARTER.md`](docs/governance/CHARTER.md) for details.

---

## Contributing

1. Read the [specification](SPEC.md)
2. Submit an [RFC](.github/ISSUE_TEMPLATE/rfc-template.md)
3. Implement and test
4. Join the [Discord](https://discord.gg/zigzag)

---

## License

Apache 2.0 — see [LICENSE](LICENSE)
