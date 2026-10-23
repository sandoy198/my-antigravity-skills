# My Antigravity Skills

Koleksi kustom *skills* untuk Google Antigravity dan Gemini CLI guna mempercepat alur kerja pengembangan perangkat lunak, standardisasi arsitektur, otomasi orkestrasi subagent, dan optimasi efisiensi token (*tokenomics*).

---

## 📦 Daftar Skill Tersedia

| Skill | Deskripsi Singkat | Trigger Utama |
| :--- | :--- | :--- |
| **`orchestrator`** | Multi-subagent orchestrator pipeline dengan gerbang persetujuan ketat (*strict approval gates*): Analisis $\rightarrow$ Approval $\rightarrow$ Plan Antigravity 2.0 (`/plan`) $\rightarrow$ Eksekusi Subagent Builder $\rightarrow$ Standby. Dilengkapi aturan *Smart Threshold Subagent Builder* (default 1 builder, max 2 paralel), pembersihan lifecycle otomatis, integrasi E2E Playwright portabel, kueri ANSI SQL, dan optimasi token presisi. | `/orchestrate`, `orchestrate:`, `mode orchestrator`, `jalankan orchestrator` |
| **`scaffold-steering`** | Inisialisasi struktur *steering files* modular dan *vendor-neutral* (`AGENTS.md` maks 80 baris dengan *Codebase Hot-Paths*, `.agents/rules/`, `docs/prd.md`) via wawancara interaktif (*Grill-Me interview*), terintegrasi pemetaan E2E Orchestrator, serta migrasi dari `GEMINI.md`. | `/scaffold-steering`, `init steering`, `setup rules`, `buat steering files` |

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
Jika Anda hanya ingin memasang skill spesifik (contoh: `orchestrator` atau `scaffold-steering`) tanpa meng-clone seluruh repositori:

**Windows (PowerShell):**
```powershell
# Contoh memasang skill orchestrator
$target = "$HOME\.gemini\config\skills\orchestrator"
New-Item -ItemType Directory -Force -Path $target
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/sandoy198/my-antigravity-skills/main/orchestrator/SKILL.md" -OutFile "$target\SKILL.md"

# Contoh memasang skill scaffold-steering
$target = "$HOME\.gemini\config\skills\scaffold-steering"
New-Item -ItemType Directory -Force -Path $target
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/sandoy198/my-antigravity-skills/main/scaffold-steering/SKILL.md" -OutFile "$target\SKILL.md"
```

**macOS / Linux (Bash):**
```bash
# Contoh memasang skill orchestrator
mkdir -p ~/.gemini/config/skills/orchestrator
curl -fsSL https://raw.githubusercontent.com/sandoy198/my-antigravity-skills/main/orchestrator/SKILL.md -o ~/.gemini/config/skills/orchestrator/SKILL.md
```

---

### Opsi 3: Pasang per Proyek / Workspace (Team-wide via Git)
Jika Anda ingin skill tertentu langsung tersedia bagi seluruh anggota tim di dalam repositori proyek:

1. Buat direktori `.agents/skills/<nama-skill>` pada root proyek:
   ```text
   <project-root>/
   └── .agents/
       └── skills/
           ├── orchestrator/
           │   └── SKILL.md
           └── scaffold-steering/
               └── SKILL.md
   ```
2. Salin file `SKILL.md` ke dalam folder tersebut, lalu *commit* dan *push* ke Git repositori tim Anda.
3. Rekan tim yang melakukan `git pull` akan langsung dapat menggunakannya di workspace proyek tersebut.

---

## 💡 Cara Penggunaan

Setelah terpasang, panggil skill langsung melalui prompt Antigravity menggunakan *slash command* atau frasa pemicu (*trigger phrases*).

### 1. Penggunaan `orchestrator`:
Gunakan saat menangani tiket perbaikan bug kompleks atau pengembangan fitur baru dengan delegasi penuh ke subagent:
```text
/orchestrate
Perbaiki bug pada kalkulasi diskon di modul pembayaran
```
Pipeline akan menjalankan:
1. **Fase 1 (Analisis)**: Subagent Analyzer tunggal memetakan masalah hingga koordinat baris presisi, lalu subagent analyzer ditutup untuk menghemat resource.
2. **Fase 2 (Planning)**: Menyusun `implementation_plan.md` dengan persetujuan tombol **Proceed** dan konfirmasi strategi verifikasi E2E.
3. **Fase 3 (Eksekusi & Smart Threshold)**: Subagent Builder (default 1 builder) mengeksekusi perubahan kode dan verifikasi, kemudian seluruh subagent aktif dibersihkan saat tuntas.

### 2. Penggunaan `scaffold-steering`:
Gunakan saat memulai atau merapikan aturan proyek:
```text
/scaffold-steering
```
atau dengan kalimat langsung:
```text
init steering untuk proyek ini
```
Agent akan memindai konfigurasi proyek, melakukan *Grill-Me interview*, dan men-generate `AGENTS.md` (maksimal 80 baris dengan *Codebase Hot-Paths*) serta aturan pendukung yang siap di-*track* Git.

---

## 📁 Struktur Direktori

```text
my-antigravity-skills/
├── orchestrator/
│   ├── README.md
│   └── SKILL.md
├── scaffold-steering/
│   ├── README.md
│   └── SKILL.md
├── .gitignore
├── AGENTS.md
├── LICENSE
└── README.md
```

---

## 🔄 Pembaruan Skill

Untuk memperbarui seluruh skill ke versi terbaru:

```powershell
cd "$HOME\.gemini\config\skills\my-antigravity-skills"
git pull
```

---

## 📄 Lisensi

Didistribusikan di bawah lisensi MIT. Lihat file [LICENSE](LICENSE) untuk informasi lebih lanjut.
