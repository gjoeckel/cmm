# Frame.io API – Leveraging in Canvas Course Video Manager Context

**Created:** 2026-02-18  
**Updated:** 2026-02-18 (validated research, best practices)  
**Purpose:** Document Frame.io API capabilities and best practices for the Canvas Course Video Manager (Box-based media index)

**Source research:** [frameio_premiere_workflow.md](./frameio_premiere_workflow.md) — validated via web research.

---

## 1. Frame.io Overview

Frame.io is a **video collaboration platform** (now part of Adobe) for review, approval, and feedback on video content. It supports:

- Time-stamped comments and annotations
- Integration with Premiere Pro, After Effects, DaVinci Resolve, Final Cut Pro
- Review Links (single URL to share assets for feedback)
- Version stacks and transcoding
- Webhooks for workflow automation

**Key difference from Box:** Frame.io is built for video review/collaboration; Box is general file storage. They serve different purposes and can be used together.

---

## 2. API Structure

**Hierarchy:** Accounts → Teams → Projects → Assets → Comments

- **root_asset_id:** Each Project has a unique root asset; the file tree lives beneath it
- **REST API:** `https://api.frame.io/v2/...` over HTTPS
- **Authentication:** Developer tokens (single-user) or OAuth 2.0 (multi-user)
- **Scopes:** Assets (create/read), Projects (read), Review Links (create/read/update), Webhooks, Comments

