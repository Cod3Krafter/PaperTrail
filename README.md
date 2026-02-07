# PaperTrail

**Internal document comparison tool** — upload two documents, instantly detect changes, and visualize every difference.

## Why

A tampered contract was sent to our legal team disguised as the agreed-upon version. PaperTrail exists so any employee can verify document integrity in seconds.

## How It Works

1. **Upload** two documents (PDF, DOCX, or TXT)
2. **Hash check** — SHA-256 hashes are compared first. If they match, the files are byte-for-byte identical. Done.
3. **Diff view** — If hashes differ, extracted text is compared word-by-word. Additions and deletions are highlighted clearly.
4. **History** — Every comparison is saved for future reference and audit.

## Project Status

**Pre-development** — Repository and design documents are being set up. See `docs/` for the UI design document.

## Documentation

- [`docs/UI_DESIGN_DOCUMENT.md`](docs/UI_DESIGN_DOCUMENT.md) — Full UI/UX design spec for the designer

## Planned Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React, TypeScript, Vite |
| Backend | Node.js, Express, TypeScript |
| Database | SQLite (via better-sqlite3) |
| File parsing | pdf-parse, mammoth (DOCX), plain fs (TXT) |
| Diffing | SHA-256 hashing + word-level text diff |
