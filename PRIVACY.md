# TabCtrl Privacy Policy

**Last updated:** August 22, 2026
**Applies to:** TabCtrl Chrome / Edge extension, version 1.0.0 and later
**Extension ID:** `bniefocpdldneagigjlhbllgdjohmeie`

> A 中文版本 follows the English text below.

---

## 1. Summary

TabCtrl is an agentic browser extension that runs on a **Bring-Your-Own-Key (BYOK)** model. The TabCtrl developer **does not operate any backend service** and **does not receive, store, or process any of your data** on its own infrastructure.

In short:

- TabCtrl does not have a server that could see your prompts, pages, screenshots, or API keys.
- Settings, conversations, recorded teaching cases, schedules, run history and queued tasks, recent-task prompts, and execution-memory records are stored in your own browser. Task content may still be sent directly to the model endpoint you configured, as described in Section 4.
- When you give the agent a task, the extension talks **directly** from your browser to the model endpoint(s) **you** configured. That provider is governed by **its own** privacy policy, not this one.

## 2. Data the Developer Collects

**None.**

TabCtrl does **not**:

- Send telemetry, analytics, crash reports, or usage statistics to the developer.
- Include third-party analytics, advertising, or tracking SDKs (Google Analytics, Mixpanel, Sentry, etc.).
- Maintain any backend that receives user data.
- Read or transmit your browsing history.
- Use your data to train any model.
- Sell or share user data — there is no user data on the developer side to sell or share.

## 3. Data Stored Locally on Your Device

The following data is persisted on your computer through Chrome's `storage` API or local IndexedDB. Persisting data locally does not mean it is never transmitted: when you run a task, the relevant prompt, page data, tool results, memory hints, or schedule content may be sent directly to the model endpoint you configured, as described in Section 4.

| Data | Storage location | Lifetime |
|---|---|---|
| Model configurations and API keys | `chrome.storage.local` | Until you delete the entry or uninstall the extension |
| App settings and site policies | `chrome.storage.local` | Until you change them or uninstall |
| Teaching cases you record | `chrome.storage.local` | Until you delete them |
| Imported Skills | `chrome.storage.local` | Until you remove them |
| Execution memory records | Local IndexedDB | Disabled by default; until you clear them, uninstall, or local soft caps prune them |
| Recent first-turn task prompts (up to 20) | `chrome.storage.local` | Enabled by default; until you clear them or uninstall. You can disable future recording in Settings → Lab |
| Run history (up to 50 runs, 120 events each) and queued tasks (up to 25) | `chrome.storage.local` | Finished runs remain until you clear them, local caps prune them, or you uninstall; queue items are removed when run or cancelled |
| Scheduled-task definitions, prompts, and result metadata | `chrome.storage.local` | Until you delete the schedule or uninstall |
| Current conversation and plan state | `chrome.storage.session` | Cleared when the browser closes |
| Page snapshots, screenshots, vision-cache entries | `chrome.storage.session` | Cleared when the browser closes; subject to LRU caps |
| Approval / audit decisions for the current session | `chrome.storage.session` | Cleared when the browser closes |

Uninstalling the extension removes all of the above.

## 4. Data Sent to Third Parties (Your Configured Model Providers)

When you submit a task, TabCtrl makes HTTPS requests **directly** from your browser to the model endpoint(s) you set up in **Settings → Models**. Depending on the task, those requests may contain:

- The task prompt you typed in the side panel.
- Page content extracted from the active tab via the accessibility tree (text, roles, element references, frame structure).
- Screenshots of the active tab, when a vision-capable model is used.
- Metadata (title, URL) of tabs the agent reads or operates on.
- Conversation history within the current task and any context summaries TabCtrl produced.
- Short execution-memory hints, only if you enabled Execution memory in Settings → Lab.
- Tool-call results.
- The saved prompt, optional start URL, page content, and tool results for a scheduled task when that schedule fires.
- Your API key, sent in the standard `Authorization` header understood by that provider.

These requests go to whatever endpoint **you** configured. Common examples include:

- The official Anthropic API (`api.anthropic.com`)
- The official OpenAI API (`api.openai.com`)
- DeepSeek, Moonshot, Qwen / DashScope, OpenRouter, and other OpenAI-compatible services
- An enterprise inference gateway or self-hosted model on your intranet
- A local model server such as vLLM, Ollama, or LM Studio

