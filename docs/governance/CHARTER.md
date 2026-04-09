# AgentUIManifest Governance Charter

**Version:** 1.0  
**Status:** Active  
**Effective Date:** April 9, 2026  

---

## 1. Purpose

This charter establishes the governance framework for the AgentUIManifest standard, ensuring its long-term health, interoperability, and adoption while maintaining backward compatibility.

AgentUIManifest is a declarative JSON schema that enables MCP agents to declare their human-facing UI. This charter governs how the specification evolves, how decisions are made, and how the community participates.

---

## 2. Guiding Principles

1. **Open by Default** — Anyone can propose changes; the barrier to participation is low
2. **Implementation Proof** — Reference implementations are preferred over abstract designs
3. **Backward Compatibility** — Breaking changes are rare and carefully managed
4. **Durability** — The standard that survives 20 years beats the one that changes often
5. **Transparency** — All decisions, discussions, and deliberations are public

---

## 3. Governance Phases

| Phase | Timeline | Model | BDFL/Chair |
|-------|----------|-------|------------|
| **V1** | 0-12 months | Benevolent dictatorship (core team) | Zigzag Team |
| **V2** | 12-24 months | Technical steering committee (3 rotating seats) | Elected |
| **V3** | 24+ months | Elected committee with formal voting | Elected |

### 3.1 Phase V1 (Current)

During the initial phase, the Zigzag Team serves as BDFL (Benevolent Dictator For Life):

- **Authority**: Final say on all specification changes
- **Responsibility**: Maintain backward compatibility, publish releases
- **Limitation**: Must provide written rationale for all decisions
- **Transition**: Phase V1 ends when 10+ implementations exist OR 12 months elapsed

### 3.2 Phase V2

Technical Steering Committee (TSC) with rotating seats:

- **Composition**: 3 elected members, 1-year terms
- **Elections**: Annual, simple majority vote
- **Authority**: Approve minor/major changes
- **Chair**: Rotates among TSC members quarterly

### 3.3 Phase V3

Formal foundation transfer:

- **Model**: Elected committee with formal voting procedures
- **IP Transfer**: Specification transferred to neutral foundation
- **Trademark**: "AgentUIManifest" trademark held by foundation

---

## 4. Change Classification

| Change Type | Description | Approval Required | Notice Period |
|-------------|-------------|------------------|---------------|
| **Patch** | Bug fixes, clarifications, typo corrections | BDFL/TSC approval | None |
| **Minor** | New features, backward-compatible additions | Simple majority (50%+1) | None |
| **Major** | Breaking changes, deprecations | Super-majority (66%+1) | 90 days |
| **Emergency** | Security, critical bugs | 48-hour objection | Immediate |

---

## 5. Voting Procedures

### 5.1 Eligibility

- All registered contributors who have submitted at least one PR
- One vote per individual
- No organizational voting blocks

### 5.2 Quorum

- **Minor changes**: 3+ votes cast
- **Major changes**: 5+ votes cast, including at least 2 TSC members
- **Emergency**: 2+ TSC members must respond

### 5.3 Voting Timeline

1. **Proposal Posted**: GitHub issue with "RFC" label
2. **Discussion Period**: 30 days for minor, 60 days for major
3. **Vote Called**: Explicit motion by proposer or TSC member
4. **Voting Period**: 7 days for minor, 14 days for major
5. **Result Announced**: Within 48 hours of vote close

### 5.4 Abstentions

Abstentions count toward quorum but not toward the vote threshold.

---

## 6. RFC Process

### 6.1 Submitting an RFC

1. Create a GitHub issue with:
   - Title: `RFC: [Brief Description]`
   - Labels: `rfc`, `agentuimanifest`
   - Body: Problem statement, proposed solution, alternatives considered
   - Reference implementation (strongly preferred)

2. Fill out RFC template:
   ```
   ## Summary
   ## Motivation
   ## Detailed Design
   ## Adoption Strategy
   ## Migration Path
   ## References
   ```

### 6.2 Review Process

1. **BDFL/TSC Acknowledgment**: Within 7 days
2. **Public Comment**: 30 days minimum
3. **TSC Deliberation**: Up to 14 days
4. **Decision**: Approved, rejected, or deferred

### 6.3 After Approval

1. Implementation in reference repo
2. Update specification document
3. Publish as PR to main branch
4. Release according to versioning policy

---

## 7. Versioning Policy

