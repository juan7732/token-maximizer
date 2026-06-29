# Runbook: Incident Response

**Category:** Ops · Runbook

1. **Declare** — severity, owner, channel.
2. **Stabilize** — stop bleeding: rollback, scale, feature-flag off. Mitigate before diagnose.
3. **Diagnose** — recent deploy? error rate, latency, saturation. Check dashboards + logs.
4. **Fix** — minimal change; verify metrics recover.
5. **Communicate** — status updates on cadence.
6. **Postmortem** — blameless, timeline, root cause, action items.

**First moves:** what changed? roll back. Capacity? scale. Dependency down? circuit-break.

**See also:** memory-leak.md, checklists/pre-deploy.md.
