# Ronix Tool Hub

**One sign-in for a branding team's production work.** Eight print and content tools I built from scratch, the platform they run in, and the sign-in for the company's other internal apps.

| | |
|---|---|
| **Role** | Sole builder: brief, design, code, deployment, documentation |
| **When** | August – October 2026 |
| **Stack** | Next.js 16, React 19, TypeScript, Tailwind 4, SQLite (better-sqlite3), sharp, nginx, systemd |
| **Size** | ~48,000 lines of TypeScript · 143 commits · 49 merged pull requests |
| **Status** | In production on the company's Linux server, used daily by the branding team |

> The source is private company code at Ronix Tools. I'm glad to walk through it in an interview. → [Portfolio](https://arvinabedi.github.io)

## The problem

Product cards, carton labels, QR codes, catalog sheets, infographics and landing pages were made by hand in Illustrator, one at a time, by whoever in the branding team had time. Every request queued behind a designer, every output looked slightly different, and nothing was logged or repeatable.

## What I built

**Eight tools**, each replacing a manual design job:

| Tool | Replaces |
|---|---|
| Card Generator (Persian) | Hand-typeset dealer cards, with correct Persian shaping and letter spacing in the exported JPG |
| Card Generator (English) | The two-sided international card, QR included |
| Label Generator | Carton labels from a spreadsheet; every position measured from the approved Illustrator templates, in millimetres |
| QR Code Generator | One-off QR exports for packaging |
| One-page Catalog Designer | Retyping specs off the website: it syncs a product range from the live site, converts images to 300-dpi TIFF, and blocks printing on bad data |
| Landing Page Editor | Hand-built product landing pages; keeps every version and lets a new file be uploaded onto an old one |
| Infographic Designer | Product infographics: guides with snapping, 115° leader lines, an icon library, movable and resizable copy |
| Infographic Translator | Rebuilding the same infographic in another language, spec values included |

**The platform around them:**
- One login with owner, admin and user roles, plus per-person access to each tool and app
- Invite-only onboarding: the first visit to `/setup` creates the one admin, then the page closes for good
- Searchable run history, so any output can be found and made again
- Every PDF written in CMYK for the printer
- **Single sign-on for the other internal apps.** Content OS, PDMS and the DAM all sign in through Tool Hub, and PDMS roles are assigned from it.

## Decisions worth explaining

**The server pulls; nothing pushes in.** The company firewall blocks port 22 and there is no CI runner. A cron job on the server checks `main` every two minutes over SSH on port 443, takes a lock, builds, and restarts the service. A push goes live in about two minutes, and two deploys can never overlap.

**The database lives outside the app folder.** A deploy replaces the whole app without touching a single account.

**The catalog tool refuses to guess.** Images that go soft past 65 mm, products with no spec table, and labels misspelled on the website come back as a list a person decides on. A wrong value never reaches the printer.

**Built for an IT team with no Node experience.** One server, one SQLite file, an idempotent setup script with self-checks, and an install manual a non-technical admin can finish in thirty minutes.

---

[Portfolio](https://arvinabedi.github.io) · [All case studies](https://github.com/arvinabedi)