AgentUIManifest uses Semantic Versioning (SemVer):

```
MAJOR.MINOR.PATCH
```

### 7.1 Version Definitions

| Increment | When | Backward Compatible |
|-----------|------|---------------------|
| **MAJOR** | Incompatible API changes | No |
| **MINOR** | New backward-compatible features | Yes |
| **PATCH** | Bug fixes, clarifications | Yes |

### 7.2 Deprecation Policy

- Deprecated features must be announced with 90-day notice
- Deprecated features remain functional for at least 1 major version
- Migration guides required for all major changes

### 7.3 Current Stable Version

**v1.0.0** — Initial release (April 9, 2026)

---

## 8. IP & Licensing

### 8.1 License

AgentUIManifest specification is licensed under **Apache License 2.0**.

Implementations may use any license; the specification itself is not copyleft.

### 8.2 Trademark

"AgentUIManifest" is a registered trademark held by the governance body:

- **Phase V1-V2**: Zigzag Team
- **Phase V3+**: Neutral foundation

### 8.3 Contribution Requirements

All contributions to the specification require:

1. Signed CLA (Contributor License Agreement)
2. Explicit copyright assignment to the trademark holder
3. Compliance with the code of conduct

---

## 9. Code of Conduct

All governance interactions are governed by the **Contributor Covenant Code of Conduct**:

- **Be respectful** — Disagreement is fine; disrespect is not
- **Be inclusive** — Welcome contributors from all backgrounds
- **Be patient** — Not everyone has the same level of experience
- **Be collaborative** — Work together toward the best solution
- **Be transparent** — All decisions must be documented

### Enforcement

Violations may result in:

1. Private warning
2. Public reprimand
3. Temporary suspension (up to 30 days)
4. Permanent expulsion

Appeals may be filed with the TSC (Phase V2+) or BDFL (Phase V1).

---

## 10. Working Groups

### 10.1 Formation

Working groups may be formed for:

- New display modes
- New field types
- Integration with other standards
- Reference implementations

### 10.2 Requirements

- Minimum 2 contributors
- Public meeting notes
- Quarterly progress reports
- Disband after 6 months if inactive

### 10.3 Current Working Groups

| Group | Focus | Leads |
|-------|-------|-------|
| TBD | — | — |

---

## 11. Repository Structure

```
agentuimanifest/
├── SPEC.md                    # Master specification
├── CHANGELOG.md               # Version history
├── GOVERNANCE.md              # This charter
├── CLA.md                     # Contributor license agreement
├── examples/                  # Reference manifests
│   ├── scout-form.json
│   ├── wizard-example.json
│   └── chat-example.json
├── implementations/            # Known implementations
│   └── zigzag/
└── .github/
    ├── ISSUE_TEMPLATE/
    └── workflows/
```

---

## 12. Contact & Communication

| Channel | Purpose | Response Time |
|---------|--------|---------------|
| [GitHub Issues](https://github.com/zigzag-ai/agentuimanifest) | RFCs, bugs, questions | 7 days |
| [Discord #agentuimanifest](https://discord.gg/zigzag) | Discussion, announcements | Best effort |
| [agentuimanifest@zigzag.ai](mailto:agentuimanifest@zigzag.ai) | Admin, trademarks | 14 days |

---

## 13. Amendment Process

This charter may be amended through:

1. **Major change process** (Section 5)
2. **Unanimous consent** of TSC for procedural changes
3. **BDFL decree** for Phase V1 emergency changes

Amendments require 90-day notice before taking effect.

---

## Appendix A: Change Log

| Date | Version | Changes |
|------|---------|---------|
| 2026-04-09 | 1.0 | Initial charter adoption |

---

## Appendix B: Glossary

| Term | Definition |
|------|------------|
| **BDFL** | Benevolent Dictator For Life — final authority during Phase V1 |
| **RFC** | Request For Comments — formal proposal process |
| **SemVer** | Semantic Versioning — version number scheme |
| **TSC** | Technical Steering Committee — governance body for Phase V2+ |
| **PR** | Pull Request — code contribution |

---

## Appendix C: Related Documents

- [AgentUIManifest Specification](../SPEC_AGENTUIMANIFEST.md)
- [Contributor License Agreement](./CLA.md)
- [Code of Conduct](./CODE_OF_CONDUCT.md)
- [Contributing Guide](./CONTRIBUTING.md)

---

_AgentUIManifest Governance Charter — Built with transparency, maintained with care._