**TabCtrl does not choose the provider, does not proxy the request, and does not log it.** Whatever data ends up at that endpoint is governed by **that provider's** privacy policy and data-retention rules. You are responsible for choosing a provider you trust for the data you are sending.

In enterprise self-hosted scenarios where the configured endpoint is on your intranet, the request never leaves your corporate network.

## 5. Permissions and Why TabCtrl Requests Them

| Permission | Why it is needed |
|---|---|
| `<all_urls>` host permission, `activeTab`, `scripting` | Read and act on the page you are working on, including across frames |
| `tabs`, `tabGroups`, `webNavigation` | Manage and observe tabs across multi-step tasks; enforce protocol and URL guards |
| `declarativeNetRequest` | Block task-tab main-frame and sub-frame requests before they leave the browser, then allow only an exact, approved GET URL |
| `storage`, `unlimitedStorage` | Persist settings, model configs, skills, teaching cases, and session state locally |
| `sidePanel` | Render the side-panel UI |
| `alarms` | Keep the service worker awake during long tasks and trigger schedules you created |
| `notifications` | Notify you when a task completes, or fire a notification schedule you created |
| `offscreen` | Play audio notifications |
| `debugger` | Perform precise element interactions where ordinary scripting is insufficient; for unattended task tabs, deny response-triggered file downloads |
| `nativeMessaging` | Optional. Only used when you explicitly enable Lab Beta and install the native bridge to call allow-listed local CLI tools |

`<all_urls>` and `debugger` are broad capabilities. TabCtrl uses them only for a task you started in the side panel or a scheduled task you explicitly created and enabled. It does not inspect unrelated tabs or browsing history.

## 6. Native Messaging (Optional, Disabled by Default)

The Lab Beta is **disabled by default**. If you enable it and install the native messaging host, TabCtrl can call locally pre-installed command-line tools that you have explicitly added to `bridge.config.json`. Constraints:

- Only command names listed in your local `bridge.config.json` allowlist are accepted.
- Each command's executable path must match the configuration exactly.
- The agent cannot inject environment variables, choose a working directory, or execute arbitrary shell commands.
- Diagnostic `ping`, `which`, and `diagnose` calls do not prompt. A non-destructive command on your **Auto-approve native commands** list may also run without an interactive prompt. Destructive native calls always ask during an attended task, and native calls are denied in unattended scheduled tasks.
- Native output is processed locally by the extension and may be included in the next request to your configured model so the task can continue. The TabCtrl developer cannot see it.

Uninstalling or disabling Lab Beta stops all native messaging activity. The native host independently enforces its own command allowlist even when TabCtrl skips an interactive prompt.

## 7. Scheduled Tasks and Background Activity

When you create and enable a scheduled task, TabCtrl may run it while the browser is open even if the side panel is closed. The extension opens a task tab, sends the saved prompt and relevant page/tool context directly to your configured model provider, and closes the task tab after the run.

Scheduled tasks have no person available to answer an approval prompt. Unattended browsing therefore requires HTTPS and an exact hostname entry in **Allowed domains**; an empty list or wildcard does not grant scheduled access. Only observation, scrolling, tab switching, and reviewed GET navigation are eligible for automatic execution. A reviewed link is opened directly without dispatching its DOM click handler. Typing, key presses, buttons, forms, downloads, destructive-looking URLs, native-bridge calls, and actions on hard-blocked or sensitive sites are denied. A run that does not explicitly call `done(summary)` with a non-empty summary, encounters a policy denial, or fails navigation is recorded as failed. You can disable or delete schedules in **Settings → Schedules**.

Image export is also fail-closed against DNS rebinding: for hostname URLs, TabCtrl only serializes pixels from an image already loaded in the current page and does not initiate a new fetch. Active network fallback is limited to literal public IPv4 URLs. Each image is capped at 2 MB, each task at 4 MB, each canvas export at 2 million source pixels, and repeated export work at 4 million pixels per page; compression and encoding are time-bounded.

## 8. Sensitive and Restricted Sites

To protect users, TabCtrl applies the following hard-coded safeguards regardless of user settings:

- **Hard-blocked**: Identity-provider login pages such as `accounts.google.com`, `login.microsoftonline.com`, `login.live.com`, `appleid.apple.com`. The agent will not read or operate on these pages.
- **Always require explicit per-action approval**: Major payment, banking, brokerage, and cryptocurrency-exchange domains.
- **Refused entirely**: Non-`http/https` URLs (`javascript:`, `data:`, `blob:`, `file:`, `chrome:`, `chrome-extension:`, `about:`, etc.).

