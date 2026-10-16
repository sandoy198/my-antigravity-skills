# Antigravity Skills Repository Steering Guidelines

Repositori ini adalah koleksi kustom *skills* dan otomasi *workflow* untuk Google Antigravity dan Gemini CLI.

## 📌 Prinsip & Tata Kelola Repositori

1. **Standar Struktur Skill**:
   - Setiap skill harus ditempatkan di dalam foldernya sendiri pada root repositori (contoh: `<nama-skill>/`).
   - Setiap folder skill wajib memiliki:
     - `SKILL.md`: Berisi YAML frontmatter lengkap (`name`, `description`) dan alur kerja terperinci.
     - `README.md`: Dokumentasi teknis terdedikasi untuk skill tersebut.
2. **Sinkronisasi Dokumentasi Wajib**:
   - Setiap kali terjadi penambahan atau pembaruan pada `SKILL.md`:
     - Mutakhirkan `<nama-skill>/README.md`.
     - Mutakhirkan tabel katalog dan panduan pada [README.md](README.md) utama di root repositori.
3. **Alur Kerja Pasca `git pull` & Penyiapan Commit**:
   - Ketika ada perubahan baru yang di-*pull* dari remote, cocokkan isi skill dengan dokumentasi yang ada.
   - Jika terdapat perbedaan atau penyesuaian baru, siapkan *staged commit* (`git add`) dengan pesan commit deskriptif (Conventional Commits) agar repositori selalu siap di-*push* berkala.
4. **Git Version Control**:
   - Seluruh folder skill, dokumentasi, dan aturan repositori harus selalu ter-track di Git untuk sinkronisasi tim.
