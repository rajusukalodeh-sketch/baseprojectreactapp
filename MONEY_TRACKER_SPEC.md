# 💰 Money Tracker: Spesifikasi Final

> Aplikasi pencatat keuangan pribadi untuk **Android** (iOS ditunda), dibangun dengan **Expo (React Native) + JavaScript**. Offline-first, data tersimpan lokal, tampilan modern.

---

## Daftar Isi

1. [Ringkasan](#1-ringkasan)
2. [Teknologi](#2-teknologi)
3. [Fitur Lengkap](#3-fitur-lengkap)
4. [Desain UI/UX](#4-desain-uiux)
5. [Layout Layar Utama](#5-layout-layar-utama)
6. [Skema Database](#6-skema-database)
7. [Aturan Bisnis Penting](#7-aturan-bisnis-penting)
8. [Integrasi API](#8-integrasi-api)
9. [Struktur Project](#9-struktur-project)
10. [Roadmap Pengerjaan](#10-roadmap-pengerjaan)
11. [Setup & Build](#11-setup--build)
12. [Ditunda](#12-ditunda)

---

## 1. Ringkasan

| Item | Keputusan |
|---|---|
| Platform | Android (iOS ditunda) |
| Pemakaian | Pribadi, tidak dipublikasikan |
| Framework | Expo (React Native), JavaScript |
| Data | Lokal (SQLite), bisa offline penuh |
| Mata uang dasar | IDR (Rupiah), dengan dukungan multi mata uang |
| Distribusi | APK via EAS Build, install langsung ke HP |
| Bahasa UI | Indonesia |

**Tujuan utama:** satu aplikasi untuk mencatat arus kas, mengelola utang (CC, paylater, cicilan), memantau investasi, dan melihat **net worth** secara real-time.

---

## 2. Teknologi

### Inti
| Kebutuhan | Pilihan |
|---|---|
| Framework | Expo SDK terbaru + JavaScript |
| Navigasi | React Navigation (bottom tabs + native stack) |
| Database | `expo-sqlite` |
| State | Zustand |
| Grafik | `react-native-gifted-charts` + `react-native-svg` |

### UI & Pengalaman
| Kebutuhan | Pilihan |
|---|---|
| Animasi | `react-native-reanimated` |
| Gesture | `react-native-gesture-handler` (swipe to delete/edit) |
| Bottom sheet | `@gorhom/bottom-sheet` |
| Ikon | `lucide-react-native` |
| Font | Plus Jakarta Sans via `@expo-google-fonts/plus-jakarta-sans` |
| Haptic | `expo-haptics` |
| Gradient | `expo-linear-gradient` |

### Utilitas
| Kebutuhan | Pilihan |
|---|---|
| Kunci aplikasi | `expo-local-authentication` + `expo-secure-store` (PIN) |
| Notifikasi lokal | `expo-notifications` |
| Export/Import/Backup | `expo-file-system`, `expo-sharing`, `expo-document-picker` |
| Tanggal | `date-fns` |
| Kalkulasi uang | Simpan dalam **integer** (satuan terkecil), hindari float |

---

## 3. Fitur Lengkap

### 3.1 Transaksi
- Catat **pemasukan**, **pengeluaran**, dan **transfer** antar akun
- Field: nominal, kategori, tanggal, akun, catatan, mata uang
- Riwayat dikelompokkan per hari, urut terbaru
- Edit dan hapus (swipe atau tap)
- Filter dan pencarian: kategori, tanggal, akun, rentang nominal, kata kunci
- Kategori bawaan + custom (nama, ikon, warna)
- Transaksi berulang (langganan, gaji bulanan, tagihan tetap)
- Template/transaksi favorit untuk input cepat

### 3.2 Akun / Dompet
- Jenis: tunai, rekening bank, e-wallet, (kripto opsional)
- Setiap akun punya **mata uang sendiri**
- Saldo per akun dan saldo total
- Transfer antar akun (termasuk beda mata uang, dengan biaya admin)
- Arsip akun yang sudah tidak dipakai

### 3.3 Multi-Sumber Pemasukan
- Sumber custom: Gaji, Freelance, Bisnis, Dividen, Bunga, Bonus, Hadiah, dll
- Setiap pemasukan ditandai sumbernya
- Grafik dan persentase kontribusi tiap sumber
- Pemasukan berulang otomatis

### 3.4 Utang

**Kartu kredit**
- Limit, tanggal cetak tagihan, tanggal jatuh tempo
- Pemakaian dan sisa limit (indikator warna)
- Tagihan bulan ini vs minimum payment
- Catat pembayaran: penuh, minimum, sebagian
- Estimasi bunga jika tidak dibayar penuh

**Paylater & cicilan**
- Pokok, tenor, bunga/admin, nominal per bulan
- Progres (misal 4 dari 12), sisa utang, perkiraan tanggal lunas
- Status per bulan: lunas, belum, telat

**Pinjaman lain**
- KTA, KPR, kredit kendaraan, pinjol
- Utang ke orang dan **piutang** (pembayaran sebagian, status)

**Analisis utang**
- Total utang dan total cicilan per bulan
- Kalender jatuh tempo
- Debt-to-income ratio dengan peringatan
- Simulasi pelunasan: **snowball** vs **avalanche**
- Pengingat jatuh tempo (H-3, H-1)

### 3.5 Investasi
- Aset: saham, reksa dana, emas, kripto, obligasi/SBN, deposito
- Transaksi: beli, jual, dividen/bunga/kupon
- Total modal, nilai sekarang, untung/rugi (Rp dan %)
- Harga otomatis via API atau manual per aset
- Alokasi aset (donut) dan pertumbuhan portofolio (line)
- Dividen/bunga otomatis tercatat sebagai pemasukan
- Beli aset memotong saldo akun tanpa dihitung sebagai pengeluaran
- Laba/rugi kurs dipisah dari laba/rugi harga

### 3.6 Multi Mata Uang
- Base currency: IDR
- Nominal asli + kurs + nilai IDR disimpan per transaksi (laporan lama tidak berubah)
- Kurs otomatis via API, cache untuk offline, tanda "terakhir update"
- Kurs manual (override)
- Net worth selalu dikonversi ke IDR dengan kurs terbaru

### 3.7 Ringkasan & Analisis
- Dashboard: saldo, pemasukan, pengeluaran, sisa bulan ini
- Ganti bulan
- Donut pengeluaran per kategori
- Tren harian/bulanan
- Perbandingan dengan bulan sebelumnya
- **Net worth** = saldo + investasi + piutang − utang, plus grafik pergerakannya

### 3.8 Budget
- Budget per kategori (bulanan)
- Progress bar dan peringatan (80% dan 100%)
- Sisa budget per hari (saran pengeluaran harian)

### 3.9 Utilitas
- Format Rupiah dan format sesuai mata uang
- Export/import CSV
- Backup/restore database (file `.db` atau `.json`)
- Kunci aplikasi (PIN / sidik jari)
- Mode terang, gelap, dan ikut sistem
- Notifikasi: jatuh tempo, pengingat catat harian, budget hampir habis
- Mode privasi: sembunyikan nominal dengan satu tap

---

## 4. Desain UI/UX

### 4.1 Prinsip Desain
1. **Cepat**: mencatat transaksi maksimal 3 tap dan 1 layar.
2. **Bersih**: banyak ruang kosong, satu fokus per layar.
3. **Angka adalah bintang**: nominal besar, tebal, mudah dibaca.
4. **Warna bermakna**: hijau = masuk, merah = keluar, ungu = investasi, jingga = peringatan utang.
5. **Halus**: animasi singkat (150–300 ms), haptic ringan pada aksi penting.
6. **Aman dari salah**: konfirmasi sebelum hapus, ada Undo lewat snackbar.

### 4.2 Gaya Visual
Modern fintech: kartu membulat, bayangan lembut, gradient halus di kartu utama, tipografi tegas.

- **Radius:** kartu 20, tombol 14, chip 999 (pill), bottom sheet 28
- **Bayangan:** lembut dan tipis (mode terang). Mode gelap memakai perbedaan warna surface, bukan bayangan
- **Kartu utama (saldo/net worth):** gradient indigo → violet
- **Ikon:** Lucide, stroke 1.75, ukuran 20–24
- **Kategori:** lingkaran berwarna pastel dengan ikon di tengah

### 4.3 Palet Warna

**Warna semantik (sama di kedua mode)**

| Peran | Hex | Pemakaian |
|---|---|---|
| Primary | `#6366F1` | Tombol utama, tab aktif, aksen |
| Primary Dark | `#4F46E5` | Pressed, gradient |
| Income | `#10B981` | Pemasukan, untung |
| Expense | `#F43F5E` | Pengeluaran, rugi |
| Invest | `#8B5CF6` | Investasi |
| Debt | `#F59E0B` | Utang, peringatan |
| Info | `#0EA5E9` | Transfer, informasi |

**Mode terang**

| Token | Hex |
|---|---|
| `background` | `#F8FAFC` |
| `surface` | `#FFFFFF` |
| `surfaceAlt` | `#F1F5F9` |
| `border` | `#E2E8F0` |
| `textPrimary` | `#0F172A` |
| `textSecondary` | `#64748B` |
| `textMuted` | `#94A3B8` |

**Mode gelap**

| Token | Hex |
|---|---|
| `background` | `#0B1120` |
| `surface` | `#131C2E` |
| `surfaceAlt` | `#1B2740` |
| `border` | `#26334D` |
| `textPrimary` | `#F1F5F9` |
| `textSecondary` | `#94A3B8` |
| `textMuted` | `#64748B` |

> Di mode gelap, warna semantik dibuat sedikit lebih terang agar kontras tetap nyaman (misal Income `#34D399`, Expense `#FB7185`).

**Palet kategori (12 warna pastel untuk pilihan custom)**
`#F97316` `#EAB308` `#84CC16` `#10B981` `#14B8A6` `#06B6D4` `#3B82F6` `#6366F1` `#8B5CF6` `#D946EF` `#EC4899` `#F43F5E`

### 4.4 Tipografi
Font: **Plus Jakarta Sans** (modern, ramah, angkanya jelas).

| Gaya | Ukuran / Berat | Pemakaian |
|---|---|---|
| Display | 36 / 800 | Saldo dan net worth utama |
| Title 1 | 24 / 700 | Judul layar |
| Title 2 | 18 / 700 | Judul seksi |
| Body | 15 / 500 | Teks umum |
| Caption | 13 / 500 | Keterangan, tanggal |
| Micro | 11 / 600 | Label kecil, badge |

Nominal memakai **tabular numbers** agar digit sejajar di daftar.

### 4.5 Spacing & Grid
- Skala 4 pt: `4, 8, 12, 16, 20, 24, 32, 40`
- Padding layar horizontal: **20**
- Jarak antar kartu: **12–16**
- Area sentuh minimal **48×48**

### 4.6 Navigasi

Bottom tab dengan tombol tambah di tengah:

```
┌─────────────────────────────────────────────┐
│  🏠        🧾       [ ＋ ]       💳        📈  │
│ Beranda  Transaksi            Utang   Investasi│
└─────────────────────────────────────────────┘
```

- **＋ (FAB tengah)** membuka bottom sheet "Tambah Transaksi" (tab bar tidak berganti layar).
- **Menu "Lainnya"** (akun, budget, kategori, kurs, pengaturan) diakses dari ikon di header Beranda.
- Layar detail memakai stack dengan transisi slide halus.
- Aksi cepat: **tekan lama** tombol ＋ untuk memilih Pemasukan / Pengeluaran / Transfer.

### 4.7 Komponen Inti

| Komponen | Deskripsi |
|---|---|
| `BalanceCard` | Kartu gradient: net worth atau saldo, tombol sembunyikan nominal |
| `StatPill` | Kapsul kecil: pemasukan (↑ hijau) dan pengeluaran (↓ merah) |
| `TransactionItem` | Ikon kategori, judul, akun, nominal berwarna, swipe untuk hapus/edit |
| `SectionHeader` | Judul seksi + aksi "Lihat semua" |
| `AmountInput` | Angka besar, keypad kustom, format ribuan langsung |
| `CategoryChip` | Pill dengan ikon, terpilih = latar berwarna |
| `ProgressBar` | Untuk budget, cicilan, limit CC (warna berubah 70% → 90%) |
| `DonutChart` | Dengan total di tengah dan legend di bawah |
| `MonthSwitcher` | Panah kiri/kanan + nama bulan, bisa swipe |
| `EmptyState` | Ilustrasi sederhana + penjelasan + tombol aksi |
| `Snackbar` | Notifikasi bawah dengan aksi Undo |
| `Skeleton` | Placeholder saat memuat |

### 4.8 Input Transaksi (Layar Terpenting)

Dirancang agar secepat mungkin:

1. Segmented control: **Keluar / Masuk / Transfer**
2. Nominal besar di tengah dengan keypad kustom (tombol `000` untuk mempercepat)
3. Baris chip kategori (scroll horizontal, kategori terakhir dipakai muncul duluan)
4. Baris ringkas: **Akun** · **Tanggal** (default hari ini) · **Catatan**
5. Tombol **Simpan** lebar di bawah + haptic sukses

Jika akun berbeda mata uang, muncul baris kurs otomatis yang bisa diedit.

### 4.9 Animasi & Interaksi
- Angka saldo **count-up** saat pertama muncul
- Donut dan bar **menggambar** dengan animasi singkat
- Item baru **fade + slide** masuk ke daftar
- Progress bar terisi halus
- Haptic: ringan saat simpan, sedang saat hapus, peringatan saat budget terlampaui
- Pull-to-refresh untuk memperbarui kurs dan harga
- Transisi mode terang/gelap dengan fade

### 4.10 Status Kosong & Error
- Setiap layar punya **empty state** yang mengajak bertindak ("Belum ada transaksi. Catat yang pertama!")
- Gagal ambil harga/kurs: tampilkan data terakhir + label "Diperbarui 2 jam lalu" (bukan error penuh)
- Semua aksi destruktif: konfirmasi atau Undo

### 4.11 Aksesibilitas
- Kontras teks minimal 4.5:1
- Warna tidak jadi satu-satunya penanda (tambahkan ikon ↑ ↓ dan tanda + / −)
- Dukung ukuran font sistem
- `accessibilityLabel` pada semua tombol ikon

---

## 5. Layout Layar Utama

### 5.1 Beranda

```
┌───────────────────────────────┐
│ Halo, Nama 👋            ⚙️ 👁 │
│                               │
│ ╔═══════════════════════════╗ │
│ ║ Net Worth                 ║ │
│ ║ Rp 125.400.000            ║ │
│ ║ ▲ 2,4% bulan ini          ║ │
│ ║ [↑ Masuk 8jt] [↓ Keluar 5jt]║ │
│ ╚═══════════════════════════╝ │
│                               │
│ Akun                    Semua │
│ [BCA] [GoPay] [Tunai] [USD]→  │
│                               │
│ Bulan ini: ‹ September 2026 › │
│ ┌───────────┐ ┌─────────────┐ │
│ │ Donut     │ │ Budget      │ │
│ │ kategori  │ │ ▓▓▓▓░ 72%   │ │
│ └───────────┘ └─────────────┘ │
│                               │
│ ⚠️ Jatuh tempo segera         │
│ CC BCA · 3 hari · Rp 2,1jt    │
│                               │
│ Transaksi terbaru       Semua │
│ 🍜 Makan siang      -Rp 35rb  │
│ 💼 Gaji            +Rp 8jt    │
└───────────────────────────────┘
```

### 5.2 Transaksi
- Header: pencarian + tombol filter
- Chip filter cepat: Semua · Masuk · Keluar · Transfer
- Ringkasan bulan di atas (masuk, keluar, selisih)
- Daftar dikelompokkan per tanggal dengan total harian

### 5.3 Utang
- Kartu ringkasan: **Total utang**, **Cicilan/bulan**, **DTI %** (dengan warna indikator)
- Tab segmen: **Kartu Kredit · Cicilan · Pinjaman · Piutang**
- Setiap item: nama, progress bar, sisa, jatuh tempo, badge status
- Tombol "Kalender jatuh tempo" dan "Simulasi pelunasan"

### 5.4 Investasi
- Kartu: nilai portofolio, untung/rugi (Rp dan %), toggle 1H / 1M / 1B / 1T / Semua
- Grafik line pertumbuhan
- Donut alokasi aset
- Daftar aset: kode, jumlah, harga terakhir, untung/rugi berwarna, badge **Auto / Manual**
- Label "Harga diperbarui: 14:32"

### 5.5 Lainnya
Akun · Kategori · Sumber Pemasukan · Budget · Transaksi Berulang · Kurs · Backup & Export · Keamanan · Tampilan · Notifikasi

---

## 6. Skema Database

> Semua nominal disimpan sebagai **integer** (satuan terkecil, misal sen atau rupiah penuh). ID memakai UUID/text. Kolom mata uang sudah ada sejak awal.

```sql
-- Pengaturan
settings(key TEXT PRIMARY KEY, value TEXT)
-- base_currency, theme, pin_enabled, privacy_mode, dll

-- Akun
accounts(
  id, name, type,            -- cash | bank | ewallet | crypto
  currency, initial_balance,
  color, icon, is_archived, created_at
)

-- Kategori
categories(
  id, name, type,            -- income | expense
  icon, color, is_default, sort_order
)

-- Sumber pemasukan
income_sources(id, name, icon, color, is_archived)

-- Transaksi
transactions(
  id, type,                  -- income | expense | transfer | invest | debt_payment
  account_id, category_id, income_source_id,
  currency, amount_original, exchange_rate, amount_base,
  date, note,
  transfer_pair_id,          -- untuk transfer (dua sisi)
  related_type, related_id,  -- tautan ke debt / investment
  recurring_id, created_at, updated_at
)

-- Transaksi berulang
recurring_rules(
  id, template_json, frequency, day_of_month,
  next_run, end_date, is_active
)

-- Budget
budgets(id, category_id, amount_base, period, year_month)

-- Utang
debts(
  id, type,                  -- credit_card | paylater | loan | personal | receivable
  name, creditor, currency,
  principal, interest_rate, admin_fee, tenor_months,
  monthly_payment, start_date, due_day, status
)
credit_cards(
  debt_id, credit_limit, statement_day, due_day, min_payment_pct
)
debt_payments(
  id, debt_id, account_id, amount, currency,
  due_date, paid_date, status,  -- paid | pending | late
  transaction_id
)

-- Investasi
investments(
  id, name, symbol, asset_type,  -- stock | fund | gold | crypto | bond | deposit
  currency, price_source, auto_price,
  last_price, last_price_at
)
investment_transactions(
  id, investment_id, account_id, type,  -- buy | sell | dividend | interest
  quantity, price, fee, currency, exchange_rate, date, transaction_id
)
price_history(id, investment_id, date, price)

-- Kurs
exchange_rates(currency, rate_to_base, source, updated_at, is_manual)
```

**Index penting:** `transactions(date)`, `transactions(account_id)`, `transactions(category_id)`, `debt_payments(due_date)`, `price_history(investment_id, date)`.

---

## 7. Aturan Bisnis Penting

| Peristiwa | Efek |
|---|---|
| Pengeluaran biasa | Saldo akun berkurang, masuk laporan pengeluaran |
| Pengeluaran via CC/paylater | Saldo kas **tidak** berkurang, utang bertambah, tetap masuk laporan pengeluaran per kategori |
| Bayar tagihan CC/cicilan | Saldo akun berkurang, utang berkurang, **bukan pengeluaran baru** (kecuali bunga/admin) |
| Transfer antar akun | Bukan pemasukan/pengeluaran; selisih atau biaya admin dicatat sebagai pengeluaran |
| Beli aset | Saldo akun berkurang, nilai portofolio naik, **bukan pengeluaran** |
| Jual aset | Saldo naik; selisih terhadap modal = realized gain/loss |
| Dividen / bunga | Pemasukan dengan sumber sesuai jenis |
| Piutang dibayar | Saldo naik, piutang berkurang, bukan pemasukan |
| Transaksi valas | Simpan `amount_original`, `exchange_rate`, `amount_base` saat itu (tidak berubah lagi) |
| Net worth | Saldo semua akun + nilai investasi + piutang − utang (semua dalam IDR, kurs terbaru) |
| DTI | Total cicilan bulanan ÷ rata-rata pemasukan bulanan; peringatan di 30% (kuning) dan 40% (merah) |

---

## 8. Integrasi API

Setiap sumber dibuat sebagai **modul terpisah** dengan antarmuka sama, sehingga mudah diganti.

| Data | Sumber kandidat | Catatan |
|---|---|---|
| Kurs valas | Frankfurter / ExchangeRate-API | Gratis |
| Kripto | CoinGecko | Mendukung IDR |
| Saham IDX | Yahoo Finance (`BBCA.JK`) | Tidak resmi, bisa berubah, ada jeda |
| Saham AS / ETF | Yahoo Finance / Twelve Data / Alpha Vantage | Perlu API key, kuota terbatas |
| Emas | Harga XAU dikonversi ke IDR/gram | Perkiraan, bukan harga Antam |
| Reksa dana, SBN, deposito | **Manual** | Belum ada API gratis yang andal |

**Aturan umum**
- Refresh saat aplikasi dibuka dan saat pull-to-refresh
- Hasil disimpan ke DB lokal (offline tetap tampil + waktu update)
- Jika API gagal → fallback ke harga terakhir/manual, tanpa error yang mengganggu
- Batasi frekuensi request (throttle) agar tidak melebihi kuota
- **Cek syarat dan kuota terbaru** tiap API sebelum implementasi

Antarmuka modul harga:

```js
// src/services/prices/index.js
export interface PriceProvider {
  supports(asset)        // apakah aset didukung
  fetchPrice(asset)      // { price, currency, timestamp }
}
```

---

## 9. Struktur Project

```
money-tracker/
├── App.js
├── app.json
├── eas.json
└── src/
    ├── theme/
    │   ├── colors.js          # palet terang & gelap
    │   ├── typography.js
    │   ├── spacing.js
    │   └── ThemeProvider.js
    ├── db/
    │   ├── database.js        # koneksi & inisialisasi
    │   ├── migrations/        # versi skema (001_init.js, ...)
    │   └── repositories/      # accounts, transactions, debts, investments, ...
    ├── store/                 # Zustand: useAccounts, useTransactions, useSettings
    ├── services/
    │   ├── currency/          # kurs API + konversi
    │   ├── prices/            # coingecko.js, yahoo.js, gold.js, manual.js
    │   ├── notifications.js
    │   ├── backup.js
    │   ├── csv.js
    │   └── recurring.js       # eksekusi transaksi berulang
    ├── navigation/
    ├── screens/
    │   ├── Home/
    │   ├── Transactions/
    │   ├── AddTransaction/
    │   ├── Debts/
    │   ├── Investments/
    │   ├── Budget/
    │   ├── Accounts/
    │   └── Settings/
    ├── components/            # BalanceCard, TransactionItem, DonutChart, ...
    ├── utils/
    │   ├── money.js           # format, parse, konversi integer
    │   ├── date.js
    │   └── calc.js            # DTI, snowball/avalanche, bunga, P&L
    └── constants/
```

**Prinsip arsitektur**
- **Migrations berversi** dari awal, jadi skema bisa berubah tanpa kehilangan data
- **Repository pattern**: UI tidak menulis SQL langsung
- Logika hitung (DTI, P&L, snowball) berupa fungsi murni di `utils/calc.js` agar mudah diuji
- Operasi yang menyentuh banyak tabel (misal bayar CC) dibungkus **SQL transaction**

---

## 10. Roadmap Pengerjaan

| Tahap | Isi | Hasil |
|---|---|---|
| **0** | Setup project, tema, font, navigasi, DB + migrasi | Kerangka siap |
| **1** | Akun, kategori, transaksi (CRUD), input cepat, riwayat | Sudah bisa dipakai mencatat |
| **2** | Sumber pemasukan, ringkasan bulanan, dashboard | Beranda hidup |
| **3** | Utang: CC → paylater/cicilan → pinjaman → piutang, pengingat | Kewajiban terkontrol |
| **4** | Multi mata uang: kurs API + manual, transaksi valas | Siap akun valas |
| **5** | Investasi: transaksi, P&L, harga manual → otomatis per sumber | Portofolio berjalan |
| **6** | Budget, grafik lanjutan, net worth, transaksi berulang | Analisis lengkap |
| **7** | Export/import CSV, backup, kunci aplikasi, notifikasi, polish animasi | Siap pakai harian |

**Catatan:** kolom mata uang sudah ada sejak tahap 1 (UI valas baru di tahap 4), supaya tidak perlu migrasi data.

---

## 11. Setup & Build

### Development (Windows)
```bash
# 1. Install Node.js LTS dan VS Code
# 2. Buat project
npx create-expo-app@latest money-tracker --template blank
cd money-tracker

# 3. Install library
npx expo install expo-sqlite expo-font expo-haptics expo-linear-gradient \
  expo-local-authentication expo-secure-store expo-notifications \
  expo-file-system expo-sharing expo-document-picker \
  react-native-svg react-native-reanimated react-native-gesture-handler \
  react-native-safe-area-context react-native-screens
npm install zustand react-native-gifted-charts lucide-react-native date-fns \
  @react-navigation/native @react-navigation/bottom-tabs \
  @react-navigation/native-stack @gorhom/bottom-sheet \
  @expo-google-fonts/plus-jakarta-sans

# 4. Jalankan, lalu scan QR dengan Expo Go
npx expo start
```

### Build APK untuk dipakai harian
```bash
npm install -g eas-cli
eas login
eas build:configure
eas build -p android --profile preview   # hasil: file APK
```
Download APK dari link EAS, lalu install di HP (aktifkan "install dari sumber tidak dikenal").

> **Catatan:** beberapa fitur (notifikasi terjadwal, PIN/biometrik) bisa terbatas di Expo Go. Untuk pengujian penuh, gunakan **development build** atau APK preview.

---

## 12. Ditunda

- iOS (butuh akun Apple Developer)
- Sinkronisasi cloud antar perangkat (Supabase/Firebase)
- Scan struk dengan kamera/OCR
- Widget home screen Android
- Import mutasi rekening otomatis

---

## ✅ Langkah Berikutnya

1. Setup project dan **theme system** (warna, font, dark mode)
2. Buat **migrasi database v1** (akun, kategori, transaksi, pengaturan, dengan kolom mata uang)
3. Bangun layar **Tambah Transaksi** dan **Riwayat**
4. Bangun **Beranda** dengan BalanceCard dan ringkasan bulan