# 📋 PROGRESS: Money Tracker

> File ini adalah **sumber kebenaran** status pengerjaan. Baca di awal setiap sesi, perbarui di akhir setiap sesi.
> Spesifikasi lengkap ada di `MONEY_TRACKER_SPEC.md`.

**Terakhir diperbarui:** (isi tanggal)
**Tahap aktif:** Tahap 0
**Status keseluruhan:** Belum mulai

---

## Cara Memakai File Ini

1. **Awal sesi:** baca bagian *Status Ringkas*, *Tahap Aktif*, dan 3 entri terakhir di *Log*.
2. **Selama sesi:** kerjakan hanya item di tahap aktif, satu per satu. Jangan loncat tahap.
3. **Akhir sesi:** centang item selesai, tambah entri di *Log*, perbarui *Tahap aktif* dan *Next Up*.
4. Jika ada keputusan teknis penting, catat di *Keputusan Teknis*. Jika ada masalah belum beres, catat di *Masalah Terbuka*.

**Legenda:** `[ ]` belum · `[~]` sedang dikerjakan · `[x]` selesai dan sudah dites di HP · `[!]` terblokir

**Definition of Done tiap item:** kode jalan di HP tanpa error, tampil benar di mode terang dan gelap, empty state ada, tidak ada nominal memakai float.

---

## Status Ringkas

| Tahap | Nama | Status | Progres |
|---|---|---|---|
| 0 | Setup & fondasi | ⬜ Belum | 0/9 |
| 1 | Akun, kategori, transaksi | ⬜ Belum | 0/12 |
| 2 | Sumber pemasukan & dashboard | ⬜ Belum | 0/7 |
| 3 | Utang | ⬜ Belum | 0/12 |
| 4 | Multi mata uang | ⬜ Belum | 0/8 |
| 5 | Investasi & harga otomatis | ⬜ Belum | 0/11 |
| 6 | Budget, grafik, net worth, berulang | ⬜ Belum | 0/8 |
| 7 | Utilitas & polish | ⬜ Belum | 0/10 |

Ikon status: ⬜ belum · 🟡 berjalan · ✅ selesai

---

## Next Up

> Satu langkah konkret berikutnya (perbarui tiap akhir sesi):

- [ ] Tahap 0, item 0.1: buat project Expo

---

## Tahap 0: Setup & Fondasi

**Tujuan:** kerangka aplikasi jalan di HP dengan tema, navigasi, dan database siap.

- [ ] 0.1 Buat project Expo (`--template blank`), jalankan di Expo Go
- [ ] 0.2 Install library inti (lihat bagian Setup di SPEC)
- [ ] 0.3 Susun struktur folder `src/` sesuai SPEC
- [ ] 0.4 Theme system: `colors.js` (terang/gelap), `typography.js`, `spacing.js`, `ThemeProvider` (ikut sistem + manual)
- [ ] 0.5 Load font Plus Jakarta Sans + splash screen
- [ ] 0.6 Navigasi: bottom tab (Beranda, Transaksi, ＋, Utang, Investasi) + stack
- [ ] 0.7 Database: koneksi `expo-sqlite`, sistem **migrasi berversi**
- [ ] 0.8 Migrasi 001: tabel `settings`, `accounts`, `categories`, `income_sources`, `transactions` (sudah ada kolom mata uang)
- [ ] 0.9 Seed data: kategori bawaan + sumber pemasukan bawaan + akun "Tunai"

**Selesai jika:** app terbuka di HP, tab berpindah, tema terang/gelap berfungsi, DB terbuat dan terisi seed.

---

## Tahap 1: Akun, Kategori, Transaksi

**Tujuan:** sudah bisa dipakai mencatat sehari-hari.

- [ ] 1.1 Komponen dasar: `Button`, `Card`, `Chip`, `SectionHeader`, `EmptyState`, `Snackbar`
- [ ] 1.2 Util `money.js` (format Rupiah, parse, semua integer) + `date.js`
- [ ] 1.3 Repository + store: accounts
- [ ] 1.4 Layar Akun: daftar, tambah, edit, arsip, saldo per akun
- [ ] 1.5 Repository + store: categories; layar kelola kategori (nama, ikon, warna)
- [ ] 1.6 Repository + store: transactions
- [ ] 1.7 Layar **Tambah Transaksi** (bottom sheet): segmented Keluar/Masuk/Transfer, `AmountInput` dengan keypad, chip kategori, akun, tanggal, catatan
- [ ] 1.8 Transfer antar akun (dua sisi, `transfer_pair_id`)
- [ ] 1.9 Layar Riwayat: dikelompokkan per hari, total harian
- [ ] 1.10 Edit dan hapus transaksi (swipe + konfirmasi + Undo)
- [ ] 1.11 Pencarian dan filter (kategori, tanggal, akun, kata kunci)
- [ ] 1.12 Saldo akun terhitung benar setelah tambah/edit/hapus (uji manual)

