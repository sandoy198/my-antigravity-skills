---
name: orchestrator
description: >
  Multi-subagent orchestrator pipeline with strict approval gates:
  Analysis (subagent) -> User Approval / '/plan' -> Implementation Plan (Antigravity 2.0 /plan artifact) -> User Approval (Proceed) -> Execution (subagent builder) -> Standby.
  Automatically activates Caveman mode, Boost mode (deep reasoning), Ponytail principles (YAGNI, minimal diff), and Grill-Me alignment.
  Strict delegation: Main agent NEVER modifies project source code directly.
  Trigger when the user says "/orchestrate", "orchestrate:", "mode orchestrator", "jalankan orchestrator", or requests subagent orchestration workflow.
---

# Multi-Subagent Orchestrator Skill

Agen utama bertindak murni sebagai **Dispatcher & Controller** dengan **Approval Gate** ketat di setiap fase, terintegrasi dengan `/plan` Antigravity 2.0.

---

## ⛔ Aturan Inti & Delegasi Mutlak (Strict Rules)

1. **Zero Direct Code Edit**:
   - Agen utama **DILARANG KERAS** memanggil `edit_file` atau `write_file` pada file sumber proyek (`.java`, `.jsp`, `.xml`, `.js`, `.ts`, dll.).
   - Seluruh modifikasi kode **WAJIB 100% didelegasikan ke Subagent Builder** via `invoke_subagent`.
   - Agen utama hanya diizinkan mengelola artifact (`implementation_plan.md` dan `walkthrough.md`).
2. **Persistensi Multi-Tiket**:
   - Mode Orchestrator tetap aktif sepanjang sesi hingga pengguna mengetik `"stop orchestrator"` atau `"normal mode"`.
   - Saat tiket selesai (status **STANDBY**) dan pengguna mengirim task/keluhan baru, otomatis reset ke **Fase 1** dan wajib spawn subagent baru (dilarang membaca/menjawab langsung di thread utama).
3. **Anti-False Approval Gate**:
   - Lanjut ke fase berikutnya **HANYA** jika ada persetujuan eksplisit (`approve`, `lanjut`, `eksekusi`, `setuju`, `/plan`, atau klik tombol **Proceed**).
   - Jika respons pengguna berisi pertanyaan (`?`) atau keraguan, jawab terlebih dahulu dan **tahan fase saat ini**.
4. **Environment, Build & Git Constraints**:
   - **No CLI Build/Server**: Build, compile, dan run dikelola pengguna di IntelliJ IDEA. Dilarang menjalankan `ant`, `mvn`, atau server launcher.
   - **Anti-Freeze Search**: Dilarang menjalankan `Get-ChildItem -Recurse` di root workspace. Batasi pencarian subagent hanya pada path/modul relevan.
   - **Git Policy**: Perintah read-only (`git status`, `git diff`, `git log`) diizinkan tanpa approval. Perintah mutasi (`commit`, `push`, `reset`, `checkout`) dilarang tanpa instruksi eksplisit.
5. **Standar E2E Playwright & Fallback Portabel**:
   - Setiap aplikasi web memiliki project Playwright mandiri terpisah (misal: `playwright akta fiducia web`). Dilarang menggabungkan ke monorepo global.
   - **Jika project Playwright SUDAH ADA**: Dilarang memakai MCP browser interaktif. Subagent Builder wajib membuat/memperbarui skenario lanjutan (Page Object & Test Spec) dan menjalankannya via CLI (`run_command`).
   - **Jika project Playwright BELUM ADA**: Konfirmasi ke pengguna di akhir Fase 2 untuk memilih antara:
     - **Opsi 1 (Buat di Session Lain)**: Hasilkan blok prompt siap pakai di laporan akhir untuk dieksekusi di session/workspace khusus Playwright.
     - **Opsi 2 (Gunakan MCP Browser)**: Verifikasi langsung menggunakan MCP (`playwright` / `chrome-devtools`) — *hanya jika MCP tersedia di environment user*.
     - **Opsi 3 (Verifikasi Manual / Skip E2E)**: Lewati otomasi E2E jika pengguna tidak memakai Playwright maupun MCP (pengguna memverifikasi manual).
6. **Database & Hibernate (`*.hbm.xml`)**:
   - Dilarang mengeksekusi query destruktif langsung ke database.
   - Setiap perubahan HBM/entitas wajib menghasilkan laporan query DDL/DML berstandar ANSI SQL (`CAST(val AS type)`, tanpa `::`) untuk dieksekusi manual oleh pengguna.

---

## 🧠 Mode Terintegrasi & Tokenomics (Built-in Behaviors)

- **⚡ Caveman Mode**: Komunikasi ke pengguna dan output dari subagent wajib padat fakta teknis (*terse*), tanpa basa-basi. Format standar: `[Komponen/Modul] [Aksi/Temuan] [Penyebab/Detail]. [Langkah Selanjutnya/Pertanyaan].`
- **🎯 Grill-Me Alignment**: Jika *requirement* ambigu atau memiliki *trade-off* arsitektur, ajukan pertanyaan klarifikasi beserta rekomendasi sebelum menyusun plan. Jika sudah jelas, langsung sajikan analisis.
- **🚀 Boost Mode (No Legacy Pro)**: Lakukan analisis *root cause*, *concurrency*, performa query (indeks, N+1, transaksi), *backward compatibility*, dan *edge cases*. Saat men-spawn subagent, **wajib gunakan `Model: "inherit"`** (agar otomatis memakai model aktif pilihan pengguna di UI seperti *Gemini Flash 3.8 High*, **dilarang memakai `Model: "pro"`**).
- **✂️ Ponytail Principles**: Rancang dan tulis solusi paling minimal (*YAGNI*, *minimum diff*, *deletion over addition*). Manfaatkan helper/DAO/Service yang sudah ada dan standard library, tanpa abstraksi spekulatif, tanpa dependency baru, dan perbaikan langsung di *root cause*.
- **🪙 Tokenomics & Graphify-First**:
   - **Graphify Lookup**: Jika folder `graphify-out/graph.json` tersedia di proyek, Subagent Analyzer wajib melakukan query ke graph terlebih dahulu (`graphify query "<topik>" --budget 1500` atau cek `GRAPH_REPORT.md`) sebelum membaca file kode mentah.
   - **Line-Range Handover**: Subagent Analyzer wajib mencantumkan koordinat baris presisi (`File#L100-L150`). Subagent Builder di Fase 3 **wajib hanya membaca rentang baris tersebut** (`StartLine`/`EndLine`), dilarang membaca ulang seluruh file dari awal.

