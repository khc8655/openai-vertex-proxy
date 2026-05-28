# OpenAI to Google Vertex AI Proxy (Cloudflare Workers)

这个项目是把 Google Vertex AI (Agent Platform) 转换为 OpenAI 标准 API 格式的高性能、超轻量代理中转服务，支持在 Cloudflare Workers 上一键免费部署。

本版本相比于传统 AI Studio 代理，针对 Vertex AI 及最新 OpenAI 思考机制进行了深度适配，完美支持 `reasoning_content` 及 `reasoning_effort`。

## 🌟 特性

- **首发对齐 `reasoning_content`**：非流式和流式（SSE）中完美剥离出 Google 的原生思考过程，并输出为标准 OpenAI 格式的 `reasoning_content` 字段。完美兼容 OpenWebUI、Dify 等第三方客户端。
- **全等级 `reasoning_effort` 映射**：
  - 完美适配 OpenAI 最新 o1/o3-mini 的思考等级约束参数（`low` / `medium` / `high` / `none`）。
  - 在 Gemini 3.5 下自适应映射为 `thinkingLevel` 控制字。
  - 在 Gemini 2.5 下自适应映射为 `thinkingBudget` (Token数限制)。
- **静态 `/v1/models` 端点**：极速本地返回静态支持的模型列表，0ms 时延。
- **完全兼容 Cloudflare Workers**：支持通过 Wrangler 命令行或一键部署按钮进行部署。

## ⚡ 部署方式

### 方式一：使用 Cloudflare Workers Playground (最简方式)

1. 打开 [Cloudflare Workers Playground](https://workers.cloudflare.com/playground)。
2. 将本地 `src/worker.mjs` 的全部内容复制，粘贴并替换 Playground 里的代码。
3. 点击页面右上角的 **Deploy** 按钮当场完成一键部署。

### 方式二：命令行部署 (Wrangler CLI)

在本项目根目录下，执行以下命令：
```bash
# 安装依赖
npm install

# 本地开发预览
npm run dev

# 一键部署到 Cloudflare
npm run deploy
```

## 🛠️ 配置与使用

部署成功后，将您的第三方客户端（例如 OpenWebUI、Dify、NextChat）中的 API 配置修改为：

1. **API Base (代理地址)**：`https://<您的Worker域名>/v1`
2. **API Key (秘钥)**：您的 Google Vertex AI 接口密钥 (API Key)。由于此代理属于纯代理中转，不托管您的 API 密钥，您的秘钥将直接透传至 Google。
3. **支持模型**：
   - `google/gemini-2.5-flash`
   - `google/gemini-3.5-flash`

---

## 📄 开源许可
MIT License.