## 9. Children

TabCtrl is not directed at children under 13 and does not knowingly collect any data from children.

## 10. Security

- API keys and other settings are stored using Chrome's extension storage and are subject to the same isolation as the rest of your browser profile.
- The extension's own pages run under a strict Content Security Policy that disallows remote script loading and `unsafe-eval` / `unsafe-inline`.
- TabCtrl is built on Manifest V3, which forbids loading remote code at runtime.
- The extension does not transmit your API keys to anyone other than the model endpoint you configured.

You are responsible for protecting your own device and the API keys stored on it. If you believe a key has been exposed, rotate it through your provider's dashboard.

## 11. Your Choices

- **Stop sending data to a provider**: remove or disable the model configuration in Settings → Models.
- **Delete the conversation**: start a new conversation in the side panel; current-session data is cleared.
- **Stop keeping future recent-task prompts**: turn off task-prompt history in Settings → Lab.
- **Delete recent-task prompts**: use Recent tasks → Clear in the side panel, or Clear recent tasks in Settings → Lab.
- **Delete finished run history or queued work**: use Clear finished or the per-item Cancel action in the side panel's Run center.
- **Stop a scheduled task**: disable or delete it in Settings → Schedules.
- **Delete teaching cases or skills**: remove them in Settings.
- **Wipe everything**: uninstall the extension. All locally stored data is removed by Chrome.

## 12. Changes to This Policy

If this policy materially changes, the updated version will be published at the same URL listed in the Chrome Web Store listing, with an updated "Last updated" date above.

## 13. Contact

For privacy-related questions about TabCtrl itself, contact the developer at **dyyxml@gmail.com**, or open an issue on the project repository.

---

# TabCtrl 隐私政策（中文版）

**最后更新：** 2026 年 8 月 22 日
**适用范围：** TabCtrl Chrome / Edge 扩展 1.0.0 及后续版本
**扩展 ID：** `bniefocpdldneagigjlhbllgdjohmeie`

## 1. 摘要

TabCtrl 是一款基于 **BYOK（Bring Your Own Key，自带 API Key）** 模型的代理式浏览器扩展。TabCtrl 开发方**不运营任何后端服务**，**不接收、不存储、也不处理任何用户数据**。

也就是说：

- 开发方没有任何服务器能看到你的提问、页面内容、截图或 API Key。
- 设置、对话、教学案例、定时任务、运行历史与任务队列、最近任务 prompt 和执行记忆记录保存在你自己的浏览器中；执行任务时，相关内容仍可能按第 4 节所述直接发送到你配置的模型端点。
- 当你下达任务时，扩展会**直接**从你的浏览器向你自己配置的模型端点发送请求。该模型提供商遵循其**自身**的隐私政策，与本政策无关。

## 2. 开发方收集的数据

**无。**

TabCtrl **不会**：

- 向开发方发送遥测、统计、崩溃报告或使用数据；
- 集成任何第三方分析、广告或追踪 SDK（Google Analytics、Mixpanel、Sentry 等）；
- 运营任何接收用户数据的后端；
- 读取或上报你的浏览历史；
- 使用你的数据训练任何模型；
- 出售或共享用户数据——开发方侧根本没有用户数据。

## 3. 仅存储于本机的数据

以下数据通过 Chrome `storage` API 或本地 IndexedDB 持久化在你的设备上。“本地持久化”并不代表绝不传输：执行任务时，相关 prompt、页面数据、工具结果、记忆提示或定时任务内容可能按第 4 节所述直接发送到你配置的模型端点。