**Selesai jika:** bisa catat masuk/keluar/transfer, saldo akurat, data tetap ada setelah app ditutup.

---

## Tahap 2: Sumber Pemasukan & Dashboard

- [ ] 2.1 CRUD sumber pemasukan (custom)
- [ ] 2.2 Pemasukan wajib memilih sumber
- [ ] 2.3 `MonthSwitcher` + query ringkasan bulanan
- [ ] 2.4 `BalanceCard` (gradient, count-up, tombol sembunyikan nominal)
- [ ] 2.5 Beranda: kartu akun, ringkasan bulan, donut pengeluaran per kategori, transaksi terbaru
- [ ] 2.6 Grafik kontribusi sumber pemasukan (persentase)
- [ ] 2.7 Perbandingan dengan bulan sebelumnya

---

## Tahap 3: Utang

Urutan: CC → paylater/cicilan → pinjaman → piutang → analisis.

- [ ] 3.1 Migrasi 002: `debts`, `credit_cards`, `debt_payments`
- [ ] 3.2 Kartu kredit: data kartu, limit, tanggal cetak/jatuh tempo
- [ ] 3.3 Transaksi via CC (utang bertambah, kas tidak berkurang)
- [ ] 3.4 Tagihan bulan ini, minimum payment, catat pembayaran (penuh/min/sebagian)
- [ ] 3.5 Estimasi bunga CC
- [ ] 3.6 Paylater/cicilan: pokok, tenor, bunga/admin, jadwal, progres
- [ ] 3.7 Status per bulan (lunas/belum/telat) + perkiraan tanggal lunas
- [ ] 3.8 Pinjaman lain (KTA, KPR, kendaraan, pinjol)
- [ ] 3.9 Utang ke orang dan piutang (pembayaran sebagian)
- [ ] 3.10 Layar Utang: ringkasan (total, cicilan/bulan, DTI) + tab jenis
- [ ] 3.11 Kalender jatuh tempo + simulasi snowball/avalanche
- [ ] 3.12 Notifikasi jatuh tempo (H-3, H-1)

---

## Tahap 4: Multi Mata Uang

- [ ] 4.1 Migrasi 003: `exchange_rates` (cek kolom mata uang di tabel lain sudah lengkap)
- [ ] 4.2 Service kurs (Frankfurter/ExchangeRate-API), cache + waktu update
- [ ] 4.3 Kurs manual (override)
- [ ] 4.4 Akun dengan mata uang berbeda
- [ ] 4.5 Input transaksi valas (nominal asli, kurs, nilai IDR tersimpan)
- [ ] 4.6 Transfer beda mata uang + biaya admin
- [ ] 4.7 Total, dashboard, net worth dikonversi ke IDR
- [ ] 4.8 Layar Kurs (daftar, refresh, override)

---

## Tahap 5: Investasi & Harga Otomatis

- [ ] 5.1 Migrasi 004: `investments`, `investment_transactions`, `price_history`
- [ ] 5.2 CRUD aset + transaksi beli/jual/dividen/bunga
- [ ] 5.3 Beli memotong akun tanpa jadi pengeluaran; dividen jadi pemasukan
- [ ] 5.4 Hitung modal, nilai sekarang, untung/rugi (Rp, %)
- [ ] 5.5 Update harga manual + riwayat harga
- [ ] 5.6 Antarmuka `PriceProvider` + fallback ke manual
- [ ] 5.7 Provider kripto (CoinGecko)
- [ ] 5.8 Provider saham IDX/AS (Yahoo, modular)
- [ ] 5.9 Provider emas (XAU → IDR/gram)
- [ ] 5.10 Layar Investasi: kartu portofolio, grafik pertumbuhan, donut alokasi, daftar aset (badge Auto/Manual)
- [ ] 5.11 Laba/rugi kurs dipisah dari laba/rugi harga

