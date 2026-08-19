---
name: Generate a client SDK from an OpenAPI definition
description: Use the hosted Swagger Generator to pick a valid target language, discover its options, and download a generated client SDK as a ZIP.
api: openapi/swagger-codegen-generation-api-openapi.yml
operations: [languages, listOptions, generate]
generated: '2026-08-06'
method: generated
source: https://generator3.swagger.io/openapi.json
---

# Generate a client SDK

Base URL: `https://generator3.swagger.io/api` — the `/api` base path is required. No authentication, no API key, no account.

## 1. Pick a valid target

Call `languages` — `GET /api/client/V3` (path is `/{type}/{version}`, `type` ∈ `client|server|documentation|config`, `version` ∈ `V2|V3`).

Returns a bare JSON array of generator identifiers, e.g. `["csharp","dart","go","java","javascript","kotlin-client","php","python","ruby","typescript-axios", ...]`.

**Never guess the identifier.** Anything not in this array is rejected — `listOptions` answers `400 Unsupported target <x> supplied.` and `generate` fails. Validate `type` and `version` client-side too: an invalid path segment returns **500**, not 400.

## 2. Discover the generator's options

Call `listOptions` — `GET /api/options?language=python&version=V3`.

Returns a map of option name → `CliOption` descriptor. This is the machine-readable form of the CLI's `config-help -l python`. There is no fixed schema for `Options`; the valid keys are whatever this call returns for that language.

## 3. Generate

Call `generate` — `POST /api/generate`, `Content-Type: application/json`, body is a `GenerationRequest`:

```json
{
  "lang": "python",
  "type": "CLIENT",
  "codegenVersion": "V3",
  "specURL": "https://petstore.swagger.io/v2/swagger.json",
  "options": { "packageName": "petstore_client" }
}
```

Rules:
- `lang` is required.
- Supply **exactly one** of `spec` (the OpenAPI definition inline as an object) or `specURL` (a remote URL). Omitting both returns `400 Error processing generation request: Error processing input options: input spec or URL must be specified`.
- `codegenVersion` selects the engine generation, not the version of your spec: `V3` for OpenAPI 3.0 input, `V2` for Swagger 2.0 input.
- Only put keys in `options` that step 2 returned for that language.

The response is `application/octet-stream` — a **binary ZIP**, with the filename in `Content-Disposition`. Write it to disk; do not try to parse it as JSON.

## Alternative: GET form

`generateFromURL` — `GET /api/generate?codegenOptionsURL=<url>` — where the URL serves the `GenerationRequest` JSON itself (not the OpenAPI spec). Use it only when you cannot POST.

## Handling failure

Errors are **plain text**, not JSON, and none of them are declared in the contract. See `errors/swagger-codegen-problem-types.yml`. There is no request id, so a failure cannot be correlated with support — log the request body yourself.

## Cautions

- `AuthorizationValue` in the request body is the credential the generator uses to **fetch your `specURL`**. It does not authenticate you to this service. Do not send secrets to the public instance for any other reason.
- The service is free, shared and unmetered — no rate-limit headers exist. Self-throttle. For volume, run `swaggerapi/swagger-generator` locally (see `sandbox/swagger-codegen-sandbox.yml`) or use the CLI (`cli/swagger-codegen-cli.yml`).
- The hosted service reports version 3.0.75 while the released engine is 3.0.82; output can differ from a current CLI.
