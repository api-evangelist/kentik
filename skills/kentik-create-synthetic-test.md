---
name: Create and monitor a synthetic test
description: Stand up a Kentik synthetic test, pick agents, activate it, and read results.
api: openapi/kentik-synthetics-openapi.json
operations: [ListAgents, CreateTest, SetTestStatus, GetTest, GetResultsForTests]
auth: X-CH-Auth-Email + X-CH-Auth-API-Token headers
base_url: https://grpc.api.kentik.com
---

# Create a synthetic test

Use the Kentik V6 Synthetics API to monitor availability and performance from Kentik agents.

## Steps

1. **Authenticate** with the `X-CH-Auth-Email` / `X-CH-Auth-API-Token` headers.
2. **List agents.** Call `ListAgents` and choose the agent ids to run the test from.
3. **Create the test.** Call `CreateTest` with the test type (http, ping, dns, hostname, ip, url, network_grid, bgp, transaction) and settings, referencing the chosen agent ids.
4. **Activate.** Call `SetTestStatus` to set the test to `TEST_STATUS_ACTIVE`.
5. **Read results.** Call `GetResultsForTests` (or `GetResultsForTestsCsv`) for time-series health/latency/loss; use `GetTraceForTest` for path data.
6. **Manage.** `GetTest`, `UpdateTest`, `DeleteTest` maintain the test; `ListAgentAlerts` / `CreateAgentAlert` govern agent alerting.

## Rules
- Errors are `rpcStatus`; `8 RESOURCE_EXHAUSTED` indicates test-credit limits. See `errors/kentik-error-codes.yml`.
