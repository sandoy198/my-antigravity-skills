# Scaffold Steering Skill

Skill otomasi untuk menginisialisasi dan menyusun konfigurasi *steering* AI, aturan proyek (*rules*), pondasi arsitektur, dan ringkasan PRD secara interaktif melalui wawancara terarah (*Grill-Me interview*). Skill ini berfokus murni pada **inisialisasi proyek**, bukan untuk fase implementasi fitur aktif.

---

## 🌟 Fitur Utama

1. **🛡️ Kebijakan Non-Destruktif & Smart-Merge**:
   - **Zero Source-Code Modification**: Tidak akan pernah mengubah atau menghapus kode sumber aplikasi (`src/`, `app/`, dsb.).
   - **Preserve Existing Rules/Docs**: Jika file seperti `GEMINI.md` atau `docs/prd.md` sudah ada, skill membaca dan mempertahankan isinya, hanya melakukan *smart-merge* untuk bagian yang belum tersedia.
   - **No Duplicate Git Excludes**: Mengecek `.git/info/exclude` sebelum menambahkan *entry* baru agar tidak terjadi duplikasi.

2. **🎤 Grill-Me Interview (Konsultasi Pondasi Proyek)**:
   Sebelum men-generate file, AI akan menganalisis *stack* proyek dan mengajukan pertanyaan terarah khusus pondasi arsitektur:
   - Dokumen referensi eksternal (API spec third-party, skema database SQL, arsitektur flow).
   - Arsitektur target, database, dan ORM/caching layer.
   - Standar autentikasi & otorisasi (JWT, Session, OAuth2, RBAC).
   - Kebutuhan test runner dan deployment runbook.

3. **🔒 Keamanan Git Lokal (`.git/info/exclude`)**:
   Secara otomatis mendaftarkan file konfigurasi lokal ke dalam `.git/info/exclude` agar aturan AI personal tidak mengotori *commit* repositori tim.

---

## 📁 Struktur File yang Di-generate

Ketika skill ini dijalankan pada suatu proyek, struktur fondasi berikut akan disiapkan:

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
2. Mengajukan 4 pertanyaan arsitektur dan fondasi sistem.
3. Men-generate seluruh struktur *rules*, *skills*, dan *docs* fondasi di atas.
4. Memastikan `.git/info/exclude` mengamankan file steering AI dari Git commit tim.
