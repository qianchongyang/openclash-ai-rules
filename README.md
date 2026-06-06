# OpenClash AI Rules

开源的 OpenClash / Clash Meta AI 分流规则集，重点补强 Claude，同时覆盖 ChatGPT、Gemini、GitHub Copilot、Grok、Perplexity、Poe、Meta AI、Groq、Mistral 等常见 AI 服务和相关访问链路。YouTube / Google Video 已拆成独立视频规则源，避免普通视频流量混入 AI 策略组。

## 仓库结构

```text
Rules/
  AI.list
  YouTube.list
README.md
```

## 规则目标

- **Claude 全覆盖优先**：补充 `claude.com` 家族、MCP、平台页、支持页、遥测、认证以及 Anthropic IP / ASN 兜底
- **兼顾主流 AI 服务**：覆盖 OpenAI / ChatGPT、Gemini / Google AI、GitHub Copilot / GitHub AI 等
- **视频规则单独维护**：YouTube / Google Video 拆到 `Rules/YouTube.list`，可单独绑定视频策略组
- **尽量减少误伤**：不把普通网站整站粗暴纳入；关键词规则仅放最后兜底

## 当前覆盖

- Claude / Anthropic
- OpenAI / ChatGPT
- Gemini / Google AI
- GitHub Copilot / GitHub AI
- xAI / Grok
- Perplexity
- Poe
- Meta AI
- Groq
- Mistral / Le Chat
- NotebookLM

## 文件说明

### `Rules/AI.list`

Clash classical rules 格式，可直接用于 OpenClash / Clash Meta 的规则源。

特点：

- Claude 相关域名补得更完整
- 带部分认证、遥测、CDN、IP / ASN 兜底
- GitHub 只纳入 Copilot / GitHub AI 相关流量，不把整个 `github.com` 全量纳入
- 关键词规则在最后，避免优先误伤

### `Rules/YouTube.list`

Clash classical rules 格式，用于 YouTube / Google Video 视频流量。

特点：

- 从 AI 规则源剥离，避免视频大流量进入 AI 策略组
- 可在主配置中绑定到独立的视频策略组

## 使用方式

### Shadowrocket 简化配置

手机可直接导入这个配置：

```text
https://raw.githubusercontent.com/qianchongyang/openclash-ai-rules/main/Shadowrocket/shadowrocket_minimal.conf
```

规则目标：YouTube / Google Video 和 AI 服务优先走指定节点；国内访问直连；其他国外流量统一走手动选择的节点。

该配置同时做了基础防泄露处理：DNS 默认使用随代理出口的 DoH，劫持常见硬编码 DNS，节点不支持 UDP 时拒绝直连回退，并对代理流量屏蔽 QUIC。

### 方式一：作为独立 AI 规则源

在主配置里新增一个 rule-provider，例如：

```yaml
rule-providers:
  AI / Domain:
    type: http
    behavior: classical
    path: ./rule_provider/AI.list
    url: https://raw.githubusercontent.com/<owner>/<repo>/main/Rules/AI.list
    interval: 86400
  YouTube / Domain:
    type: http
    behavior: classical
    path: ./rule_provider/YouTube.list
    url: https://raw.githubusercontent.com/<owner>/<repo>/main/Rules/YouTube.list
    interval: 86400
```

然后在 `rules:` 里加入：

```yaml
- RULE-SET,AI / Domain,AI所有
- RULE-SET,YouTube / Domain,YouTube视频
```

建议把 YouTube 和 AI 规则都放在普通国外规则之前，并分别绑定到视频策略组和 AI 策略组。

### 方式二：替换现有 AI Suite

如果你原来就有一个综合 AI 规则源，也可以把它的 URL 直接改到本仓库的 `Rules/AI.list`。如果原配置把 YouTube 也放在 AI 策略组里，建议额外新增 `Rules/YouTube.list` 规则源。

## 注意事项

- 本仓库不是完整的全站分流配置，只包含 **AI 相关规则、必要访问链路和独立 YouTube 视频规则**
- 不能直接替代整个 `2026.yaml`
- 若你的主配置已经有单独的 ChatGPT / Gemini / Claude 规则，请注意规则顺序，避免被更早的 AI 总规则提前匹配
- 规则会尽量保守，但关键词兜底仍可能带来少量误伤，建议结合自己的环境实测

## 开源说明

本仓库内容以开放方式分享，欢迎自行 fork、修改、补充和验证。

如果后续发现新的 AI 域名、认证链路、遥测域名或 IP 段，也欢迎继续补充。