| 数据 | 存储位置 | 生命周期 |
|---|---|---|
| 模型配置与 API Key | `chrome.storage.local` | 直到你删除条目或卸载扩展 |
| 应用设置与站点策略 | `chrome.storage.local` | 直到你修改或卸载 |
| 你录制的教学案例 | `chrome.storage.local` | 直到你删除 |
| 已导入的 Skill | `chrome.storage.local` | 直到你移除 |
| 执行记忆记录 | 本地 IndexedDB | 默认关闭；直到你清空、卸载，或本地软上限自动裁剪 |
| 最近首轮任务 prompt（最多 20 条） | `chrome.storage.local` | 默认开启；直到你清除或卸载。可在 Settings → Lab 关闭未来记录 |
| 运行历史（最多 50 条、每条 120 个事件）与任务队列（最多 25 项） | `chrome.storage.local` | 已结束运行会保留到你清理、本地上限裁剪或卸载；队列项会在执行或取消后移除 |
| 定时任务定义、prompt 与结果元数据 | `chrome.storage.local` | 直到你删除该定时任务或卸载 |
| 当前对话与计划状态 | `chrome.storage.session` | 浏览器关闭即清空 |
| 页面快照、截图、视觉问答缓存 | `chrome.storage.session` | 浏览器关闭即清空，受 LRU 上限约束 |
| 当前会话内的审批 / 审计决策 | `chrome.storage.session` | 浏览器关闭即清空 |

卸载扩展会清除以上全部数据。

## 4. 发送给第三方（你配置的模型提供商）的数据

当你提交任务时，TabCtrl 会**直接**从你的浏览器向你在 **Settings → Models** 中配置的模型端点发起 HTTPS 请求。根据任务不同，请求可能包含：

- 你在侧边栏输入的任务文本；
- 由可访问性树提取的当前标签页内容（文本、角色、元素引用、frame 结构）；
- 当使用具备视觉能力的模型时，当前标签页的截图；
- 代理读取或操作的标签页元数据（标题、URL）；
- 当前任务内的对话历史以及 TabCtrl 生成的上下文摘要；
- 简短执行记忆提示（仅当你在 Settings → Lab 启用执行记忆时）；
- 工具调用结果；
- 定时任务触发时保存的 prompt、可选起始 URL、相关页面内容和工具结果；
- 你的 API Key（通过该模型提供商规定的标准 `Authorization` 头部携带）。

这些请求会发往**你自己配置**的端点，常见示例包括：

- Anthropic 官方 API（`api.anthropic.com`）；
- OpenAI 官方 API（`api.openai.com`）；
- DeepSeek、Moonshot、Qwen / DashScope、OpenRouter 等 OpenAI 兼容服务；
- 企业内网推理网关或私有化部署的模型；
- 本机运行的模型服务，如 vLLM、Ollama、LM Studio。

**TabCtrl 不会代你选择服务商、不会代理这些请求，也不会记录它们。** 这些数据到达端点之后的留存与处理，由**该模型提供商**的隐私政策决定。请你为发送的数据自行选择可信任的提供商。

在企业内网自托管场景下，只要端点位于内网地址，请求**不会离开企业网络**。

## 5. 权限说明

| 权限 | 用途 |
|---|---|
| `<all_urls>` 主机权限、`activeTab`、`scripting` | 读取并操作你正在处理的页面，包括跨 frame |
| `tabs`、`tabGroups`、`webNavigation` | 多步骤任务中的标签页管理与导航；执行协议与 URL 校验 |
| `declarativeNetRequest` | 在任务标签页的主 frame / 子 frame 请求发出前阻断，只精确放行已批准的 GET URL |
| `storage`、`unlimitedStorage` | 在本机持久化设置、模型、Skill、教学案例与会话状态 |
| `sidePanel` | 提供侧边栏 UI |
| `alarms` | 长任务期间维持 Service Worker 存活，并触发你创建的定时任务 |
| `notifications` | 任务完成时通知，或触发你创建的通知类定时任务 |
| `offscreen` | 播放提示音 |
| `debugger` | 在普通脚本无法精确触达的页面执行精确元素操作；在无人值守任务标签页中拒绝响应触发的文件下载 |
| `nativeMessaging` | 可选；仅在你显式启用实验室 Beta 并安装本机桥接后用于调用白名单内的本地 CLI |

`<all_urls>` 与 `debugger` 是较强的能力。TabCtrl 只会用于你在侧边栏发起的任务，或你明确创建并启用的定时任务；不会检查无关标签页或浏览历史。

## 6. Native Messaging（可选，默认禁用）

实验室 Beta **默认关闭**。仅当你启用并安装本机消息主机后，TabCtrl 才可调用你已加入 `bridge.config.json` 白名单的本地命令行工具。约束如下：