> Sebelum 5.7–5.9: cek syarat, kuota, dan endpoint terbaru tiap API.

---

## Tahap 6: Budget, Grafik, Net Worth, Berulang

- [ ] 6.1 Budget per kategori + progress bar + peringatan 80%/100%
- [ ] 6.2 Saran pengeluaran harian dari sisa budget
- [ ] 6.3 Grafik tren harian/bulanan
- [ ] 6.4 Net worth = saldo + investasi + piutang − utang
- [ ] 6.5 Grafik pergerakan net worth (snapshot harian/bulanan)
- [ ] 6.6 Transaksi berulang (`recurring_rules`) + eksekusi saat app dibuka
- [ ] 6.7 Pemasukan berulang (gaji)
- [ ] 6.8 Template transaksi favorit

---

## Tahap 7: Utilitas & Polish

- [ ] 7.1 Export CSV
- [ ] 7.2 Import CSV (dengan validasi + preview)
- [ ] 7.3 Backup dan restore database
- [ ] 7.4 Kunci aplikasi: PIN + biometrik
- [ ] 7.5 Mode privasi
- [ ] 7.6 Notifikasi pengingat harian + budget
- [ ] 7.7 Animasi dan haptic (count-up, donut, item masuk)
- [ ] 7.8 Skeleton loading + empty state semua layar
- [ ] 7.9 Aksesibilitas (kontras, label, ukuran font)
- [ ] 7.10 Build APK via EAS dan install di HP

---

## Log Pengerjaan

> Entri terbaru di **atas**. Format tetap agar mudah dibaca ulang.

### Template entri
```
### [YYYY-MM-DD] Sesi N: judul singkat
**Tahap/item:** contoh 1.7
**Dikerjakan:**
- ...
**File diubah/dibuat:**
- src/...
**Cara tes:** langkah singkat memverifikasi di HP
**Masalah/temuan:** ...
**Next:** langkah berikutnya
```

### [tanggal] Sesi 0: Perencanaan
**Dikerjakan:**
- Spesifikasi final dibuat (`MONEY_TRACKER_SPEC.md`)
- File progress dibuat
**Next:** Tahap 0, item 0.1

---

## Keputusan Teknis

> Catat keputusan penting agar tidak diperdebatkan ulang.

| Tanggal | Keputusan | Alasan |
|---|---|---|
| (awal) | Nominal disimpan sebagai integer | Hindari error pembulatan float |
| (awal) | Kolom mata uang ada sejak tahap 1 | Hindari migrasi data di tahap 4 |
| (awal) | Migrasi database berversi | Skema aman berubah tanpa kehilangan data |
| (awal) | Repository pattern, UI tidak menulis SQL | Kode rapi dan mudah diuji |
| (awal) | Provider harga modular, satu file per sumber | API gratis bisa berubah sewaktu-waktu |
| (awal) | Operasi multi-tabel dibungkus SQL transaction | Data konsisten (misal bayar CC) |

---

## Masalah Terbuka

| # | Masalah | Tahap | Status |
|---|---|---|---|
| — | (belum ada) | — | — |

---

## Ide / Backlog (di luar rencana)

- iOS (butuh akun Apple Developer)
- Sinkronisasi cloud (Supabase/Firebase)
- Scan struk (OCR)
- Widget home screen
- Import mutasi rekening

---

## Aturan Main untuk Asisten AI

1. Baca `PROGRESS.md` dan `MONEY_TRACKER_SPEC.md` sebelum menulis kode.
2. Kerjakan **hanya** item di tahap aktif, berurutan. Jangan menambah fitur di luar daftar.
3. Semua nominal **integer**. Jangan pakai float untuk uang.
4. UI tidak boleh menulis SQL langsung; lewat repository.
5. Setiap layar wajib: mode terang + gelap, empty state, dan warna dari theme (tidak hard-code).
6. Perubahan skema **selalu lewat file migrasi baru**, jangan ubah migrasi lama yang sudah dipakai.
7. Beri kode lengkap per file (bukan potongan), sebutkan path-nya, dan beri **langkah tes di HP**.
8. Jika ada API eksternal, cek dokumentasi terbaru dan beri fallback saat gagal.
9. Di akhir sesi: perbarui centang, tabel status, *Next Up*, dan tambah entri *Log*.
10. Jika ada yang ambigu, ajukan **satu** pertanyaan singkat lalu lanjut dengan asumsi yang dicatat.