# Canvas Course Page Video Editor – Project Overview

**Status:** Draft v0.1  
**Created:** 2026-02-18  
**Source:** Leverages [canvas_2879](https://github.com/gjoeckel/canvas_2879) patterns

---

## 1. Purpose

A **Canvas Course Page Video Editor** provides a navigator/index for course media assets (audio, graphics, video sequences, captioned videos) stored in Box, with links to open or download project files for editing in desktop apps (Audacity, Illustrator, After Effects, Premiere Pro). It is not an in-browser editor.

---

## 2. Source of Truth

| Resource | Role |
|----------|------|
| **AD-365-V4 Box folder** (184899190488) | Media assets: m1–m5, orientation, Ae, Ai, Audio, Canvas, Pr, Practice files |
| **Box map** (AD-365-V4-map.json) | Folder/file inventory with Box IDs and paths |
| **canvas_2879** | Template for GitHub Pages UI, Canvas/Box integration, local build workflow |

---

## 3. Supported File Types

| Extension | Application | In-browser preview? | Primary action |
|-----------|-------------|---------------------|----------------|
| .aup3 | Audacity | No | Download / Open in Box |
| .ai | Adobe Illustrator | Limited | Download / Open in Box |
| .aep | After Effects | No | Download / Open in Box |
| .prproj | Premiere Pro | No | Download / Open in Box |
| .mp4, .mov | Rendered video | Yes (Box) | Preview in Box |

Project files (.aup3, .ai, .aep, .prproj) require desktop apps; the UI links to Box for download/open.

---

## 4. Build Workflow (Decided)

- **Build locally** (dev, test, iterate)
- **Push to GitHub** (version control, deployment)
- **GitHub Pages** serves the static site
- canvas_2879 repo provides the template; fork or copy as needed

---

## 5. Open Questions / Feedback Requested

### Q1. Base repository
- **Option A:** Fork canvas_2879 and adapt for Video Editor
- **Option B:** New repo; copy selected scripts and patterns
- **Option C:** Extend canvas_2879 with a Video Editor section

*Your preference?*

### Q2. Data source for the index
- **Option A:** Use AD-365-V4-map.json directly (filter by extension)
- **Option B:** Generate a dedicated `video-editor-index.json` from the map
- **Option C:** Pull from Canvas API (like canvas_2879) and map to Box IDs

*Your preference?*

### Q3. Scope of first iteration (MVP)
- **Minimal:** Single page listing files by type, with Box links
- **Medium:** Module/section structure from Box paths, filters by type
- **Full:** Canvas page mapping, dual links (project + preview), metadata

*Your preference?*

---

## 6. Documentation Index

| Document | Purpose |
|----------|---------|
| VIDEO-EDITOR-PROJECT-OVERVIEW.md | This file – purpose, scope, open questions |
| VIDEO-EDITOR-UI-SPEC.md | UI requirements, file-type display, filters, layout options |
| VIDEO-EDITOR-TECH-SPEC.md | Data flow, file-type mapping, build artifacts |
| VIDEO-EDITOR-IMPLEMENTATION-PLAN.md | Phased implementation, estimates |

---

*Feedback on Q1–Q3 will guide the next documentation iteration.*
