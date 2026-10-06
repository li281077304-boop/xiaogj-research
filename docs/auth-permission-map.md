# 认证与权限静态地图

- 分析日期：2026-10-07。
- 源码：`li281077304-boop/blog/main`，HEAD `5561802e8fc2ed699f30abc59e5e942269660db2`，范围 `wtwo/`。
- 只阅读公开前端；没有调用登录/业务接口，没有生成、伪造或破解凭据。只记录凭据/header 名称与传递关系，不记录值。
- 路径均为该 SHA 的 `wtwo/` 相对路径。[固定版本源码](https://github.com/li281077304-boop/blog/tree/5561802e8fc2ed699f30abc59e5e942269660db2/wtwo)。

## 1. 主站 API 如何识别登录用户？

`src/api/index.ts:9-29` 定义 GET `/api/login/login`（params: any）、POST `/api/login/logout`（data: any）、POST `/api/user/whoami`。testUrl 为空，三者是同源相对路径。`src/main.ts:102-130` 读 whoami.Data，保存用户对象、CompanyID、SID、Config、CampusList。

**初步推断 Medium**：主站看起来沿用浏览器会话/Cookie。普通 FetchService（`common/tool/http/fetch.ts:138-156`）没有显式 credentials、Authorization 或 SID header；同源请求按浏览器默认行为可携带 Cookie。不能确认 Cookie 名称、SID 是否就是 Cookie、后端会话机制及跨域支持。普通拦截器还附带 WTwo headers，但不能断言 Legacy 必须使用它们。

此版本未能确认独立 WTwo 登录 UI/参数模型。main.ts 导入 login 不是登录行为证据。AI 401 分支推到 `/login`，但 `src/router/index.ts:14-25` 路由汇总没有定义登录路由；是否由宿主提供未知。

## 2. 新服务如何得到租户身份？

**High（前端传递关系）**：`useLoginInfo` 保存 WTwo_CompanyID、WTwo_AuthToken（`src/store/index.ts:30-39`）。

| 场景 | CompanyID 来源 | AuthToken 来源 | 证据 |
|---|---|---|---|
| 微前端 | `window.microApp.getData().loginInfo.CompanyID` | 同一对象的 WTwo_AuthToken | `src/App.vue:60-65` |
| 单独运行 | `useUser().user.CompanyID`（whoami） | document.cookie 的 WTwo-AuthToken，decodeURIComponent | `src/App.vue:75-80`；`src/main.ts:119-124` |

JSON/Form/Blob/Binary request interceptor 添加 `WTwo-CompanyID`、`WTwo-AuthToken`（`src/api/http.ts:7-11`、`http-form.ts:7-12`、`http-blob.ts:7-11`、`http-binary.ts:6-10`），没有按服务域名限定。租户头是客户端声明；后端如何验证 tenant 与 token 绑定未知。

初始化先 whoami，后 onMounted 获取宿主信息（`src/main.ts:230-252`、`src/App.vue:45-80`），不能保证所有部署不存在首次空 header/宿主更新时序问题。

## 3. AI Agent 如何得到身份上下文？

- SSE POST `/aiagent/api/chat/stream` 发 `Xgj-Sid`、`Xgj-Param-Compatible`、`WTwo-CompanyID`、`WTwo-AuthToken`，显式 credentials include（`src/api/ai.ts:45-75,225-226`）。SID 来自 whoami.Data.SID（`src/main.ts:122-124`）。High。
- 消息历史/会话列表 GET 额外带 Xgj-Sid、Xgj-Param-Compatible（`src/api/ai.ts:246-280`），WTwo 两个 header 由普通拦截器补充；没有显式 include，与 SSE Cookie 策略不同。
- SSE 401 清除 localStorage 键 `xgj-aiagent-user-token` 并推到 `/login`（`src/api/ai.ts:93-101`）；未见此键在请求中被读取成 Authorization Bearer，不能仅凭键名确认认证协议。
- 内部链接白名单分支把 Xgj-Sid 放进 URL 查询（`src/services/ai/useChat.ts:230-250`）。后续 HAR/截图/URL 必须去掉其值；该客户端行为不证明后台接受任意 SID。

## 4. 校区权限来自哪里？

whoami.Data.CampusList 写入 useUserCampuses 并初始化当前校区 ID（`src/main.ts:129-135`）；`src/components/business/select/campus-select.vue:96-105` 从此 store 建下拉列表。宿主 data.campus 可初始化或通过全局监听变更**当前选择**（`src/App.vue:66-71`）。应区分可用校区集与界面筛选校区，当前选中字符串不能当作授权凭证。

组织目录有 `/api/depart/QueryWithEmployeeRight`（`src/api/dept.ts:13-18`），名称提示按员工权限查询，但无后端实现可证明实际约束。

## 5. 前端权限表现

`src/main.ts:170-173` 用 whoami.Rights/IsAdmin 定义 `window.$xgj.op(pName)`：管理员或 Rights 包含权限名则通过。组件用布尔值控制 tab、操作入口、复选框和编辑能力。

| 作用 | 权限名示例 | 证据 |
|---|---|---|
| 新增班级/学员/预约课 | NewCourse_ClassCourse、NewCourse_StudentCourse、NewCourse_SubscribeCourse | `src/pages/scheduleManage/scheduleManage.vue:264-266` |
| 修改/取消/删除/调课 | NewCourse_CourseEdit、NewCourse_CourseCancel、NewCourse_CourseDelete、NewCourse_CourseStudentAdjustLesson | `src/pages/scheduleManage/popup/arrangeInfo.vue:862-871` |
| 学员增删 | NewCourse_CourseStudentEdit、NewCourse_CourseStudentRemove | 同上 `:865-866` |
| 允许关闭冲突检查 | NewCourse_IngoreCourseConflict（源码拼写） | `src/pages/scheduleManage/popup/addArrangeForm.vue:115,147`；`CourseDraftPublishDialog.vue:107` |
| 复制/移动 | NewCourse_CoursePlanViaCopy / 某些视图用 CoursePlanViaCopy | `src/pages/scheduleManage/arrangeCanlendar/arrangeCanlendar.vue:536`；`timeCanlendar/TimeCanlendarView.vue:2210` |
| 预约取消/导出 | NewCourse_CancelSubscribeCourseRecords、NewCourse_ExportSubscribeCourseHistory | `src/pages/scheduleManage/popup/arrangeSubscribeRecord.vue:368-369` |
| 考试/成绩导出 | ExamQuery、ExamScoreQueryNew、ExamScoreExport | `src/pages/examManage/examManage.vue:151-152`；`scoreQueryList/scoreQueryList.vue:153` |
| 财务 | LockFiscalPeriod、FiscalPeriodEdit、FinanceAutoLockRule、FinanceLockScopeRule；另判 IsAdmin | `src/pages/financialManage/financialManage.vue:196-199,375` |

异写保持原样。调用页发现权限判断不等于服务端完整 ACL。`src/router/index.ts` 此汇总文件未见 beforeEach/路由授权 guard，不代表宿主或后端无授权。

共用 response interceptor 对业务 ErrorCode 407 reject；登录跳转片段是注释。200 为业务成功，声明 code 的错误交页面处理，其余弹 ErrorMsg（`src/api/handler.ts:7-35`）；407 确切语义仍未知。HTTP 非 2xx 在 FetchService 被另行拒绝（`common/tool/http/fetch.ts:157-160`）。

## 6. Static logging and exposure notes

- `src/main.ts:122-125` stores `whoami.Data.SID` and logs the SID value to the browser console. This is a source-level exposure risk; this research record does not include any runtime SID value.
- `src/api/ai.ts:56,122-140` logs a message prefix and parsed SSE session/event data. Treat browser console output as potentially sensitive; no conversation content was collected or submitted here.
- `src/services/ai/useChat.ts:247-250` can append SID to an allowed internal link's query string. Any later URL capture must remove the query value.

## 7. 前端无法确认

1. 登录字段、验证码/MFA/SSO；Cookie 名称、Domain/SameSite/HttpOnly/有效期。
2. SID/AuthToken 生成、签名、轮换、撤销和租户绑定。
3. Legacy 是否验证 WTwo headers；跨域 CORS/Cookie 策略。
4. 每个 API 对 Rights、IsAdmin、校区、对象归属的后端强制检查。
5. 角色继承、管理员边界、跨租户隔离、组织 ACL。
6. 宿主身份来源和传递契约、登录路由归属。
7. 后端 Secret、数据库、服务端源码与内部鉴权服务。
8. ErrorCode/IsSuccess/success/data 响应混用的现网兼容性。

本研究为自有系统兼容/迁移设计提供参考。后续正常 Network 观察只保存字段结构/header 名称，不能把客户端身份字段视为绕过授权办法。
