# Scaffold Steering Skill

Skill otomasi untuk menginisialisasi dan menyusun konfigurasi *steering* AI universal (`AGENTS.md`), aturan proyek (*rules*), panduan arsitektur, dan ringkasan PRD secara interaktif melalui wawancara terarah (*Grill-Me interview*).

Dirancang ramah *multi-vendor* (kompatibel dengan Antigravity, Cursor, Windsurf, Copilot, Claude Code, Aider, dll.), berfokus pada efisiensi token (*tokenomics*), integrasi orkestrasi subagent (*orchestrator-ready*), dan **sepenuhnya di-*track* oleh Git** agar dapat di-*push* dan disinkronkan berkala bersama tim.

---

## 🌟 Fitur Utama

1. **🌐 Standar Universal `AGENTS.md` & Smart Migration**:
   - Men-generate file kemudi utama bernama **`AGENTS.md`** di root proyek, yang merupakan standar terbuka antar vendor AI agent.
   - **Smart Migration**: Jika proyek sudah memiliki `GEMINI.md`, skill akan:
     1. Membaca seluruh isi aturan lama dari `GEMINI.md`.
     2. Memindahkan dan menggabungkannya ke `AGENTS.md` tanpa menghapus aturan yang sudah ada.
     3. Mengubah `GEMINI.md` menjadi *pointer* ringan yang mengarah ke `AGENTS.md`.

2. **🪙 Tokenomics & Codebase Hot-Paths (Anti-Bloat)**:
   - **Batas Maksimal 80 Baris**: File `AGENTS.md` dijaga ultra-padat (maksimal 80 baris) agar tidak memboroskan *input token* pada setiap sesi obrolan.
   - **Codebase Hot-Paths**: Memetakan path modul inti (Action, Service, DAO, HBM/ORM, JSP/UI) langsung di `AGENTS.md` agar subagent tidak melakukan *blind grep/search* yang memboroskan token.
   - **On-Demand Generation**: Menolak template generik; file aturan turunan (`.agents/rules/`) atau folder dokumen (`docs/`) hanya dibuat jika benar-benar dibutuhkan oleh *tech stack* proyek.

3. **🛡️ Kebijakan Non-Destruktif & Smart-Merge**:
   - **Zero Source-Code Modification**: Tidak akan pernah mengubah atau menghapus kode sumber aplikasi (`src/`, `app/`, `web/`, dsb.).
   - **Preserve Existing Rules/Docs**: Jika file aturan atau dokumentasi sudah ada, skill membaca dan mempertahankan isinya, hanya melakukan *smart-merge* untuk bagian baru yang relevan.

4. **🎭 Integrasi Orchestrator & E2E Mapping**:
   - Mendeteksi dan mencatat status keberadaan project E2E Playwright mandiri (lokasi path dan command runner).
   - Mendeteksi keberadaan indeks Knowledge Graph (`graphify-out/graph.json`) untuk sinkronisasi investigasi *Graphify-First* subagent.

5. **🎤 Grill-Me Interview (Konsultasi Pondasi Proyek)**:
   Sebelum men-generate file, AI menganalisis *stack* proyek dan mengajukan pertanyaan terarah:
   - Konfirmasi arsitektur target, build/run system, dan ORM/database.
   - Kebutuhan integrasi E2E Playwright & Graphify.
   - Kebutuhan pemisahan file rule (`.agents/rules/`), master PRD (`docs/prd.md`), atau referensi eksternal (`docs/references/`).

6. **🚀 Git Version-Controlled (Siap Push & Kolaborasi Tim)**:
   Seluruh file steering (`AGENTS.md`, `.agents/`, `docs/`) secara default di-*track* oleh Git, sehingga seluruh tim dapat menyinkronkan standar proyek secara berkala melalui `git push` dan `git pull`.

---

## 📁 Struktur File yang Di-generate

Ketika skill ini dijalankan pada suatu proyek, struktur fondasi berikut dapat disiapkan sesuai kebutuhan:

```text
<project-root>/
├── AGENTS.md                            # Universal project steering (maks 80 baris, hot-paths map)
├── GEMINI.md                            # (Opsional/Pointer) Mengarah ke AGENTS.md jika sebelumnya sudah ada
├── .agents/                             # (Opsional/On-demand)
│   ├── rules/
│   │   ├── code-conventions.md          # Konvensi penamaan, style guide, dan clean code spesifik stack
│   │   ├── api-standards.md             # Format standar response API (REST / GraphQL)
│   │   └── security-policies.md         # Validasi input, sanitasi, dan proteksi kredensial
│   └── skills/
│       ├── testing/SKILL.md             # Instruksi test runner spesifik
│       └── deployment/SKILL.md          # Runbook build dan prosedur deployment
└── docs/                                # (Opsional/On-demand)
    ├── prd.md                           # Master Product Requirement Document (High-level overview & scope)
    └── references/                      # Dokumen referensi eksternal (API spec, schema)
```

---

## ⚡ Kata Kunci & Triggers

Skill ini akan aktif secara otomatis jika Anda menggunakan salah satu pemicu berikut di chat Antigravity:

- `/scaffold-steering` *(Slash command)*
- `init steering` / `scaffold steering`
- `setup rules` / `buat steering files`
- `inisialisasi aturan proyek`

---

## 🛠️ Contoh Alur Penggunaan

### Inisialisasi Proyek Baru / Eksisting
Ketik di chat:
```text
/scaffold-steering
```
atau:
```text
init steering untuk proyek ini
```

AI akan:
1. Memindai konfigurasi proyek (`pom.xml`, `package.json`, `build.xml`, dsb.) dan memeriksa keberadaan `graphify-out/` serta `AGENTS.md`/`GEMINI.md`.
2. Mengajukan *Grill-Me interview* singkat seputar arsitektur/build, Playwright/Graphify, dan kebutuhan dokumen tambahan.
3. Men-generate atau memigrasikan aturan ke `AGENTS.md` (maksimal 80 baris dengan *Codebase Hot-Paths*) serta aturan pendukung sesuai kebutuhan.
4. Menampilkan ringkasan direktori akhir yang siap di-*commit* ke repositori Git proyek.
