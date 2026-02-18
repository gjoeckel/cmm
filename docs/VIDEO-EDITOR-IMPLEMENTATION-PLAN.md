# Canvas Course Page Video Editor – Implementation Plan

**Status:** Draft v0.1  
**Depends on:** PROJECT-OVERVIEW, UI-SPEC, TECH-SPEC

---

## Phase 1: MVP (Single-page index)

1. Create `video-editor-index.html` (or extend existing viewer)
2. Load AD-365-V4-map.json
3. Filter items by extensions: .aup3, .ai, .aep, .prproj, .mp4, .mov
4. Render table: Path, Name, Type, Open in Box link
5. Add type filter (dropdown or checkboxes)
6. Deploy to GitHub Pages

**Estimate:** 1–2 days

---

## Phase 2: Enhancements

- Path/module filter (m1, m2, …, orientation, Ae, Ai, etc.)
- Dual links (project + preview) for .prproj where render exists
- Size column, sorting
- Layout choice (tabs vs single table vs tree) per UI-SPEC feedback

**Estimate:** 1–2 days

---

## Phase 3: Optional automation

- `build-video-editor-index.mjs` for pre-filtered JSON
- Canvas page mapping (if linking to Canvas pages is required)
- Refresh workflow (re-run map script, commit, push)

---

## Dependencies

- AD-365-V4-map.json (existing)
- Box OAuth (for map refresh)
- GitHub repo (fork of canvas_2879 or new repo)
- GitHub Pages enabled
