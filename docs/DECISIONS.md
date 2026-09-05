# Xiayi-blog 技术决策记录

> 本文件只记录影响项目结构、技术选型和部署方式的取舍。格式沿用 auto-coding 的 ADR 规范（背景 / 候选方案 / 选择 / 放弃 / 验证 / 改判条件）。

## 索引

| ID | 决策 | 状态 | 证据 |
|---|---|---|---|
| ADR-XB-001 | 采用 CuteLeaf/Firefly 作为博客底座 | accepted | 本文件、git log |
| ADR-XB-002 | 目录位置约束与文档布局 | accepted | AGENTS.md、docs/ |
| ADR-XB-003 | 内容专注中文，不引入多语言内容路由 | accepted | siteConfig.ts、ROADMAP |

## ADR-XB-001：采用 CuteLeaf/Firefly 作为博客底座

- **背景**：从零建设个人博客，需要中文生态成熟、可由 AI 持续小步改造的静态博客方案；用户认可 Fuwari 风格，并希望以 rainzt.cn（Firefly 的二改作品 Aemeath）为长期外观参考。
- **候选方案**：原版 saicaca/fuwari；Hugo 系（PaperMod / stack / Blowfish）；Hexo Butterfly；timlrx/tailwind-nextjs-starter-blog；CuteLeaf/Firefly。
- **选择**：CuteLeaf/Firefly。理由：Fuwari 直接二开，功能覆盖（六语言界面、Pagefind 搜索、说说 / 友链 / 相册 / 音乐 / 看板娘等）远超原版；中文社区活跃（2068 stars，2026-09 仍在持续提交）；文档与配置注释全中文；MIT 协议；Astro 7 + TS 代码结构清晰，适合 AI 小步改造。
- **放弃**：原版 fuwari（功能少、更新慢）；Hugo 系（模板语法对 AI 迭代不如 Astro/TS 友好，且用户已认可 Fuwari 视觉）；Next.js starter（最重，维护成本高）；Hexo Butterfly（生态偏老）。
- **验证**：仓库基于 upstream/master（db331cf）检出。M1 全链路 PASS——pnpm install（59.2s，pnpm 11.22.0）；pnpm dev（localhost:4321 HTTP 200）；浏览器渲染验证截图 `references/m1-home-light-1280x720.png`；pnpm check（253 文件，0 错误 0 警告）；标题热更新生效。
- **改判条件**：上游停更超过 6 个月，或本地改动与上游出现无法维护的结构性冲突；届时基于当时最新版评估重建。

## ADR-XB-002：目录位置约束与文档布局

- **背景**：用户要求杜绝散乱文件与多余隐藏文件夹；同时需要跨会话记忆与流程文档支撑长期迭代。
- **候选方案**：独立 AI 记忆目录（如 ai_pipeline/）；全部并入 docs/；不做约束任其散落。
- **选择**：项目文档集中在 `docs/`（ROADMAP / PROCESS / DECISIONS，ERROR_MEMORY 按需创建）；参考材料进 `references/`；自建脚本进 `scripts/`（`xiayi-` 前缀与上游脚本区分）；临时产物进 `tmp/` 或 `/tmp`（gitignore）。上游自带的 AGENTS.md / CLAUDE.md / docs 内容保持原位，项目约定追加在 AGENTS.md 尾部。
- **放弃**：新建顶层记忆目录；在仓库外存放项目文件；覆盖上游文档。
- **验证**：AGENTS.md 尾部"项目约定"小节；`.gitignore` 追加 `tmp/`。
- **改判条件**：references/ 或 scripts/ 规模膨胀到影响检索时，再评估细分。

## ADR-XB-003：内容专注中文，不引入多语言内容路由

- **背景**：初期规划包含中英双语内容方案；用户明确删去，专注中文内容与中文社区。
- **候选方案**：内容级双语（/en/ 路由 + 翻译工作流）；仅界面语言设为 zh_CN；维持双语路线图。
- **选择**：界面语言 zh_CN（Firefly 默认即中文），内容只写中文；不规划、不实现多语言内容路由，双语里程碑从 ROADMAP 移除。
- **放弃**：内容双语路线图及其全部关联任务。
- **验证**：`src/config/siteConfig.ts` 中 `SITE_LANG = resolveSiteLang("zh_CN")`（上游默认）。
- **改判条件**：用户重新提出双语需求时，另立 ADR 并恢复对应里程碑。
