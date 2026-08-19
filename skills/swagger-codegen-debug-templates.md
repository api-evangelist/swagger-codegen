---
name: Debug a custom generator template
description: Inspect the intermediate codegen model for a spec and render a Mustache template against data, so custom generator templates can be developed without running a full generation pass.
api: openapi/swagger-codegen-utilities-api-openapi.yml
operations: [generateBundle, renderTemplate]
generated: '2026-08-06'
method: generated
source: https://generator3.swagger.io/openapi.json
---

# Debug a custom generator template

Swagger Codegen is template-driven: the engine builds an intermediate model ("bundle") from your OpenAPI definition, then feeds that model to Mustache templates. Two utility operations let you inspect each half separately instead of generating a whole project and reading the output.

Base URL: `https://generator3.swagger.io/api`. No authentication.

## 1. See what the engine actually gives your templates

`generateBundle` — `POST /api/model`, body is the same `GenerationRequest` you would send to `generate`:

```json
{
  "lang": "python",
  "type": "CLIENT",
  "codegenVersion": "V3",
  "specURL": "https://petstore.swagger.io/v2/swagger.json"
}
```

Returns the intermediate model as a **JSON object** (untyped — the contract declares only `type: object`). This is the variable namespace your Mustache templates see: use it to find the real property names before writing `{{...}}` references, rather than guessing from the generated output.

Same input rules as `generate`: `lang` required, exactly one of `spec` or `specURL`.

## 2. Render a template against data

`renderTemplate` — `POST /api/render`, body is a `RenderRequest`:

```json
{
  "template": "class {{classname}}:\n{{#vars}}    {{name}}: {{datatype}}\n{{/vars}}",
  "data": { "classname": "Pet", "vars": [{"name": "id", "datatype": "int"}] }
}
```

Returns the rendered output. Paste a slice of the step-1 bundle into `data` to test a template fragment against the engine's real model shape in one round trip.

## Workflow

1. `generateBundle` with your spec and target language → capture the model.
2. Pick the sub-object your template iterates (e.g. an operation or a model entry).
3. `renderTemplate` with your candidate template and that sub-object as `data`.
4. Iterate on the template until the output is right.
5. Only then run the real generation — locally with `swagger-codegen-cli generate -t <template-dir>`, since the hosted API has no custom-template upload. See `cli/swagger-codegen-cli.yml`.

## Cautions

- Custom template directories are a **CLI-only** capability (`-t/--template-dir`). The hosted API can render a template string and expose the model, but it cannot generate a project from your template set. Do the loop above against the API, then run the generation with the CLI or Docker image.
- Errors return plain text with a 4xx, not JSON. See `errors/swagger-codegen-problem-types.yml`.
