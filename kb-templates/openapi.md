# OpenAPI 3.1 Starter

```yaml
openapi: 3.1.0
info: { title: Service, version: 1.0.0 }
servers: [{ url: https://api.example.com/v1 }]
paths:
  /users/{id}:
    get:
      operationId: getUser
      parameters: [{ name: id, in: path, required: true, schema: { type: string } }]
      responses:
        "200": { $ref: "#/components/responses/User" }
        "404": { $ref: "#/components/responses/Error" }
components:
  schemas:
    User: { type: object, required: [id], properties: { id: {type: string}, email: {type: string} } }
    Error: { type: object, properties: { code: {type: string}, message: {type: string} } }
  responses:
    User: { description: ok, content: { application/json: { schema: { $ref: "#/components/schemas/User" } } } }
    Error: { description: error, content: { application/json: { schema: { $ref: "#/components/schemas/Error" } } } }
```

Customize: paths, schemas, security schemes. Gen clients/server; treat as contract.
