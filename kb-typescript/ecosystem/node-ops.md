# Node Operations

**Language:** TypeScript · **Category:** Ecosystem

- Graceful shutdown: drain on SIGTERM, close server + pools.
- Health: `/livez` (process up), `/readyz` (deps ok).
- One process per core; cluster/PM2 or k8s replicas.
- Env via Zod-validated config; fail fast on bad config.
- Structured JSON logs (pino), trace id propagation.

```ts
process.on('SIGTERM', async()=>{ await server.close(); await db.end(); process.exit(0); });
```

**Gotchas:** unhandled rejection crashes; blocking event loop; no infinite keepalive on shutdown.

**See also:** performance/profiling-guide.md.
