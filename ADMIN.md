# OE Biolink 后台使用说明

## 两个页面

| 地址 | 用途 | 谁能改 |
|---|---|---|
| `https://link.ores.cc/` | 前台展示页 | 任何人都能看，没人能改 |
| `https://link.ores.cc/admin.html` | 管理后台 | 只有持有你 GitHub Token 的人能改 |

## 免密码登录原理

后台**没有密码**。它的「钥匙」是你自己的 GitHub Personal Access Token：

- Token 存在你本机浏览器的 localStorage，只有这台电脑读得到
- 点保存时，前端直接调 GitHub API 写 `data/site.json`
- **没有 Token 的人打不开后台、也改不了数据**
- 退出按钮会清掉本机 Token

## 生成 Token（只需一次）

1. GitHub → 右上角头像 → **Settings**
2. 左侧最下面 → **Developer settings**
3. **Personal access tokens** → **Tokens (classic)**
4. **Generate new token (classic)**
5. Note 随便填（如 `oe-biolink`）
6. **勾选 `repo`**（读写仓库内容）— 其他不用勾
7. 拉到底点 Generate token，复制那串 `ghp_...`

## 登录后台

1. 打开 `https://link.ores.cc/admin.html`
2. 用户名填 `poe112233`
3. 仓库填 `poe112233/oe-biolink`
4. Token 粘贴刚生成的那串
5. 点「连接并载入」

下次直接打开就是登录态，不用重复填。

## 后台能干什么

| 页面 | 能改什么 |
|---|---|
| 链接管理 | 增删链接区块、拖动排序、开关显示、编辑标题/副标题/地址/图标/角标/样式 |
| 个人资料 | 头像（文字或图片 URL）、名称、句柄、简介、标签 |
| 社交图标 | 增删图标、改图标种类/名称/地址、单独开关 |
| 主题配色 | 主色、渐变色、背景色、圆角、字号；4 套预设一键套用 |
| 站点信息 | 浏览器标题、SEO 描述、地址栏主题色、页脚两行 |
| JSON 数据 | 直接编辑原始 JSON，有统计卡片和格式校验 |

### 支持的区块类型

- **links** — 一组链接按钮（可设某个为「主按钮」高亮）
- **code** — 邀请码，带一键复制
- **table** — 表格，末列填 `↑` `↓` 自动显示红涨绿跌
- **text** — 纯文本段落

## 保存生效

点右上角 **「保存并发布」** → 写入 GitHub → **GitHub Pages 约 1 分钟后自动更新**。

没点保存的改动只在本机，刷新页面会丢。

## 常见问题

**Q：提示「Token 缺少 repo 权限」**
A：生成 Token 时没勾 `repo`。重新生成，勾上。

**Q：提示「仓库里还没有 data/site.json」**
A：首次部署时文件没推上去，或被误删。用本地脚本推一次即可。

**Q：提示「文件已被其他改动更新」**
A：你或另一个页面先保存过。点 JSON 页的「放弃修改，重新载入」再保存。

**Q：换设备/换浏览器怎么进后台？**
A：重新粘一次 Token。Token 不跨设备同步，这是设计如此。

**Q：别人能改我的页面吗？**
A：不能。改数据必须通过 GitHub API 且带有效 Token。

## 警告

- **别把 Token 发给别人**。谁拿到谁能改你的页面。
- `admin.html` 已加 `noindex`，不会被搜索引擎收录，但仍建议**不要把后台地址发到公开场合**。
