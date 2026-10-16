# My Antigravity Skills

Koleksi kustom *skills* untuk Google Antigravity dan Gemini CLI guna mempercepat alur kerja pengembangan perangkat lunak, standardisasi arsitektur, dan otomasi *prompting*.

---

## 📦 Daftar Skill Tersedia

| Skill | Deskripsi Singkat | Trigger Utama |
| :--- | :--- | :--- |
| **`scaffold-steering`** | Inisialisasi struktur *steering files* proyek (`GEMINI.md`, `.agents/rules/`, `docs/prd.md`, `docs/specs/`) melalui wawancara interaktif (*Grill-Me interview*) serta proteksi otomatis `.git/info/exclude`. | `/scaffold-steering`, `init steering`, `setup rules`, `buat spec` |

---

## 🚀 Panduan Instalasi

Pilih salah satu metode instalasi berikut sesuai kebutuhan lingkungan kerja Anda:

### Opsi 1: Pasang Seluruh Koleksi (Global - Direkomendasikan)
Gunakan opsi ini agar semua skill dalam repositori ini otomatis terdeteksi dan dapat dipanggil di semua proyek/workspace lokal Anda.

**Windows (PowerShell):**
```powershell
git clone https://github.com/sandoy198/my-antigravity-skills.git "$HOME\.gemini\config\skills\my-antigravity-skills"
```

**macOS / Linux (Bash):**
```bash
git clone https://github.com/sandoy198/my-antigravity-skills.git ~/.gemini/config/skills/my-antigravity-skills
```

> **Catatan:** Antigravity secara otomatis mendeteksi setiap subfolder di dalam direktori `skills/` yang memiliki file `SKILL.md`.

---

### Opsi 2: Pasang Hanya Skill Tertentu (Global)
Jika Anda hanya ingin memasang skill spesifik (contoh: `scaffold-steering`) tanpa meng-clone seluruh repositori:

**Windows (PowerShell):**
```powershell
$target = "$HOME\.gemini\config\skills\scaffold-steering"
New-Item -ItemType Directory -Force -Path $target
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/sandoy198/my-antigravity-skills/main/scaffold-steering/SKILL.md" -OutFile "$target\SKILL.md"
```

**macOS / Linux (Bash):**
```bash
mkdir -p ~/.gemini/config/skills/scaffold-steering
curl -fsSL https://raw.githubusercontent.com/sandoy198/my-antigravity-skills/main/scaffold-steering/SKILL.md -o ~/.gemini/config/skills/scaffold-steering/SKILL.md
```

---

### Opsi 3: Pasang per Proyek / Workspace (Team-wide via Git)
Jika Anda ingin skill tertentu langsung tersedia bagi seluruh anggota tim di dalam repositori proyek:

1. Buat direktori `.agents/skills/<nama-skill>` pada root proyek:
   ```text
   <project-root>/
   └── .agents/
       └── skills/
           └── scaffold-steering/
               └── SKILL.md
   ```
2. Salin file `SKILL.md` ke dalam folder tersebut, lalu *commit* dan *push* ke Git repositori tim Anda.
3. Rekan tim yang melakukan `git pull` akan langsung dapat menggunakannya di workspace proyek tersebut.

---

## 💡 Cara Penggunaan

Setelah terpasang, panggil skill langsung melalui prompt Antigravity menggunakan *slash command* atau frasa pemicu (*trigger phrases*).

### Contoh Penggunaan `scaffold-steering`:
Ketik perintah berikut di chat:
```text
/scaffold-steering
```
atau dengan kalimat langsung:
```text
init steering untuk proyek ini
```

Agent akan:
1. Mendeteksi *tech stack* dan *framework* yang ada di root proyek.
2. Melakukan wawancara interaktif (*Grill-Me interview*) mengenai database, autentikasi, referensi dokumen eksternal, dan prioritas fitur.
3. Mengonfigurasi `.git/info/exclude` agar file aturan AI tidak mengotori repositori tim.
4. Men-generate dokumen arsitektur dan spesifikasi secara terstruktur (`GEMINI.md`, `.agents/rules/`, `docs/prd.md`, `docs/specs/`).

---

## 📁 Struktur Direktori

```text
my-antigravity-skills/
├── scaffold-steering/
│   └── SKILL.md
├── LICENSE
└── README.md
```

---

## 🔄 Pembaruan Skill

Untuk memperbarui skill ke versi terbaru:

```powershell
cd "$HOME\.gemini\config\skills\my-antigravity-skills"
git pull
```

---

## 📄 Lisensi

Didistribusikan di bawah lisensi MIT. Lihat file [LICENSE](LICENSE) untuk informasi lebih lanjut.
