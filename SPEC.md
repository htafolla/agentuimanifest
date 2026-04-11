# AgentUIManifest Specification

**Version:** 1.0  
**Status:** Draft  
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
- **Minimal**: 4 display modes, 6 field types, 3 result formats
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

type FieldType = 'text' | 'textarea' | 'url' | 'number' | 'select' | 'toggle';
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
}
```

---

## 3. Display Modes

| Mode | Description | Use Case |
|------|-------------|----------|
| **form** | Single-page form, one tool call | 1-5 inputs, straightforward tools |
| **chat** | Multi-turn streaming conversation | Agents that clarify intent |
| **wizard** | Multi-step sequential forms | Complex workflows with decisions |
| **viewer** | Read-only data display | Dashboards, results |

---

## 4. Field Types

| Type | Description | Validation |
|------|-------------|------------|
| **text** | Single-line input | minLength, maxLength, pattern |
| **textarea** | Multi-line input | minLength, maxLength |
| **url** | URL input | pattern |
| **number** | Numeric input | min, max, step |
| **select** | Dropdown | options[] |
| **toggle** | Boolean switch | — |

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

### Guiding Principles

1. **Open by default** — Anyone can propose changes
2. **Implementation proof** — Reference implementation preferred
3. **Minimal bureaucracy** — Don't over-engineer a simple spec
4. **Don't break consumers** — Backward compatibility preferred

### Model

For v1, @htafolla is the maintainer.

- Submit RFCs as GitHub issues
- Decisions made by maintainer with community input
- Add formal governance when there are 10+ active contributors

### Simple Rules

- No CLA required — DCO implicit via PRs
- No trademark restrictions — Use the name freely
- 90 days deprecation warning for breaking changes (if needed)
- Semantic versioning: `MAJOR.MINOR.PATCH`

### How to Propose Changes

1. Open a GitHub issue with your RFC
2. Discuss in the comments
3. PR with implementation
4. Merged by maintainer

---

## 7. Adoption

| Phase | Goal |
|--------|------|
| **Prove** | Build tooling, 3-5 pilot agents |
| **Grow** | 10+ implementations |
| **Standardize** | Independent governance, compliance |

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

### [1.1] - 2026-04-11

Simplified governance. Removed multiselect, readonly field types. Dropped CLA in favor of DCO. Status: Draft.

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
