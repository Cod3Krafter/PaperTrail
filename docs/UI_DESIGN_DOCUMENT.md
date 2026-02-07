# PaperTrail — UI Design Document

> **Version:** 0.1
> **Date:** 2026-02-07
> **Audience:** UI/UX Designer
> **Status:** Draft

---

## 1. Product Overview

### Problem
A legal team received a tampered contract — an edited version disguised to look identical to the previously agreed-upon document. There was no quick way to verify whether a document had been altered or to see exactly what changed.

### Solution
**PaperTrail** is an internal web tool that lets employees compare two documents side by side, instantly detect whether they differ (via file hashing), and then clearly visualize every difference. It also keeps a searchable history of all past comparisons for audit purposes.

### Supported File Types
- PDF (`.pdf`)
- Word documents (`.docx`)
- Plain text (`.txt`)

---

## 2. User Personas

| Persona | Description | Primary Goal |
|---------|-------------|--------------|
| **Legal Associate** | Reviews contracts and agreements daily | Quickly verify a received document matches the agreed version |
| **Compliance Officer** | Audits document integrity across teams | Browse comparison history to spot patterns of tampering |
| **General Employee** | Occasionally needs to compare document revisions | Simple drag-and-drop comparison without training |

---

## 3. Information Architecture

```
PaperTrail
├── Compare (home / default view)
│   ├── Upload Panel A (Original Document)
│   ├── Upload Panel B (Document to Verify)
│   ├── Hash Status Indicator
│   └── Diff Results View
├── History
│   ├── Search / Filter Bar
│   └── Comparison List (paginated)
└── Result Detail (permalink for a past comparison)
    ├── Document Metadata
    ├── Hash Status
    └── Diff Results View
```

---

## 4. Page-by-Page Specifications

### 4.1 Compare Page (Home — `/`)

This is the primary screen. It should feel immediate and approachable — the user's entire workflow lives here.

#### Layout
- **Top bar:** App logo ("PaperTrail") on the left, navigation links (Compare, History) on the right.
- **Main area:** Two upload zones placed side by side horizontally on desktop, stacked vertically on mobile.
- **Bottom area:** Results section (hidden until a comparison is run).

#### Upload Zones

Each upload zone (Document A / Document B) should contain:

| Element | Detail |
|---------|--------|
| **Drop area** | Large rectangular zone with a dashed border. Centered icon (document icon with an up-arrow). |
| **Label** | Document A: "Original Document" / Document B: "Document to Verify" |
| **Hint text** | "Drag & drop a file here, or click to browse" |
| **Accepted formats badge** | Small pill badges: `PDF` `DOCX` `TXT` |
| **File size limit** | "Max 20 MB" in muted text |
| **Uploaded state** | Replace the drop area content with: file icon, file name (truncated if long), file size, a green checkmark, and a small "Remove" (x) button to clear and re-upload |

#### Compare Button

- Centered below both upload zones.
- **Label:** "Compare Documents"
- **State:** Disabled (greyed out) until both documents are uploaded. Enabled with a primary-color fill when ready.
- **Loading state:** Spinner icon replaces text, button disabled during processing.

#### Hash Status Indicator

Appears immediately after comparison completes, above the diff results.

| Scenario | Visual |
|----------|--------|
| **Hashes match** | Green banner: checkmark icon + "Documents are identical — file hashes match." No diff view shown below. |
| **Hashes differ** | Amber/orange banner: warning icon + "Documents differ — differences are highlighted below." Diff view appears below. |

#### Diff Results View

Shown only when hashes differ. Two possible display modes the user can toggle between:

**A. Unified View (default)**
- A single scrollable text panel.
- Unchanged text in default (dark) color.
- **Added text** (present in Document B but not A): highlighted with a green background, green left border.
- **Removed text** (present in Document A but not B): highlighted with a red background, red left border, strikethrough text.
- Line numbers displayed in a left gutter.

**B. Side-by-Side View**
- Two scrollable panels, synced scrolling.
- Left panel: Document A. Removed sections highlighted in red.
- Right panel: Document B. Added sections highlighted in green.
- A thin vertical divider between the panels.

