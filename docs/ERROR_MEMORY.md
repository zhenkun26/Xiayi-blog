# 错误记忆

## 2026-09-06：主题扩散圆心在 HiDPI 环境中偏移

- 错误：之前把顶部中央扩散归因于事件代理，仅改用 getElementById 取按钮位置。线上脚本已包含该修复，但问题仍在。
- 证据：本次在 DPR=2 的内置 Chromium 中，DOM 按钮中心为 (550.75, 31.5)，计算样式中的 circle 圆心也是该坐标，截图的实际圆心却约为横坐标的一半。CSS circle(px) 和 polygon(px) 均出现偏移，换成百分比后圆心回到按钮。证据支持快照裁切像素缩放问题，不足以断言具体 Chromium 版本或 GPU 驱动缺陷。
- 修复：圆心与半径都转为相对视口的百分比，由 CSS 关键帧驱动快照；circle 百分比半径按视口对角线 / sqrt(2) 换算。组件通过 bind:this 取按钮；坐标不可用时直接切主题；等待 Svelte tick 后捕获新视图，过渡结束清理变量，期间忽略重入。
- 验证：check / type-check / build PASS；DPR=2 中间帧 `references/m4-theme-percent-light.png`。实体 Chrome 的控制台粘贴保护未绕过，后续 UI 操作多次被窗口变更中断；实体 Chrome/Edge 最终验收待完成。
- 预防：出现圆心漂移时同时核对 DOM 坐标、裁切计算值、动画截图；不能把内置浏览器通过写成实体浏览器通过。无效坐标不再兜底到屏幕中央。排查方案中 CSS px 与 polygon px 都未解决，应保留百分比而非仅换动画 API。
- 生产预览复核：`http://127.0.0.1:4322/`，最终 circle(%) 方案中间帧 PASS，截图 `references/m4-theme-percent-production.png`。实体 Edge 关于页点击切换成功，终态截图 `references/m4-theme-percent-edge.png`；原生截图取得时动画已结束，不能据此宣称实体 Edge 的动画起点已完成验收。预览保留供用户直接验证。
- 工具教训：构建的 tsx IPC 管道与预览监听需要沙箱外运行；首次受限失败后按权限流程重试。仅格式化修改过的组件，避免全仓 lint --write 扩大改动。
