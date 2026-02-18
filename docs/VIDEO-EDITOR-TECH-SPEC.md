# Canvas Course Page Video Editor – Technical Specification

**Status:** Draft v0.1  
**Depends on:** VIDEO-EDITOR-PROJECT-OVERVIEW.md

---

## 1. Data Flow

```
AD-365-V4 (Box) → map-box-folder.mjs → AD-365-V4-map.json
                                              ↓
                                    video-editor-index.json (optional)
                                              ↓
                                    Static HTML/JS (GitHub Pages)
```

**Option A:** Consume AD-365-V4-map.json directly; filter by extension in client JS.  
**Option B:** Build script generates video-editor-index.json with pre-filtered items and computed metadata.  
*Choice depends on Q2 in PROJECT-OVERVIEW.*

---

## 2. File-Type Mapping

| Extension | Type label | Box preview? |
|-----------|------------|--------------|
| .aup3 | Audio | No |
| .ai | Graphics | Limited |
| .aep | AE | No |
| .prproj | Premiere | No |
| .mp4, .mov, .webm | Video | Yes |

---

## 3. Project + Preview Linking

For .prproj (Premiere), link to rendered output when available:
- **Convention:** Same folder, same base name, e.g. `lesson.prproj` → `lesson.mp4`
- **Fallback:** No preview link if no matching render

*Feedback: do you have an existing naming convention for renders?*

---

## 4. Build Artifacts

| Artifact | Location | Purpose |
|----------|----------|---------|
| AD-365-V4-map.json | resources/canvas_media_manager | Full Box map (existing) |
| video-editor-index.html | github-pages/ or root | Main Video Editor page |
| video-editor.js | scripts/ or inline | Filter, table render, links |
| video-editor.css | styles/ or inline | Type badges, layout |

---

## 5. Scripts (Optional)

| Script | Purpose |
|--------|---------|
| build-video-editor-index.mjs | Filter map by extensions, emit video-editor-index.json |
| (Reuse) map-box-folder.mjs | Refresh Box map |

---

## 6. GitHub Pages Config

- Same as canvas_2879: static HTML/JS, no server
- Source: `/` or `/docs` or `/github-pages` per repo settings
- CORS: Same-origin for JSON; no cross-origin fetch needed if JSON is in repo
