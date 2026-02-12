# Plan: Adapt Workflow for Economics Dissertation Project

**Status:** COMPLETED
**Date:** 2026-02-12
**Session:** claude/economics-dissertation-work-clJ1K

---

## Context

The user is starting work on their economics PhD dissertation. The current repository is configured for creating lecture slides (Beamer/LaTeX + Quarto + R). The user needs it adapted for dissertation research involving:

- **Data analysis:** Stata (primary tool)
- **Numerical methods:** Fortran and Matlab for solving models
- **Writing:** LaTeX (primary) and Word
- **Output:** Publication-ready figures, tables, and manuscripts

The user wants:
1. Structured, precise, rigorous collaboration
2. Publication-ready visuals
3. Plan-first workflow for non-trivial tasks
4. Contractor mode autonomy after plan approval
5. Smart memory of decisions and corrections

---

## Objectives

1. Update CLAUDE.md with dissertation project details
2. Adapt folder structure for dissertation workflow
3. Create/adapt rules for Stata, Fortran, Matlab conventions
4. Modify skills: remove slide-specific, add dissertation-specific
5. Adapt agents: manuscript review instead of slide review
6. Update quality gates for dissertation context
7. Configure workflows for paper writing, numerical analysis, data work
8. Preserve the core plan-first + contractor mode + multi-agent patterns

---

## Approach

### Phase 1: Update Core Configuration (CLAUDE.md)

**File:** `CLAUDE.md`

**Changes:**
- Replace placeholders: project name, institution, branch
- Update folder structure section for dissertation layout
- Replace Beamer/Quarto commands with Stata/Fortran/Matlab/LaTeX commands
- Remove slide-specific sections (Beamer environments, Quarto CSS)
- Add dissertation-specific sections:
  - Chapter/paper tracking table
  - Tool-specific conventions summary
  - Common workflows (replication, numerical simulation, paper drafting)

**New structure proposal:**
```
dissertation/
├── CLAUDE.md
├── .claude/
├── Bibliography.bib
├── Figures/
├── Tables/
├── Papers/               # Individual papers/chapters
│   ├── Paper1/
│   │   ├── manuscript.tex
│   │   ├── manuscript.docx
│   │   └── figures/
│   └── Paper2/
├── Code/                 # All analysis code
│   ├── Stata/           # Data analysis scripts
│   ├── Fortran/         # Numerical solutions
│   ├── Matlab/          # Numerical solutions
│   └── R/               # Publication figures (if needed)
├── Data/
│   ├── Raw/
│   ├── Cleaned/
│   └── Results/         # RDS, CSVs for tables/figures
├── Templates/           # LaTeX templates
├── Preambles/           # LaTeX headers
├── explorations/        # Research sandbox
├── quality_reports/
└── scripts/             # Utility scripts
```

### Phase 2: Adapt Rules

**Keep (with minor edits):**
- `plan-first-workflow.md` — core workflow
- `orchestrator-protocol.md` — contractor mode
- `session-logging.md` — logging protocol
- `quality-gates.md` — scoring system (adapt thresholds)
- `verification-protocol.md` — task completion checks
- `single-source-of-truth.md` — no duplication principle
- `exploration-folder-protocol.md` — research sandbox
- `exploration-fast-track.md` — rapid prototyping
- `orchestrator-research.md` — simplified research orchestrator
- `replication-protocol.md` — replicate before extending
- `pdf-processing.md` — safe PDF handling

**Remove/archive:**
- `beamer-quarto-sync.md` — slides-specific
- `no-pause-beamer.md` — slides-specific
- `tikz-visual-quality.md` — slides-specific
- `proofreading-protocol.md` — keep but adapt for manuscripts

