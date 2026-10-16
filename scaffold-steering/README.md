# Scaffold Steering Skill

Skill otomasi untuk menginisialisasi dan menyusun file konfigurasi *steering* AI, aturan proyek (*rules*), spesifikasi fitur (*specs*), dan dokumen PRD secara interaktif melalui wawancara terarah (*Grill-Me interview*).

---

## 🌟 Fitur Utama

1. **🛡️ Kebijakan Non-Destruktif & Smart-Merge**:
   - **Zero Source-Code Modification**: Tidak akan pernah mengubah atau menghapus kode sumber aplikasi (`src/`, `app/`, dsb.).
   - **Preserve Existing Rules/Docs**: Jika file seperti `GEMINI.md` atau `docs/prd.md` sudah ada, skill akan membaca dan mempertahankan isinya, hanya melakukan *smart-merge* untuk bagian yang belum tersedia.
   - **No Duplicate Git Excludes**: Mengecek `.git/info/exclude` sebelum menambahkan *entry* baru agar tidak terjadi duplikasi.

2. **🎤 Grill-Me Interview (Konsultasi Interaktif)**:
   Sebelum men-generate file, AI akan menganalisis *stack* proyek dan mengajukan pertanyaan terarah seputar:
   - Dokumen referensi eksternal (API spec, skema SQL, dsb.).
   - Arsitektur target, database, dan ORM/caching layer.
   - Standar autentikasi & otorisasi (JWT, Session, OAuth2, RBAC).
   - Kebutuhan testing dan deployment runbook.
   - Prioritas fitur utama untuk PRD.

3. **🔒 Keamanan Git Lokal (`.git/info/exclude`)**:
   Secara otomatis mendaftarkan file konfigurasi lokal ke dalam `.git/info/exclude` agar aturan AI personal tidak mengotori *commit* repositori tim.

---

## 📁 Struktur File yang Di-generate

Ketika skill ini dijalankan pada suatu proyek, struktur berikut akan disiapkan:

```text
<project-root>/
├── GEMINI.md                            # Konteks proyek dan prinsip kerja agent utama
├── .agents/
│   ├── rules/
│   │   ├── code-conventions.md          # Konvensi penamaan, style guide, dan clean code
│   │   ├── api-standards.md             # Format standar response API (REST / GraphQL)
│   │   └── security-policies.md         # Validasi input, sanitasi, dan proteksi kredensial
│   └── skills/
│       ├── testing/SKILL.md             # Instruksi test runner dan panduan pengujian
│       └── deployment/SKILL.md          # Runbook build dan prosedur deployment
└── docs/
    ├── prd.md                           # Master Product Requirement Document
    ├── references/                      # Dokumen referensi eksternal (jika ada)
    └── specs/
        └── <feature-name>.md            # Spesifikasi teknis terperinci per fitur/tiket
```

---

## ⚡ Kata Kunci & Triggers

Skill ini akan aktif secara otomatis jika Anda menggunakan salah satu pemicu berikut di chat Antigravity:

- `/scaffold-steering` *(Slash command)*
- `init steering` / `scaffold steering`
- `setup rules` / `buat steering files`
- `inisialisasi aturan proyek`
- `buat spec` / `tambah spec` / `job baru` / `new spec`

---

## 🛠️ Contoh Skenario Penggunaan

### 1. Inisialisasi Proyek Baru / Eksisting
```text
/scaffold-steering
```
AI akan memindai *dependencies* (misal: `pom.xml`, `package.json`, `go.mod`), mengajukan 5 pertanyaan arsitektur, lalu men-generate seluruh struktur *steering files* di atas.

### 2. Membuat Job / Spesifikasi Fitur Baru
```text
buat spec untuk fitur integrasi payment gateway
```
AI akan membuat file spesifikasi terdedikasi di `docs/specs/payment-gateway.md` yang memuat:
- Overview & Business Goal
- Impact & Scope Analysis
- API Contracts (DTO Request/Response)
- Database & Schema Changes (ANSI SQL / Entity)
- Business Logic & Validations