**Documentation:** [developer.frame.io](https://developer.frame.io/docs)

---

## 3. Relevant API Capabilities

### 3.1 Reading the File Tree

- `GET /v2/accounts` → user accounts
- `GET /v2/accounts/:id/teams` → teams
- `GET /v2/teams/:id/projects` → projects (each has `root_asset_id`)
- `GET /v2/assets/:id/children?type=folder|file|version_stack` → list folder contents

**Use case:** Build a directory-style index (similar to our Box map) for Frame.io projects. Could mirror AD-365-V4 structure in Frame.io if assets are synced.

### 3.2 Uploading Assets

- Create asset: `POST /v2/assets/:folder_id/children` with type, name, filesize, filetype
- Receive upload URLs for direct PUT
- Supports version stacks for multiple versions of same asset

**Use case:** Programmatically upload rendered videos (mp4) or project exports to Frame.io for review. Bridge: Box → Frame.io for review workflow.

### 3.3 Review Links

- `POST /v2/projects/:id/review_links` → create Review Link
- `POST /v2/review_links/:id/assets` → add assets to link
- Review Links provide a short URL (e.g. `https://f.io/_aBcDeF`) for external reviewers without project access

**Use case:** For each lesson/activity (e.g. 1-1-1), create a Review Link that includes the relevant video(s). Share with reviewers (instructors, TAs, accessibility auditors) for feedback. Options: `allow_approvals`, `enable_downloading`, `expires_at`.

### 3.4 Comments

- `GET /assets/:id/comments`
- `POST /assets/:id/comments` (time-stamped feedback)

**Use case:** Collect structured feedback on video content. Could surface comment counts or status in the Video Manager UI.

### 3.5 Webhooks

Events: `asset.created`, `asset.ready`, `asset.versioned`, `reviewlink.created`, comment events, etc. Payloads are JSON with resource, user, team.

**Use case:** When a new version is uploaded or approved in Frame.io, trigger sync or notifications. E.g. update Video Manager when `asset.versioned` fires.

---

## 4. How Frame.io Could Complement the Current Setup

| Current (Box) | Frame.io addition |
|---------------|-------------------|
| Storage for .prproj, .aep, .aup3, .ai | Review workflow for rendered .mp4/.mov |
| Open in Box links | Review Links for each activity/lesson |
| No in-browser playback for project files | In-browser playback and time-stamped comments for video |
| Static index | Dynamic status (e.g. “Under review”, “Approved”) from Frame.io |

### Suggested Model: Box + Frame.io

- **Box:** Primary storage for project files (.prproj, .aep, etc.) and raw assets
- **Frame.io:** Review and approval for rendered video outputs
- **Video Manager UI:** Add a “Review” column that links to Frame.io Review Links (when available)

---

## 5. Integration Options

### Option A: Add Frame.io Review Links to Video Manager

1. Create Frame.io projects (or one project) mirroring module/section/activity structure
2. Upload rendered mp4/mov to Frame.io
3. Create Review Links per activity (e.g. 1-1-1)
4. Store Review Link URLs in metadata (JSON or config)
5. Video Manager displays “Review” link next to “Open in Box”

**Effort:** Medium. Requires Frame.io account, API token, and a script to create projects/assets/Review Links.

### Option B: Sync Box → Frame.io

1. Use Box API to detect new/updated .mp4 in AD-365-V4
2. Use Frame.io API to upload those files and create/update Review Links
3. Webhook or scheduled job to keep Frame.io in sync

**Effort:** Higher. Needs mapping (Box path ↔ Frame.io project/folder) and automation.

### Option C: Frame.io as Primary for Rendered Video

1. Change workflow: export from Premiere → upload to Frame.io (instead of or in addition to Box)
2. Video Manager reads from both Box (project files) and Frame.io API (rendered video + Review Links)

**Effort:** High. Shifts workflow; requires Frame.io adoption by editors.

---

## 6. Premiere Pro / Frame.io Workflow Best Practices (Validated)

*Source: frameio_premiere_workflow.md; validated via Adobe/Frame.io docs and support.*

### 6.1 Premiere Pro 25.6+ and Frame.io V4

- **Panel:** `Window > Frame.io > Frame.io V4` — full panel (browse projects, import media, export/share sequences, comment markers)
- **Export & Share:** Renders via Media Encoder and uploads to Frame.io; shareable link generated
- **New Version:** One-click re-upload; versions stack; same link continues to work
- **Comment markers:** Comments appear as timeline markers; playhead jumps to timecode
- **Requirement:** Premiere Pro 25.6 or later for full V4 experience

### 6.2 Subscription (Creative Cloud)

| Tier | Cost | Notes |
|------|------|-------|
| **Included with CC All Apps** | No extra cost | 2 users, 5 projects, 100GB — sufficient for pilot |
| **Pro** | $15/user/mo | Unlimited projects, 2TB, branded shares, passphrase |
| **Team** | $25/user/mo | Up to 15 users, internal comments, restricted projects |

**Reviewers via Share links are free and unlimited** — use Share links for instructors/TAs/accessibility reviewers; avoid project invites for external feedback.

### 6.3 Frame.io vs Team Projects

- **Team Projects:** Simultaneous co-editing of the same .prproj; project file in cloud; conflict resolution
- **Frame.io:** Review and approval of rendered outputs; no project-file editing; async feedback loop
- **Best practice:** Use Team Projects or Productions for editing; Frame.io for review and delivery. Complementary, not competing.

### 6.4 Workflow Constraints

- **Render-and-upload:** Every review cycle requires render + upload (no real-time project sync)
- **Structure:** Mirror Premiere bin structure in Frame.io folders — V4 preserves folder structure on import
- **External stakeholders:** Keep them on Share/Review links only; reserve paid seats for uploaders/organizers

---

## 7. Prerequisites

- **Frame.io account:** Included with CC All Apps (2 users, 5 projects, 100GB) for pilot; Pro/Team for production
- **Developer token:** From [developer.frame.io](https://developer.frame.io/) (scopes: Assets, Projects, Review Links)
- **Premiere Pro 25.6+** for full Frame.io V4 panel

---

## 8. Recommended Next Steps (If Adopting)

1. **Pilot:** Create one Frame.io project, upload a few sample mp4s, create a Review Link, and add a “Review” link in Video Manager
2. **Data model:** Define how Review Links map to activity paths (e.g. `1-1-1` → Frame.io project/asset ID or Review Link URL)
3. **Script:** `create-frameio-review-links.mjs` – given Box map + config, create/update Frame.io assets and Review Links
4. **UI:** Add “Review in Frame.io” column to Video Manager for assets that have a Review Link

---

## 9. References

- [frameio_premiere_workflow.md](./frameio_premiere_workflow.md) — Premiere/Team Projects research (validated)
- [Frame.io API Docs](https://developer.frame.io/docs)
- [Reading the File Tree](https://developer.frame.io/docs/workflows-assets/reading-the-file-tree)
- [Uploading Assets](https://developer.frame.io/docs/workflows-assets/uploading-assets)
- [Working with Review Links](https://developer.frame.io/docs/workflows-projects/working-with-review-links)
- [Webhooks Overview](https://developer.frame.io/docs/automations-webhooks/webhooks-overview)
- [Adobe Frame.io API (Adobe Developer)](https://developer.adobe.com/frameio)