**Create new:**
- `stata-conventions.md` — Stata coding standards, reproducibility
- `fortran-conventions.md` — Fortran coding standards
- `matlab-conventions.md` — Matlab coding standards
- `manuscript-quality.md` — LaTeX manuscript standards
- `numerical-methods-verification.md` — Verify numerical solutions
- `data-provenance.md` — Track data sources and transformations
- `table-figure-standards.md` — Publication-ready output standards

**Adapt:**
- `r-code-conventions.md` → keep but lower priority (Stata is primary)

### Phase 3: Adapt/Create Skills

**Remove (slide-specific):**
- `/compile-latex` — adapt for papers not slides
- `/deploy` — remove (no GitHub Pages for papers)
- `/extract-tikz` — likely keep for diagrams
- `/proofread` — adapt for manuscripts
- `/visual-audit` — adapt or remove
- `/pedagogy-review` — remove (teaching-specific)
- `/qa-quarto` — remove
- `/slide-excellence` — remove
- `/translate-to-quarto` — remove
- `/create-lecture` — remove

**Keep/adapt:**
- `/validate-bib` — keep
- `/devils-advocate` — keep
- `/commit` — keep
- `/lit-review` — keep
- `/research-ideation` — keep
- `/interview-me` — keep
- `/review-paper` — keep (this is perfect!)
- `/data-analysis` — adapt for Stata

**Create new:**
- `/compile-paper [chapter]` — Full LaTeX compilation for manuscripts
- `/run-stata [script]` — Execute Stata do-files with logging
- `/run-fortran [program]` — Compile and run Fortran
- `/run-matlab [script]` — Execute Matlab scripts
- `/check-replication [script]` — Verify code reproduces results
- `/numerical-verification [program]` — Verify numerical solver accuracy
- `/export-to-word [tex-file]` — Convert LaTeX to Word via pandoc
- `/manuscript-review [paper]` — Multi-agent manuscript review
- `/table-to-latex [data-file]` — Generate publication tables
- `/figure-polish [figure]` — Publication-ready figure review

### Phase 4: Adapt Agents

**Remove:**
- `pedagogy-reviewer` — teaching-specific
- `slide-auditor` — slides-specific
- `beamer-translator` — slides-specific
- `quarto-critic` — slides-specific
- `quarto-fixer` — slides-specific
- `tikz-reviewer` — keep if using TikZ diagrams

**Keep:**
- `proofreader` — adapt for manuscripts
- `verifier` — keep
- `domain-reviewer` — customize for economics

**Create new:**
- `stata-reviewer.md` — Stata code quality and correctness
- `fortran-reviewer.md` — Fortran numerical code review
- `matlab-reviewer.md` — Matlab code review
- `manuscript-reviewer.md` — Paper structure, argumentation, clarity
- `econometrics-reviewer.md` — Methods correctness, specification
- `numerical-reviewer.md` — Numerical methods correctness
- `table-reviewer.md` — Table clarity and formatting
- `figure-reviewer.md` — Figure quality and clarity

### Phase 5: Update Templates

**Adapt:**
- `session-log.md` — keep structure, update examples
- `quality-report.md` — keep structure, update rubric
- `exploration-readme.md` — keep

**Create new:**
- `paper-outline.md` — Manuscript structure template
- `replication-checklist.md` — Code replication verification
- `numerical-verification-log.md` — Numerical accuracy checks

### Phase 6: Update MEMORY.md Framework

Currently empty. Add structure for:
- Tool-specific corrections (Stata, Fortran, Matlab)
- Econometric method preferences
- Numerical solver configurations
- Figure/table style preferences
- Writing conventions

### Phase 7: Create Initial Documentation

**Update README.md:**
- Change description from "lecture slides" to "dissertation research"
- Update prerequisites (Stata, Fortran, Matlab instead of Quarto)
- Update quick start for dissertation context
- Update examples

**Create dissertation-specific guides:**
- Quick reference for common workflows
- Tool setup instructions

---

## Files to Modify

### Core Configuration
1. `CLAUDE.md` — complete rewrite for dissertation
2. `README.md` — update description and examples
3. `MEMORY.md` — add structure

