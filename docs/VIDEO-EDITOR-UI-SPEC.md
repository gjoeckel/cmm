# Canvas Course Page Video Editor – UI Specification

**Status:** Draft v0.1  
**Depends on:** VIDEO-EDITOR-PROJECT-OVERVIEW.md

---

## 1. UI Changes vs canvas_2879

The canvas_2879 index focuses on DOCX and HTML. The Video Editor adds support for:

| Current (canvas_2879) | Video Editor addition |
|-----------------------|------------------------|
| View DOCX, Edit DOCX | Download / Open in Box (project files) |
| View Canvas, Edit Canvas | Project link, Preview link (where applicable) |
| Page/section rows | File-type column, type-specific actions |
| Single link type | Dual links (project + rendered preview) for video |

---

## 2. File-Type Display

| Type | Icon/badge | Actions |
|------|------------|---------|
| Audio (.aup3) | 🎵 Audio | Open in Box, Download |
| Graphics (.ai) | 🎨 Graphics | Open in Box, Download |
| Video seq (.aep) | 🎬 AE | Open in Box, Download |
| Captioned (.prproj) | 🎞️ Premiere | Open in Box, Download; *Preview* (mp4) if available |
| Rendered (.mp4, .mov) | ▶️ Video | Preview in Box, Download |

---

## 3. Table Columns (Proposed)

| Column | Source | Notes |
|--------|--------|-------|
| Path | Box map `path` | e.g. AD-365-V4/m1/1-1/1-1-1/audio |
| Name | Box map `name` | Filename |
| Type | Derived from extension | Audio, Graphics, AE, Premiere, Video |
| Open in Box | `https://usu.app.box.com/file/{id}` | Primary action |
| Preview | Optional; rendered output | For .prproj → linked .mp4 |
| Size | Box map `size` | Formatted (KB, MB) |

---

## 4. Filters

- **By type:** Audio, Graphics, AE, Premiere, Rendered video
- **By path:** Module (m1–m5), orientation, tool folder (Ae, Ai, Audio, etc.)
- **By extension:** .aup3, .ai, .aep, .prproj, .mp4, .mov

---

## 5. Layout Options

**Option A – Single table:** All files in one table with filters (simpler).  
**Option B – Tabbed:** One tab per type (Audio, Graphics, AE, Premiere, Video).  
**Option C – Tree + detail:** Folder tree (from map) with file list in right pane (matches Box map viewer).

*Feedback: which layout fits your workflow best?*

---

## 6. Responsive / Accessibility

- Reuse canvas_2879 styling (Bootstrap or similar)
- Ensure links work with keyboard and screen readers
- Mobile: collapsible filters, horizontal scroll on table if needed
