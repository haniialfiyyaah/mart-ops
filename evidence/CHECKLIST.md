# Evidence checklist (instructor + submission)

Do **not** show tokens, API keys, passwords, or bot secrets.

Mark `[x]` when the file exists.

## Already done

- [x] deploy `01`–`03` (HTTPS, compose ps, curl)
- [x] canvas WF1–WF5 (+ WF3b)
- [x] wf1 `02-executions.png`, `03-stock-count-205.png` (on disk — commit soon)

---

## A · Executions per workflow

| Done | File | Shot |
| --- | --- | --- |
| [x] | `wf1/02-executions.png` | WF1 Executions list (success rows) |
| [ ] | `wf2/02-executions.png` | WF2 list (MCP runs) |
| [ ] | `wf3/02-executions.png` | WF3 list |
| [ ] | `wf3/10-wf3b-executions.png` | WF3b list (Telegram Trigger) |
| [ ] | `wf4/02-executions.png` | WF4 list |
| [ ] | `wf5/02-executions.png` | WF5 / Error Workflow list |

---

## B · Telegram chats

| Done | File | Shot |
| --- | --- | --- |
| [x] | `wf1/09-digest-telegram.png` | Digest pagi |
| [x] | `wf5/04-alert-telegram.png` | Alert + business key |
| [x] | `wf4/06-approval-before.png` | Approve/Decline **before** click |
| [x] | `wf4/07-approval-after.png` | Same thread **after** (Show Outcome) |
| [x] | `wf3/08-telegram-chat.png` | Asisten reply (allowlisted) |

---

## C · SQL + retry + Kuma

| Done | File | Command / action |
| --- | --- | --- |
| [x] | `wf1/03-stock-count-205.png` | `SELECT count(*) FROM core.merchant_stock;` → ~205 |
| [x] | `wf1/04-merge-product-id.png` | Sync exec → **Merge Catalog** output `product_id` |
| [x] | `wf5/03-dead-letter.png` | `SELECT * FROM ops.dead_letter;` (M009 / related) |
| [x] | `wf3/04-escalate-ticket.png` | Escalation exec + `SELECT * FROM ops.tickets ORDER BY created_at DESC LIMIT 5;` |
| [x] | `wf4/03-voucher-issued.png` | `ops.compensations` voucher issued |
| [ ] | `wf4/04-refund-pending.png` | large refund `pending_approval` |
| [x] | `wf4/05-refund-refunded.png` | after Approve → `refunded` (`ops.refund_actions` / compensations) |
| [x] | `wf1/05-retry-stock-api.png` | Sync exec: stock API fail then success |
| [x] | `wf1/06-admin-stock-calls.png` | tunnel `GET /admin/stock-calls` |
| [x] | `wf5/06-kuma-up.png` | Kuma **Up** (after manual sync heartbeat) |
| [x] | `wf5/07-kuma-down.png` | Kuma **Down** (pause monitor / miss heartbeat) |

### SQL helpers (SSH VPS)

```bash
docker compose -f compose.yaml -f compose.vps.yaml exec -T postgres \
  psql -U h8mart -d h8mart -c "SELECT count(*) FROM core.merchant_stock;"

docker compose -f compose.yaml -f compose.vps.yaml exec -T postgres \
  psql -U h8mart -d h8mart -c "SELECT * FROM ops.dead_letter ORDER BY created_at DESC LIMIT 20;"

docker compose -f compose.yaml -f compose.vps.yaml exec -T postgres \
  psql -U h8mart -d h8mart -c "SELECT * FROM ops.tickets ORDER BY created_at DESC LIMIT 10;"

docker compose -f compose.yaml -f compose.vps.yaml exec -T postgres \
  psql -U h8mart -d h8mart -c "SELECT * FROM ops.compensations ORDER BY created_at DESC LIMIT 10;"

docker compose -f compose.yaml -f compose.vps.yaml exec -T postgres \
  psql -U h8mart -d h8mart -c "SELECT * FROM ops.refund_actions ORDER BY updated_at DESC LIMIT 10;"
```

### Orders / refund curls (laptop)

```bash
# 200
curl -s -i -X POST "https://h8mart-hani.duckdns.org/webhook/order-created" \
  -H "X-H8Mart-Token: YOUR_TOKEN" -H "Content-Type: application/json" \
  -d @fixtures/event-order-created-ok.json

# 409
curl -s -i -X POST "https://h8mart-hani.duckdns.org/webhook/order-created" \
  -H "X-H8Mart-Token: YOUR_TOKEN" -H "Content-Type: application/json" \
  -d @fixtures/event-order-created-stok-habis.json

# voucher
curl -s -i -X POST "https://h8mart-hani.duckdns.org/webhook/ops-events" \
  -H "X-H8Mart-Token: YOUR_TOKEN" -H "Content-Type: application/json" \
  -d @fixtures/event-delivery-late-ringan.json

# large refund → Telegram approval
curl -s -i -X POST "https://h8mart-hani.duckdns.org/webhook/ops-events" \
  -H "X-H8Mart-Token: YOUR_TOKEN" -H "Content-Type: application/json" \
  -d @fixtures/event-order-failed-besar.json
```

Save: `wf1/07-order-200.png`, `wf1/08-order-409.png` (mask token).

---

## D · Dev set · injection · foreign chat (nilai penuh / instructor)

| Done | File | Action |
| --- | --- | --- |
| [x] | `wf3/03-answered-structured.png` | WF3 exec `dijawab` + structured fields |
| [ ] | `wf3/05-fallback.png` | Force AI fail / fallback path if available |
| [x] | `wf3/06-devset-table.png` | Table: 10 cases from `fixtures/dev-set-asisten.json` (label vs actual) |
| [x] | `wf3/07-injection.png` | `fixtures/question-catatan-injeksi.json` → instruksi palsu **not** followed |
| [x] | `wf3/09-foreign-chat-reject.png` | Other Telegram account → polite reject, no WF3 agent |
| [ ] | `wf2/03-mcp-from-wf3.png` | WF2 exec from WF3; tool + params visible |

Injection curl:

```bash
curl -s -X POST "https://h8mart-hani.duckdns.org/webhook/merchant-assistant" \
  -H "X-H8Mart-Token: YOUR_TOKEN" -H "Content-Type: application/json" \
  -d @fixtures/question-catatan-injeksi.json
```

Escalate example (use a vague/out-of-scope question from dev-set, or one that triggers `perlu_eskalasi`).

---

## E · Instructor ops (not only screenshots)

| Done | Item |
| --- | --- |
| [x] | `docs/02` skenario terburuk ROI (updated) |
| [x] | `docs/03` sumber harga + UU PDP **Pasal 16 & 39** (updated) |
| [x] | **VPS:** WF2 → Settings → Error Workflow = WF5 → Save → Publish (export JSON already patched) |
| [x] | Re-export WF2 after setting on VPS (or keep patched `workflows/wf2-….json`) |

---

## Suggested session order today

1. VPS: set **WF2 Error Workflow = WF5**, Publish  
2. Executions screenshots: wf2, wf3, wf3b, wf4, wf5  
3. SQL: dead_letter, tickets, compensations/refund_actions  
4. Telegram: digest (Execute Path C once), approval before/after, alert, asisten chat  
5. Curls: 200 + 409  
6. Merge + retry + Kuma Up/Down  
7. Injection + foreign chat (if 2nd account) + dev-set table  
8. Commit evidence batches  
