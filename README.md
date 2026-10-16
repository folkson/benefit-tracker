# Benefit Tracker — 权益与羊毛管理

单页应用（纯前端 + 可选 Worker 同步）：优惠券 / 储值卡 / 信用卡权益 / 资产 / 知识库统一管理。

**线上地址**：https://benefit-tracker.pages.dev （Cloudflare Pages，main 分支自动部署源为 `pages-dist/index.html` 单文件版）

## 版本

- **v2.16.0**（当前）：全站图标系统符号化——移除 Material Symbols 图标字体与 Inter 字体、删除 Google Fonts CDN 依赖，图标全部改为系统字体符号/emoji（⚙ ← ＋ ⋯ ✎ 🗑 › ✓ ▾ ▴ 📄 🎓 🛠️ 🧠 👥 等）。任何设备/网络环境（含大陆）零外部请求，显示 100% 可靠。单文件体积 514KB → 375KB。
- v2.15.x：dock 纯文字化（移除底部导航图标）。

## 目录结构

| 路径 | 说明 |
|---|---|
| `app.js` / `index.html` / `style.css` | 源码三件套（开发用，可分开改） |
| `pages-dist/index.html` | 单文件交付版（三件套 + 无字体依赖，直接部署） |
| `bt-merge.cjs`（见 .deploy） | 三件套 → 单文件合并脚本 |

## 构建单文件版

```bash
node bt-merge.cjs   # 输出 benefit-tracker.html，内容与 pages-dist/index.html 一致
```

## 部署

```bash
cd pages-dist
CLOUDFLARE_API_TOKEN=xxx npx wrangler pages deploy . --project-name=benefit-tracker --branch=main
```

## 设计约定

- **零外部依赖**：无 CDN、无字体请求、无 npm 运行时依赖；图标一律用系统符号/emoji，禁止引入图标字体（Material Symbols 在部分设备上解码失败导致图标错乱，v2.16.0 已根治）
- 数据存 localStorage；推送同步走 Cloudflare Worker + KV（bt-push-worker）
