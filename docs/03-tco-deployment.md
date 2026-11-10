# 03 · TCO 12 Bulan + Keputusan Deployment · H8Mart Quick-Commerce Ops

**Nama:**

> Template dokumen Milestone (target selesai W3D5 23.59, maksimal W4D4 23.59). Isi langsung berkas ini.
> Harga dan kurs ditulis dengan **tanggal dan sumber** (halaman harga resmi). Jangan menebak.

## 1. Opsi yang Dibandingkan

| Opsi | Bentuk | Biaya awal | Biaya bulanan | Biaya 12 bulan | Catatan kepatuhan / data |
| --- | --- | --- | --- | --- | --- |
| Cloud (n8n Cloud) | | | | | |
| Self-hosted (VPS + Docker Compose) | | | | | |
| On-premise (server H8Mart) | | | | | |

> Rincian self-hosted: instance, storage, backup, domain, waktu admin (jam × tarif). Rumus ditulis di kolom Catatan atau di bawah tabel.

## 2. Keputusan & Justifikasi

Opsi yang dipilih dan alasannya (dua alasan teknis atau bisnis, dan satu syarat kepatuhan yang relevan untuk data order dan pelanggan):

## 3. Rencana Deployment (final)

| Item | Nilai |
| --- | --- |
| Penyedia & spesifikasi instance | |
| `compose.yaml`: service yang dijalankan | |
| Variabel penting di `.env` (tanpa nilai rahasia) | |
| Database n8n | |
| HTTPS (domain + reverse proxy + sertifikat) | |
| Production URL yang dipanggil penilai | |
| Backup (apa, seberapa sering, uji restore) | |

## 4. Bukti Verifikasi

> Tempel path screenshot di `evidence/`: browser dengan HTTPS, `docker compose ps`, eksekusi Production berhasil.

| Bukti | File |
| --- | --- |
| | |
