---
name: Onboard a cloud flow-log export
description: Configure Kentik to ingest VPC/flow logs from AWS, Azure, GCP, or OCI.
api: openapi/kentik-cloud-export-openapi.json
operations: [ListCloudExports, CreateCloudExport, GetCloudExport, UpdateCloudExport]
auth: X-CH-Auth-Email + X-CH-Auth-API-Token headers
base_url: https://grpc.api.kentik.com
---

# Onboard a cloud export

Use the Kentik V6 Cloud Export API to stream cloud telemetry into Kentik.

## Steps

1. **Authenticate** with the `X-CH-Auth-Email` / `X-CH-Auth-API-Token` headers.
2. **Review existing exports.** Call `ListCloudExports` to avoid duplicates.
3. **Create the export.** Call `CreateCloudExport` with the `cloud_provider` (aws | azure | gcp | oci) and the provider-specific properties (role/bucket for AWS, subscription for Azure, etc.).
4. **Verify.** Call `GetCloudExport` and confirm ingest status; adjust with `UpdateCloudExport`.

## Rules
- Errors return `rpcStatus`; `3 INVALID_ARGUMENT` flags a malformed provider config. See `errors/kentik-error-codes.yml`.
- Cloud provider prerequisites (IAM roles, endpoints) are in the Kentik KB cloud configuration guides.
