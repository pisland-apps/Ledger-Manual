# Ledger 记账本 用户手册（PWA 版）

一个可以安装到手机主屏幕、断网也能阅读的电子书。全部内容都在这个文件夹里，不需要服务器端程序。

## 文件说明

| 文件 | 作用 |
|---|---|
| `index.html` | 电子书本体（含全部 21 节内容，对应 App v601；右下角有「手册对应 App v601」角标） |
| `manifest.webmanifest` | 安装信息：名称、图标、独立窗口显示 |
| `sw.js` | Service Worker：负责离线缓存 |
| `_headers` | 安全头与缓存规则（Cloudflare Pages / Netlify 使用，GitHub Pages 会忽略） |
| `icons/` | 各尺寸图标（浏览器标签、主屏幕、安卓自适应图标） |

## 部署到 GitHub Pages

1. 在 GitHub 新建一个仓库（例如 `ledger-manual`），选择 **Public**。
2. 进入仓库，点 **Add file → Upload files**，把这个文件夹里的**所有文件和 `icons` 文件夹**拖进去，点 **Commit changes**。
   - 注意：要上传的是文件夹里面的内容，`index.html` 必须在仓库最外层，不能多套一层文件夹。
3. 点 **Settings → Pages**，Source 选 **Deploy from a branch**，Branch 选 `main`，文件夹选 `/ (root)`，保存。
4. 等 1–2 分钟，页面顶部会显示网址：`https://你的用户名.github.io/ledger-manual/`。

> 用 Cloudflare Pages、Netlify 也可以：直接上传这个文件夹即可，不需要任何构建设置。

## 迁到 Cloudflare Pages / Netlify

- 上传整个文件夹即可，`_headers` 必须在最外层（和 `index.html` 同级），不需要构建设置。
- `_headers` 已包含：安全响应头、CSP、`index.html` / `sw.js` 不缓存、图标缓存 1 天、manifest 的 Content-Type。
- CSP 为 `default-src 'none'`，只放行本站资源与内嵌脚本/样式、`data:` 图片。如果以后手册里加了外部字体或脚本，需要同步改 CSP。
- 迁完后用浏览器开发者工具 → Network 检查响应头是否生效，再确认 Console 没有 CSP 报错、离线仍可打开。

## 安装到手机

- **iPhone（Safari）**：打开网址 → 分享按钮 → **添加到主屏幕**。
- **安卓（Chrome）**：打开网址 → 右上角 ⋮ → **安装应用**（或点手册里的“安装到手机主屏幕”按钮）。
- 第一次需要联网打开一次，之后断网也能阅读。

## 更新手册

- 直接替换仓库里的 `index.html` 即可。页面采用“联网优先”，用户联网打开时会自动换成新版，断网时用旧缓存。
- 如果更换了 `icons/` 里的图标，请同时把 `sw.js` 第一段的 `VERSION` 改一个新值（例如 `v601-2`），旧缓存才会被清掉。

## 常见问题

- **网址打开是 404**：检查 `index.html` 是否在仓库最外层；Pages 首次生成需要等几分钟。
- **手机上看不到“安装”**：iPhone 不会有弹窗，请用分享菜单添加；安卓请用 Chrome，并确认是 https 网址。
- **想确认离线可用**：安装后，把手机切换到飞行模式再打开图标，应该能正常看到手册。
- **隐私**：Public 仓库任何人都能看到手册内容。手册里只有功能说明，没有你的账本数据。

## 更新手册到新的 App 版本时（版本号要改三处）

手册里的 App 版本号分散在三个地方，一起改，部署后才不会让读者看到前后不一致的版本：

1. `index.html` 里的 `META` 的 `"version"`：右下角角标「手册对应 App vXXX」读的就是它；
2. `index.html` 封面的「适用版本 vXXX」和第二十一节的标题 / 摘要（`v1–vXXX`）；
3. `sw.js` 第一段的 `VERSION`（只影响离线缓存名，不显示在页面上）。

角标只说明「这份手册写的是哪一版 App」，不说明浏览器当前实际在跑哪一版手册。部署后如果角标不是你期望的版本，先强制刷新（Ctrl/Cmd+Shift+R），或在开发者工具里清掉该网站的 Service Worker 和缓存。
