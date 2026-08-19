---
name: Generate a server stub or documentation package
description: Discover the server frameworks and documentation generators the Swagger Generator supports, then produce a server stub or docs bundle from an OpenAPI definition.
api: openapi/swagger-codegen-generation-api-openapi.yml
operations: [languages, languagesMulti, listOptions, generate]
generated: '2026-08-06'
method: generated
source: https://generator3.swagger.io/openapi.json
---

# Generate a server stub or documentation package

Same engine and same endpoint as the client-SDK flow; what changes is the generator `type`.

Base URL: `https://generator3.swagger.io/api`. No authentication.

## 1. List the frameworks for the type you want

- Server frameworks: `languages` — `GET /api/server/V3`
- Documentation generators: `languages` — `GET /api/documentation/V3`
- Config generators: `languages` — `GET /api/config/V3`

To pull several types in one call use `languagesMulti` — `GET /api/types?types=server,documentation&version=V3`.

**Always pass `types` and `version` to `languagesMulti`.** Called bare, `GET /api/types` returned **500** during probing.

Deprecated equivalents still exist and still answer — `clientLanguages` (`GET /api/clients`), `serverLanguages` (`GET /api/servers`), `documentationLanguages` (`GET /api/documentation`) — but upstream marks all three `deprecated: true` and directs callers to `languages`. Do not build on them.

## 2. Check the options for that framework

`listOptions` — `GET /api/options?language=<framework>&version=V3`. Java-based server generators typically expose `groupId`, `artifactId`, `artifactVersion`, `apiPackage`, `modelPackage`, `invokerPackage`, `sourceFolder`.

## 3. Generate

`generate` — `POST /api/generate`:

```json
{
  "lang": "<framework from step 1>",
  "type": "SERVER",
  "codegenVersion": "V3",
  "spec": { "openapi": "3.0.0", "info": {"title": "My API", "version": "1.0.0"}, "paths": {} },
  "options": { "groupId": "com.example", "artifactId": "my-api-server" }
}
```

Set `type` to `SERVER`, `DOCUMENTATION` or `CONFIG` to match the generator you chose in step 1. Mismatching `lang` and `type` is the most common failure.

Response is a binary ZIP (`application/octet-stream`); the filename is in `Content-Disposition`.

## Verify before you ship

The generated project is not validated against your spec by this service. Build the artifact locally before committing it. For reproducible CI, prefer the Maven plugin `io.swagger.codegen.v3:swagger-codegen-maven-plugin` over calling the public endpoint — see `cli/swagger-codegen-cli.yml` and `packages/swagger-codegen-packages.yml`.
