# Data Modeling: Serialization

**Category:** Data Modeling · **Applies to:** All

| Format      | Use                 | Note                        |
| ----------- | ------------------- | --------------------------- |
| JSON        | APIs, config, debug | human, slow, no schema      |
| Protobuf    | RPC, internal       | small/fast, schema, codegen |
| MessagePack | binary JSON         | compact, no schema          |
| CBOR        | IoT/edge            | binary, std                 |
| Avro        | data pipelines      | schema registry, evolution  |

## Rules

- Boundaries JSON; perf paths binary.
- Validate at parse; don't trust input.
- Version the schema; never reuse field tags.

## Related

`modeling.md` · `schema-evolution.md`