**Toggle control:** A small segmented button above the diff view: `[Unified | Side-by-Side]`

**Summary bar** (above the diff content):
- "X additions, Y deletions" with colored counts (green/red).
- Percentage: "~Z% of content changed"

---

### 4.2 History Page (`/history`)

A clean table/list showing all past comparisons.

#### Layout
- **Top bar:** Same as Compare page.
- **Filter bar:** Search input ("Search by file name...") + date range picker + status filter dropdown ("All", "Identical", "Different").
- **Table/List:**

| Column | Description |
|--------|-------------|
| **Date** | When the comparison was run (e.g., "Feb 7, 2026 at 2:15 PM") |
| **Document A** | File name of the original |
| **Document B** | File name of the compared document |
| **Status** | Badge: green "Identical" or amber "Different" |
| **Action** | "View" link → navigates to `/result/:id` |

- **Pagination:** Bottom of table. "Showing 1-20 of 154 comparisons". Previous / Next buttons, page number links.
- **Empty state:** If no comparisons yet, show an illustration with text: "No comparisons yet. Go to Compare to get started." with a link/button to the Compare page.

---

### 4.3 Result Detail Page (`/result/:id`)

A permalink view of a previously run comparison. Identical layout to the results section on the Compare page, but with added metadata.

#### Metadata Header
- **Compared on:** date/time
- **Document A:** file name, size, hash (truncated with copy button)
- **Document B:** file name, size, hash (truncated with copy button)
- **Status:** Identical / Different badge

#### Diff View
- Same Unified / Side-by-Side toggle and diff rendering as on the Compare page (reuse the same component).

#### Actions
- "New Comparison" button → navigates back to `/`
- "Back to History" link

---

## 5. Visual Design Guidelines

### Color Palette

| Role | Color | Usage |
|------|-------|-------|
| **Primary** | `#2563EB` (Blue 600) | Buttons, active nav links, focus rings |
| **Primary Hover** | `#1D4ED8` (Blue 700) | Button hover state |
| **Success / Identical** | `#16A34A` (Green 600) | Identical badge, hash-match banner, added-text highlight bg |
| **Success Background** | `#F0FDF4` (Green 50) | Added-text highlight background |
| **Warning / Different** | `#D97706` (Amber 600) | Different badge, hash-mismatch banner |
| **Danger / Removed** | `#DC2626` (Red 600) | Removed-text highlight, deletion count |
| **Danger Background** | `#FEF2F2` (Red 50) | Removed-text highlight background |
| **Neutral Background** | `#F9FAFB` (Gray 50) | Page background |
| **Card Background** | `#FFFFFF` | Cards, panels, upload zones |
| **Border** | `#E5E7EB` (Gray 200) | Card borders, dividers |
| **Text Primary** | `#111827` (Gray 900) | Headings, body text |
| **Text Secondary** | `#6B7280` (Gray 500) | Hint text, timestamps, secondary labels |

### Typography

| Element | Font | Weight | Size |
|---------|------|--------|------|
| **App title** | Inter | 700 | 20px |
| **Page heading** | Inter | 600 | 24px |
| **Section heading** | Inter | 600 | 18px |
| **Body text** | Inter | 400 | 14px |
| **Diff text** | `monospace` (system) | 400 | 13px |
| **Small/caption** | Inter | 400 | 12px |
| **Button label** | Inter | 500 | 14px |

### Spacing & Layout

- **Page max-width:** 1200px, centered.
- **Card padding:** 24px.
- **Grid gap between upload zones:** 24px.
- **Border radius:** 8px for cards, 6px for buttons, 4px for badges.
- **Shadows:** Subtle `0 1px 3px rgba(0,0,0,0.1)` on cards.

### Responsive Breakpoints

| Breakpoint | Behavior |
|------------|----------|
| **>= 1024px** (Desktop) | Upload zones side by side. Side-by-side diff available. |
| **768–1023px** (Tablet) | Upload zones side by side (narrower). Side-by-side diff still works but tighter. |
| **< 768px** (Mobile) | Upload zones stacked vertically. Unified diff only (side-by-side toggle hidden). |

