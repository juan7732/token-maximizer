# Error Handling: Taxonomy

**Category:** Error Handling · **Applies to:** All

Classify before handling. Axes are independent.

- **Transient vs permanent:** does retry help? Blip yes, bad request no.
- **Retryable vs fatal:** safe and useful to retry? Non-idempotent writes are not.
- **User-facing vs internal:** human acts, or logs only? Sets message tone.
- **Expected vs unexpected:** contract (validation) or bug? Expected → type; unexpected → loud.

| Failure           | Transient | Retryable        | Audience | Expected |
| ----------------- | --------- | ---------------- | -------- | -------- |
| Timeout           | yes       | yes (idempotent) | internal | yes      |
| 400 bad input     | no        | no               | user     | yes      |
| Unique constraint | no        | no               | user     | yes      |
| Nil deref         | no        | no               | internal | no       |
| Disk full         | maybe     | after cleanup    | internal | no       |

## Use

Transient+retryable → backoff with cap. Permanent+user → clear message, no retry. Unexpected → fail loud, alert, never swallow.

## Project taxonomy

Name categories once, map each to a policy (retry/surface/alert), reuse names across services. [CAPTURE] specifics.

## Related

`philosophy.md` · `propagation.md`
