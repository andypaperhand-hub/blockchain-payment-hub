# Simulating a Delivery

In production, GHN / GHTK / Viettel Post send webhooks to the oracle automatically as parcels are scanned. For local testing, you simulate those confirmations by hand.

## Send confirmations

Open a new terminal and post to the oracle service (port 3002). Replace `ORDER_ID` with the order ID from the dashboard.

```bash
# GHN confirms delivery
curl -X POST http://localhost:3002/webhook/ghn \
  -H "Content-Type: application/json" \
  -d '{"orderId": "ORDER_ID", "status": "delivered"}'

# GHTK confirms delivery
curl -X POST http://localhost:3002/webhook/ghtk \
  -H "Content-Type: application/json" \
  -d '{"orderId": "ORDER_ID", "status": "delivered"}'
```

Two confirmations meet the **2-of-3** threshold. The oracle then calls the smart contract and funds release to the seller automatically.

## Available webhook endpoints

The oracle service exposes one endpoint per provider, plus a mock shortcut:

| Endpoint | Purpose |
| --- | --- |
| `POST /webhook/ghn` | GHN delivery confirmation |
| `POST /webhook/ghtk` | GHTK delivery confirmation |
| `POST /webhook/viettel` | Viettel Post delivery confirmation |
| `POST /webhook/mock/deliver/:orderId` | Test shortcut to simulate delivery |
| `POST /mapping/register` | Register a tracking-code → order mapping |
| `GET /mapping` | List registered mappings |
| `GET /oracle/status/:orderId` | Current oracle/consensus status for an order |
