---
name: Triage alerts and start a mitigation
description: Review active Kentik alerts, acknowledge them, and trigger a DDoS/traffic mitigation.
api: openapi/kentik-alerting-openapi.json
operations: [List, Get, Ack, AddComment, AvailableActions, Act]
auth: X-CH-Auth-Email + X-CH-Auth-API-Token headers
base_url: https://grpc.api.kentik.com
---

# Triage alerts and mitigate

Use the Kentik V6 Alerting and Mitigation APIs to respond to detected anomalies (including DDoS).

## Steps

1. **Authenticate** with the `X-CH-Auth-Email` / `X-CH-Auth-API-Token` headers.
2. **List alerts.** Call `List` (Alerting API, `openapi/kentik-alerting-openapi.json`) to enumerate active alerts.
3. **Inspect.** Call `Get` for a specific alert; review the triggering policy (Alert Policy API `Get`).
4. **Acknowledge.** Call `Ack` to take ownership and `AddComment` to record context; `UnAck` / `Clear` reverse or close.
5. **Choose a mitigation.** Call `AvailableActions` (Mitigation API, `openapi/kentik-mitigation-openapi.json`) to see valid actions for the situation.
6. **Mitigate.** Call `Create` then `Act` to start the mitigation (RTBH, Flowspec, adaptive, or third-party). Use `AvailableActionsForMitigation` and `Get` to drive it to completion.

## Rules
- All errors return `rpcStatus`; `7 PERMISSION_DENIED` means the token lacks mitigation rights. See `errors/kentik-error-codes.yml`.
- Notification channels (webhook/Slack/PagerDuty) attached to the policy fire on state change — see `asyncapi/kentik-webhooks.yml`.
