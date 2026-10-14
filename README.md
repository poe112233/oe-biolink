# OE Link-in-bio

OE 品牌简介聚合页。纯静态单文件（HTML+CSS+JS 全内联，约 19KB），零外部依赖，托管于 GitHub Pages。

**线上地址**：https://link.ores.cc （备用：https://poe112233.github.io/oe-biolink/）

## 文件结构

```
index.html     页面本体
CNAME          自定义域名声明，内容为 link.ores.cc
.nojekyll      禁用 Jekyll 处理，保证静态文件原样发布
README.md      本文件
```

## DNS 解析（用户手动配置）

在 `ores.cc` 的 DNS 服务商处添加：

| 类型 | 主机记录 | 值 | TTL |
|---|---|---|---|
| CNAME | `link` | `poe112233.github.io` | Auto / 300 |
| TXT | `_github-challenge-link` | `<GitHub 后台生成的验证值>` | Auto |

> 第二个 TXT 记录用于 GitHub 的**自定义域名所有权验证**，未添加时自定义域名不会生效。值需在 GitHub 仓库 Settings → Pages → Custom domain 填入 `link.ores.cc` 后由 GitHub 生成。

## 启用 Pages（一次性）

仓库 → **Settings → Pages** → Source 选 `Deploy from a branch` → Branch `main` / Folder `/ (root)` → Save。

> API 调用 `POST /repos/{owner}/{repo}/pages` 返回 403，因为当前 PAT 是 fine-grained 且缺 `pages:write` scope，必须在 UI 手动开一次。开完即可永久自动部署。

## 更新页面

```bash
git add -A && git commit -m "update"
git push origin main
```

若 `git push` 因本机代理报 502，可用 API 上传替代：

```bash
python - <<'PY'
import json,base64,urllib.request
j=json.load(open(r'C:\Users\10493\.workbuddy\github\game-price-board.json'))
tok=j['GITHUB_TOKEN']; repo='poe112233/oe-biolink'
H={'Authorization':'Bearer '+tok,'Accept':'application/vnd.github+json','User-Agent':'wb','Content-Type':'application/json'}
f='index.html'
b64=base64.b64encode(open(f,'rb').read()).decode()
g=urllib.request.Request(f'https://api.github.com/repos/{repo}/contents/{f}',headers=H)
sha=json.load(urllib.request.urlopen(g))['sha']
body=json.dumps({'message':'update '+f,'content':b64,'sha':sha,'branch':'main'}).encode()
r=urllib.request.Request(f'https://api.github.com/repos/{repo}/contents/{f}',data=body,method='PUT',headers=H)
print(urllib.request.urlopen(r).status)
PY
```

## 页面维护

- **换链接**：搜 `class="link"` 区块，替换 `href`
- **加链接**：复制一段 `<a class="link">...</a>` 改内容
- **换主色**：改 `:root` 里的 `--accent` / `--accent-2`
- **邀请码**：改 `#code` 元素文本
- **行情/尺码表**：改 `<table>` 内的 `<tr>` 行