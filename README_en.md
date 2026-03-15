**English** | [中文](./README.md)

# MusicFree Skills

A collection of AI Agent Skills for the [MusicFree](https://github.com/maotoumao/MusicFree) music player. Skills are packaged instructions and reference documents that extend AI coding agent capabilities.

Skills follow the [Agent Skills](https://agentskills.io/) format.

## Available Skills

### musicfree-plugin-dev

Complete guide to developing MusicFree plugins from scratch. AI-driven site analysis, data integration, code generation, and automated testing — full workflow guidance.

Use when:

- Writing or creating music source plugins for MusicFree
- Adapting a music website or API into a MusicFree plugin
- Analyzing a site's public interfaces and generating data integration code

Capabilities covered:

- Plugin protocol with full method signatures (search, playback, lyrics, playlists, charts — 14 methods total)
- Sandbox environment and built-in modules (axios / cheerio / crypto-js / dayjs, etc.)
- Site analysis methodology (static HTML parsing / public API integration / Playwright network capture)
- Media type definitions (IMusicItem / IAlbumItem / IArtistItem / IMusicSheetItem, etc.)
- Automated testing and iterative fix workflow

### musicfree-themepack-dev

Complete guide to developing MusicFree desktop theme packs from scratch. AI provides color scheme suggestions, user confirms visually, collaborative theme design.

Use when:

- Creating or designing custom themes for MusicFree desktop
- Generating theme packs from reference images, colors, or style keywords
- Building advanced themes with dynamic effects (iframe animations)

Capabilities covered:

- CSS variable system (complete override rules for 150+ semantic variables)
- Color design paradigms (dark / light / AMOLED pure black / cyberpunk, etc.)
- Static themes and dynamic iframe theme development
- Asset optimization and packaging/testing workflow
- Theme marketplace submission guide (tags, validation, PR process)

## Installation

```bash
npx skills add maotoumao/musicfree-skills
```

## Usage

Skills are automatically available once installed. The agent will use them when relevant tasks are detected.

Example prompts:

```
Build a MusicFree plugin using https://example.com as the data source
```

```
Create a cyberpunk-style theme for MusicFree desktop
```

```
I have a music API doc — adapt it into a MusicFree plugin
```

## Skill Structure

Each skill contains:

- `SKILL.md` — Instructions for the AI agent
- `references/` — Supporting documentation (protocol definitions, variable references, development playbooks, etc.)

```
skills/
├── musicfree-plugin-dev/
│   ├── SKILL.md
│   └── references/
│       ├── media-types.md
│       ├── plugin-protocol.md
│       └── site-analysis-playbook.md
└── musicfree-themepack-dev/
    ├── SKILL.md
    └── references/
        ├── color-paradigms.md
        ├── css-variable-reference.md
        ├── iframe-guide.md
        └── packaging-checklist.md
```

## License

AGPL-3.0
