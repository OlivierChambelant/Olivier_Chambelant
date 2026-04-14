# CLAUDE.md

This file gives AI assistants (Claude Code and similar) the context they need to work effectively in this repository.

## Repository purpose

This is **not a software project**. It is the personal professional portfolio of **Olivier Chambelant** (Directeur TI / Directeur IA, 20 ans d'expérience). The repository is used as a public, link-sharable hub for:

- His CV (French, one canonical PDF + a Markdown mirror)
- Scanned diplomas, certifications, and training attestations (PDF / PNG)
- Academic coursework documenting the **Alfred** AI assistant project (TÉLUQ — *Programme court en IA dans les entreprises*)

There is no build, no test suite, no runtime code. All work in this repo is **documentary**: prose in Markdown, binary credentials in PDF/PNG.

## Repository layout

```
.
├── README.md                       # Top-level landing page (FR) — public face of the repo
├── CV/
│   ├── README.md                   # Full CV in Markdown (mirror of the PDF, kept in sync)
│   └── Olivier_CHAMBELANT.pdf      # Canonical CV
├── diplomes-certifications/
│   ├── README.md                   # Index table of all credentials
│   ├── academiques/                # Diplomas (DUT, Licence, Maîtrise, TÉLUQ, équivalence Québec)
│   ├── certifications/             # Professional certs (CCNP, CCNA Wireless, ECMS2, ITIL4, PRINCE2)
│   └── attestations/               # Training attestations (Harvard Leadership, Cegos, Réalisation)
└── travaux_universitaire/
    ├── README.md                   # Overview of the Alfred project deliverables
    ├── assistant-ia-analyse-documentaire-plan-de-projet.md
    ├── assistant-ia-analyse-documentaire-prototype-et-tests-preliminaires.md
    └── assistant-ia-analyse-documentaire-mvp.md
```

## Content conventions

- **Language**: All prose is in **French**. Keep it that way when editing or adding content. Do not translate existing French content to English unless explicitly asked.
- **Tone**: Professional, factual, first-person where appropriate. The repo represents a senior executive (DSI / Directeur IA).
- **Markdown style**: GitHub-flavored Markdown. Horizontal rules use `-----` (five dashes) in the top-level and `diplomes-certifications` READMEs, and `---` (three dashes) in the CV and `travaux_universitaire` READMEs. Match the style of the file you are editing rather than mixing.
- **Accents**: The top-level `README.md` and `travaux_universitaire/` use full French accents (é, è, à, ç, œ). `CV/README.md` is **intentionally unaccented** (written as plain ASCII: "experience" not "expérience"). Preserve whichever convention the file already uses — do not "fix" the unaccented CV.
- **Tables**: Pipe tables. Columns in `diplomes-certifications/README.md` use narrow spacing; keep alignment as-is.
- **Links**: External links use bare Markdown (`[text](url)`). LinkedIn, GitHub, Substack URLs appear in both the root README and the CV README — if one changes, change the other.

## Keeping CV sources in sync

The CV exists in **two places** that must stay consistent:

1. `CV/Olivier_CHAMBELANT.pdf` — the canonical artifact.
2. `CV/README.md` — a Markdown transcription.

If the user updates one (especially the PDF), flag the other as potentially stale. Do not silently regenerate the PDF — that's out of scope for this repo (no toolchain is committed).

The root `README.md`, `CV/README.md`, and `diplomes-certifications/README.md` all list certifications and formations. When adding or removing a credential, update **all three** plus the appropriate PDF under `diplomes-certifications/`.

## Binary files

- PDFs and images under `diplomes-certifications/` are official scans. **Never edit, re-encode, or rename them without explicit instruction.** Their filenames are sometimes referenced externally.
- Filenames mix conventions (snake_case, Title Case with spaces, dashes). Match the surrounding convention when adding a new file in the same directory; don't mass-rename.

## The Alfred project (`travaux_universitaire/`)

These Markdown files are academic deliverables for a TÉLUQ course. They describe **Alfred**, a documentary-analysis AI assistant built on the **Clawdbot** framework (Claude Sonnet 4.5 via Anthropic API, deployed on a Mac Mini). The repo contains only the written reports — not the Clawdbot source, prompts, or test corpus (those live elsewhere).

When editing these files:
- Preserve the report structure (plan / prototype / MVP-final).
- Results tables and measured detection rates are empirical — do not "round" or rewrite numbers.
- The system prompt described (8 XML blocks) is a factual description of Alfred's design; don't refactor it.

## Git workflow

- Default branch: `main`.
- Feature work happens on branches like `claude/<slug>`; the current working branch for AI-assisted changes is set per-session by the harness.
- Commit messages in history are short, imperative, in English or French (mixed). Match the style — one-line subject, no body needed for doc tweaks.
- **Never force-push** and never rewrite `main`. Open a PR from the working branch.
- After pushing a working branch, open a **draft PR** against `main`.

## What Claude should and shouldn't do here

**Do**
- Fix typos, broken Markdown, inconsistent accents *within a single file*, dead links.
- Add new credentials / experiences when the user provides the source material, updating every README that lists them.
- Help draft new sections (e.g., new job experience, new certification entry) in the existing tone and language.
- Respect the bilingual positioning (France + Québec): currencies are €, dates are French format, but Québec-relevant framing is welcome.

**Don't**
- Don't introduce a build system, package manager, CI, linter, or formatter — this repo deliberately has none.
- Don't translate the repo into English or "modernize" the French.
- Don't generate or replace the CV PDF from the Markdown (or vice versa) without being asked.
- Don't invent credentials, dates, employers, or metrics. Every fact here is the user's real professional record.
- Don't commit private contact details beyond what is already public (email and phone already appear in `CV/README.md` — that's the user's deliberate choice).

## Contact surface (already public in this repo)

- Email: `olivier.chambelant@gmail.com`
- Phone: `+1 (819) 489-1233`
- LinkedIn: `linkedin.com/in/olivier-chambelant`
- Substack: `olivierchambelant.substack.com`
- GitHub: `github.com/OlivierChambelant/Olivier_Chambelant`
