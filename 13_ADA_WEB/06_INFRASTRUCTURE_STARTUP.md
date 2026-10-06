# ADA Web — Infrastructure Startup

Estado: **CURRENT — GENERIC DURABLE PREPARATION + INTEGRATED OPERATIONS LOCAL HARNESS**

## Principle

Web runtime startup and physical resource preparation are separate operations.

The Web process consumes already available resources.

It must not silently provision Blob/Cosmos on every application startup or every worker startup.

## Existing distributed ADA evidence

The generated ADA distribution was previously instantiated in an independent consumer repository and executed through Docker.

Evidence retained:

```text
image build                          COMPLETED
python image                         python:3.14.2-slim-bookworm
Azurite                              RUNNING
Cosmos Emulator                      RUNNING
Cosmos Data Explorer                 HTTP 200 on 127.0.0.1:1234
```

Resource preparation created:

```text
Blob container       dataproduct
Cosmos database      cosmosdb-ada
Cosmos containers    6
```

Web:

```text
container            healthy
/health/live         HTTP 200
/health/ready        HTTP 200
version              0.2.26
```

## Generic durable preparation CURRENT

Generic owns resource preparation implementation:

```text
ada.web.application.generic.manager_deployment:manager_resources_main
```

Supported actions:

```text
prepare
validate
```

Conceptual order:

```text
infra
→ resource preparation
→ Web runtime
```

## Integrated Operations local harness CURRENT

`ada-integrated-operations-application` now contains:

```text
deployment/local/compose.yaml
```

Services:

```text
Azurite
Cosmos Emulator
```

Host ports:

```text
Azurite Blob      ${ADA_LOCAL_AZURITE_BLOB_PORT:-10000}:10000
Cosmos ready      ${ADA_LOCAL_COSMOS_READY_PORT:-8080}:8080
Cosmos gateway    ${ADA_LOCAL_COSMOS_PORT:-8081}:8081
```

The Cosmos Emulator is configured with:

```text
PROTOCOL=http
GATEWAY_PUBLIC_ENDPOINT=cosmos-emulator
```

When Web runs on the host rather than in Docker, the host must resolve `cosmos-emulator` to the local endpoint used by the published gateway.

## Integrated Operations product command CURRENT

The product exposes:

```text
ada-integrated-operations-resources
```

The entry point delegates directly to Generic:

```text
ada.web.application.generic.manager_deployment:manager_resources_main
```

There is no product-specific copy of resource topology/provisioning logic.

Current local workflow:

```text
docker compose -f deployment/local/compose.yaml up -d
uv run ada-integrated-operations-resources prepare
uv run ada-integrated-operations-resources validate
uv run ada-integrated-operations-application
```

## Local environment contract CURRENT

The application `.env.detail` documents:

```text
ATLANTICUS_ENVIRONMENT=local
ADA_PERSISTENCE_MODE=durable
ADA_APPLICATION_NAMESPACE
ADA_TOOL_NAMESPACE
ADA_STORAGE_*
ADA_COSMOS_*
optional COSMOS_CONSUMPTION_*
```

For host execution:

```text
Azurite can use 127.0.0.1:10000
Cosmos uses the published gateway and hostname required by the emulator
```

## Qualification state

Implementation presence on `atlanticus:main`:

```text
compose harness                 VERIFIED
product resource entry point   VERIFIED
.env.detail workflow            VERIFIED
```

User reported the resulting application/runtime working with durable local resources.

Exact output from:

```text
ada-integrated-operations-resources prepare
ada-integrated-operations-resources validate
```

was not captured in this closure.

Classification:

```text
UNVERIFIED
```

## Readiness limitation retained

Previous distributed Generic Web readiness body was observed as:

```json
{"checks": {}, "status": "ready"}
```

Therefore dependency readiness is not proven solely by `/health/ready`.

## Existing host sync limitation retained

The known macOS CPython 3.14.2 binary-only `rcssmin==1.2.2` limitation remains separate.

## Recovery gate

Destructive recovery qualification remains a separate test and must not be inferred from normal startup success.
