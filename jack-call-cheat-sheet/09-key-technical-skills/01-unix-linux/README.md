# Unix/Linux command checklist

In day-to-day exchange operations you spend much of your time on Linux servers inspecting logs, checking service statuses, and verifying connectivity.

| Command | What it does | Day-to-day at Aquis |
| --- | --- | --- |
| `tail -f <log_file>` | Views logs in real time | Tail matching-engine or client-gateway logs live while a client connects or places a test order |
| `grep -i "pattern" <file>` | Case-insensitive search | Error codes, client IDs, order IDs. Example: `grep "ClOrdID_12345" exchange.log` |
| `grep -C 5 "ERROR" <file>` | Search plus context lines | Extract snippets around an outage or exception to attach to a Jira ticket |
| `awk` / `sed` | Filter and reshape column or text streams | Pull execution prices and timestamps from structured log columns |
| `ps aux \| grep <service>` | List processes and filter by name | Verify a FIX gateway process or market-data feed daemon is running |
| `ss -tulpn` / `netstat` | Active connections and listening ports | Check an exchange socket or client listening port is open and accepting TCP |
| `nc -zv <host> <port>` | Test TCP connectivity to a remote IP and port | Firewall or network when setting up a new client venue connection |
| `chmod` / `chown` | Permissions and ownership | Deployment scripts or log directories have the right rights for service accounts |
| `systemctl status <service>` | Show whether a service is active | Confirm a FIX gateway or market-data feed is up after a restart |

**What good looks like:** You follow the right log, pull the order or error with context, and confirm the process and the listening port before you update the ticket.