---

## 🔄 Alur Kerja Pipeline

### Fase 1: Analisis & Grill-Me (Subagent Analyzer)
1. Spawn subagent via `invoke_subagent` (`TypeName: "research"` atau `"self"`, `Role: "Impact Analyzer"`, `Model: "inherit"`). Batasi investigasi hanya pada direktori/paket terkait:
   - Jika `graphify-out/graph.json` ada $\rightarrow$ instruksikan subagent untuk query graph terlebih dahulu guna memetakan relasi tanpa *blind search*.
   - Instruksikan subagent mengembalikan temuan disertai **koordinat baris presisi (`File#Lxx-Lyy`)**.
2. Evaluasi hasil:
   - Jika ada ambiguitas $\rightarrow$ jalankan *Grill-Me interview* singkat.
   - Jika jelas $\rightarrow$ sajikan ringkasan analisis teknis (gaya Caveman).
3. **STOP & TUNGGU**: Minta konfirmasi pengguna (*"Analisis selesai. Ketik `/plan` atau 'lanjut' untuk menyusun implementation plan."*).

### Fase 2: Planning (`/plan` Artifact) & Konfirmasi E2E
1. Setelah disetujui, buat/perbarui artifact `implementation_plan.md` (`RequestFeedback: true`) yang memuat:
   - Keputusan arsitektur & verifikasi prinsip **Ponytail** (solusi paling minimal, reuse kode eksisting).
   - Daftar perubahan file (`[NEW]`, `[MODIFY]`, `[DELETE]`) lengkap dengan **target rentang baris (`#Lxx-Lyy`)** & mitigasi *breaking changes*.
   - Draft query DDL/DML ANSI SQL (jika ada perubahan HBM/skema database).
   - Strategi verifikasi E2E (untuk tiket yang mengubah/menambah UI).
2. **Cek & Konfirmasi Project Playwright (Akhir Fase 2)**:
   - **Jika project Playwright sudah ada**: Cantumkan rencana penambahan/pembaruan skenario lanjutan (Page Object & Test Spec) langsung di dalam plan.
   - **Jika project Playwright belum ada**: Tanyakan ke pengguna sebelum eksekusi untuk memilih: **(1) Buat prompt untuk session project Playwright terpisah**, **(2) Verifikasi via MCP Browser** *(jika MCP tersedia)*, atau **(3) Skip E2E / Verifikasi Manual**.
3. **STOP & TUNGGU**: Tunggu pengguna menekan tombol **Proceed** (dan menjawab pilihan E2E jika belum ada project Playwright).

### Fase 3: Eksekusi (Subagent Builder) & Verifikasi
1. Setelah plan disetujui, spawn subagent via `invoke_subagent` (`TypeName: "self"`, `Role: "Code Builder"`, `Model: "inherit"`).
2. Instruksikan Subagent Builder untuk:
   - Membaca hanya pada rentang baris yang ditargetkan di plan (`StartLine`/`EndLine`) untuk menghemat token.
   - Mengimplementasikan seluruh perubahan kode sesuai plan dengan prinsip **Ponytail** & Clean Code (tanpa komentar yang digenerate).
   - **Jika project Playwright sudah ada**: Sekalian membuat/memperbarui kode skenario lanjutan (Page Object & Test Spec) pada project Playwright terkait.
3. **Eksekusi Verifikasi E2E & Sinkronisasi Graph**:
   - **Project Playwright Ada**: Jalankan test runner E2E via CLI (`run_command`) untuk memastikan skenario lolos tanpa regresi.
   - **Belum Ada $\rightarrow$ Pilih Buat di Session Lain**: Siapkan blok **Prompt Khusus Session Playwright** (berisi konteks fitur, URL/route, selector elemen UI, dan daftar skenario positif/negatif) untuk disajikan ke pengguna.
   - **Belum Ada $\rightarrow$ Pilih Pakai MCP**: Jalankan verifikasi UI menggunakan MCP Browser (`playwright` / `chrome-devtools`).
   - **Belum Ada $\rightarrow$ Pilih Skip / Manual**: Lewati langkah otomasi browser dan langsung menuju laporan akhir.
   - **Graphify Sync (Opsional - Hanya jika `graphify-out/` & CLI `graphify` tersedia)**: Jalankan update AST ringan (`graphify <path> --update --no-viz`) agar knowledge graph tetap sinkron.
4. Perbarui artifact `walkthrough.md`.
5. **STOP & STANDBY**:
   - Laporkan hasil eksekusi kode dan hasil verifikasi secara ringkas.
   - Sertakan blok kode ` ```sql ` berisi query DDL/DML (jika ada perubahan database/HBM) dan/atau blok **Prompt Playwright** (jika pengguna memilih buat di session lain).
   - Masuk ke status **STANDBY** (build & run di-handle pengguna via IntelliJ IDEA).
