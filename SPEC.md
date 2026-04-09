# AgentUIManifest Specification

**Version:** 1.0  
**Status:** Final  
**Authors:** Zigzag Team  
**License:** Apache 2.0  

---

## Table of Contents

1. [Overview](#1-overview)
2. [Schema Reference](#2-schema-reference)
3. [Display Modes](#3-display-modes)
4. [Field Types](#4-field-types)
5. [Validation](#5-validation)
6. [Governance](#6-governance)
7. [Adoption](#7-adoption)
8. [Examples](#8-examples)
9. [Changelog](#9-changelog)

---

## 1. Overview

AgentUIManifest is a declarative JSON schema enabling MCP agents to declare their human-facing UI.

### The Problem

MCP tools expose `inputSchema` designed for LLMs — humans can't use them without guidance.

```json
// LLM-friendly (what we have)
{ "repo": { "type": "string" } }

// Human-friendly (what we need)
{ "label": "GitHub Repository", "description": "Paste a GitHub URL", "fieldType": "url" }
```

### Design Principles

- **Declarative**: Describes WHAT to show, not HOW to build it
- **Minimal**: 4 display modes, 8 field types, 3 result formats
- **Validated**: Zod schema for registration-time + runtime validation
- **Extensible**: Future modes via extension points

---

## 2. Schema Reference

### Top-Level

```typescript
interface AgentUiManifest {
  version: '1';
  displayMode: 'form' | 'chat' | 'wizard' | 'viewer';
  
  // Form mode
  primaryTool?: string;
  fields?: Field[];
  resultFormat?: 'markdown' | 'structured' | 'file';
  
  // Wizard mode
  wizardSteps?: WizardStep[];
  finalTool?: string;
  
  // Chat mode
  chat?: ChatConfig;
  
  // Viewer mode
  viewerTool?: string;
  
  // Display
  icon?: string;
  accentColor?: string;
  showPoweredBy?: boolean;
  exampleQueries?: string[];
}
```

### Field

```typescript
interface Field {
  id: string;
  label: string;
  description?: string;
  fieldType: FieldType;
  placeholder?: string;
  defaultValue?: string | number | boolean;
  validation?: FieldValidation;
  conditionalOn?: {
    fieldId: string;
    operator: 'equals' | 'notEquals' | 'contains' | 'exists' | 'notExists';
    value?: string | number | boolean;
  };
  hidden?: boolean;
}

type FieldType = 'text' | 'textarea' | 'url' | 'number' | 'select' | 'multiselect' | 'toggle' | 'readonly';
```

### Validation

```typescript
interface FieldValidation {
  required?: boolean;
  minLength?: number;
  maxLength?: number;
  pattern?: string;
  patternHint?: string;
  min?: number;
  max?: number;
  step?: number;
  options?: string[];
  minSelect?: number;
  maxSelect?: number;
}
```

---

## 3. Display Modes

| Mode | Description | Use Case |
|------|-------------|----------|
| **form** | Single-page form, one tool call | 1-5 inputs, straightforward tools |
| **chat** | Multi-turn streaming conversation | Agents that clarify intent |
| **wizard** | Multi-step sequential forms | Complex workflows with decisions |
| **viewer** | Read-only data display | Dashboards, auto-running agents |

---

## 4. Field Types

| Type | Description | Validation |
|------|-------------|------------|
| **text** | Single-line input | minLength, maxLength, pattern |
| **textarea** | Multi-line input | minLength, maxLength |
| **url** | URL with validation | pattern, schemes |
| **number** | Numeric input | min, max, step |
| **select** | Single dropdown | options[] |
| **multiselect** | Multi-checkboxes | options[], minSelect, maxSelect |
| **toggle** | Boolean switch | — |
| **readonly** | Display only | format (text\|badge\|code\|link) |

---

## 5. Validation

### Client-Side

```typescript
function validateFieldValue(field: Field, value: unknown): ValidationError | null;
function validateAllFields(fields: Field[], values: Record<string, unknown>): ValidationError[];
```

### Conditional Visibility

```json
{
  "id": "advanced",
  "label": "Show Advanced",
  "fieldType": "toggle"
},
{
  "id": "pattern",
  "conditionalOn": {
    "fieldId": "advanced",
    "operator": "equals",
    "value": true
  }
}
```

---

## 6. Governance

_Developed via StringRay multi-advisor skill with agent analysis._

### Guiding Principles

1. **Open by default** — Anyone can propose changes
2. **Implementation proof** — Reference implementation preferred
3. **Backward compatibility** — Breaking changes rare and carefully managed
4. **Durability** — The standard that survives 20 years beats the one that changes often

### Governance Phases

| Phase | Timeline | Model |
|-------|----------|-------|
| **V1** | 0-12 months | Benevolent dictatorship (core team) |
| **V2** | 12-24 months | Technical steering committee (3 rotating seats) |
| **V3** | 24+ months | Elected committee with formal voting |

### Voting Mechanism

- **Minor/patch**: Simple majority (50%+1)
- **Major (breaking)**: Super-majority (66%+1)
- **Emergency**: 48-hour objection window

### Versioning

Semantic versioning: `MAJOR.MINOR.PATCH`

| Change | Notice Required |
|--------|-----------------|
| Patch | None |
| Minor | None |
| Major | 90-day deprecation + migration guide |

### Proposal Process

1. Submit RFC as GitHub issue (with reference implementation preferred)
2. 30-day open comment period
3. Vote per thresholds
4. Implementation + release

### IP & Licensing

- **License**: Apache 2.0
- **Trademark**: "AgentUIManifest" reserved
- **Contributions**: CLA required

---

## 7. Adoption

### Phases

| Phase | Timeline | Goal |
|--------|----------|------|
| **Prove** | 0-6 months | Build tooling, 3-5 pilot agents |
| **Grow** | 6-12 months | 10+ implementations, registry launched |
| **Standardize** | 12-24 months | Foundation transfer, compliance cert |

### Success Metrics

| Metric | Year 1 | Year 2 |
|--------|--------|--------|
| Implementations | 10+ | 50+ |
| Contributors | 50+ | 200+ |
| GitHub Stars | 1K+ | 3K+ |

---

## 8. Examples

### Scout: GitHub Security Scanner

```json
{
  "version": "1",
  "displayMode": "form",
  "primaryTool": "analyze_repo",
  "fields": [
    {
      "id": "repo",
      "label": "GitHub Repository",
      "description": "Paste a GitHub URL to scan for vulnerabilities",
      "fieldType": "url",
      "placeholder": "https://github.com/owner/repo",
      "validation": {
        "required": true,
        "pattern": "^https://github\\.com/.+"
      }
    },
    {
      "id": "branch",
      "label": "Branch",
      "fieldType": "text",
      "placeholder": "main",
      "validation": { "required": false }
    }
  ],
  "resultFormat": "markdown",
  "showPoweredBy": true
}
```

---

## 9. Changelog

### [1.0] - 2026-04-09

Initial release.

---

## Appendix: Related Standards

- **MCP Protocol** — Agent communication
- **A2A Agent Cards** — Agent discovery
- **MCP Apps (SEP-1865)** — Server-declared UIs
- **OpenAPI** — API specification

---

_AgentUIManifest — Declarative agent UI for humans._
