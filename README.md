# Temporal (temporal-io)

Temporal is a durable execution platform for orchestrating long-running, fault-tolerant workflows. Its primary API surface is **gRPC** - the WorkflowService, OperatorService, and (on Temporal Cloud) the Cloud Operations API are defined as protobuf in `github.com/temporalio/api` and `github.com/temporalio/cloud-api`. Temporal also ships a first-party **HTTP API**: a grpc-gateway that maps a REST/JSON subset of the WorkflowService onto paths under `/api/v1`, for automation and environments where gRPC is impractical. Temporal is open source (MIT) and self-hostable, and is also available as the managed **Temporal Cloud**.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/temporal-io/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/temporal-io/refs/heads/main/apis.yml)

## Transport Reality

Temporal is **gRPC-first**. The complete API is the set of gRPC services in the protobuf contracts:

- **WorkflowService (gRPC)** — the core data-plane API used by SDKs and workers: start/signal/query/update/describe/list/reset/terminate/cancel workflows, poll and complete workflow/activity/Nexus tasks, and manage schedules, updates, and batch operations. Default port `7233`.
- **OperatorService (gRPC)** — cluster administration: search attributes, Nexus endpoints, remote clusters, namespace deletion.
- **Cloud Operations API / CloudService (gRPC + HTTP)** — Temporal Cloud control plane: namespaces, users, service accounts, API keys, regions, account. gRPC at `saas-api.tmprl.cloud:443` (with a `temporal-cloud-api-version` header) and an HTTP grpc-gateway under `https://saas-api.tmprl.cloud/api/v1` (public preview).
- **Temporal HTTP API (REST)** — a first-party grpc-gateway that exposes a **subset** of the WorkflowService as REST/JSON under `/api/v1`. Not all gRPC methods are exposed; worker task-polling RPCs remain gRPC-only.

There is **no documented public WebSocket API**. Worker task delivery uses gRPC long-polling (client-initiated), not WebSocket or server-sent events. See `review.yml`.

## Tags

- Durable Execution
- Workflow Orchestration
- gRPC
- Workflows
- Open Source
- Temporal Cloud

## Timestamps

- **Created:** 2026-07-02
- **Modified:** 2026-07-02

## APIs

### Temporal Workflow Service API

The core Temporal gRPC service. Clients and workers use it to start, signal, query, describe, list, reset, terminate, and cancel workflow executions, to poll and complete workflow/activity/Nexus tasks, and to manage schedules, updates, and batch operations. Defined as protobuf (WorkflowService) and served over gRPC on port 7233; there is no public REST surface for the full service - only the HTTP API subset (see the Temporal HTTP API).

- **Human URL:** [https://docs.temporal.io/references/api-reference](https://docs.temporal.io/references/api-reference)
- **Base URL:** `https://docs.temporal.io/references/api-reference`

#### Tags

- Workflows
- gRPC
- Durable Execution

#### Properties

- [Documentation](https://docs.temporal.io/references/api-reference)
- [Source Code](https://github.com/temporalio/api/blob/master/temporal/api/workflowservice/v1/service.proto)
- [OpenAPI](openapi/temporal-io-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)

### Temporal HTTP API

A first-party grpc-gateway that maps a REST/JSON subset of the WorkflowService onto HTTP paths under `/api/v1` (for example `POST /api/v1/namespaces/{namespace}/workflows/{workflow_id}` to start a workflow). It supports the same core operations as the gRPC API - start, describe, list, signal, query, terminate, cancel, reset, and read history - using standard HTTP and Bearer (API key) auth. Not all gRPC methods are exposed over HTTP; the worker task-polling RPCs remain gRPC-only.

- **Human URL:** [https://docs.temporal.io/develop/go/temporal-clients](https://docs.temporal.io/develop/go/temporal-clients)
- **Base URL:** `https://<namespace>.<account>.tmprl.cloud/api/v1`

#### Tags

- HTTP
- REST
- Workflows

#### Properties

- [Documentation](https://docs.temporal.io/references/api-reference)
- [OpenAPI](openapi/temporal-io-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/temporal-io.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/temporal-io.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Temporal Operator Service API

The gRPC OperatorService for cluster-level administration - managing custom search attributes (add/remove/list), Nexus endpoints (create/get/update/delete/list), remote clusters, and namespace deletion. A handful of methods (ListSearchAttributes and the Nexus endpoint operations) also carry HTTP annotations; the rest are gRPC-only.

- **Human URL:** [https://docs.temporal.io/references/api-reference](https://docs.temporal.io/references/api-reference)
- **Base URL:** `https://docs.temporal.io/references/api-reference`

#### Tags

- Operator
- Namespaces
- Search Attributes
- Nexus

#### Properties

- [Documentation](https://docs.temporal.io/references/api-reference)
- [Source Code](https://github.com/temporalio/api/blob/master/temporal/api/operatorservice/v1/service.proto)

### Temporal Cloud Operations API

The Temporal Cloud control-plane API (CloudService) for programmatically managing Temporal Cloud resources - namespaces, users, service accounts, API keys, regions, account settings, Nexus endpoints, and usage. It is exposed both as gRPC (`saas-api.tmprl.cloud:443`, with a `temporal-cloud-api-version` header) and as an HTTP/REST grpc-gateway under `https://saas-api.tmprl.cloud/api/v1` (public preview). Distinct from the per-namespace data-plane WorkflowService.

- **Human URL:** [https://docs.temporal.io/ops](https://docs.temporal.io/ops)
- **Base URL:** `https://saas-api.tmprl.cloud/api/v1`

#### Tags

- Temporal Cloud
- Control Plane
- Namespaces
- Users

#### Properties

- [Documentation](https://docs.temporal.io/ops)
- [Source Code](https://github.com/temporalio/cloud-api)

### Temporal Nexus API

Temporal Nexus connects Temporal applications across namespaces and teams via named Nexus endpoints backed by synchronous and asynchronous Nexus operations. Endpoints are registered through the OperatorService (Create/Get/Update/Delete/ListNexusEndpoints) and workers serve them through the WorkflowService Nexus task-queue RPCs (PollNexusTaskQueue, RespondNexusTaskCompleted, RespondNexusTaskFailed). Transport is gRPC.

- **Human URL:** [https://docs.temporal.io/nexus](https://docs.temporal.io/nexus)
- **Base URL:** `https://docs.temporal.io/nexus`

#### Tags

- Nexus
- Cross-Namespace
- RPC

#### Properties

- [Documentation](https://docs.temporal.io/nexus)
- [Source Code](https://github.com/temporalio/api/blob/master/temporal/api/nexus/v1/message.proto)

## Common Properties

- [GitHub Organization](https://github.com/temporalio)
- [LinkedIn](https://www.linkedin.com/company/temporal-technologies)
- [Website](https://temporal.io/)
- [Documentation](https://docs.temporal.io)
- [Plans](plans/temporal-io-plans-pricing.yml)
- [Rate Limits](rate-limits/temporal-io-rate-limits.yml)
- [Fin Ops](finops/temporal-io-finops.yml)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
