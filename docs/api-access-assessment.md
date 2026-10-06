# `wtwo` API access assessment

- Source repository: `chenzhengduan/blog`
- Scope: `wtwo/` frontend only
- Commit: `5561802e8fc2ed699f30abc59e5e942269660db2`
- Analysis date: 2026-10-07
- Status: static assessment; no DNS lookup, HTTP request, login, build, browser session, or endpoint replay was performed

## Access taxonomy

- **A** — public source already contains the complete URL, HTTP method, request/response model, and frontend call flow.
- **B** — behavior can be verified later through ordinary browser use after the user signs in legally with their own account.
- **C** — an endpoint trace exists, but parameters or response are incomplete and require later observation of ordinary live-site behavior.
- **D** — unobtainable from this repository: server source, database, internal algorithms, unauthorized data, or secret/token-generation mechanisms.

These classes describe evidence availability. They do not grant permission to call an endpoint. B and C remain unverified in this report.

## A — complete static contracts

One flow satisfies all four static criteria. The relative `/api/...` wrappers are not counted here because their deployed origin is supplied by browser context and cannot be called a complete URL from this repository alone.

| Flow | Static contract | Call flow evidence |
|---|---|---|
| Get timetable preferences | GET `https://next.xiaogj.com/api/course/TimetablePreference/GetAll` on production-class hostnames or GET `https://wtwotest.xiaogj.com/api/course/TimetablePreference/GetAll` on the explicit test/local hostname set; no request body; response `IResponse<TimetablePreference_ViewModel[]>`. | Wrapper: `wtwo/src/api/arrange.ts:90-95`; hostname selection: `wtwo/src/store/index.ts:3-25`; cache/default merge: `wtwo/src/store/timetablePreferences.ts:180-223`; model: `wtwo/src/types/model/timetable-preference.ts:63-76`. These are statically configured URL possibilities, not reachability claims. |

## B — legal-login browser verification candidates

The following facts can be checked through normal product behavior after the user signs in with an authorized account. They have **not** been checked here:

1. The application first calls `whoami`, then stores user/config/campus/rights data and derives frontend permission checks from `IsAdmin` or `Rights`. Evidence: `wtwo/src/api/index.ts:24-29`; `wtwo/src/main.ts:99-137,170-173`.
2. In the micro-frontend path, company ID and auth token are supplied by the host application. In standalone mode, the token is read from an existing browser cookie. Evidence: `wtwo/src/App.vue:60-80`.
3. JSON, form, blob and binary request clients attach company/token header names from the same Pinia state. Evidence: `wtwo/src/api/http.ts:6-13`; `http-form.ts:5-27`; `http-blob.ts:6-13`; `http-binary.ts:5-12`.
4. The ordinary UI can expose read-only requests for who-am-I, fields, dictionaries, timetable preferences, course/calendar data and exam lists. The source contains their page/store callers, so DevTools can later confirm the observed method, path, status and schema keys without replaying requests.
5. The AI UI uses the current user SID plus the same company/token state and cookies for its normal stream request. Evidence: `wtwo/src/api/ai.ts:45-76,223-227`.

Two same-origin flows have complete request/response models and call chains but remain B rather than A because their page origin is supplied by the browser: custom fields (`wtwo/src/api/comm.ts:3-11`; `wtwo/src/store/fields.ts:18-34`) and dictionary lookup (`wtwo/src/api/comm.ts:13-27`; `wtwo/src/store/dict.ts:17-45,54-82`).

B means “observable through authorized normal use,” not “public” or “anonymous.”

## C — endpoint traces with incomplete contracts

