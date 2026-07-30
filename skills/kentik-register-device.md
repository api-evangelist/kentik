---
name: Register a device and organize it with sites and labels
description: Onboard a network device into Kentik and classify it by site and labels so its telemetry is grouped correctly.
api: openapi/kentik-device-openapi.json
operations: [ListSites, CreateDevice, ListLabels, UpdateDeviceLabels, GetDevice]
auth: X-CH-Auth-Email + X-CH-Auth-API-Token headers
base_url: https://grpc.api.kentik.com
---

# Register a device in Kentik

Use the Kentik V6 Device, Site, and Label APIs to onboard a device that exports flow/SNMP/streaming telemetry.

## Steps

1. **Authenticate.** Send `X-CH-Auth-Email` and `X-CH-Auth-API-Token` headers on every request (see `conventions/kentik-conventions.yml`).
2. **Find or create the site.** Call `ListSites` (Site API) to get the `site_id` the device belongs to. Create one with `CreateSite` if needed.
3. **Create the device.** Call `CreateDevice` with the device name, type, sending IPs, and the `site_id`. For bulk onboarding use `CreateDevices`.
4. **Apply labels.** Call `ListLabels` to resolve label ids, then `UpdateDeviceLabels` to attach them so the device is grouped in dashboards and policies.
5. **Verify.** Call `GetDevice` (or `GetDeviceByName`) and confirm status.

## Rules
- Errors return `rpcStatus` (gRPC code + message); `6 ALREADY_EXISTS` means the device name is taken, `16 UNAUTHENTICATED` means bad headers. See `errors/kentik-error-codes.yml`.
- No idempotency-key header exists; guard retries by checking `GetDeviceByName` first.
