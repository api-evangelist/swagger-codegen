# Swagger Codegen

Swagger Codegen is an open-source, template-driven code generation tool that automatically generates client libraries, server stubs, and API documentation from OpenAPI Specification definitions. It supports 40+ client languages and 20+ server frameworks. Available as a CLI, Docker image, Maven/Gradle plugin, and online REST API.

**URL:** [https://swagger.io/tools/swagger-codegen/](https://swagger.io/tools/swagger-codegen/)

## Tags

- Client Libraries, Code Generation, Open Source, OpenAPI, SDK

## APIs

### Swagger Generator API

Online REST API at generator3.swagger.io for generating client SDKs and server stubs from OpenAPI V2/V3 specifications.

**Human URL:** [https://generator3.swagger.io/](https://generator3.swagger.io/)

#### Tags

- Client Libraries, Code Generation, Generator, OpenAPI, SDK, Server Stubs

#### Properties

- [Documentation](https://github.com/swagger-api/swagger-codegen/wiki)
- [OpenAPI](openapi/swagger-generator-openapi.yml)
- [OpenAPI Source](https://generator3.swagger.io/openapi.json)
- [GitHub Repository](https://github.com/swagger-api/swagger-codegen)

### Swagger Codegen CLI

Command-line interface for local code generation with full template customization.

**Human URL:** [https://github.com/swagger-api/swagger-codegen#getting-started](https://github.com/swagger-api/swagger-codegen#getting-started)

## Artifacts

### OpenAPI

| File | Description |
|---|---|
| [openapi/swagger-generator-openapi.yml](openapi/swagger-generator-openapi.yml) | Swagger Generator API — online code generation REST API |

### Rules

| File | Description |
|---|---|
| [rules/swagger-codegen-rules.yml](rules/swagger-codegen-rules.yml) | Spectral ruleset for Swagger Generator API conventions |

### Capabilities

| File | Description |
|---|---|
| [capabilities/code-generation.yaml](capabilities/code-generation.yaml) | Code generation workflow capability |
| [capabilities/shared/swagger-generator.yaml](capabilities/shared/swagger-generator.yaml) | Shared Swagger Generator API definitions |

### JSON Schema

| File | Description |
|---|---|
| [json-schema/swagger-codegen-generation-request-schema.json](json-schema/swagger-codegen-generation-request-schema.json) | JSON Schema for GenerationRequest |

### JSON Structure

| File | Description |
|---|---|
| [json-structure/swagger-codegen-structure.json](json-structure/swagger-codegen-structure.json) | GenerationRequest structural documentation |

### JSON-LD

| File | Description |
|---|---|
| [json-ld/swagger-codegen-context.jsonld](json-ld/swagger-codegen-context.jsonld) | JSON-LD context for Swagger Codegen concepts |

### Examples

| File | Description |
|---|---|
| [examples/swagger-codegen-generate-python-client-example.json](examples/swagger-codegen-generate-python-client-example.json) | Example: Generate Python client SDK |
| [examples/swagger-codegen-list-languages-example.json](examples/swagger-codegen-list-languages-example.json) | Example: List available client generator languages |

### Vocabulary

| File | Description |
|---|---|
| [vocabulary/swagger-codegen-vocabulary.yml](vocabulary/swagger-codegen-vocabulary.yml) | Swagger Codegen domain vocabulary |

## Common Properties

- [Portal](https://swagger.io/tools/swagger-codegen/)
- [Documentation](https://github.com/swagger-api/swagger-codegen/wiki)
- [Website](https://swagger.io/)
- [GitHub Organization](https://github.com/swagger-api)
- [GitHub Repository](https://github.com/swagger-api/swagger-codegen)
- [Issues](https://github.com/swagger-api/swagger-codegen/issues)
- [License](https://github.com/swagger-api/swagger-codegen/blob/master/LICENSE)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com

## Timestamps

- **Created:** 2026-03-16
- **Modified:** 2026-05-02
