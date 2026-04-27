# TabCtrl Privacy Policy

**Last updated:** April 27, 2026
**Applies to:** TabCtrl Chrome / Edge extension, version 1.0.0 and later
**Extension ID:** `bniefocpdldneagigjlhbllgdjohmeie`


## 1. Summary

TabCtrl is an agentic browser extension that runs on a **Bring-Your-Own-Key (BYOK)** model. The TabCtrl developer **does not operate any backend service** and **does not receive, store, or process any of your data** on its own infrastructure.

In short:

- TabCtrl does not have a server that could see your prompts, pages, screenshots, or API keys.
- All settings, conversations, and recorded teaching cases are stored only in your own browser.
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

The following data is kept only on your computer through Chrome's `storage` API. It never leaves your device unless **you** explicitly send it to a model you configured.

| Data | Storage location | Lifetime |
|---|---|---|
| Model configurations and API keys | `chrome.storage.local` | Until you delete the entry or uninstall the extension |
| App settings and site policies | `chrome.storage.local` | Until you change them or uninstall |
| Teaching cases you record | `chrome.storage.local` | Until you delete them |
| Imported Skills | `chrome.storage.local` | Until you remove them |
| Current conversation, plan state, and recent task history | `chrome.storage.session` | Cleared when the browser closes |
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
- Tool-call results.
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
| `storage`, `unlimitedStorage` | Persist settings, model configs, skills, teaching cases, and session state locally |
| `sidePanel` | Render the side-panel UI |
| `alarms` | Keep the service worker awake during long tasks |
| `notifications` | Notify you when a long task completes |
| `offscreen` | Play audio notifications |
| `debugger` | Perform precise element interactions on pages where ordinary scripting is insufficient (e.g. complex editors, OS-level events) |
| `nativeMessaging` | Optional. Only used when you explicitly enable Lab Beta and install the native bridge to call allow-listed local CLI tools |

`<all_urls>` and `debugger` are broad capabilities. TabCtrl only uses them as part of an agent loop **you** started in the side panel. It does not run on pages in the background or read pages you have not asked it to act on.

## 6. Native Messaging (Optional, Disabled by Default)

The Lab Beta is **disabled by default**. If you enable it and install the native messaging host, TabCtrl can call locally pre-installed command-line tools that you have explicitly added to `bridge.config.json`. Constraints:

- Only command names listed in your local `bridge.config.json` allowlist are accepted.
- Each command's executable path must match the configuration exactly.
- The agent cannot inject environment variables, choose a working directory, or execute arbitrary shell commands.
- Each native call requires your in-app approval.
- All output stays on your machine. The developer cannot see it.

Uninstalling or disabling Lab Beta stops all native messaging activity.

## 7. Sensitive and Restricted Sites

To protect users, TabCtrl applies the following hard-coded safeguards regardless of user settings:

- **Hard-blocked**: Identity-provider login pages such as `accounts.google.com`, `login.microsoftonline.com`, `login.live.com`, `appleid.apple.com`. The agent will not read or operate on these pages.
- **Always require explicit per-action approval**: Major payment, banking, brokerage, and cryptocurrency-exchange domains.
- **Refused entirely**: Non-`http/https` URLs (`javascript:`, `data:`, `blob:`, `file:`, `chrome:`, `chrome-extension:`, `about:`, etc.).

## 8. Children

TabCtrl is not directed at children under 13 and does not knowingly collect any data from children.

## 9. Security

- API keys and other settings are stored using Chrome's extension storage and are subject to the same isolation as the rest of your browser profile.
- The extension's own pages run under a strict Content Security Policy that disallows remote script loading and `unsafe-eval` / `unsafe-inline`.
- TabCtrl is built on Manifest V3, which forbids loading remote code at runtime.
- The extension does not transmit your API keys to anyone other than the model endpoint you configured.

You are responsible for protecting your own device and the API keys stored on it. If you believe a key has been exposed, rotate it through your provider's dashboard.

## 10. Your Choices

- **Stop sending data to a provider**: remove or disable the model configuration in Settings → Models.
- **Delete the conversation**: start a new conversation in the side panel; current-session data is cleared.
- **Delete teaching cases or skills**: remove them in Settings.
- **Wipe everything**: uninstall the extension. All locally stored data is removed by Chrome.

## 11. Changes to This Policy

If this policy materially changes, the updated version will be published at the same URL listed in the Chrome Web Store listing, with an updated "Last updated" date above.

## 12. Contact

For privacy-related questions about TabCtrl itself, contact the developer at **dyyxml@gmail.com**, or open an issue on the project repository.