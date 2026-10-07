# Module map

Status: updated 2026-10-07 from static source at `li281077304-boop/blog/main`, HEAD `5561802e8fc2ed699f30abc59e5e942269660db2`. No code was executed and no API was called.

| Source path | Module | Observed responsibility | Evidence / notes |
|---|---|---|---|
| `wtwo/src/pages/scheduleManage/` | course / scheduling | Calendar and table scheduling, course plan/draft edits, publish checks, student changes, subscription records/queues, attendance UI | API wrappers in `src/api/arrange.ts`; flow map in [scheduling-api-flow.md](scheduling-api-flow.md). |
| `wtwo/src/pages/examManage/` | exam / scores | Exam lists/details, score import/query/analysis and export screens | `src/api/exam.ts`; request and response schemas mostly `any`. |
| `wtwo/src/pages/financialManage/` | finance | Financial period query/edit and lock/unlock UI | `src/api/financial.ts`; backend accounting behavior is not present here. |
| `wtwo/src/api/comm.ts`, `dept.ts` | user / organization / common dictionaries | Employee, campus, department, area, shift, class, custom fields, dictionary and user-setting calls | Most wrappers use `any`; organization store shapes are mapped in [data-model-map.md](data-model-map.md). |
| `wtwo/src/api/ai.ts`, `src/services/ai/` | AI Agent | SSE chat and conversation history UI | Mounted under `${apiUrl}/aiagent`; see [api-topology.md](api-topology.md). |
| `wtwo/src/services/oss.ts`, `src/types/base.d.ts` | files / upload | STS request and browser OSS upload adapter | SDK transfer URL and STS server behavior are not defined in this frontend. |
| `wtwo/src/api/http*.ts`, `handler*.ts` | transport / auth context | JSON, form, Blob, binary request clients, request headers and response handling | Header names and caveats in [auth-permission-map.md](auth-permission-map.md). |
| `wtwo/src/store/` | client state | User/session, campus selection, organization, dictionaries, custom fields and timetable preferences | These client state objects do not prove backend entities or persistence relations. |
| `wtwo/src/router/`, `src/main.ts`, `App.vue` | application shell | Route setup, who-am-I bootstrap and host/micro-frontend context | Host selection in [api-topology.md](api-topology.md). |

The API inventory contains 181 unique method/path entries after deduplicating aliases. By business module: course 96, exam 36, finance 6, organization 20, user 18, AI 3, other/legacy 2. These module labels classify frontend endpoints; they do not assert server microservice boundaries.

`chenzhengduan/blog/wtwo` remains `PRIMARY_RESEARCH_TARGET`. The earlier `klaus-caichang/vueInteraction` fork remains a historical front-end reference; it was not analyzed in this pass.


## Live navigation comparison (2026-10-07)

The current account exposes six first-level areas in the live portal: 招生营销、前台业务、教务管理、人事管理、家校服务、报表中心. The 32 direct menu destinations and their page states are listed in [live-product-map.md](live-product-map.md). Exact WTwo page matches are 排课管理 and 考试管理; course/class/timetable/make-up and several report pages are partial; CRM, HR workflow and family-school functions are predominantly live-only.
