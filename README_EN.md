# open-claude-code

<p align="center">
  <strong>English</strong> · <a href="./README.md">中文</a>
</p>

`open-claude-code` is reconstructed from sourcemap information related to `@anthropic-ai/claude-code@2.1.88`. Based on that recovery work, this repository includes build fixes, structural restoration, and runtime adjustments. Current status: the project has been repaired and can be run locally for code reading and study.

**The recovery, build repair, and documentation work in this repository are driven by Codex.**

## Future Direction

This repository is not intended to replace the official release. It exists as a relatively complete local project for source reading, engineering analysis, build process research, and reference on terminal agent implementations.

## Project Overview

This repository mainly preserves the core engineering structure of the Claude Code terminal tool, including but not limited to:

- `src/`: restored source code
- `src/entrypoints/`: CLI entrypoints and related initialization logic
- `vendor/`: platform-specific binaries or runtime dependencies
- `stubs/`: local stub dependencies
- `images/`: demo screenshots

### Private MCP/NAPI compatibility and related status

The items below describe the current state of the private MCP and related native / NAPI compatibility layers in this recovered build. The table explicitly separates **import/build coverage** from **real runtime capability**:

| Component | Progress | Availability |
| --- | --- | --- |
| `@ant/claude-for-chrome-mcp` | A minimal compatibility layer is in place, and the browser MCP tool definitions are restored for local builds. | Tool listing works, and read-style calls return empty payloads or unavailable responses; it **does not** reconnect the real browser extension, pairing flow, or browser bridge execution backend. |
| `@ant/computer-use-mcp` | A minimal compatibility layer is in place, including the main package and required subpath exports. | It can serve minimal state-oriented interfaces such as `request_access`, `list_granted_applications`, `current_display`, `switch_display`, and `list_displays`; real Computer Use actions such as mouse, keyboard, screenshot, and clipboard control are still placeholder / unavailable. |
| `@ant/computer-use-input` | A minimal stub is in place. | It currently only exposes the degraded semantic `isSupported = false` to avoid missing-package failures; it **does not** provide a real input injection backend. |
| `@ant/computer-use-swift` | A minimal macOS native compatibility shape is in place. | Permission, app, and display queries return empty or default values; screenshot capture, app launching, and capture preparation are still not restored. |
| `image-processor-napi` | Recovered through an open replacement based on `sharp`, with compatible exports added. | Regular image reading, resizing, and compression paths are basically usable; however, `getNativeModule()` currently returns `null`, so image / clipboard flows that expected the native fast path fall back to the existing non-native path. |
| `color-diff-napi` | No longer depends on the native package; it has been replaced with a local TypeScript implementation. | The main structured diff and syntax-highlighting flow is usable; however, it is based on `highlight.js` rather than the original native stack, so details such as `BAT_THEME` are still compatibility behavior, not a 1:1 restoration. |
| `audio-capture-napi` | A minimal stub is in place so voice features no longer crash on missing-package import. | Windows still needs a real native recording backend and is currently unavailable; Linux / macOS can still try the existing `arecord` / `rec` fallback chain in the project, but that is not the same as restoring the original native audio capture implementation. |

> In plain terms: these layers are now good enough to keep the recovered project buildable, runnable for study, and readable; `image-processor-napi` and `color-diff-napi` are closer to practical substitutes, while browser bridge, computer-use, and native audio are still mostly in graceful-degradation territory.

## Requirements

- Node.js 18 or later
- npm
- Bun

## Quick Start

Install dependencies:

```bash
bun install or npm install
```

Check the version:

```bash
bun run version or npm run version
```

Start directly:

```bash
bun run dev or npm run dev
```

Rebuild if needed:

```bash
bun run build or npm run build
```

## Screenshots

### Startup

![Startup](./images/snapshot1.png)

### Interaction

![Interaction](./images/snapshot2.png)

![Interaction2](./images/snapshot3.png)

## Disclaimer

This repository is not an official Anthropic project and does not represent Anthropic in any way.

We do not own Claude Code, and we do not claim ownership over the original Claude Code source code, name, trademarks, branding, or any derivative rights related to it. The original source code and related rights associated with Claude Code belong to Anthropic, Inc. or the relevant rights holders.

This repository is provided for learning, research, discussion, and reference only. Do not use it for any commercial activity, including but not limited to:

- commercial distribution
- paid resale
- closed-source integration
- deployment as a paid service
- sublicensing
- any use that may infringe on the rights of the original owner

Users are solely responsible for evaluating and assuming all risks arising from the use of this repository, including but not limited to compliance risks, intellectual property risks, and any direct or indirect losses caused by such use.

Any use of this project to infringe upon Anthropic PBC's legitimate rights and interests or to circumvent product  policies is unrelated to this project and undertaken at your own risk.

If you are a relevant rights holder and believe any content in this repository should not be publicly displayed or distributed, please contact through the repository channel for handling.

## License Notice

This repository does not grant any additional license to Anthropic's original code, nor does it imply any relicensing of the upstream project. Except for rights otherwise granted by applicable law, this repository should not be treated as a substitute for an open-source license to the original Claude Code codebase.

## Acknowledgements

- Thanks to Anthropic for the original Claude Code project
- Thanks to Codex for contributing to the recovery, build repair, and documentation of this repository
