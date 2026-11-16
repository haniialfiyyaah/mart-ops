# 04 · Governance & Integration Readiness Checklist · H8Mart Quick-Commerce Ops

**Nama:** Hani Alfiyyah

Status mengacu ke desain + export di repo. Bukti yang sudah ada: `evidence/deploy/` (HTTPS, compose ps, curl luar) dan canvas WF1–WF5. Bukti spesifik lain (retry, approval, Kuma Down, dll.) dilengkapi bertahap di folder `evidence/wf*`.

## 1. Credential & Akses

| Pemeriksaan                                                                              | Status                    | Bukti                                                                                                      |
| ---------------------------------------------------------------------------------------- | ------------------------- | ---------------------------------------------------------------------------------------------------------- |
| Semua credential disimpan di n8n Credentials, bukan di node/Code/repo                    | Siap                      | Export memakai `GANTI-ID-*`; tidak ada token bot/OpenAI di JSON                                            |
| Role DB dipisah: `mcp_reader` (read-only) untuk baca, `ops_writer` untuk menulis `ops.*` | Siap                      | WF2/WF3 tools + WF4 `Load Order` memakai `PG mcp_reader`; tulis pakai `PG ops_writer`; `seed/03-roles.sql` |
| Webhook dilindungi Header Auth                                                           | Siap                      | Node webhook: `Header Auth Webhook` (`X-H8Mart-Token`)                                                     |
| `.env` tidak ada di repo; `.env.example` tanpa nilai nyata                               | Siap                      | `.gitignore` + `.env.example`                                                                              |
| Token push Uptime Kuma diganti `ISI_TOKEN_PUSH_KUMA` di JSON export sebelum commit       | Siap                      | WF1 `Heartbeat Uptime Kuma` URL di `workflows/wf1-sinkron-order-digest.json`                               |
| Token bot Telegram hanya di credential n8n (bukan repo, export, screenshot)              | Siap                      | `chatId` = `ISI_CHAT_ID`; credential `GANTI-ID-TELEGRAM`                                                   |
| Chat Telegram dibatasi allowlist `chat_id` ke `merchant_id`; chat asing ditolak          | Siap (uji asing menyusul) | WF3b + `ops.telegram_merchants`; evidence penolakan asing: `evidence/wf3/` saat akun kedua tersedia        |
| Akses ke instance n8n dibatasi (user/owner, port yang terbuka)                           | Siap                      | SG: 22 My IP; 80/443 publik; 5690/5440/8080/3011 tidak dibuka; Caddy hanya proxy n8n · `evidence/deploy/`  |

## 2. Kesiapan Sistem Existing

| Sistem                  | Cara terhubung                                         | Rate limit / SLA yang diketahui | Perilaku bila down                                                     | Siapa dihubungi            |
| ----------------------- | ------------------------------------------------------ | ------------------------------- | ---------------------------------------------------------------------- | -------------------------- |
| Merchant Stock API      | HTTP `mock-api:8080` + `X-Api-Key`                     | Lab mock; retryOnFail di WF1    | Merchant → dead-letter + alert; merchant lain lanjut                   | Ops / maintainer mock      |
| Payment API             | HTTP vouchers/refunds + `Idempotency-Key`              | Lab mock; retryOnFail           | Hard fail → dead-letter / Error Workflow                               | Finance Ops                |
| Notification (mock API) | POST `/notify` channels ops-alerts / ops-digest        | Lab mock                        | Alert Telegram tetap jalur utama                                       | Ops                        |
| Telegram Bot API        | Credential Telegram; Send / Send and Wait / Trigger    | Rate Telegram Bot API           | Approval/digest tertunda; WF5 alert gagal kirim terlihat di Executions | Ops (BotFather / akun Ops) |
| PostgreSQL              | Host `postgres`; role terpisah                         | Lokal Docker healthcheck        | Workflow gagal koneksi → Error Workflow                                | Admin VPS                  |
| LLM provider            | OpenAI credential · model `gpt-6-luna` · Responses API | Kuota key instruktur            | Fallback / eskalasi WF3; jangan hasil rekaan                           | Instruktur (key kelas)     |

## 3. Pra-Production

| Pemeriksaan                                                         | Status                     | Bukti                                                                                                     |
| ------------------------------------------------------------------- | -------------------------- | --------------------------------------------------------------------------------------------------------- |
| Error Workflow terpasang di semua workflow                          | Siap                       | Settings WF1–WF4 → WF5; `workflows/wf5-reliability-error-workflow.json`                                   |
| Retry dan dead-letter diuji dengan kegagalan disengaja              | Siap (lengkapi screenshot) | M009 dead-letter; Payment `fail-next`; tambahkan PNG ke `evidence/wf1/` · `wf4/` · `wf5/`                 |
| Heartbeat + deteksi sinkron 0 baris / digest kosong + ambang durasi | Siap (lengkapi screenshot) | Node ada di WF1; bukti Up/Down & silent-fail → `evidence/wf5/` (Kuma merah saat schedule off = normal)    |
| Data pelanggan di log/alert tidak berlebihan (masking)              | Siap                       | Alert pakai business key (`order_id` / `merchant_id`), bukan PII penuh                                    |
| Rencana rollback / Unpublish                                        | Siap                       | Unpublish workflow bermasalah; re-import dari `workflows/`; kedua Schedule WF1 sudah `disabled` di export |
| Biaya token LLM dipantau                                            | Siap                       | Usage di dashboard provider; batasi uji WF3; Schedule Digest dimatikan di VPS/export agar tidak spam      |
