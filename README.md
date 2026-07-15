# Qiuyufei Apps 官网（矩阵 App #000）

静态官网，托管全部矩阵 App 的产品页、隐私政策和支持页。
部署为 GitHub Pages 用户主页仓库：`2645809444.github.io`。

## 线上地址

- 首页：https://2645809444.github.io/
- 各 App：`https://2645809444.github.io/<app>/`（产品页）、
  `/<app>/privacy.html`（隐私政策）、`/<app>/support.html`（支持页）

## 目录结构（页面由生成器产出，不要手改仓库根的 HTML）

```
.site-src/apps.json            站点配置：App 清单、31 语言清单
.site-src/i18n/<lang>.json     ★ 唯一的内容源（en + 30 语言，键必须与 en 对齐）
.site-src/build.py             生成器：python3 .site-src/build.py（产出 155 页）
.site-src/shots/raw-*.png      截图原图（生成器压缩后进 assets/img/）
index.html + <app>/…           en 版（根目录）——生成产物
<lang>/…                       其他 30 语言版本——生成产物
<app>/privacy.html|support.html  手写法务页，生成器绝不碰（App 内引用其 URL）
assets/site.css / site.js      设计系统与增强脚本（零第三方、无跟踪）
sitemap.xml / robots.txt       生成产物（含全语言 hreflang）
```

多语言机制：每种语言一套静态页面（`/` 英文、`/<lang>/` 其他），SEO 友好
（hreflang 互链）；根页有首访自动跳转脚本（按浏览器语言，选择存 localStorage），
导航栏有手动语言菜单。改文案 = 改 `.site-src/i18n/*.json` → 跑 build.py → push。

JS 铁律：自写自托管、零第三方、无跟踪；音效全部 WebAudio 合成（无采样素材）。
产品页下载按钮在 App 上架前是置灰的 "Coming Soon"，上架后回填真实链接
（改 en.json 等全部语言的 JSON + build）。

## 新增 App 流程

1. `.site-src/apps.json` 的 `apps` 加条目；每个 `.site-src/i18n/<lang>.json` 的 `apps`
   里加该 App 的完整文案（从 aso-copy 的 listing.md 取；隐私政策内容见 privacy-page
   技能，三方口径一致），31 语言键必须与 en 对齐（build.py 会校验）
2. 手写 `<app>/privacy.html` 和 `<app>/support.html`（生成器不管理法务页）
3. 跑 `python3 .site-src/build.py`，本地 `python3 -m http.server` 肉眼过一遍
4. commit + push，等 1-2 分钟后 `curl -sI` 验证 200
5. 工程内 `AppConfig`/`AppLinks` 填入正式 URL

## 部署

推送 main 分支即自动发布（Pages 开在 main 根目录）。构建在本地完成，线上无依赖。
