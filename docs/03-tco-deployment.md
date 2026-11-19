# 03 · TCO 12 Bulan + Keputusan Deployment · H8Mart Quick-Commerce Ops

**Nama:** Hani Alfiyyah

Harga dicek **16 Nov 2026**. Kurs asumsi **1 USD ≈ Rp16.500** / **1 EUR ≈ Rp17.800** (sesuaikan dengan kurs resmi hari submit bila berbeda).

## 1. Opsi yang Dibandingkan

| Opsi                               | Bentuk                                                                   | Biaya awal                                          | Biaya bulanan                                                                                                                                                                                                             | Biaya 12 bulan                                                     | Catatan kepatuhan / data                                                                                                                                                                                                                            |
| ---------------------------------- | ------------------------------------------------------------------------ | --------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Cloud (n8n Cloud)                  | n8n Cloud **Starter** (€20/mo billed annually)                           | €0 setup (trial lalu bayar)                         | €20 ≈ Rp356.000                                                                                                                                                                                                           | ≈ **Rp4.272.000**                                                  | Data workflow di cloud n8n (Frankfurt per pricing page). Sumber: [n8n.io/pricing](https://n8n.io/pricing/) 16 Nov 2026. Catatan: sync 15 menit ≈ 2.880 eksekusi/bulan → Starter 2.500 sering kurang; realistis naik **Pro** €50/mo ≈ Rp10,7jt/tahun |
| Self-hosted (VPS + Docker Compose) | AWS EC2 **t3.small** **ap-southeast-2 (Sydney)** + Caddy HTTPS + DuckDNS | Setup ~8 jam admin (Rp600.000 tenaga) + key pair    | Compute ≈ US$0,0264/jam × 730 ≈ **US$19,3**; IPv4 publik ≈ US$0,005/jam ≈ **US$3,7**; EBS 20 GiB ≈ **US$2**; DuckDNS **US$0**; admin 2 jam/bulan × Rp75.000 = Rp150.000 → total ≈ **US$25 + Rp150.000 ≈ Rp562.000**/bulan | ≈ **Rp6,7jt** (atau ~Rp4,3jt bila hanya biaya cloud tanpa tenaga)  | Data order/stok di VPS; port DB/mock/Kuma tidak publik. Harga On-Demand Linux `t3.small` Sydney **US$0,0264/jam** (dicek **16 Nov 2026**, [AWS EC2 Pricing](https://aws.amazon.com/ec2/pricing/) / public price list)                               |
| On-premise (server H8Mart)         | 1 mini-PC/server rack di kantor + listrik + UPS                          | Hardware ~Rp8–12jt (asumsi server bekas/baru entry) | Listrik ~100 W × 24 × 30 × Rp1.500/kWh ≈ Rp108.000; admin 4 jam/bulan ≈ Rp300.000; internet dedicated (asumsi) Rp500.000 → ≈ **Rp908.000**/bulan                                                                          | Hardware amortisasi 3 th ≈ Rp3jt/tahun + opex ≈ **Rp14jt** tahun-1 | Data tidak keluar kantor; paling sesuai klaim kedaulatan data, tapi CapEx tinggi untuk kelas/milestone                                                                                                                                              |

Rumus self-hosted bulanan (inti):  
`(harga_jam_EC2 + harga_jam_IPv4) × 730 + EBS_bulan + (jam_admin × tarif) + domain`.

## 2. Keputusan & Justifikasi

**Dipilih: Self-hosted VPS (Docker Compose + Caddy HTTPS).**

1. **Teknis:** Milestone mensyaratkan instance self-hosted Published + Telegram Trigger/approval butuh HTTPS publik; Cloud Starter rawan kehabisan eksekusi karena Schedule 15 menit.
2. **Bisnis:** Biaya kontrolable untuk lab/penilaian; Community Edition n8n tanpa meter eksekusi. Milestone ini memakai **AWS Free Tier credit US$100**; per **16 Nov 2026** estimasi terpakai ≈ **US$2,22** (Billing/Credits) — masih jauh di bawah kredit, selaras estimasi SETUP (~US$13 untuk jendela lab).
3. **Kepatuhan:** Data order & stok diproses di VPS yang kita kendalikan (bukan n8n Cloud). Merujuk **UU No. 27 Tahun 2022 tentang Perlindungan Data Pribadi**: **Pasal 16** (asas pemrosesan, termasuk kehati-hatian dan keamanan) dan **Pasal 39** (pengendali wajib melindungi data pribadi dari akses/pengungkapan tidak sah). Penerapan di solusi: secret hanya di credential n8n + `.env` (bukan repo); Header Auth webhook; role DB terpisah; port DB/mock/Kuma tidak publik; allowlist Telegram. (Bukan legal advice.)

## 3. Rencana Deployment (final)

| Item                                             | Nilai                                                                                                                                                                                  |
| ------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Penyedia & spesifikasi instance                  | AWS EC2, region **Asia Pacific (Sydney) / ap-southeast-2**, Ubuntu 24.04, **t3.small** (2 GiB) + swap 2 GiB                                                                            |
| `compose.yaml`: service yang dijalankan          | n8n + task runners, postgres, mock-api, uptime-kuma; VPS menambah **caddy** via `compose.vps.yaml`                                                                                     |
| Variabel penting di `.env` (tanpa nilai rahasia) | `DOMAIN_NAME`, `SSL_EMAIL`, `N8N_ENCRYPTION_KEY`, `WEBHOOK_TOKEN`, `MOCK_API_KEY`, password Postgres / role `mcp_reader` & `ops_writer`                                                |
| Database n8n                                     | SQLite bawaan di volume `n8n_data` (PostgreSQL stack hanya untuk data H8Mart)                                                                                                          |
| HTTPS (domain + reverse proxy + sertifikat)      | DuckDNS (`h8mart-hani.duckdns.org`) + Caddy automatic HTTPS                                                                                                                            |
| Production URL yang dipanggil penilai            | `https://h8mart-hani.duckdns.org` (path webhook di DM penilai)                                                                                                                         |
| Backup (apa, seberapa sering, uji restore)       | Export JSON workflow ke `workflows/` tiap perubahan material; volume Docker di-snapshot sebelum perubahan besar; uji restore = `docker compose up` + re-import export di instance baru |

## 4. Bukti Verifikasi

| Bukti                               | File                                                          |
| ----------------------------------- | ------------------------------------------------------------- |
| Browser HTTPS valid                 | `evidence/deploy/01-https.png`                                |
| `docker compose ps` di VPS          | `evidence/deploy/02-compose-ps.png`                           |
| `curl` Production URL dari luar VPS | `evidence/deploy/03-curl-outside.png` · `03-curl-outside.txt` |