---

## 6. Component Inventory

These are the reusable UI components the designer should create:

| Component | Variants / States |
|-----------|-------------------|
| **TopBar** | Logo, nav links (active/inactive) |
| **UploadZone** | Empty, drag-hover, file-uploaded, error |
| **PrimaryButton** | Default, hover, disabled, loading |
| **HashStatusBanner** | Identical (green), Different (amber) |
| **DiffViewer** | Unified mode, Side-by-side mode |
| **DiffSegment** | Added (green bg), Removed (red bg + strikethrough), Unchanged |
| **ViewToggle** | Segmented control: Unified / Side-by-Side |
| **DiffSummaryBar** | Addition count, deletion count, change percentage |
| **HistoryTable** | With rows, empty state |
| **HistoryRow** | Date, doc names, status badge, view link |
| **StatusBadge** | "Identical" (green), "Different" (amber) |
| **Pagination** | Page numbers, prev/next, item count |
| **SearchBar** | Text input with search icon |
| **FilterDropdown** | Single-select dropdown |
| **DateRangePicker** | Start/end date inputs |
| **MetadataHeader** | Document names, sizes, hashes, date |
| **HashDisplay** | Truncated hash string + copy-to-clipboard button |
| **EmptyState** | Illustration + message + CTA button |
| **FileTypeBadge** | Small pills: PDF, DOCX, TXT |

---

## 7. Interaction & Animation Notes

| Interaction | Behavior |
|-------------|----------|
| **Drag over upload zone** | Border changes from dashed gray to solid primary blue. Background subtly tints to blue-50. |
| **File drop** | Brief scale-down "catch" animation (100ms). Transition to uploaded state. |
| **Compare button click** | Button shows spinner. A skeleton/shimmer placeholder appears in the results area. |
| **Hash result appears** | Banner slides down smoothly (200ms ease-out). |
| **Diff content loads** | Fade-in (150ms). |
| **View toggle** | Crossfade between unified and side-by-side (200ms). |
| **History row hover** | Row background subtly highlights to gray-50. |
| **Copy hash** | Tooltip changes from "Copy" to "Copied!" for 2 seconds. |

---

## 8. Accessibility Requirements

- All interactive elements must be keyboard-navigable (tab order, enter/space to activate).
- Upload zones must be operable via keyboard (Enter/Space opens file picker).
- Color is never the only indicator — always pair with icons and/or text labels (e.g., the status badge says "Identical" in addition to being green).
- Diff highlights use both color AND a left border + text decoration (strikethrough for removals) so they remain distinguishable without color vision.
- Minimum contrast ratio: 4.5:1 for normal text, 3:1 for large text.
- Screen reader announcements for: file upload success, comparison result (identical/different), and diff summary.
- `aria-live="polite"` on the results area so screen readers announce when results appear.

---

## 9. User Flow Diagram

