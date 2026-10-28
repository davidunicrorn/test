# SQL and databases

Audit trading activity, investigate missing execution reports, and generate reports for compliance and client queries.

**Core:** `SELECT`, `WHERE`, `ORDER BY`, `GROUP BY`, `HAVING`, `JOIN` (`INNER` / `LEFT`), timestamp functions.

Plain SQL below. Illustrative, not a production schema.

## Order state verification

Client says they got no execution report. Was the order matched, cancelled, or rejected?

```sql
SELECT OrderID, ClOrdID, Status, EventTime
FROM Orders
WHERE ClOrdID = 'ClOrdID_12345'
ORDER BY EventTime;
```

## Volume and message audits

`COUNT(*)` grouped by client ID (`SenderCompID`) over a timestamp range, for subscription message-traffic billing. `HAVING` keeps clients over the included allowance.

```sql
SELECT SenderCompID, COUNT(*) AS MsgCount
FROM Messages
WHERE MsgTime >= CAST('2026-10-01' AS TIMESTAMP)
  AND MsgTime <  CAST('2026-11-01' AS TIMESTAMP)
GROUP BY SenderCompID
HAVING COUNT(*) > :included_msgs
ORDER BY MsgCount DESC;
```

## Reconciliation

`JOIN` Orders to Executions `ON Orders.OrderID = Executions.OrderID` to check trade details against client reports.

```sql
SELECT Orders.OrderID, Orders.ClOrdID, Executions.LastPx, Executions.LastQty
FROM Orders
INNER JOIN Executions ON Orders.OrderID = Executions.OrderID
WHERE Orders.ClOrdID = 'ClOrdID_12345';
```

`LEFT JOIN` on the same key leaves `Executions.OrderID` null when the order has no execution report.

**What good looks like:** You pick one query for the question in front of you: order status, message count by `SenderCompID` and time, or orders joined to executions.
