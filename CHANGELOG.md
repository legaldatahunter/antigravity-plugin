# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

## [1.1.2] - 2026-11-20

### Changed

- `report_source_issue` is now optional and consent-based across all skills: the agent tells the user about a data gap, offers a report, shows the exact proposed text, and files only after the user agrees to that specific report.
- Report descriptions are limited to source, issue type, and what was missing or broken; user query text, facts, names, and task descriptions are excluded unless the user approves the exact wording.
- Removed skill instructions that pushed the agent to file reports often or on its own.
- Bumped plugin version to 1.1.2.

## [1.1.1] - 2026-11-19

### Added

- MIT `LICENSE` file.

### Changed

- Set the plugin manifest (`plugin.json`) `license` field to `MIT`.
- Bumped plugin version to 1.1.1.

## [1.1.0]

- Previous release.
