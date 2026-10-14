# OE Link-in-bio

OE 品牌简介聚合页。纯静态单文件，零依赖，Cloudflare Pages 托管。

## 文件

```
public/index.html     页面本体（HTML+CSS+JS 全内联，19KB）
wrangler.toml         Cloudflare Pages 配置
```

## 部署（Cloudflare Pages）

**方式 A：GitHub 直连（推荐）**

1. 新建仓库并推送本目录
2. Cloudflare Dashboard → Workers & Pages → Create → Pages → Connect to Git → 选仓库
3. 构建配置：Framework preset = `None`，Build command **留空**，Build output directory = `public`
4. 部署完成得到 `https://oe-biolink.pages.dev`

**方式 B：wrangler CLI**

```bash
npx wrangler pages deploy public --project-name=oe-biolink
```

## 后续扩展

- **换链接**：编辑 `public/index.html`，搜索 `class="link"` 区块替换 `href`
- **加链接**：复制一段 `<a class="link">...</a>` 改内容
- **换主色**：改 `:root` 里的 `--accent` / `--accent-2`
- **绑域名**：Pages → Custom domains → 加子域（如 `bio.ores.cc`），DNS 加一条 CNAME 指向 `<project>.pages.dev`。**不需要改注册商 NS**