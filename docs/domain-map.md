# Domain map

Status: updated 2026-10-07 from static inspection of `wtwo/` at `5561802e8fc2ed699f30abc59e5e942269660db2`. This map records frontend references only; “related” does not mean a database foreign key.

| Domain area | Frontend evidence | Relationships / limits | Detailed map |
|---|---|---|---|
| Company and tenant | `useUser().user.CompanyID/CompanyName`; `loginInfo.WTwo_CompanyID` | Inline state only; no dedicated exported Company model under `src/types/model`. | `data-model-map.md` — requested entity coverage. |
| Campus and organization | Local `ICampusModel`, `IAreaModel`, `IDepartModel`, department tree; whoami `CampusList` | Campus references Area; department references Area/parent; these are client-side shapes, not server schema. | `src/store/organization.ts`; `data-model-map.md`. |
| User and permissions | Inline whoami user fields, `SID`, `IsAdmin`, `Rights`; `CampusList` | Rights and current campus affect UI gates/filters; server enforcement remains unknown. | `auth-permission-map.md`. |
| Course and draft | `Request`, `TableCourseClass`, draft view/save/publish/precheck DTOs | Draft references campus, class, classroom, student, shift/course, subject, teachers and assistants. Some UI and generated shapes conflict. | `data-model-map.md`; `scheduling-api-flow.md`. |
| Schedule and resources | Calendar queries, schedule conflict list, teacher/assistant/classroom references | Conflict model distinguishes draft/course/schedule; no canonical exported Schedule entity. | `scheduling-api-flow.md`. |
| Subscription course | Draft subscription flags, record/queue APIs, local subscription UI fields | Rule and queue behavior is only partly visible; no full learner-initiated enqueue contract. | `scheduling-api-flow.md`. |
| Exam and score | Exam and score endpoints/pages | No dedicated exported Exam/Score model under `src/types/model`; payloads are mostly `any`. | `api-map.md`; `data-model-map.md`. |
| Financial period | Period query/lock/edit APIs and local page state | No exported FinancialPeriod model; local list is `any[]`. | `api-map.md`; `data-model-map.md`. |
| User preference | Timetable and color preference request/view models; user settings store | Timetable preferences are the best-defined user-scoped model set in this source snapshot. | `data-model-map.md`. |

The complete field-level inventory for 35 exported `src/types/model` declarations, shared API field declarations, and the user-requested entity checklist is in [data-model-map.md](data-model-map.md). The APIs themselves are inventoried in [api-map.md](api-map.md).
