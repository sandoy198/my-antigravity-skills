# Scaffold Steering Skill

Skill otomasi untuk menginisialisasi dan menyusun konfigurasi *steering* AI universal (`AGENTS.md`), aturan proyek (*rules*), panduan arsitektur, dan ringkasan PRD secara interaktif melalui wawancara terarah (*Grill-Me interview*). 

Dirancang ramah *multi-vendor* (kompatibel dengan Antigravity, Cursor, Windsurf, Copilot, Claude Code, Aider, dll.), berfokus murni pada **inisialisasi proyek**, dan **sepenuhnya di-*track* oleh Git** agar dapat di-*push* dan disinkronkan berkala bersama tim.

---

## 🌟 Fitur Utama

1. **🌐 Standar Universal `AGENTS.md` & Smart Migration**:
   - Men-generate file kemudi utama bernama **`AGENTS.md`** di root proyek, yang merupakan standar terbuka antar vendor AI agent.
   - **Smart Migration**: Jika proyek sudah terlanjur memiliki `GEMINI.md`, skill akan:
     1. Membaca seluruh isi aturan lama dari `GEMINI.md`.
     2. Memindahkan dan menggabungkannya ke `AGENTS.md` tanpa menghapus satu pun aturan yang ada.
     3. Mengubah `GEMINI.md` menjadi *pointer* ringan yang mengarah ke `AGENTS.md`.

2. **🛡️ Kebijakan Non-Destruktif & Smart-Merge**:
   - **Zero Source-Code Modification**: Tidak akan pernah mengubah atau menghapus kode sumber aplikasi (`src/`, `app/`, dsb.).
   - **Preserve Existing Rules/Docs**: Jika file aturan atau dokumentasi sudah ada, skill membaca dan mempertahankan isinya, hanya melakukan *smart-merge* untuk bagian baru yang relevan.

3. **🎤 Grill-Me Interview (Konsultasi Pondasi Proyek)**:
   Sebelum men-generate file, AI akan menganalisis *stack* proyek dan mengajukan pertanyaan terarah khusus pondasi arsitektur:
   - Dokumen referensi eksternal (API spec third-party, skema database SQL, arsitektur flow).
   - Arsitektur target, database, dan ORM/caching layer.
   - Standar autentikasi & otorisasi (JWT, Session, OAuth2, RBAC).
   - Kebutuhan test runner dan deployment runbook.

4. **🚀 Git Version-Controlled (Siap Push & Kolaborasi Tim)**:
   Seluruh file steering (`AGENTS.md`, `.agents/`, `docs/`) sepenuhnya di-*track* oleh Git, sehingga seluruh tim dapat menyinkronkan standar proyek secara berkala melalui `git push` dan `git pull`.

---

## 📁 Struktur File yang Di-generate

Ketika skill ini dijalankan pada suatu proyek, struktur fondasi berikut akan disiapkan:

```text
<project-root>/
├── AGENTS.md                            # Universal project steering (dibaca oleh Antigravity & vendor AI lain)
├── GEMINI.md                            # (Opsional/Pointer) Mengarah ke AGENTS.md jika sebelumnya sudah ada
├── .agents/
│   ├── rules/
│   │   ├── code-conventions.md          # Konvensi penamaan, style guide, dan clean code
│   │   ├── api-standards.md             # Format standar response API (REST / GraphQL)
│   │   └── security-policies.md         # Validasi input, sanitasi, dan proteksi kredensial
│   └── skills/
│       ├── testing/SKILL.md             # Instruksi test runner dan panduan pengujian
│       └── deployment/SKILL.md          # Runbook build dan prosedur deployment
└── docs/
    ├── prd.md                           # Master Product Requirement Document (High-level overview & scope)
    └── references/                      # Dokumen referensi eksternal (jika ada)
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
1. Memindai *dependencies* (misal: `pom.xml`, `package.json`, `go.mod`, dsb.).
2. Mendeteksi apakah sudah ada `GEMINI.md` atau `AGENTS.md` untuk persiapan migrasi/merge.
3. Mengajukan 4 pertanyaan arsitektur dan fondasi sistem.
4. Men-generate atau memigrasikan seluruh aturan ke `AGENTS.md` serta struktur *rules*, *skills*, dan *docs* fondasi.
5. Memastikan seluruh file siap di-*commit* ke repositori Git proyek.
