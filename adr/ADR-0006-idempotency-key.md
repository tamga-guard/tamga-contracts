# ADR-0006 — idempotency_key formatı

**Format (v1):**
`v1:<project_id>:<stage>:<host>:<bucket_1m>`

- `bucket_1m` = `ts_utc` dakikaya yuvarlanmış ISO (örn. `2025-10-17T12:34Z`)
- Panel tarafında ayrıca `idempotency_hash = sha256(<format_string>)` tutulabilir (opsiyonel).

**Gerekçe**
- İnsan okunur ve deterministik.  
- CT/Probe/SERP yinelenmelerinde doğal tekilleştirme sağlar.  
- Panel DB'de `UNIQUE(idempotency_key)` ile conflict-free upsert kolaylaşır.

**Kapsam**
- `event.v1` için zorunlu alan.  
- Webhook tüketicileri `409/OK-duplicate` semantiğini kullanabilir (idempotent).
