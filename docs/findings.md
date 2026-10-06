# Findings

Status: updated 2026-10-07; findings below are based on static source inspection pinned to commit `5561802e8fc2ed699f30abc59e5e942269660db2`.

This repository is a place for research notes and inventory only. Third-party source code is not copied here. Source archaeology is deferred to a later session.

## Research log

| Date | Finding | Evidence / repository and commit | Confidence |
|---|---|---|---|
| 2026-10-06 | Archived the two requested GitHub repositories as public forks. No code analysis was performed. | See [repo-inventory.md](repo-inventory.md) | High |


## 2026-10-07 — WTwo API archaeology

1. The scanned tree has 254 tracked files. Active client code yields 181 unique method + normalized path entries after alias deduplication, including direct QR/download routes and one SSE stream. No live API request was made. Details and counts: [api-map.md](api-map.md).
2. `apiUrl` switches between `wtwotest.xiaogj.com` and `next.xiaogj.com` by `window.location.hostname`; `testUrl` is the empty string, so login/whoami paths remain same-origin. This describes frontend URL construction, not current deployment routing. See [api-topology.md](api-topology.md).
3. The shared client adds `WTwo-CompanyID` and `WTwo-AuthToken`; whoami supplies user/SID/campus/rights state. AI SSE additionally passes `Xgj-Sid` and explicitly includes browser credentials. Server-side validation and authorization rules are not in this repository. See [auth-permission-map.md](auth-permission-map.md).
4. Scheduling UI exposes separate draft save, precheck and publish stages. Publish responses may contain per-draft restrictions, conflicts or invalid fields even when `IsSuccess` is true. Resource-conflict DTOs distinguish draft, course and schedule conflicts. See [scheduling-api-flow.md](scheduling-api-flow.md).
5. Only three active API flows bind both request and response models: custom fields, dictionaries and timetable preferences. Draft/course, exam, score and financial wrappers are mostly `any`; 35 exported model declarations do not constitute a complete backend schema. See [data-model-map.md](data-model-map.md).
6. Host/domain strings and routes are public-source evidence only. A frontend reference to a host or route does not prove reachability, current service ownership or response behavior. The repository has no GitHub-detected license; no source code or real personal/student data was copied into these notes.
7. A later browser pass is planned as passive observation of normal read-only UI actions under the user's own authorized account, with all values redacted. It has not been performed. See [network-verification-plan.md](network-verification-plan.md).
