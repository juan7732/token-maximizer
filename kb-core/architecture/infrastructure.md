# Infrastructure

**Category:** Architecture

**Containers:** small base (distroless/alpine), multi-stage, non-root, pinned digests. One process; health endpoints.

**CI/CD:** PR = build+test+lint+scan. Main = artifact. Deploy = staged (canary/blue-green), auto-rollback on SLO breach.

**IaC:** Terraform (multi-cloud), Pulumi (code), CDK (AWS). State remote+locked. Plan before apply.

**Local dev:** compose/devcontainer mirrors prod deps. One command up.

**Deploy targets:** Cloudflare/edge for static+functions; k8s when scale/team needs it; PaaS otherwise. Don't k8s a side project.

**See also:** kb-ops/checklists/pre-deploy.md.
