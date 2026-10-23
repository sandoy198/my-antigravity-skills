---
name: scaffold-steering
description: >
  Scaffold and initialize workspace steering files (AGENTS.md, .agents/rules/, .agents/skills/, docs/) via an interactive "Grill-Me" interview tailored to the detected project stack.
  Supports smart-merge on existing files, migration from legacy GEMINI.md to AGENTS.md, and Playwright E2E project mapping for orchestrator integration.
  Trigger when the user says "init steering", "scaffold steering", "setup rules", "buat steering files", "inisialisasi aturan proyek", or "/scaffold-steering".
---

# Scaffold Steering & Interactive Setup Skill

Inisialisasi aturan proyek (`AGENTS.md`, `.agents/`, `docs/`) secara modular, *vendor-neutral*, dan *version-controlled* di Git. Agen wajib mendeteksi stack terlebih dahulu dan melakukan *Grill-Me interview* singkat sebelum membuat folder/file agar bebas dari *bloat* atau template generik yang tidak relevan.

---

## 🛡️ Kebijakan Non-Destruktif & Smart-Merge (CRITICAL)

1. **Zero Source-Code Modification**: Dilarang mengubah/menghapus kode sumber proyek (`src/`, `app/`, `web/`, dll.).
2. **Preserve & Smart-Merge**: Jika file target (`AGENTS.md`, `GEMINI.md`, `docs/prd.md`, `.agents/rules/*.md`) sudah ada:
   - **WAJIB BACA TERLEBIH DAHULU** sebelum menulis.
   - Pertahankan seluruh aturan kustom dan konteks yang sudah ada; hanya tambahkan/perbarui bagian yang belum ada sesuai hasil interview.
3. **Migrasi `GEMINI.md` $\rightarrow$ `AGENTS.md`**:
   - Jika `GEMINI.md` ada namun `AGENTS.md` belum ada: Pindahkan seluruh isinya ke `AGENTS.md`, lalu ubah isi `GEMINI.md` menjadi pointer ringan:
     ```markdown
     # Project Steering
     See [AGENTS.md](AGENTS.md) for full project rules and instructions.
     ```
4. **On-Demand Creation & Token Budget Cap (Anti-Bloat)**:
   - Jangan membuat folder/file kosong atau rule yang tidak relevan dengan stack proyek.
   - **Tokenomics Rule**: File `AGENTS.md` wajib ditulis ultra-padat (**maksimal 80 baris**) agar tidak membebani *input token* di setiap sesi percakapan. Detail panjang wajib dipisah ke `.agents/rules/` atau `docs/`.
5. **Git Tracked**: Pastikan file steering tidak masuk ke `.gitignore` (kecuali diminta pengguna) agar tersinkronisasi di repositori tim.

---

## 🔄 Alur Eksekusi (Sequential Workflow)

### Step 1: Deteksi Stack, Graphify & Konfigurasi Eksisting
Periksa root workspace secara terarah (misal: `build.xml`, `pom.xml`, `package.json`, `go.mod`, `composer.json`, struktur folder `src/` atau `web/`, keberadaan `graphify-out/graph.json`, serta `AGENTS.md` / `GEMINI.md`) tanpa melakukan *full recursive scan* yang lambat.

### Step 2: Grill-Me Interview (Tanya Dulu Sebelum Generate!)
Sajikan ringkasan singkat stack yang terdeteksi, lalu ajukan pertanyaan konfirmasi ke developer (gunakan tool `ask_question` atau daftar pertanyaan ringkas):

1. **Arsitektur, Build & Database**:
   - Konfirmasi framework/ORM utama (misal: Struts/WebWork, JSP, Hibernate `.hbm.xml`, Spring, atau Node/Go) dan cara build/run (misal: dikelola via IntelliJ IDEA atau CLI tertentu).
2. **Integrasi E2E Playwright & Graphify (Sinkronisasi Orchestrator)**:
   - *"Apakah aplikasi ini sudah memiliki project E2E Playwright mandiri? Jika ada, di mana path foldernya dan apa command test runner-nya?"*
   - Jika `graphify-out/` belum ada: *"Apakah ingin sekalian mengaktifkan indeks Knowledge Graph (`graphify`) atau mencatat statusnya di `AGENTS.md` untuk menghemat token pencarian?"*
3. **Kebutuhan Rule Tambahan & Dokumen (`docs/prd.md` / `docs/references/`)**:
   - *"Selain `AGENTS.md` utama, apakah perlu dibuatkan file terpisah untuk `.agents/rules/` (code conventions / API / security), `docs/prd.md`, atau folder `docs/references/`?"*

**STOP & TUNGGU** jawaban developer sebelum membuat file.

### Step 3: Generate File Steering Sesuai Jawaban

1. **`AGENTS.md` (Wajib - Root Proyek, Maksimal 80 Baris)**:
   - Ikhtisar arsitektur, tech stack, dan konvensi kode spesifik proyek (pola Action/Controller, Service, DAO, atau Page Object).
   - **Codebase Hot-Paths (Token-Saver Map)**: Daftar path eksak ke direktori modul inti (misal: letak folder Action, DAO, `.hbm.xml`, JSP/UI, dan konfigurasi utama) agar subagent tidak perlu melakukan *blind search/grep* berulang.
   - Aturan Build & Environment (misal: *No CLI build* jika dikelola di IntelliJ IDEA, standar ANSI SQL untuk perubahan `.hbm.xml`).
   - **Mapping Orchestrator (Playwright & Graphify)**: Catat status keberadaan project Playwright (path & command runner) serta keberadaan `graphify-out/graph.json` (instruksi *Graphify-First query* sebelum baca file).
2. **`.agents/rules/*.md` (Opsional - Sesuai Permintaan/Stack)**:
   - Buat hanya jika diminta atau jika aturan terlalu panjang untuk ditaruh di `AGENTS.md` (misal: `code-conventions.md`, `api-standards.md`, atau `security-policies.md` yang disesuaikan dengan stack asli proyek, bukan template generik).
3. **`.agents/skills/` & `docs/` (Opsional - Sesuai Permintaan)**:
   - `docs/prd.md`: Konteks produk, *scope* sistem, dan *non-functional requirements*.
   - `docs/references/`: Folder dokumen referensi eksternal (API spec, skema DB).
   - `.agents/skills/<nama-skill>/SKILL.md`: Hanya jika proyek membutuhkan workflow otomasi khusus (testing/deployment).

### Step 4: Konfirmasi Akhir
Tampilkan ringkasan pohon direktori (*directory tree*) dari file yang berhasil dibuat atau diperbarui.
