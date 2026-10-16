---
name: scaffold-steering
description: >
  Scaffold and initialize standard workspace steering files, rules (GEMINI.md, .agents/rules/),
  skills (.agents/skills/), PRD documentation (docs/prd.md), and feature specifications (docs/specs/<feature-name>.md)
  for the active project, with an interactive "Grill-Me" interview that asks the developer first regarding their needs,
  architecture, and any external references/docs before generating them.
  Automatically configures local .git/info/exclude to ensure AI steering files remain private and safe from team Git commits.
  Trigger when the user says "init steering", "scaffold steering", "setup rules", "buat steering files",
  "inisialisasi aturan proyek", "/scaffold-steering", "buat spec", "tambah spec", "job baru", "new spec",
  or requests project rule setup with grill-me.
---

# Scaffold Steering Files & Interactive Setup Skill

Use this skill to establish a clean, modular, and privacy-safe steering setup. The agent will interactively consult the developer first to understand their specific architecture, preferences, and external reference documents before finalizing the files.

## 🛡️ Non-Destructive & Smart-Merge Policy (CRITICAL)

When operating on **existing projects** or repositories where some steering files already exist:
1. **Zero Source-Code Modification**: Never alter or delete existing source code (`src/`, `app/`, etc.).
2. **Preserve Existing Rules/Docs**: If a target file (e.g., `GEMINI.md`, `docs/prd.md`, `.agents/rules/*.md`) already exists:
   - **READ IT FIRST**.
   - **DO NOT OVERWRITE BLINDLY**.
   - Preserve all existing custom rules, project context, and user notes.
   - Only merge/append new missing sections or update with new answers from the interview.
3. **No Duplicate Git Excludes**: Check `.git/info/exclude` and only append entries that are not already present.

---

## Execution Workflow

Execute the following steps in sequence:

### Step 1: Detect Project Stack & Existing Configs
Examine the workspace root (e.g., `package.json`, `go.mod`, `pom.xml`, `requirements.txt`, `composer.json`, `.eslintrc`, etc.) to understand existing frameworks, linters, and conventions.

### Step 2: Establish Core Directory Hierarchy
Create baseline directories:
- `.agents/rules/`
- `.agents/skills/testing/`
- `.agents/skills/deployment/`
- `docs/specs/`

### Step 3: Interactive Consultation & Grill-Me Interview (Ask First!)
Before generating heavy content or arbitrary reference files, present what was detected and **ask the developer targeted questions**:

1. **External References & Docs**:
   - *"Apakah Anda memiliki dokumen referensi eksternal yang ingin dimasukkan (misal: API spec third-party, skema database SQL, arsitektur flow)? Jika ada, saya akan siapkan folder `docs/references/` dan mengaitkannya ke PRD."*
2. **Target Architecture & DB**:
   - *"Database, ORM, atau caching layer apa yang menjadi prioritas di project ini?"*
3. **Authentication & Authorization**:
   - *"Metode autentikasi apa yang digunakan (JWT, Session, OAuth2, RBAC)?"*
4. **Testing & Deployment Needs**:
   - *"Apakah ada alur test atau deployment khusus yang butuh skrip pembantu (`scripts/`) atau panduan teknis (`references/`)?"*
5. **Core Feature Priorities**:
   - *"Fitur utama apa yang ingin diprioritaskan di dalam `docs/prd.md` saat ini?"*

### Step 4: Generate Tailored Rule Files

1. **`GEMINI.md`** (Project Root):
   - Project overview and core principles aligned with developer responses.
   - Explicit reference: *"Product specifications are defined in `docs/prd.md`."*
   - If references were requested: Link to `docs/references/`.

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

### Step 6: Generate or Merge Master PRD & Job Feature Specs

#### A. Master PRD (`docs/prd.md`)
Populate `docs/prd.md` with:
- Feature/Product Overview & Goals based on the interview.
- User Personas & User Flow.
- Functional Requirements (Checklist format `- [ ]`) based on developer's priority features.
- Acceptance Criteria.

#### B. Dedicated Job / Feature Spec (`docs/specs/<feature-name>.md`)
When the developer introduces a new job, ticket, or distinct feature:
- Create a dedicated specification file: `docs/specs/<feature-name>.md`.
- Populate with standard feature spec sections:
  1. **Overview & Business Goal**: Ringkasan kebutuhan bisnis dan latar belakang tiket/job.
  2. **Impact & Scope Analysis**: Komponen terdampak (Controller/Endpoint, Service, Repository, Database Tables).
  3. **API Contracts**: Detail DTO Request dan Response (mengikuti standar kode proyek, misal `JsonResponse` atau `JsonCallback` tanpa wrapper `data`).
  4. **Database & Schema Changes**: Tabel, kolom baru, query ANSI SQL, atau migrasi JPA Entity.
  5. **Business Logic & Validations**: Validasi input, batasan hak akses (RBAC), penanganan exception (`ResponseStatus` / `ResponseCode`).
  6. **Acceptance Criteria & Task Checklist**: Checklist implementasi (`- [ ]`).
  7. **Tautan Referensi**: Link ke `docs/prd.md` dan alur teknis terkait di `docs/`.

### Step 7: Configure Local Git Exclude (Privacy Safeguard)
If the project root is a Git repository (`.git/` folder exists):
- Ensure `.git/info/exclude` contains the following lines (without duplicating existing lines):
  ```text
  GEMINI.md
  AGENTS.md
  .agents/
  docs/
  ```

### Step 8: Confirmation & Summary
Print a clean summary tree of the generated structure, showing all created rules, skills, references, and confirming Git exclude status.