```
┌──────────────────────────────────────────────────────────┐
│                     COMPARE PAGE                         │
│                                                          │
│  ┌─────────────────┐       ┌─────────────────┐          │
│  │                 │       │                 │          │
│  │   Drop Zone A   │       │   Drop Zone B   │          │
│  │   (Original)    │       │   (To Verify)   │          │
│  │                 │       │                 │          │
│  └────────┬────────┘       └────────┬────────┘          │
│           │                         │                    │
│           └──────────┬──────────────┘                    │
│                      ▼                                   │
│            ┌─────────────────┐                           │
│            │    Compare      │                           │
│            │    Documents    │                           │
│            └────────┬────────┘                           │
│                     │                                    │
│                     ▼                                    │
│         ┌──── Hash Check ────┐                           │
│         │                    │                           │
│    Match ▼              No Match ▼                       │
│  ┌──────────────┐    ┌───────────────┐                   │
│  │  GREEN        │    │  AMBER         │                  │
│  │  "Identical"  │    │  "Different"   │                  │
│  │  banner       │    │  banner        │                  │
│  └──────────────┘    └───────┬───────┘                   │
│                              │                           │
│                              ▼                           │
│                    ┌──────────────────┐                   │
│                    │   Diff Viewer    │                   │
│                    │ (Unified or      │                   │
│                    │  Side-by-Side)   │                   │
│                    └──────────────────┘                   │
│                                                          │
│         ┌─ Saved to History automatically ─┐             │
└─────────┴──────────────────────────────────┴─────────────┘

┌──────────────────────────────────────────────────────────┐
│                    HISTORY PAGE                           │
│                                                          │
│  ┌─ Search ──┐  ┌─ Date Range ─┐  ┌─ Status Filter ─┐  │
│  └───────────┘  └──────────────┘  └─────────────────┘  │
│                                                          │
│  ┌────────────────────────────────────────────────────┐  │
│  │  Date  │  Doc A  │  Doc B  │  Status  │  Action   │  │
│  ├────────┼─────────┼─────────┼──────────┼───────────┤  │
│  │  Feb 7 │ v1.pdf  │ v2.pdf  │Different │  View →   │  │
│  │  Feb 6 │ a.docx  │ b.docx  │Identical │  View →   │  │
│  │  ...   │  ...    │  ...    │  ...     │  ...      │  │
│  └────────────────────────────────────────────────────┘  │
│                                                          │
│              ‹ 1  2  3  ...  8 ›                         │
└──────────────────────────────────────────────────────────┘
```

---

## 10. Edge Cases & Empty States

| Scenario | Behavior |
|----------|----------|
| **Unsupported file type dropped** | Upload zone border turns red. Error message below: "Unsupported format. Please use PDF, DOCX, or TXT." |
| **File exceeds 20 MB** | Same red border. Message: "File too large. Maximum size is 20 MB." |
| **Same file uploaded to both slots** | Allow it — comparison runs normally and shows "Identical" result. |
| **One slot empty, Compare clicked** | Button is disabled; this shouldn't happen. But if it does, show a toast: "Please upload both documents." |
| **Comparison fails (server error)** | Red banner in results area: "Something went wrong. Please try again." with a retry button. |
| **No history entries** | Friendly empty state with illustration, text, and CTA (see Component Inventory). |
| **Very large diff output** | Virtualized/windowed rendering — only visible diff segments are in the DOM. A "Jump to next change" floating button helps navigate. |
| **Document with no extractable text** | Warning banner: "Could not extract text from this document. The file may be image-based." Suggest using an OCR-capable version. |

---

## 11. Future Considerations (Out of Scope for v1)

These are not needed now but the design should not paint us into a corner:

- **Multi-document comparison** (comparing more than 2 documents)
- **Folder/batch upload** (compare all files in a folder)
- **User accounts & permissions** (currently no auth needed — internal tool on internal network)
- **Comments & annotations** on diffs
- **Export diff report** as PDF
- **OCR support** for scanned/image-based PDFs
- **Webhook/email notifications** when a comparison detects differences

---

## 12. Technical Context for Designer

The designer does **not** need to worry about implementation, but this context helps make informed decisions:

- **Stack:** React + TypeScript frontend, Node.js backend
- **Diff engine:** Word-level diffing (not character-level, not line-level). Each diff segment is a chunk of words marked as `added`, `removed`, or `unchanged`.
- **Hash algorithm:** SHA-256 on the raw file bytes. The hash comparison happens server-side before any text diffing.
- **File parsing:** Text is extracted server-side from PDF/DOCX/TXT. The frontend works with plain text diffs, not rendered document layouts.
- **History:** Stored in a database. Every comparison is auto-saved. No manual save action needed.

---

## Deliverables Requested from Designer

1. **Figma file** with all pages (Compare, History, Result Detail) in desktop and mobile breakpoints.
2. **Component library** in Figma matching the Component Inventory table above.
3. **Prototype** with clickable flow: upload → compare → view result → navigate to history → view past result.
4. **Asset exports** if any custom icons or illustrations are created (SVG preferred).
