# OpenClash AI Rules

开源的 OpenClash / Clash Meta AI 分流规则集，重点补强 Claude，同时覆盖 ChatGPT、Gemini、YouTube / Google Video、GitHub Copilot、Grok、Perplexity、Poe、Meta AI、Groq、Mistral 等常见 AI 服务和相关访问链路。

## 仓库结构

```text
Rules/
  AI.list
README.md
```

## 规则目标

- **Claude 全覆盖优先**：补充 `claude.com` 家族、MCP、平台页、支持页、遥测、认证以及 Anthropic IP / ASN 兜底
- **兼顾主流 AI 服务**：覆盖 OpenAI / ChatGPT、Gemini / Google AI、YouTube / Google Video、GitHub Copilot / GitHub AI 等
- **尽量减少误伤**：不把普通网站整站粗暴纳入；关键词规则仅放最后兜底

## 当前覆盖

- Claude / Anthropic
- OpenAI / ChatGPT
- Gemini / Google AI
- YouTube / Google Video
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

## 使用方式

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
```

然后在 `rules:` 里加入：

```yaml
- RULE-SET,AI / Domain,AI所有
```

建议把它放在 AI 相关规则前部，放在普通国外规则之前。

### 方式二：替换现有 AI Suite

如果你原来就有一个综合 AI 规则源，也可以把它的 URL 直接改到本仓库的 `Rules/AI.list`。

## 注意事项

- 本仓库不是完整的全站分流配置，只包含 **AI 相关规则和必要访问链路**
- 不能直接替代整个 `2026.yaml`
- 若你的主配置已经有单独的 ChatGPT / Gemini / Claude 规则，请注意规则顺序，避免被更早的 AI 总规则提前匹配
- 规则会尽量保守，但关键词兜底仍可能带来少量误伤，建议结合自己的环境实测

## 开源说明

本仓库内容以开放方式分享，欢迎自行 fork、修改、补充和验证。

如果后续发现新的 AI 域名、认证链路、遥测域名或 IP 段，也欢迎继续补充。
