# WTwo source vs live product gap map

- Live map date: 2026-10-07.
- Live evidence: normal Chrome GUI menu/page observation only (`LIVE_UI_OBSERVED`); no Network panel or API behavior was captured.
- Source baseline: `li281077304-boop/blog/wtwo`, SHA `5561802e8fc2ed699f30abc59e5e942269660db2`.
- Research baseline before this map: `xiaogj-research/main` SHA `ad406d9b42edb24030a060fe94b7e01308ebfea8`.
- The six first-level modules exposed 32 direct menu pages. The AI family-school section exposed a further 25 nested menu entries. See [live-product-map.md](live-product-map.md) for the complete menu/page inventory and field labels.
- Evidence tags are separate from coverage tags: `LIVE_UI_OBSERVED` is visible page/menu structure, `STATIC` is the pinned WTwo source, `INFERRED` is a product interpretation, `UNKNOWN` is unresolved, and `NETWORK_CONFIRMED` has no entries in this pass because Network events were not used.
- Coverage counts are page-level judgments: **2 SOURCE_COVERED**, **11 PARTIAL_SOURCE**, **19 LIVE_ONLY** among the 32 direct menu pages. Nested family-school pages are classified separately in `live-product-map.md`.

## WTwo 已覆盖

- **教务管理 → 排课管理**: WTwo `scheduleManage` covers schedule list/calendar/table, course drafts, precheck/publish flow, resource checks, schedule/copy/adjust operations. Live UI showed matching list, calendar, and table entry points; calendar modes include teacher, 1:1 learner, class, classroom, and campus.
- **教务管理 → 考试管理**: WTwo `examManage` corresponds to the live exam-management and score-query tabs. This confirms page presence only; no Network contract was compared.

## WTwo 部分覆盖

- **课程管理、班级管理**: WTwo has course/class selectors and course-plan/class references; the live system exposes full catalogue/class-management pages and fields that are not represented as complete WTwo screens/models.
- **课表和点名、补课管理**: WTwo contains calendar, attendance and student-adjustment flows; the live system also has card-swipe attendance records and a separate make-up/absence workflow. Exact parity is unknown.
- **学员信息管理**: WTwo uses learner IDs/basic display names and selection APIs, but the live profile page exposes a broader record, including student number, phone, remaining quantity, homeroom teacher, class count, grade and status. No values were retained.
- **学员分析、收费、课消、业绩、班级 reports**: WTwo has related exam, course, attendance, finance-period or employee traces, but the live report tabs are broader and are not represented as matching report pages.
- **AI家校圈 / 学员总结 / AI助手设置**: WTwo has a generic AI Agent chat; the live family-school app exposes structured lesson feedback, learner dynamics, summaries and family-facing service flows. The generic chat does not establish coverage of these workflows.

## 现网独有

- 招生 leads and CRM pipeline, prospective-customer import, enrollment registration and learner import.
- General student profile/communication management, announcement/draft workflow, approvals and payroll.
- Family-school notification, check-in, homework, group/personal messages, teacher/parent ratings.
- Certificate management, recommendation-card reporting, operational dashboard, student albums and student-grade report workflows.
- Dedicated family-school AI summary create/publish/manage, BI analysis pages and service/settings menus. These are live UI entries observed under the current account; no backend schema was inferred.

## 我们未来系统值得做

1. A unified learner record linking profile, class/course enrollments, schedule, attendance/consumption, communication and renewal state.
2. Teacher/assistant/class/room assignment with weekly views and visible conflict/availability checks.
3. A lesson record connected to the scheduled course: what was taught, attendance, actual progress and evidence.
4. A handout/material mapping by course/subject/grade and a per-learner “next lesson” plan.
5. A teacher-centric weekly learner list and a learner-centric recent-course history with privacy-aware role filters.
6. A controlled parent communication timeline and after-class feedback workflow, with source and consent recorded.
7. AI learning summaries grounded in lesson/assessment evidence, teacher-reviewed before sharing.
8. Small, explainable reports for student status, course usage, renewals and campus operations, rather than copying every legacy report tab.
9. Audit trails for changes to schedule, attendance, finance and AI-published content.
10. A single product navigation model that links student → class/course → lesson → handout → next plan → learning analysis.

These are product-design candidates from observed workflows, not a build scope for this pass.

## 我们暂时不需要做

- Recreating every legacy report/tab, recommendation-card workflow, school notice channel or media album in v1.
- Payroll, financial-period locking, cashier/refund/wallet workflows unless the initial operating model requires them.
- Import/export automation or bulk mutation flows before an approved data/interface contract exists.
- Copying the current UI, source code, real records, brand assets or AI-generated student content.
- Treating visible buttons, frontend permissions or menu visibility as proof of backend authorization or stable API availability.

## Data boundary for a first release

```text
必须同步（仅在有正式授权接口/数据约定后）：
- 学员、教师/员工、班级、课程/Shift、校区基础标识
- 排课、实际上课/考勤等运行记录（保留来源和更新时间）

实时读取即可（首版不做长期副本，具体权限待确认）：
- 费率/收费与退费明细、绩效、工资、审批记录、家校消息
- 临时报表和学校运营仪表盘

我们自己新增并作为自有系统事实源：
- 教学记录与教师复核内容
- 当前讲义/材料映射
- 下一节课计划
- AI 学情分析、证据引用、审核与家长共享状态
- 自研系统内的工作流、配置与审计记录
```

The browser pass demonstrates that an authorized UI can display certain records; it does **not** confirm that external API synchronization is supported. Until an official interface and permission scope are confirmed, do not build against undocumented endpoints or treat screen fields as a stable contract.
