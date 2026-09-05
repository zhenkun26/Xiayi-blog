# Xiayi-blog 路线图（单一任务状态源）

> 规则：任务状态只有 未开始 / 进行中 / 完成（完成必须附验证证据）。每次会话结束更新本文件；跨会话恢复先读本文件。当前断点见文末。

## M1 MVP —— Firefly 底座本地跑通（当前里程碑）

- [x] M1-1 git init，Firefly 以 `upstream` 远端接入，本地 main 基于 upstream/master（db331cf）
- [x] M1-2 环境：Node 24.20（要求 ≥22.23）、pnpm 11.22.0（corepack，与 packageManager 锁定一致）
- [x] M1-3 Phase 0 文档骨架（AGENTS 项目约定 / ROADMAP / PROCESS / DECISIONS / .gitignore 追加）→ commit 2ffcda9
- [x] M1-4 `pnpm install` 成功（59.2s）→ PASS
- [x] M1-5 `pnpm dev` → localhost:4321 渲染验证 → PASS，截图 `references/m1-home-light-1280x720.png`
- [x] M1-6 最小配置：站点标题占位（title/subtitle/navbar.title）→ commit 4714c14

验收标准：dev 服务器正常渲染默认首页；`pnpm check` 无错误（253 文件 0 错误 0 警告 PASS）；全部改动独立 commit 可回退。

## M2 站点基础配置（未开始）

逐项拍板，每项一个 commit：

- [ ] 站点信息：title / subtitle / description / keywords / site_url / siteStartDate
- [ ] 头像与资料：profileConfig（头像图片、名称、简介、社交链接）
- [ ] 导航栏：navBarConfig（菜单项、Logo、模式）
- [ ] 侧边栏：sidebarConfig（组件取舍）
- [ ] 页面开关：siteConfig.pages（friends / guestbook / dynamic / gallery / booknav / sponsor 等，用不到的先关）
- [ ] 评论系统选型（giscus 需 GitHub 公开仓库；Waline 需额外服务端）→ 先出 ADR 再执行

验收标准：改动后 `pnpm check` + dev 渲染正常，每项独立可回退。

## M3 GitHub Pages 部署（未开始）

- [ ] ADR：部署方式（Actions 构建发布到 gh-pages 分支 / Actions + Pages）
- [ ] base path 处理（`<user>.github.io/Xiayi-blog/` 子路径）或自定义域名
- [ ] site_url 与 sitemap / RSS 对齐
- [ ] workflow 文件 + 首次部署验证

验收标准：公网可访问，HTTPS 生效，文章页 / 图片 / 搜索正常。

## M4 外观逐项迭代（参考 rainzt.cn，未开始）

原则：**一次只做一项**，完成后截图对比 → 用户确认 → 下一项。候选池（顺序待定）：

- [ ] 背景壁纸模式（backgroundWallpaper.ts：banner / 全屏 / 透明 / 纯色）
- [ ] 主题色相（hue）与明暗默认模式
- [ ] 字体方案（fontConfig.ts）
- [ ] 文章列表布局（list / grid / masonry，封面位置）
- [ ] 特效取舍（effectsConfig.ts 樱花等）
- [ ] 页脚（footerConfig）

验收标准：每项独立 commit + 前后截图留档 `references/`。

## 当前断点

- 里程碑：M1 已完成（2026-09-04），M2 待启动
- 状态：本地 dev 可用（`pnpm dev` → localhost:4321）；git 身份尚为自动生成（zhenkun@zhenkundeMacBook-Air.local），建议用户设置 `git config` 后续提交使用正确身份
- 下一步：等用户对 M2 第一项拍板（建议顺序：站点信息 → 头像资料 → 页面开关 → 导航/侧栏 → 评论系统 ADR）；部署（M3）与外观迭代（M4）其后
- 已知待办：默认横幅/头像为星穹铁道 IP 美术（Firefly 自带演示素材），M4 阶段替换为自己的图