- 仅接受本地 `bridge.config.json` 白名单中明确列出的命令名称；
- 每条命令对应的可执行文件路径必须与配置精确匹配；
- 代理无法注入环境变量、指定工作目录或执行任意 shell 命令；
- `ping`、`which`、`diagnose` 诊断调用不会弹出审批；你加入“免审批命令”列表的非破坏性命令也可能跳过交互确认。有人值守时，破坏性本地调用始终询问；无人值守定时任务会拒绝本机桥接调用；
- 本机输出由扩展在本地处理，并可能随下一轮请求发送给你配置的模型，以便继续任务；TabCtrl 开发方无法看到。

卸载扩展或关闭实验室 Beta 即停止所有本机消息活动。即使 TabCtrl 跳过交互确认，本机消息主机仍会独立执行自己的命令白名单。

## 7. 定时任务与后台活动

你创建并启用定时任务后，只要浏览器仍在运行，即使侧边栏已关闭，TabCtrl 也可能在后台执行。扩展会打开任务标签页，把保存的 prompt 以及相关页面/工具上下文直接发送给你配置的模型提供商，并在任务结束后关闭该标签页。

定时任务运行时没有用户可响应审批。因此，无人值守浏览只允许 HTTPS，目标主机名必须逐条精确列入“可操作域名”；留空或通配符不授权。只有只读观察、滚动、标签切换和复核后的 GET 导航可自动执行，链接由扩展直接打开而不会派发网页 DOM 点击事件。输入、按键、按钮、表单、下载、疑似破坏性的 URL、本机桥接调用以及硬阻止或敏感站点上的动作都会被拒绝。未显式提供非空 `done(summary)`、发生策略拒绝或导航失败的运行会记为失败。你可以在 **Settings → Schedules** 中禁用或删除定时任务。

图片导出对 DNS 重绑定采取失败闭合策略：主机名 URL 只允许序列化页面已加载图片的像素，不会再次主动请求；网络回退只允许公网 IPv4 字面量。单图上限 2 MB、单任务 4 MB、单次画布最多 200 万源像素、单页累计导出工作最多 400 万像素，并对压缩与编码设置超时。

## 8. 敏感与受限站点

无论用户如何配置，TabCtrl 都会执行以下硬性约束：

- **完全屏蔽**：身份提供方登录页，例如 `accounts.google.com`、`login.microsoftonline.com`、`login.live.com`、`appleid.apple.com`；代理不会读取或操作这些页面；
- **每次操作强制审批**：主流支付、银行、券商、加密货币交易所等域名；
- **直接拒绝**：所有非 `http/https` 的 URL（`javascript:`、`data:`、`blob:`、`file:`、`chrome:`、`chrome-extension:`、`about:` 等）。

## 9. 儿童

TabCtrl 不面向 13 岁以下儿童，也不会有意收集儿童数据。

## 10. 安全

- API Key 与其他设置通过 Chrome 扩展存储保存，与你的浏览器配置享有相同的隔离机制；
- 扩展自身页面遵循严格的 CSP，禁止远程脚本加载与 `unsafe-eval` / `unsafe-inline`；
- 扩展基于 Manifest V3 构建，运行时禁止加载远程代码；
- 除你配置的模型端点外，扩展不会将你的 API Key 发送给任何其他方。

你需要自行保护本机设备及其上保存的 API Key。如怀疑 Key 已泄露，请通过对应模型提供商的控制台轮换 Key。

## 11. 你的选择

- **停止向某个提供商发送数据**：在 Settings → Models 中移除或禁用对应模型配置；
- **删除当前对话**：在侧边栏新建对话，会话级数据将被清空；
- **停止记录未来最近任务**：在 Settings → Lab 关闭任务 prompt 历史；
- **删除最近任务 prompt**：在侧边栏“最近任务 → 清除”，或在 Settings → Lab 点击“清除最近任务”；
- **删除已结束的运行历史或排队任务**：在侧边栏运行中心使用“清理已结束”或单项“取消”；
- **停止定时任务**：在 Settings → Schedules 中禁用或删除；
- **删除教学案例或 Skill**：在 Settings 中移除；
- **彻底清除**：卸载扩展，Chrome 将自动清除所有本地数据。

## 12. 政策变更

如本政策发生重要变更，更新版本会发布在 Chrome 网上应用店列表所引用的同一 URL，并更新顶部的"最后更新"日期。

## 13. 联系方式

如有与 TabCtrl 相关的隐私问题，可发送邮件至 **dyyxml@gmail.com** 联系开发方，或在项目仓库中提交 issue。
