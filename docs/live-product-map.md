# Live product map — 校管家

- Observed: 2026-10-07, through the user's existing Chrome GUI session, normal menu navigation only.
- Site: `tms22.xiaogj.com` (main SPA; observed URL path remains `/index.html` while menu/breadcrumb state changes).
- Secondary web apps observed from the portal: `jxq.xiaogj.com` (AI family-school) and `dashboard.xiaogj.com` (operations dashboard).
- Website Cloner reference: [`JCodesMore/ai-website-cloner-template`](https://github.com/JCodesMore/ai-website-cloner-template), `master` at `ee3f5a2f31fd549b9593fa4f7cf6d2955ee593bb`. Only its Map/Observe guidance was used; Build was not run.
- Static source comparison: [`li281077304-boop/blog/wtwo`](https://github.com/li281077304-boop/blog/tree/5561802e8fc2ed699f30abc59e5e942269660db2), SHA `5561802e8fc2ed699f30abc59e5e942269660db2`.
- Evidence labels: `LIVE_UI_OBSERVED` = visible UI/menu structure; `STATIC` = source at the pinned SHA; `NETWORK_CONFIRMED` = none (no Network events were read); `INFERRED` = product-role interpretation; `UNKNOWN` = not established.
- Coverage labels: `SOURCE_COVERED`, `PARTIAL_SOURCE`, `LIVE_ONLY` compare page capability, not backend API reachability.
- Privacy: only schema/labels and page structure are recorded. No names, IDs, phone numbers, fees, class names, message bodies, or response values are retained. No raw screenshots were kept; temporary inspection crops were removed. No Network/API results are claimed.

## Portal structure

- **LIVE_UI_OBSERVED:** 6 top-level modules and 32 direct secondary menu entries were visible in this account. All 32 direct menu destinations were opened or visited; entry/forms were inspected without submitting changes.
- The TMS shell and the AI family-school and operations-dashboard apps use separate origins. The TMS pages observed here commonly remain on `/index.html`, with menu state represented by the breadcrumb/SPA view.
- The AI family-school navigation exposed **25 nested page entries**. Seven content/management views were opened at the structural level; personal-photo, grade-detail, aggregate-analysis, and setup entries were recorded from menus only.
- Report sections contain many tabs; tabs are recorded as page states, not counted as separate top-level menu pages.

## Direct menu pages

| 一级模块 | 二级页面 | Route | 核心功能 | 主要字段/操作 | 源码覆盖 |
|---|---|---|---|---|---|
| 招生营销 | 意向学员管理 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Leads/intent-customer pipeline | Tabs: 全部、已分配客户、今日新增客户；quick filters; visible selectors: 姓名/拼音/电话、负责人、客户状态、招生来源、跟进类型/沟通人; list and actions | `LIVE_ONLY` |
| 招生营销 | 意向客户导入 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Import prospective-customer records | Download-template entry; import form visible; no file selected/uploaded | `LIVE_ONLY` |
| 前台业务 | 新学员报名 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | New learner registration | Visible required fields include name, phone, sex, referring-teacher account; form not submitted | `LIVE_ONLY` |
| 前台业务 | 学员信息管理 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Learner profile/list and communication subview | Tabs: 学员信息管理、学员沟通管理; search name/initials/student number/phone; formal/non-formal filter; check-in/sign-out/bulk/custom-column controls. Visible table headers: name, student number, phone, remaining quantity, 1:1 homeroom teacher, current class count, grade, status, actions | `PARTIAL_SOURCE` — WTwo uses learner references/selectors, not this full profile page |
| 前台业务 | 公告与通知 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Notices and drafts | Tabs: 公告、通知、草稿箱; title search; internal/external scope | `LIVE_ONLY` |
| 前台业务 | 学员信息导入 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Learner import | Download-template entry; import workflow not executed | `LIVE_ONLY` |
| 教务管理 | 课程管理 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Course catalogue/charging attributes | Search by course name; visible filters for group/1:1/1:many, enabled status, regular/periodic charging, dynamic consumption; visible columns: course name, price, unit, grade, subject, type, class type, term, year, 1:1, periodic fee, dynamic consumption (remaining columns not captured) | `PARTIAL_SOURCE` — WTwo has course selection/planning APIs, not this full catalogue screen |
| 教务管理 | 班级管理 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Regular and per-day class lists | Tabs: 常规班级、按天计费班级; filters for class name/type/status; visible columns: class name, course, lead teacher, assistant, homeroom teacher, default room, lesson time, enrolled/target count, actions | `PARTIAL_SOURCE` — class selectors/references exist, no full class-management page in WTwo |
| 教务管理 | 排课管理 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | List/calendar/table scheduling | Tabs: 排课列表、课表日历、表格排课. Filters include lesson date range, learner, class, lead teacher, assistant, room; entry controls include schedule/calendar settings, booking management, availability lookup, add-schedule entry. Calendar modes: teacher, 1:1 learner, class, classroom, campus; week/month and date navigation | `SOURCE_COVERED` — closest WTwo page/module match |
| 教务管理 | 课表和点名 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Classroom timetable and attendance/card records | Tabs: 教室课表、刷卡考勤、刷卡记录; card-record filters include card number, sign-in time, learner; no attendance mutation performed | `PARTIAL_SOURCE` — WTwo has attendance/calendar flows; card-swipe pages are not confirmed as fully represented |
| 教务管理 | 补课管理 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Make-up/absence follow-up | Filters include course, class, period, absence reason/date, teacher and assistant | `PARTIAL_SOURCE` — student adjustment APIs exist, but this management screen is not in WTwo |
| 教务管理 | 考试管理 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Exam and score management/query | Tabs: 考试管理、成绩查询; exam-name search and date range; no import/edit/delete/export action used | `SOURCE_COVERED` — matching WTwo exam module |
| 教务管理 | 证书管理 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Certificate type/list lookup | Certificate-type sidebar, all-types item, certificate-name search, add entry (not opened) | `LIVE_ONLY` |
| 人事管理 | 审批中心 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Approval list/search | Filters: learner, approval business, approval status, application date; query/reset/export entries (export not clicked) | `LIVE_ONLY` |
| 人事管理 | 工资管理 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Commission schemes and lesson-fee lookup | Tabs: 提成方案设置、课时费查询、工资项目录入; filters include scheme, course, teacher, period/year/month, employee; no salary values saved | `LIVE_ONLY` |
| 家校服务 | AI家校圈 | `jxq.xiaogj.com/home` | Family-school portal landing/onboarding | Left navigation groups for lesson feedback, learner dynamics, materials, deep services, analysis, settings/service center; landing explains teacher permissions/settings and AI daily/deep-service setup | `PARTIAL_SOURCE` — WTwo has generic AI chat, not these family-school workflows |
| 家校服务 | 课后点评 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Lesson-feedback reports | Filters: class name and lesson time range; query/reset entries | `LIVE_ONLY` |
| 家校服务 | 通知 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Notification statistics and lookup | Tabs: 通知统计、环比统计、通知查询; lesson-time range | `LIVE_ONLY` |
| 家校服务 | 打卡 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Check-in statistics/query | Tabs: 打卡统计、环比统计、打卡查询; publish-time range and check-in type | `LIVE_ONLY` |
| 家校服务 | 作业 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Homework statistics/query | Tabs: 作业统计、环比统计、作业查询; publish-time range | `LIVE_ONLY` |
| 家校服务 | 群消息 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Group management and message queries | Tabs: 群组管理、消息查询、环比统计; scope radio; group name/type, disabled-group toggle, teacher filter | `LIVE_ONLY` |
| 家校服务 | 个人消息 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Personal-message query/statistics | Tabs: 消息查询、环比统计; scope radio; message-content/type and communication-date filters; no message content retained | `LIVE_ONLY` |
| 家校服务 | 老师评价学生 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Teacher-to-learner ratings/statistics | Tabs by campus/class/teacher, learner-star lookup, learner query; course/class/lesson-time filters | `LIVE_ONLY` |
| 家校服务 | 家长评价老师 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Parent ratings of teachers | Tabs: 家长评价统计、老师得分统计、评价详情查询; course/class/lesson-time filters | `LIVE_ONLY` |
| 报表中心 | 招生分析报表 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Lead conversion/referral reports | Tabs: customer status, customer analysis, conversion, referral, share conversion; activity-name filter; query/export entries | `LIVE_ONLY` |
| 报表中心 | 学员分析报表 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Learner communication, attendance, class and retention analysis | 12 tabs observed, including communication log, distribution, class count, attrition, leave, source, enrolled courses, unassigned learners, referrals and attendance summaries; filters include contact person/date, learner, source/type, valid/invalid | `PARTIAL_SOURCE` — WTwo has exam/student-score analytics, not this learner-report suite |
| 报表中心 | 电子推荐卡报表 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Referral-card issue/redemption reports | Tabs: redemption records, usage detail, teacher issuance detail; activity-name filter; query/export entries | `LIVE_ONLY` |
| 报表中心 | 收费报表 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Receipts, funds, fees, refunds, wallet and transfer reports | 14 tabs observed, including receipts, funds, course fees/sales, item sales, wallets, refunds, arrears and transfers | `PARTIAL_SOURCE` — WTwo financial-period lock APIs do not cover these fee reports |
| 报表中心 | 课消报表 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Consumption and usage reports | 9 tabs observed, including monthly fees, learner consumption/detail, alerts, overdue fees and prepaid consumption; course and date filters | `PARTIAL_SOURCE` — WTwo has course/attendance references, not this report suite |
| 报表中心 | 业绩报表 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Counselor/teacher performance | Tabs: 咨询老师业绩、老师业绩、1v1班主任业绩; date, employee and role filters | `PARTIAL_SOURCE` — WTwo has employee/commission traces, not these performance screens |
| 报表中心 | 班级报表 | `tms22.xiaogj.com/index.html` · breadcrumb SPA state | Class, attendance, promotion, renewal and room-utilization reports | 9 tabs observed: class count/roster, attendance/detail/rate, promotion/renewal/full-class rate, room use, teacher-class count; course/class/type filters | `PARTIAL_SOURCE` — WTwo has class/calendar data, not this reporting UI |
| 报表中心 | 运营数据大屏 | `dashboard.xiaogj.com/index.html` | Operations dashboard | Full-screen KPI cards and trend charts; labels indicate enrollment/activity, learner and lesson/operational metrics; all metric values excluded | `LIVE_ONLY` |

## AI family-school nested pages (25 entries)

These are internal navigation entries beneath the AI family-school app. `OPENED` means the page/form structure was viewed without submitting; `MENU_ONLY` means only the menu item was observed. Route is `UNKNOWN` where it was not safely captured.

| Parent group | Nested page | Route | Observation | Source coverage |
|---|---|---|---|---|
| 上课点评 | 发布点评 | `jxq.xiaogj.com/post-review` | `OPENED`: class/course filters, query/reset/expand; list view | `LIVE_ONLY` |
| 上课点评 | 点评管理 | `jxq.xiaogj.com/points-management` | `OPENED`: campus/learner filters; PDF generation/records, export and custom-column entries; no generation/export clicked | `LIVE_ONLY` |
| 学员动态 | 发布动态 | `jxq.xiaogj.com/publishDynamic` | `OPENED`: publish-campus and content-category fields, choose-learner area; no learner selected or post submitted | `LIVE_ONLY` |
| 学员动态 | 动态管理 | `jxq.xiaogj.com/dynamicManagement` | `OPENED`: campus/learner filters, query/reset/expand; export/share entries not clicked | `LIVE_ONLY` |
| 素材库 | 分类相册 | `UNKNOWN` | `MENU_ONLY`; album contents not opened | `LIVE_ONLY` |
| 学员总结 | 创建总结 | `UNKNOWN` | `OPENED` at structure/banner level; no learner selected and no generation started | `PARTIAL_SOURCE` — only generic WTwo AI chat exists |
| 学员总结 | 发布总结 | `jxq.xiaogj.com/publishSummary` | `OPENED`: campus/learner filters; table headers indicate task/rule/type/generation and count fields; no publish action | `PARTIAL_SOURCE` |
| 学员总结 | 总结管理 | `jxq.xiaogj.com/summaryManagement` | `OPENED`: campus/learner filters, query/reset/expand, export entry not clicked | `PARTIAL_SOURCE` |
| 学员影集 | 创建影集 | `UNKNOWN` | `MENU_ONLY`; personal images not opened | `LIVE_ONLY` |
| 学员影集 | 发布影集 | `UNKNOWN` | `MENU_ONLY`; personal images not opened | `LIVE_ONLY` |
| 学员影集 | 影集管理 | `UNKNOWN` | `MENU_ONLY`; personal images not opened | `LIVE_ONLY` |
| 学员成绩报告 | 创建成绩报告 | `UNKNOWN` | `MENU_ONLY`; student grades not opened | `LIVE_ONLY` |
| 学员成绩报告 | 发布成绩报告 | `UNKNOWN` | `MENU_ONLY`; student grades not opened | `LIVE_ONLY` |
| 学员成绩报告 | 成绩报告管理 | `UNKNOWN` | `MENU_ONLY`; student grades not opened | `LIVE_ONLY` |
| BI数据统计 | 上课点评分析 | `UNKNOWN` | `MENU_ONLY`; aggregate analytics not opened | `LIVE_ONLY` |
| BI数据统计 | 学员动态分析 | `UNKNOWN` | `MENU_ONLY`; aggregate analytics not opened | `LIVE_ONLY` |
| BI数据统计 | 学员总结分析 | `UNKNOWN` | `MENU_ONLY`; aggregate analytics not opened | `LIVE_ONLY` |
| BI数据统计 | 学员影集分析 | `UNKNOWN` | `MENU_ONLY`; aggregate analytics not opened | `LIVE_ONLY` |
| AI运营星图 | 创建星图 | `UNKNOWN` | `MENU_ONLY`; no creation flow started | `LIVE_ONLY` |
| AI运营星图 | 星图管理 | `UNKNOWN` | `MENU_ONLY`; analytics not opened | `LIVE_ONLY` |
| 家校圈设置 | AI助手设置 | `UNKNOWN` | `MENU_ONLY`; no setting changed | `PARTIAL_SOURCE` — generic WTwo AI Agent only |
| 家校圈设置 | 素材库相册设置 | `UNKNOWN` | `MENU_ONLY`; no setting changed | `LIVE_ONLY` |
| 家校圈设置 | 成长集设置 | `UNKNOWN` | `MENU_ONLY`; no setting changed | `LIVE_ONLY` |
| 服务中心 | 资产中心 | `UNKNOWN` | `MENU_ONLY`; details not opened | `LIVE_ONLY` |
| 服务中心 | 家校服务宝典 | `UNKNOWN` | `MENU_ONLY`; details not opened | `LIVE_ONLY` |

## Observation limits

- No DevTools Network data was used. This is a visual/UI map only; it does not confirm API behavior or backend schemas.
- The Chrome session showed the requested product and the page/menu states listed here. Login credentials and session values were not accessed or stored.
- Search/export/add/edit/publish controls were recorded as visible entries only. No export, submit, upload, schedule, attendance, payment, payroll, or other business action was performed.
- **Screenshot index:** no screenshots retained (`screenshot_ids: none`). Only temporary crops were used for visual inspection and deleted; none were committed.
