# H8Mart Quick-Commerce Ops · [Nama Kalian]

> Template README solusi (final). Isi langsung berkas ini; `README.md` soal di root tidak diubah.
> README harus cukup bagi penilai untuk menjalankan dan memverifikasi solusi tanpa bertanya.

## 1. Ringkasan (3 kalimat)

## 2. Arsitektur

```mermaid
---
config:
  look: classic
  flowchart:
    subGraphTitleMargin:
      top: 12
      bottom: 12
---
flowchart LR
    A([ISI]) --> B[ISI]
    linkStyle default stroke-width:3px
    classDef navy fill:#1C4587,stroke:#1C4587,color:#FFFFFF
    classDef orange fill:#F06634,stroke:#F06634,color:#FFFFFF
    classDef opt fill:none,stroke:#F06634,stroke-width:3px,stroke-dasharray:6 4
    class A orange
    class B navy
```

## 3. Daftar Workflow

| Workflow | File | Trigger | Fungsi satu kalimat |
| --- | --- | --- | --- |
| WF1 Stock Sync, Order Intake & Digest | `workflows/` | | |
| WF2 Merchant Data Tools | | | |
| WF3 Asisten AI Merchant (termasuk chat Telegram) | | | |
| WF4 Kompensasi & Refund | | | |
| WF5 Reliability & Monitoring | | | |

## 4. Cara Menjalankan

> Perintah persis (macOS/Linux/Git Bash dan PowerShell 7) dari clone repo sampai workflow jalan. Credential apa yang harus dibuat (termasuk Telegram API dan `chat_id` Ops), nilai placeholder apa yang diganti, dan cara membuat bot sendiri untuk uji.

## 5. Cara Memverifikasi (untuk penilai)

| Skenario | Perintah / aksi | Hasil yang terlihat | Bukti |
| --- | --- | --- | --- |
| Sinkron stok: retry, dead-letter, stok habis terdeteksi | | | |
| Order ditolak saat stok habis, event ganda tidak membuat order ganda | | | |
| Digest harian cocok dengan SQL | | | |
| MCP tool dipanggil deterministik dan oleh agent | | | |
| Asisten AI: jawaban dari data, eskalasi, injeksi, fallback | | | |
| Voucher/refund otomatis vs approval Ops; sekali walau event ganda | | | |
| Kegagalan disengaja -> retry -> dead-letter -> alert dengan business key | | | |
| Heartbeat Up/Down, sinkron 0 baris, ambang durasi | | | |
| Bot Telegram: digest pagi, alert, tombol approval, chat asisten (dan penolakan `chat_id` asing); username bot dan cara chat | | | |

## 6. Keputusan Desain & Batasan

## 7. Struktur Repo

```text
(isi dengan tree repo kalian)
```
