# TabCtrl Privacy Policy

**Last updated:** August 22, 2026
**Applies to:** TabCtrl Chrome / Edge extension, version 1.0.0 and later
**Extension ID:** `bniefocpdldneagigjlhbllgdjohmeie`

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