| Trace | Known statically | Missing statically | Evidence |
|---|---|---|---|
| Save timetable time range | POST path and typed request. | Response `Data` model. | `wtwo/src/api/arrange.ts:97-103`; request type at `timetable-preference.ts:99-103`. |
| Save timetable row height | POST path and typed request. | Response `Data` model. | `wtwo/src/api/arrange.ts:105-111`; type at `timetable-preference.ts:105-109`. |
| Save all timetable settings | POST path and UI flow. | Wrapper request/response are `any`; narrower imported types are only used by commented wrappers. | `wtwo/src/api/arrange.ts:63-88`. |
| Batch-save course drafts | POST path, UI change collection and generated candidate models. | Active wrapper is `Array<any>` with untyped response; generated types are not bound to it. | `wtwo/src/api/arrange.ts:137-143`; `class-table-course.vue:978-1016`; `course-draft-req-form-model.ts:9-123`. |
| Draft pre-check | POST path and `{IDList}` request; UI reads the envelope. | Response DTO is not attached to the wrapper and two local conflict shapes disagree. | `wtwo/src/api/arrange.ts:337-343`; check dialog `:107-125`; `table-course-class.ts:70-110,125-171`. |
| Publish draft | POST path, typed request and UI response handling. | Declared response type is not attached to the active wrapper. | `wtwo/src/api/arrange.ts:562-568`; `class-table-course.vue:6603-6616`; `table-course-class.ts:112-142`. |
| STS/OSS | STS path, request keys and upload result type. | STS credential response contract and SDK-internal transfer calls. | `wtwo/src/api/index.ts:31-36`; `wtwo/src/services/oss.ts:13-61`; `wtwo/src/types/base.d.ts:2-19`. |
| AI chat | stream/messages/conversations paths, request shell, auth header names and SSE event names. | Session/content payload schemas and server conversation models. | `wtwo/src/api/ai.ts:5-20,45-76,113-174,246-280`. |
| Remaining API wrappers | Method/path and callers are generally visible. | Most parameters and response payloads are `any`. | Representative declarations: `wtwo/src/api/arrange.ts:17-62,113-128`; `exam.ts:4-25`; `comm.ts:29-76`. |

No C item was promoted to A merely because a similarly named interface exists; the active wrapper must bind the model or the call flow must establish every field.

## D — unavailable from this repository

The following cannot be established from the frontend source:

- server controllers, services, validation order, authorization enforcement and error-generation logic;
- database schema, table names, indexes, retention, tenant isolation and data migrations;
- whether any endpoint is currently deployed, reachable, deprecated, feature-flagged or environment-specific;
- real credentials, token/SID generation, refresh, signing, expiry and revocation mechanisms;
- records or permissions belonging to any user, company, learner, teacher, class or campus;
- OSS signing policy and the implementation behind the browser-provided `xgjzt` SDK;
- definitive compatibility between current servers and orphaned/generated TypeScript declarations.

Frontend code only shows that tokens are received from the host application or an existing cookie and then forwarded. It does not contain token-generation logic. Evidence: `wtwo/src/App.vue:60-80`; `wtwo/src/api/http.ts:6-13`.

## Security and redaction findings

- A commented monitoring configuration contains a credential-like value. Its value is intentionally omitted here and must never be copied into research notes, issues, logs or browser captures. Evidence location only: `wtwo/vite.config.ts:34-39`.
- Runtime values for `WTwo-AuthToken`, `WTwo-CompanyID`, `Xgj-Sid`, cookies, user IDs, names, phone numbers, campus/class/student data and upload credentials are sensitive. This report records only field/header names.
- The AI helper can place SID in a query string for an allowed link. Any future URL capture must replace its value with `<REDACTED_SID>`. Evidence: `wtwo/src/services/ai/useChat.ts:229-250`.
- Response error messages are rendered with `dangerouslyUseHTMLString`. This is a frontend risk indicator, but server sanitization cannot be assessed statically. Evidence: `wtwo/src/api/handler.ts:25-34`.

## Static conclusion

The repository provides strong endpoint-discovery evidence and one complete A-class flow. It also shows how an already authenticated browser supplies tenant and session context. It does **not** establish current network reachability or server-side behavior. All B/C conclusions require a separate, user-authorized browser observation session following `network-verification-plan.md`.
