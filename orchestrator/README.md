# Multi-Subagent Orchestrator Skill

Skill orkestrasi *multi-subagent* tingkat lanjut dengan *strict approval gates* di setiap fase, terintegrasi langsung dengan fitur `/plan` Antigravity 2.0. Agen utama bertindak murni sebagai **Dispatcher & Controller**, mendelegasikan 100% investigasi dan modifikasi kode ke subagent khusus (*Analyzer* dan *Builder*).

---

## 🌟 Fitur Utama & Prinsip Kerja

1. **⛔ Delegasi Mutlak (Zero Direct Code Edit)**:
   - Agen utama **tidak pernah** memodifikasi kode sumber proyek secara langsung.
   - Semua investigasi didelegasikan ke Subagent Analyzer, dan seluruh penulisan/pengeditan kode didelegasikan ke Subagent Builder melalui `invoke_subagent`.
   - Agen utama hanya memelihara *planning artifacts* (`implementation_plan.md` dan `walkthrough.md`).

2. **🚦 Gerbang Persetujuan Ketat (Strict Approval Gates)**:
   - Alur kerja tidak akan melompat ke fase berikutnya tanpa persetujuan eksplisit pengguna (`approve`, `lanjut`, `setuju`, `/plan`, atau klik tombol **Proceed**).
   - Jika pengguna mengajukan pertanyaan atau keraguan, sistem menjawab terlebih dahulu dan menahan fase saat ini.

3. **🎭 Otomasi E2E Playwright Mandiri & Portabel**:
   - Mendukung standar arsitektur di mana project E2E Playwright berada di workspace/direktori mandiri terpisah.
   - Jika project Playwright sudah ada, Subagent Builder memperbarui skenario (Page Object & Test Spec) dan menjalankannya via CLI.
   - Jika belum ada, menyediakan 3 opsi fleksibel:
     1. Menghasilkan prompt siap pakai untuk dieksekusi di session/workspace Playwright terpisah.
     2. Menjalankan verifikasi cepat via MCP Browser (`playwright` / `chrome-devtools`).
     3. Melewati otomasi (verifikasi manual oleh developer).

4. **🪙 Efisiensi Token & Handover Presisi (Tokenomics)**:
   - **Line-Range Handover**: Subagent Analyzer mengidentifikasi lokasi baris presisi (`File#Lxx-Lyy`), sehingga Subagent Builder hanya membaca rentang baris tersebut tanpa memuat ulang seluruh isi file.
   - **Graphify-First**: Melakukan query ke indeks Knowledge Graph (`graphify-out/graph.json`) sebelum membaca file mentah jika indeks tersedia.
   - **Gaya Komunikasi Caveman**: Menggunakan ringkasan teknis padat fakta untuk menghemat *context window*.

5. **🛡️ Integritas Lingkungan & Database**:
   - Menghormati alur kerja lokal (misal: compile dan runtime dikelola di IntelliJ IDEA tanpa CLI build liar).
   - Menghasilkan query SQL standar ANSI (`CAST(val AS type)`, bebas operator non-standar `::`) untuk perubahan database atau Hibernate HBM mapping.

---

## 🔄 Alur Kerja Pipeline (3 Fase)

```text
[Input Task/Keluhan Pengguna]
            │
            ▼
┌───────────────────────────────────────┐
│ Fase 1: Analisis & Grill-Me           │
│ • Spawn Subagent Analyzer (inherit)   │
│ • Graphify lookup & baris presisi     │
│ • Analisis root cause & trade-offs    │
└───────────────────────────────────────┘
            │
    [Approval Pengguna: /plan]
            │
            ▼
┌───────────────────────────────────────┐
│ Fase 2: Implementation Plan (/plan)   │
│ • Artifact implementation_plan.md     │
│ • Minimal diff (Ponytail) & ANSI SQL  │
│ • Konfirmasi opsi verifikasi E2E      │
└───────────────────────────────────────┘
            │
    [Approval Pengguna: Proceed]
            │
            ▼
┌───────────────────────────────────────┐
│ Fase 3: Eksekusi & Verifikasi         │
│ • Spawn Subagent Builder (inherit)    │
│ • Edit baris presisi (Clean Code)     │
│ • Jalankan/siapkan E2E Playwright     │
│ • Update walkthrough.md               │
└───────────────────────────────────────┘
            │
            ▼
       [STANDBY]
(Build & Run di-handle di IDE Pengguna)
```

---

## ⚡ Kata Kunci & Triggers

Skill ini aktif secara otomatis jika Anda menggunakan salah satu pemicu berikut:

- `/orchestrate` *(Slash command)*
- `orchestrate:`
- `mode orchestrator`
- `jalankan orchestrator`
- Permintaan alur kerja orkestrasi subagent

Untuk menonaktifkan mode ini dalam sesi yang sedang berjalan, ketik `"stop orchestrator"` atau `"normal mode"`.

---

## 💡 Contoh Skenario Penggunaan

### 1. Memulai Tiket Perbaikan Bug atau Fitur Baru
```text
/orchestrate
Perbaiki validasi nomor registrasi pada modul pendaftaran agar menolak input yang mengandung spasi atau karakter khusus.
```

### 2. Respons Agen Utama pada Fase 1 (Analisis)
Agen utama akan men-spawn Subagent Analyzer untuk meneliti kode, lalu menyajikan temuan teknis ringkas:
```text
[PendaftaranAction.java#L145-L168] Validasi nomor registrasi belum memeriksa karakter spasi dan simbol.
[PendaftaranService.java#L80-L95] Method validateRegistrationNumber() siap digunakan ulang.

Analisis selesai. Ketik '/plan' atau 'lanjut' untuk menyusun implementation plan.
```

### 3. Fase 2: Perencanaan & Approval
Pengguna mengetik `/plan`. Agen menyajikan rencana perubahan minimalis (*Ponytail principle*), target baris, dan menanyakan strategi E2E jika belum terpasang. Pengguna menyetujui dengan menekan tombol **Proceed**.

### 4. Fase 3: Eksekusi & Laporan Akhir
Subagent Builder mengimplementasikan kode, memperbarui `walkthrough.md`, menyajikan DDL/DML jika relevan, dan kembali ke status **STANDBY**.
