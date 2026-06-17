# PROJECT BRIEF: SISTEM MANAJEMEN ARISAN OTOMATIS (PREMIUM UNIVERSAL)

## 1. Ringkasan Sistem
Sistem ini adalah platform manajemen arisan *end-to-end* yang terotomatisasi secara penuh. Alur data dimulai dari **Google Form** yang terhubung ke **Spreadsheet**. Logika pemrosesan dijalankan melalui **Google Apps Script (`Kode.gs`)**, didukung oleh **Dashboard Operasional** admin dan **Portal Web Premium** untuk pengecekan mandiri oleh member dengan tampilan adaptif (Desktop & Mobile).

## 2. Arsitektur Data & Spreadsheet
Spreadsheet utama (ID: `16IU7WAWJR5KQWKc_fmJisTBnaZSOC1MgknAYFizMw3I`) terdiri dari:
*   **Data Pendaftar:** Penampung Google Form dengan flag `PROCESSED` di Kolom P.
*   **Master_Member:** Database utama dengan pembersihan NIK (safe string `'`) dan format `wa.me`.
*   **DASHBOARD:** Panel monitoring populasi member, grup aktif, dan target closing.
*   **25 Sheet Produk:** Kombinasi 5 Produk (KM, KB, MM, MB, LH) dan 5 Paket (49k s/d 999k).

## 3. Standarisasi Struktur Sheet Produk
*   **Universal 9-Column:** Seluruh produk (termasuk LH) WAJIB menggunakan **9 Kolom Pembayaran** (Kolom N s/d V) dan 1 Kolom Status **DONE** (Kolom W).
*   **Layout Data:** Data member dimulai dari **Baris 6**. Baris 1-5 adalah area judul dan label grup.
*   **LH Spacing:** Khusus produk LH, setiap grup memiliki jarak **103 baris** (100 slot member + 3 baris header) untuk menjaga keteraturan visual.
*   **Produk Lain:** Menggunakan spacing **13 baris** (9 slot member + 4 baris header).

## 4. Mekanisme Otomatisasi (Apps Script - `SIAP_COPY_KODE_GS.txt`)
1.  **📥 Tarik Pendaftar Baru (`pullNewRegistrations`):** Memindahkan data form ke database master dengan proteksi duplikasi NIK.
2.  **▶️ Proses Member ke Grup (`processAllPendingMembers`):** 
    *   **Surgical Write:** Script hanya menulis Nama & NIK (Kolom B-C) untuk mencegah kerusakan format checkbox yang sudah ada di spreadsheet.
    *   **Logic:** Menggunakan `maxCols = 9` secara universal untuk sinkronisasi portal.

## 5. Portal Member Premium (Frontend - `SIAP_COPY_PORTAL.html`)
Portal telah didesain ulang dengan pendekatan **Mobile-First (Android Optimized)**:
*   **Responsive Display:**
    *   **Desktop:** Menampilkan tabel riwayat pembayaran penuh dengan scroll horizontal yang halus.
    *   **Mobile (<767px):** Mengubah tabel menjadi **Card List** yang modern. Setiap member ditampilkan dalam satu kartu dengan grid status pembayaran (Emoji ✅/❌).
*   **UI/UX Premium:** 
    *   **Layout Dashboard:** Nama (Full Width), diikuti Grup, Paket, dan Produk dalam 3 kolom seimbang.
    *   **Premium Loading:** Spinner modern dengan efek blur background dan teks "Mengambil Data Member...".
    *   **Highlight Member:** Member yang sedang login akan ditandai dengan **Border Orange Glow** dan teks "(ANDA)".
    *   **Visual:** Menggunakan font *Plus Jakarta Sans* dan *Lucide Icons*.

## 6. Struktur Workspace & File Penting
Seluruh aset penting dipusatkan di folder `Documents/Arisan Project GeminiCli/`:
*   `SIAP_COPY_KODE_GS.txt`: Sumber kode backend final.
*   `SIAP_COPY_PORTAL.html`: Sumber kode frontend portal final.
*   `rebuild_dashboard_v3.py`: Script Python untuk perbaikan dashboard.
*   `generate_all_groups_v2.py`: Script Python untuk pembuatan grup massal.
*   `/Simulations`: Folder berisi script testing dan simulasi pendaftaran.

## 7. PROTOKOL KETAT (MUST READ)
1.  **DILARANG** mengubah `maxCols` di Apps Script menjadi selain **9** tanpa instruksi eksplisit.
2.  **Surgical Update:** Jangan menulis ulang seluruh kode sheet jika hanya diperlukan perubahan data identitas. Gunakan `.setValues()` hanya pada kolom yang dituju.
3.  **Deployment:** Selalu deploy sebagai Web App dengan akses "Anyone" agar Portal dapat diakses oleh member.
4.  **Data Safety:** Gunakan string kosong `""` untuk menghapus Nama. JANGAN menggunakan `.clear()` karena akan menghapus validasi checkbox.

---
*Dokumen ini diperbarui pada 13 Juni 2026. Versi ini adalah standar FINAL untuk Sistem Arisan Premium Universal.*
