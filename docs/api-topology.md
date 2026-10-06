# API 服务拓扑

- 分析日期：2026-10-07。
- 源码：`li281077304-boop/blog/main`，HEAD `5561802e8fc2ed699f30abc59e5e942269660db2`，范围 `wtwo/`。
- 每项结论描述该版本公开前端；没有发起校管家线上请求。下列路径均为 `wtwo/` 相对路径，行号对应上述固定 SHA。[固定版本源码](https://github.com/li281077304-boop/blog/tree/5561802e8fc2ed699f30abc59e5e942269660db2/wtwo)。

## 服务分层

| 类别 | 路由/主机 | 前端证据 | 解释/置信度 |
|---|---|---|---|
| Legacy / 主站 | host-relative `/api/Student/*`、`/api/Class/*`（也有小写）、`/api/Shift/*`、`/api/depart/*`、`/api/Employee/*`、`/api/ZWFinance/*`、`/api/login/*`、`/api/user/*` 等 | `src/api/arrange.ts:168,329,345,570`；`src/api/dept.ts:5`；`src/api/financial.ts:4`；`src/api/index.ts:8` | URL 不含独立主机；由页面 origin 或部署代理解析。High。不能假定大小写等价。 |
| WTwo Course | 生产 `https://next.xiaogj.com/api/course/*`；测试 `https://wtwotest.xiaogj.com/api/course/*` | `src/store/index.ts:3`；`src/api/arrange.ts:17` | 课程、计划、草稿、日程、预约、课表偏好。High。 |
| Exam | 生产 `https://next.xiaogj.com/api/exam/*`；测试 `https://wtwotest.xiaogj.com/api/exam/*` | `src/api/exam.ts:1`；`src/store/index.ts:25` | 考试、成绩、统计、导出。High。 |
| AI Agent | 生产 `https://next.xiaogj.com/aiagent/api/chat/*`；测试 `https://wtwotest.xiaogj.com/aiagent/api/chat/*` | `src/api/ai.ts:6,67,254,273` | 流式聊天、会话历史、会话列表。不能省略 `/aiagent` 内部的 `/api/chat/`。High。 |
| AI 内部链接 | `https://wtwotest.xiaogj.com/xiaogj-ai-api`、`https://test.xiaogj.com/xiaogj-ai-api` 前缀 | `src/services/ai/useChat.ts:230` | AI 返回链接的客户端白名单；未提供统一方法/响应模型，不计为完整 API。Medium。 |
| 文件授权 | host-relative `/api/File/GetStsInfoRequest` | `src/api/index.ts:31` | 上传授权接口痕迹，仅记录结构，不记录授权值。High。 |
| CDN / 监控 | `cdn01.xiaogj.com`、`sentry2.xiaogj.com` 等 | `src/App.vue:58`；`src/main.ts:88`；`vite.config.ts:35`（此插件片段为注释） | 静态资源或运维线索，不混入业务 API 服务。公开配置不证明实时运行状态。 |

分层基于前端路径，不证明后台是独立进程、数据库或微服务。

## hostname 自动选择

`src/store/index.ts:4-25` 根据 **`window.location.hostname`** 选择新服务：

| 页面 hostname | apiUrl |
|---|---|
| `beta01.xiaogj.com`、`test.xiaogj.com`、`stage.xiaogj.com` | `https://wtwotest.xiaogj.com` |
| `localhost`、`127.0.0.1` | `https://wtwotest.xiaogj.com` |
| 前缀 `192.168.`、`10.`、`172.16.` | `https://wtwotest.xiaogj.com` |
| 其他 hostname | `https://next.xiaogj.com` |

这是字符串前缀判断，只覆盖 `172.16.*`，没有覆盖整个 `172.16/12`；不能泛化为任意私网或任意本地域名。apiUrl 在模块导入时计算。

`testUrl` 实际为空字符串（`src/store/index.ts:27`），login/whoami 的 `testUrl + '/api/…'` 因而是 **host-relative**。旁边测试站地址是注释。

Vite 本地 `/api` 代理指向 `https://beta01.xiaogj.com/`，`changeOrigin: true`（`vite.config.ts:90-95`）；绝对地址的新服务不经过此代理。该配置不能证明 Cookie 转发/CORS/生产代理的最终路由。

环境标签 `src/utils/domain/env.ts:2-10` 另用 `window.location.host`（含端口）得到 dev/test/prod。它与 apiUrl 的 hostname 判断不同；自定义本地域名可能标签 dev，但新服务仍指生产。

```mermaid
flowchart TD
    P[WTwo 页面或微前端] --> L[host-relative /api 主站]
    P --> H{window.location.hostname}
    H -->|指定测试或本地 hostname| T[wtwotest.xiaogj.com]
    H -->|其他 hostname| N[next.xiaogj.com]
    T --> C[/api/course]
    N --> C
    T --> E[/api/exam]
    N --> E
    T --> A[/aiagent/api/chat]
    N --> A
    D[Vite 本地 /api 代理] --> B[beta01.xiaogj.com]
```

## 请求载体与响应

- JSON：`common/tool/http/fetch.ts:84-108,138-164`；GET 将 data 字段拼接为查询，POST/PUT/DELETE 序列化 body，允许单次传 RequestInit。
- Form：`src/api/http-form.ts:7-24` 把序列化对象转为 URLSearchParams；嵌套字段如何被后端解释待验证。
- Blob/Binary：`src/api/http-blob.ts:7`、`http-binary.ts:6` 添加 WTwo headers，用于下载/导出，不应统一写成 JSON 返回。
- SSE：`src/api/ai.ts:67-227` 用 fetchEventSource，有 session/content/completed/done/FatalError 分支、取消控制、credentials include。
- 共用拦截器检查 `ErrorCode === 200`，407 拒绝，声明 `code` 的业务错误允许调用页处理（`src/api/handler.ts:7-35`）。草稿页另读取 IsSuccess；AI 历史页读取 success/data。协议差异是静态事实，现网兼容性待正常 Network 验证。
