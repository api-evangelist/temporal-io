# Temporal (temporal-io)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
