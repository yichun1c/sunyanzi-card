# 孙燕姿 No.001 午夜典藏 — 3D 收藏卡

互动式 3D 金箔收藏卡，部署为 Cloudflare Workers 静态资源，并挂载在个人域名的 `/sunyanzi/` 路径。

- 作品地址：https://yichun1c.dpdns.org/sunyanzi/
- 入口页：https://yichun1c.dpdns.org/
- Cloudflare 项目：`sunyanzi-card`

## 目录结构

```text
public/
├── index.html                  # 极简入口页，仅链接到作品
└── sunyanzi/
    ├── index.html              # 收藏卡本体（HTML/CSS/JS 内联）
    └── assets/
        ├── background-clean.png  # 951×1654 暮色海景
        └── subject-alpha.png     # 951×1654 人物透明 PNG
wrangler.jsonc                  # Workers 静态资源配置与路由
package.json                    # 本地预览 / 部署脚本
```

## 部署配置要点

`wrangler.jsonc` 使用 Workers Static Assets：

```jsonc
"assets": {
  "directory": "./public",
  "html_handling": "auto-trailing-slash",
  "not_found_handling": "none"
}
```

- 资源根目录是 `public`，因此 `public/sunyanzi/index.html` 对应 `/sunyanzi/`。
- `not_found_handling: "none"` 让不存在的资源返回 404，而不是回退到首页。
- Worker 路由为 `yichun1c.dpdns.org/*`；`/` 提供入口页，`/sunyanzi/*` 提供作品。
- 之前 Workers 部署报「无法检测到包含 HTML、CSS、JS 的静态文件目录」，原因是仓库缺少 `wrangler.jsonc` 与 `public` 输出目录；本配置已补齐。

## 本地预览

```bash
npm install --ignore-scripts
npm run dev
# 打开 http://localhost:8787/sunyanzi/
```

也可以直接用任意静态服务器指向 `public/`：

```bash
python -m http.server 8000 -d public
# 打开 http://localhost:8000/sunyanzi/
```

## 部署

```bash
npx wrangler deploy
```

需要 `CLOUDFLARE_API_TOKEN`（Workers Scripts:Edit、Workers Routes:Edit、DNS:Edit、Zone:Read）。

## 验收范围

- 鼠标 / 触摸拖动旋转，带惯性。
- 金箔反光随视角变化，人物层与背景层有视差分层。
- 「正面朝向」按钮重置视角。
- 移动端 390px 布局可用。
- 两张原图透明通道保留。

公开发布照片、肖像和相关作品前，请确保拥有对应使用权限。
