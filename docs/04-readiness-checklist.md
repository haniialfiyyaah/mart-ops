# 04 · Governance & Integration Readiness Checklist · H8Mart Quick-Commerce Ops

**Nama:**

> Template dokumen Milestone (target selesai W3D5 23.59, maksimal W4D4 23.59). Isi langsung berkas ini.
> Setiap baris diberi status (Siap / Belum / N/A) **dan bukti** (file, screenshot, atau query). Status tanpa bukti dianggap Belum.

## 1. Credential & Akses

| Pemeriksaan | Status | Bukti |
| --- | --- | --- |
| Semua credential disimpan di n8n Credentials, bukan di node/Code/repo | | |
| Role DB dipisah: `mcp_reader` (read-only) untuk WF2 dan membaca di WF3, `ops_writer` untuk menulis `ops.*` (bukti: nama credential di export) | | |
| Webhook dilindungi Header Auth | | |
| `.env` tidak ada di repo; `.env.example` tanpa nilai nyata | | |
| Token push Uptime Kuma diganti `ISI_TOKEN_PUSH_KUMA` di JSON export sebelum commit | | |
| Token bot Telegram hanya di credential n8n (bukan repo, export, screenshot) | | |
| Chat Telegram dibatasi allowlist `chat_id` ke `merchant_id`; chat asing ditolak (N/A bila chat asisten lewat Telegram tidak dikerjakan) | | |
| Akses ke instance n8n dibatasi (user/owner, port yang terbuka) | | |

## 2. Kesiapan Sistem Existing

| Sistem | Cara terhubung | Rate limit / SLA yang diketahui | Perilaku bila down | Siapa dihubungi |
| --- | --- | --- | --- | --- |
| Merchant Stock API | | | | |
| Payment API | | | | |
| Notification (mock API) | | | | |
| Telegram Bot API (digest, alert, approval, chat) | | | | |
| PostgreSQL | | | | |
| LLM provider | | | | |

## 3. Pra-Production

| Pemeriksaan | Status | Bukti |
| --- | --- | --- |
| Error Workflow terpasang di semua workflow | | |
| Retry dan dead-letter diuji dengan kegagalan disengaja | | |
| Heartbeat + deteksi sinkron 0 baris / digest kosong + ambang durasi | | |
| Data pelanggan di log/alert tidak berlebihan (masking) | | |
| Rencana rollback / Unpublish | | |
| Biaya token LLM dipantau | | |
