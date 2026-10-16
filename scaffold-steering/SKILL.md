---
name: scaffold-steering
description: >
  Scaffold and initialize standard workspace steering files, rules (AGENTS.md, .agents/rules/),
  skills (.agents/skills/), and PRD documentation (docs/prd.md) for the active project, with an
  interactive "Grill-Me" interview that asks the developer first regarding their tech stack,
  architecture, and any external references/docs before generating them.
  Supports smart migration from legacy GEMINI.md to universal AGENTS.md while preserving existing rules.
  Automatically configures local .git/info/exclude to ensure AI steering files remain private and safe from team Git commits.
  Trigger when the user says "init steering", "scaffold steering", "setup rules", "buat steering files",
  "inisialisasi aturan proyek", or "/scaffold-steering".
---

# Scaffold Steering Files & Interactive Setup Skill

Use this skill to establish a clean, modular, vendor-neutral, and privacy-safe steering setup during project initialization. The agent will interactively consult the developer first to understand their specific architecture, stack preferences, and external reference documents before finalizing the files.

## 🛡️ Non-Destructive & Smart-Merge Policy (CRITICAL)

When operating on **existing projects** or repositories where some steering files already exist:
1. **Zero Source-Code Modification**: Never alter or delete existing source code (`src/`, `app/`, etc.).
2. **Preserve Existing Rules/Docs**: If a target file (e.g., `AGENTS.md`, `GEMINI.md`, `docs/prd.md`, `.agents/rules/*.md`) already exists:
   - **READ IT FIRST**.
   - **DO NOT OVERWRITE BLINDLY**.
   - Preserve all existing custom rules, project context, and user notes.
   - Only merge/append new missing sections or update with new answers from the interview.
3. **Smart Migration from `GEMINI.md` to `AGENTS.md`**:
   - If `GEMINI.md` exists and `AGENTS.md` does not: Read all existing contents from `GEMINI.md`, migrate/merge them into `AGENTS.md` so that other vendor agents can read them, and convert `GEMINI.md` into a lightweight pointer:
     ```markdown
     # Project Steering
     See [AGENTS.md](AGENTS.md) for full project rules and instructions.
     ```
   - If `AGENTS.md` already exists: Smart-merge into `AGENTS.md`.
4. **No Duplicate Git Excludes**: Check `.git/info/exclude` and only append entries that are not already present.

---

## Execution Workflow

Execute the following steps in sequence:

### Step 1: Detect Project Stack & Existing Configs
Examine the workspace root (e.g., `package.json`, `go.mod`, `pom.xml`, `requirements.txt`, `composer.json`, `.eslintrc`, etc.) to understand existing frameworks, linters, and conventions.
Check whether `GEMINI.md` or `AGENTS.md` already exists to prepare for smart-merging or migration.

### Step 2: Establish Core Directory Hierarchy
Create baseline directories:
- `.agents/rules/`
- `.agents/skills/testing/`
- `.agents/skills/deployment/`
- `docs/`

### Step 3: Interactive Consultation & Grill-Me Interview (Ask First!)
Before generating heavy content or arbitrary reference files, present what was detected and **ask the developer targeted questions** focused strictly on project initialization and architecture foundation:

1. **External References & Docs**:
   - *"Apakah Anda memiliki dokumen referensi eksternal yang ingin dimasukkan (misal: API spec third-party, skema database SQL, arsitektur flow)? Jika ada, saya akan siapkan folder `docs/references/` dan mengaitkannya ke PRD."*
2. **Target Architecture & DB**:
   - *"Database, ORM, atau caching layer apa yang menjadi prioritas di project ini?"*
3. **Authentication & Authorization**:
   - *"Metode autentikasi apa yang digunakan (JWT, Session, OAuth2, RBAC)?"*
4. **Testing & Deployment Needs**:
   - *"Apakah ada alur test atau deployment khusus yang butuh skrip pembantu (`scripts/`) atau panduan teknis (`references/`)?"*

### Step 4: Generate Tailored Rule Files

1. **`AGENTS.md`** (Project Root - Universal Standard):
   - Project overview, foundational architecture, and core principles aligned with developer responses and cross-vendor agent readability.
   - Explicit reference: *"Product specifications and system overview are defined in `docs/prd.md`."*
   - If references were requested: Link to `docs/references/`.
   - If legacy `GEMINI.md` existed, ensure all previous rules are preserved inside `AGENTS.md`, and replace `GEMINI.md` with the pointer to `AGENTS.md`.

2. **`.agents/rules/code-conventions.md`**:
   - Reflect the actual coding style already used in the codebase (naming, import structuring, anti-patterns).

3. **`.agents/rules/api-standards.md`**:
   - REST / GraphQL conventions matching existing API responses.
   - Standard JSON response format: `{ success: boolean, data?: any, error?: { code: string, message: string } }`.

4. **`.agents/rules/security-policies.md`**:
   - Validation libraries (Zod, Pydantic, etc.).
   - Sanitization against SQL injection, XSS, and CSRF.
   - Prohibition against hardcoding secrets, credentials, or tokens.

### Step 5: Generate Skills & Requested References
- **`.agents/skills/testing/SKILL.md`**: Tailored test runner instructions. Create `references/` and `scripts/` if requested.
- **`.agents/skills/deployment/SKILL.md`**: Build & deployment runbook. Create `references/` and `scripts/` if requested.
- **`docs/references/`**: Created if the developer confirmed having external documents, with a starter placeholder/index.

### Step 6: Generate or Merge Master PRD (`docs/prd.md`)
Populate `docs/prd.md` with high-level foundational context (tanpa mendikte prioritas fitur implementasi):
- Product / System Overview & High-level Goals.
- User Personas & General System Scope.
- System Architecture Overview.
- Non-Functional Requirements (Security, Performance, Logging).

### Step 7: Configure Local Git Exclude (Privacy Safeguard)
If the project root is a Git repository (`.git/` folder exists):
- Ensure `.git/info/exclude` contains the following lines (without duplicating existing lines):
  ```text
  AGENTS.md
  GEMINI.md
  .agents/
  docs/
  ```

### Step 8: Confirmation & Summary
Print a clean summary tree of the generated structure, showing all created rules, skills, references, and confirming Git exclude status.