### Rules (in `.claude/rules/`)
4. Remove: `beamer-quarto-sync.md`, `no-pause-beamer.md`, `tikz-visual-quality.md`
5. Create: `stata-conventions.md`, `fortran-conventions.md`, `matlab-conventions.md`
6. Create: `manuscript-quality.md`, `numerical-methods-verification.md`
7. Create: `data-provenance.md`, `table-figure-standards.md`
8. Adapt: `quality-gates.md` (update thresholds for dissertation context)

### Skills (in `.claude/skills/`)
9. Remove slide-specific skills (list in Phase 3)
10. Create new skills (list in Phase 3)

### Agents (in `.claude/agents/`)
11. Remove slide-specific agents (list in Phase 4)
12. Create new agents (list in Phase 4)

### Templates
13. Create: `paper-outline.md`, `replication-checklist.md`, `numerical-verification-log.md`

### Directory Structure
14. Create new folder structure as needed

---

## Verification Steps

After implementation:

1. **Folder structure created:**
   - All dissertation-specific folders exist
   - No slide-specific folders remain prominent

2. **CLAUDE.md is complete:**
   - Project name, institution filled in
   - Commands section has Stata/Fortran/Matlab/LaTeX
   - Folder structure reflects dissertation layout
   - No slide-specific content remains

3. **Rules are appropriate:**
   - Stata, Fortran, Matlab convention files exist
   - Manuscript quality standards defined
   - Slide-specific rules removed or archived

4. **Skills match use case:**
   - Dissertation workflows available
   - Slide-specific skills removed
   - Tool-specific execution skills present

5. **Agents match use case:**
   - Code reviewers for Stata, Fortran, Matlab
   - Manuscript reviewers available
   - Slide reviewers removed

6. **README updated:**
   - Describes dissertation workflow
   - Prerequisites list correct tools
   - Examples are dissertation-focused

---

## Questions for User

Before finalizing:

1. **Project details:**
   - What should I call this project in CLAUDE.md?
   - What is your institution?
   - Do you have a working title or research area to mention?

2. **Tool priorities:**
   - Among Stata, Fortran, Matlab — which do you use most?
   - Do you want R support maintained for figure generation?
   - Are you writing primarily in LaTeX, or should Word export be prioritized equally?

3. **Workflow preferences:**
   - Should I keep TikZ diagram support for creating figures?
   - Do you want automated compilation on every change, or manual triggering?
   - Any specific econometric packages or numerical methods to document?

4. **Current state:**
   - Do you have existing code/papers to import?
   - Should I set up the folder structure with placeholder files?

---

## Implementation Order

1. Gather user answers to questions above
2. Update CLAUDE.md with project details and new structure
3. Create new folder structure
4. Create Stata/Fortran/Matlab convention rules
5. Create manuscript quality rules
6. Remove slide-specific rules
7. Create new skills for dissertation workflows
8. Remove slide-specific skills
9. Create new agents for code/manuscript review
10. Remove slide-specific agents
11. Update templates
12. Update README.md
13. Update MEMORY.md with initial structure
14. Verify all changes
15. Present summary and ask user to test first workflow

---

## Success Criteria

- User can describe a dissertation task and I plan + execute autonomously
- All tools (Stata, Fortran, Matlab) have clear conventions and quality standards
- Manuscript review produces actionable feedback
- Code review catches reproducibility and correctness issues
- Numerical methods are verified for accuracy
- Publication-ready outputs meet quality gates
- Plan-first workflow is preserved
- Contractor mode operates smoothly

---

## Notes

- Preserve the core workflow philosophy: plan-first, multi-agent review, quality gates
- Maintain the orchestrator protocol for autonomous execution
- Keep exploration folder for rapid prototyping
- Adapt but don't remove the session logging system
- This is a significant refactoring but the underlying patterns remain valid
