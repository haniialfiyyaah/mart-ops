# 02 · Desain Solusi + Solution Fit + ROI · H8Mart Quick-Commerce Ops

**Nama:**
**Workflow yang dibangun:** WF1 Stock Sync, Order Intake & Digest, WF2 Merchant Data Tools, WF3 Asisten AI Merchant, WF4 Kompensasi & Refund, WF5 Reliability & Monitoring

> Template dokumen Milestone (target selesai W3D5 23.59, maksimal W4D4 23.59). Isi langsung berkas ini.
> Tulis alasan, bukan hanya jawaban. Angka harus punya rumus dan asumsi yang bisa ditelusuri.

## 1. Arsitektur 5 Workflow

> Perbaiki diagram di bawah menjadi rancangan kalian (nama workflow, trigger, sistem eksternal, DB). Mermaid dirender GitHub.

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
    S([Schedule ISI]) --> WF1[WF1 ISI]
    H([Webhook ISI]) --> WF1
    WF1 -->|ISI| WF2[WF2 ISI]
    WF3[WF3 ISI] -->|ISI| WF2
    EVT([Event ISI]) --> WF4[WF4 ISI]
    WF2 --- DB[(PostgreSQL)]
    WF1 -. gagal .-> WF5[WF5 ISI]
    linkStyle default stroke-width:3px
    classDef navy fill:#1C4587,stroke:#1C4587,color:#FFFFFF
    classDef orange fill:#F06634,stroke:#F06634,color:#FFFFFF
    classDef opt fill:none,stroke:#F06634,stroke-width:3px,stroke-dasharray:6 4
    class S,H,EVT orange
    class WF1,WF2,WF3,WF4,WF5,DB navy
```

## 2. Keputusan Pendekatan per Titik Keputusan (rule-based vs AI-driven)

| Kode | Pertanyaan yang diputuskan | Jenis data | Pendekatan (rule-based / AI / manusia) | Alasan |
| --- | --- | --- | --- | --- |
| D1 | Apakah stok cukup untuk order ini? | | | |
| D2 | Apakah cancel rate atau SLA hari ini melewati ambang? | | | |
| D3 | Berapa voucher atau apakah refund butuh approval? | | | |
| D4 | Mengapa order merchant banyak batal? (pertanyaan merchant) | | | |
| D5 | Apakah jawaban AI layak dikirim ke merchant (guardrail)? | | | |

## 3. Estimasi ROI

| Komponen | Rumus / asumsi | Angka |
| --- | --- | --- |
| Waktu manual per hari untuk menyusun laporan Ops | | |
| Waktu CS per pertanyaan merchant (menarik data manual) | | |
| Waktu finance/Ops per refund atau voucher manual | | |
| Jumlah kasus per bulan (dasar dari data seed atau asumsi eksplisit) | | |
| Biaya tenaga per jam (asumsi) | | |
| Penghematan per bulan | | |
| Biaya LLM per bulan (estimasi token × harga model di halaman resmi penyedia; cantumkan sumber harga. Nilai penuh: token dari eksekusi kalian sendiri) | | |
| Biaya platform per bulan (rujuk dokumen 03) | | |
| **Net benefit per bulan** dan **payback** | | |

Risiko ROI (minimal dua) dan skenario terburuk:

## 4. Batas Automasi & Risiko

| Risiko | Dampak | Mitigasi di rancangan |
| --- | --- | --- |
| | | |
