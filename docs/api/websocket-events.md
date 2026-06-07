# WebSocket Events

The backend pushes real-time updates to the browser using **Socket.IO**. This is how the checkout page updates the moment a payment locks or a dispute resolves, without polling.

## How rooms work

The notifications gateway broadcasts each event to a **room named after the `orderId`**. The frontend joins the room for the order it is displaying and receives only that order's events.

```
Backend domain event  ──►  NotificationsService  ──►  gateway.emit(orderId, event, payload)
                                                          (Socket.IO room = orderId)
```

## Events emitted

| Event | Fired when | Payload |
| --- | --- | --- |
| `payment.locked` | Funds are locked in escrow (`EscrowLocked` on-chain) | `{ orderId }` |
| `delivery.confirmed` | Delivery is confirmed | `{ orderId }` |
| `dispute.opened` | A dispute is opened (`EscrowDisputed` on-chain) | `{ orderId }` |
| `escrow.released` | Funds are released to the seller | `{ orderId }` |

These are driven by the backend's internal event bus: on-chain events picked up by the `EventListener` are re-emitted as domain events (`escrow.locked`, `escrow.disputed`, `escrow.released`, `delivery.confirmed`), which the `NotificationsService` forwards to the matching Socket.IO room.

## Connecting from a client

```javascript
import { io } from "socket.io-client";

const socket = io("http://localhost:3001");        // backend origin
socket.emit("join", orderId);                       // join the order's room
socket.on("payment.locked",     (p) => { /* ... */ });
socket.on("delivery.confirmed", (p) => { /* ... */ });
socket.on("escrow.released",    (p) => { /* ... */ });
socket.on("dispute.opened",     (p) => { /* ... */ });
```
