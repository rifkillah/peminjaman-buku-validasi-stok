# Peminjaman Buku dan Validasi Ketersediaan Stok dengan Error Handling dan Automated Testing

Sistem sirkulasi perpustakaan **Pustaka Cendekia** yang menangani peminjaman dan pengembalian buku dengan validasi stok, penanganan error, serta pengujian otomatis.

## Fitur

- Dashboard ringkasan: eksemplar, stok tersedia, buku stok menipis, dan peminjaman jatuh tempo.
- Koleksi buku dengan pencarian dan filter status (tersedia/habis).
- Data anggota dengan filter status aktif/nonaktif.
- Peminjaman: validasi stok, jumlah, dan status anggota; stok berkurang otomatis.
- Pengembalian: stok bertambah dan denda dihitung otomatis.
- Denda keterlambatan **Rp1.000 per hari**.
- Halaman error kustom: 403, 404, 419, dan 500.
- Halaman laporan pengujian berisi 12 skenario automated test.
- Feature test dan unit test denda.

## Teknologi

- PHP 8.3+
- Laravel
- Blade Template
- MySQL

## Instalasi

1. Buat basis data MySQL:

```sql
CREATE DATABASE db_pustaka_cendekia;
```

2. Jalankan:

```bash
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate --seed
php artisan serve
```

Sesuaikan `DB_DATABASE`, `DB_USERNAME`, dan `DB_PASSWORD` pada `.env` bila perlu, lalu buka http://localhost:8000.

## Route

| Method | URI | Keterangan |
|---|---|---|
| GET | `/` | Dashboard |
| GET | `/buku` | Koleksi buku |
| GET | `/buku/{kode}` | Detail buku dan riwayat peminjaman |
| GET | `/anggota` | Daftar anggota |
| GET | `/peminjaman` | Daftar transaksi peminjaman |
| GET | `/peminjaman/baru` | Form peminjaman baru |
| POST | `/peminjaman` | Simpan peminjaman |
| GET | `/peminjaman/{kode}` | Detail peminjaman |
| POST | `/peminjaman/{kode}/kembali` | Proses pengembalian |
| GET | `/laporan-pengujian` | Laporan hasil automated test |

## Aturan Bisnis

- Peminjaman ditolak bila stok habis atau jumlah melebihi stok tersedia.
- Peminjaman ditolak untuk anggota berstatus nonaktif.
- Eksemplar terakhir tetap boleh dipinjam.
- Pengembalian ganda tidak menambah stok dua kali.
- Denda = jumlah hari keterlambatan x Rp1.000.

## Pengujian

```bash
php artisan test
```

`phpunit.xml` memakai database `db_pustaka_cendekia_test`; pastikan database tersebut tersedia.

## Struktur Penting

- `app/Http/Controllers/PeminjamanController.php` - validasi stok, transaksi, pengembalian, denda.
- `app/Http/Controllers/BukuController.php`, `AnggotaController.php`, `DashboardController.php`, `LaporanController.php`.
- `app/Models/Peminjaman.php`, `Buku.php`, `Anggota.php`, `Pengaturan.php`.
- `database/seeders/` - data buku, anggota, peminjaman, dan pengaturan.
