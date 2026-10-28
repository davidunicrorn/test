# FIX protocol

Standard for electronic order routing and market data. Tag=value (example `35=D`: tag 35 is MsgType, `D` is New Order Single).

| Tag | Name | Values |
| --- | --- | --- |
| 35 | MsgType | `A` Logon, `D` New Order Single, `8` Execution Report, `F` Order Cancel Request, `5` Logout |
| 49 / 56 | SenderCompID / TargetCompID | Who sent the message, and who it is for |
| 11 | ClOrdID | Client order id |
| 39 | OrdStatus | `0` New, `1` Partially Filled, `2` Filled, `4` Cancelled, `8` Rejected |
| 150 | ExecType | What this execution report is reporting |

## Read a reject

Read the raw FIX log to see why a New Order Single (`35=D`) got an Execution Report (`35=8`) with OrdStatus `8` (Rejected). `|` stands in for SOH:

```
35=8|49=AQUIS|56=CLIENT1|11=ClOrdID_12345|39=8|150=8|58=Unknown symbol
```

`11` ties the report to the order. `39=8` is Rejected. `150=8` is the reject on ExecType. `58` is the text reason.

**What good looks like:** You point at 35, 49/56, 11, and 39 on one line and say who sent it, and whether it is new, partial, filled, cancelled, or rejected.
