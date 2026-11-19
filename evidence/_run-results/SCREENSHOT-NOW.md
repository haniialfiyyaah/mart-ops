# Screenshot now (tests already fired)

Run: `20261119-212940` (+ WF4 seed-id retry) · Results under `evidence/_run-results/*.txt` (token redacted).

## Results

| Test | HTTP | Notes |
| --- | --- | --- |
| `wf1-07-order-200` | 200 | `accepted_low_stock` · `HM-RUN-212940-OK` |
| `wf1-08-order-409` | 409 | `out_of_stock` · `HM-TEST-002` |
| `wf1-11-order-tipis` | 200 | `accepted_low_stock` · `HM-RUN-212940-TIPIS` |
| `wf1-12-order-422` | 422 | `invalid_event` (empty items) |
| `wf3-03-answered` | 200 | `dijawab` M003 batal |
| `wf3-04-escalate` | 200 | `eskalasi_cs` D06 kompensasi |
| `wf3-07-injection` | 200 | `dijawab` (no fake instructions followed) |
| `wf4-voucher` | 200 | Late voucher `HM-SP-001` |
| `wf4-refund-besar` | 500 | Expected while Waiting Telegram Approve · check WF4 Executions |

## Ready in n8n / Telegram / SQL

| Shot | Where to look |
| --- | --- |
| `wf1/07-order-200` | WF1 Executions · `HM-RUN-212940-OK` · or `_run-results/wf1-07-order-200.txt` |
| `wf1/08-order-409` | WF1 · 409 |
| `wf3/03-answered` | WF3 · dijawab |
| `wf3/04-escalate` | WF3 + SQL `ops.tickets` |
| `wf3/07-injection` | WF3 injection exec |
| `wf2/03-mcp-from-wf3` | WF2 from answered run |
| `wf4/03-voucher` | WF4 · `HM-SP-001` · SQL compensations |
| `wf4/04–07 approval` | Telegram Approve for `HM-SP-004` |

## Still manual (no curl)

1. **Path A sync** — Execute Schedule Sync once → Merge / stock 205 / Kuma Up
2. Approve refund on Telegram → after shot + SQL refunded
3. Execute digest once → Telegram digest
4. Foreign chat / fallback AI / Kuma Down / executions list PNGs
