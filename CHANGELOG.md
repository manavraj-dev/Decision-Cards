# Changelog

All notable changes to this project are documented in this file.
The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [1.0.0] - 2026-09-22

### Added
- Weighted pros/cons cards with live Pros/Cons/Net scoring
- Standard competition ranking of options by net score
- Search and multi-key sorting (rank, net, pros, cons, title)
- Local-first persistence via `localStorage`, with corrupted-data detection and recovery flow
- Automatic backup-before-import and one-click restore
- Export to JSON, CSV, and a standalone HTML report
- JSON schema validation on import

### Fixed
- Number inputs for item weights widened so negative/decimal values no longer clip
- Search and sort controls given stable minimum widths to prevent layout shift on narrow viewports

### Accessibility
- Added labels/`aria-label`s for the search box, sort select, title/description fields, weight inputs, and icon-only delete buttons
- Added a page `<meta description>` and favicon
