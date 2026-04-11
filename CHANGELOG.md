# Changelog

All notable changes to AgentUIManifest are documented here.

## [1.1.0] - 2026-04-11

### Changed

- Simplified governance: single maintainer model, no CLA, no trademark restrictions
- Replaced CLA with DCO (Developer Certificate of Origin)
- Trimmed CHARTER.md from 313 lines to a simple 1-page doc
- Simplified CONTRIBUTING.md
- Removed `multiselect` and `readonly` field types (over-engineered, add later if needed)
- Removed `minSelect`/`maxSelect` from FieldValidation
- Status changed from Final to Draft

## [1.0.0] - 2026-04-09

### Added

- Initial specification release
- 4 display modes: `form`, `chat`, `wizard`, `viewer`
- 6 field types: `text`, `textarea`, `url`, `number`, `select`, `toggle`
- 3 result formats: `markdown`, `structured`, `file`
- Conditional field visibility
- Field validation with patterns, ranges, and options
- Wizard mode with sequential steps
- Chat mode with streaming support
- Viewer mode for read-only displays