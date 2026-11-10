# AGENT.md — 小鯨记忆目录工作约定

## 身份与目标

本目录是「小鯨记忆」的开发工作区：给小鯨（DSH 里的伙伴人格）做一套长期记忆系统，主线分三层——**行动机制**（怎么形成、过滤、遗忘、沉淀）、**内容机制**（对话之外的经验与读书笔记怎么进来）、**展示机制**（记忆星图 + 时间轴）。

实现参考上游 `dsh-fish-memory`，但目标是自己的记忆形态，不是给它打补丁。

## 硬性约束

1. **文档沉淀**：所有分析、方案、实施记录写入 `docs/`，命名 `<序号>_<事项>.md`（如 `00_小鯨记忆开发记录.md`），序号两位递增、不复用、不跳号。
2. **长回答同步存档（2026-11-11 约定）**：讨论中产出长回答时，**原文完整沉淀**进 docs（新增文档或追加进现有文档由小鯨判断，拿不准就问小主），不自行简化浓缩；供小主慢慢回看。
2. **文档只留确定性结论**：没定下来的事留在对话里推进，定了再落笔；文档里不出现「待确认 / ❓ / 需要对方回答」这类内容。
3. **临时文件**放 `tmp/`（不入库），最终交付物放仓库根或 `docs/`。
4. **上游只读**：`dsh-fish-memory/`（行动机制参考，v1.5.8）与 `3d-memory-sea/`（展示机制参考，v0.1.0）都是 clone 的上游源码，只读、不直接改；要改造先另建自己的包目录再动。
5. **记忆数据不入库**：用户真实记忆内容（json/jsonl/sqlite 等运行产物）只在本机，仓库里只放代码、schema 与脱敏样例。
6. **git 远程一律 HTTPS**：本机无 SSH 密钥；代理 127.0.0.1:7897 不稳时用 `git -c http.proxy= -c https.proxy= push` 直连。
7. **沙箱注意**：pwsh 里 Node / Git msys 子进程用 `stdio:'pipe'` 会 EPERM、credential helper 会崩——`git push` 这类需要完整进程能力的命令要用 `sandbox_permissions: danger-full-access` 放行执行。

## 环境备忘

- 工作区：`D:\Work\DSH\大肥鱼\小鯨记忆`
- DSH_HOME：`C:\Users\dupengair\.dsh`
- DSH 实现：`D:\Software\DSH\resources\app.asar`（单文件）
- 上游 `dsh-fish-memory`：`node >= 22`、`dsh >= 0.2.1-rc.2`，入口 `lib/index.js`（宿主）+ `lib/client.js`（浏览器半侧），peer 依赖 `@deepseek-ai/dsh-tools`、`@deepseek-ai/dsh-llm`
- 上游 `3d-memory-sea`（`asdfd3/3d-memory-sea`，包名 `memory-sea`）：`node >= 20`，零运行时依赖，纯前端 Canvas 2D；`src/server.mjs` 起服务（默认 `http://127.0.0.1:4173`），数据层三个纯函数模块 `memory-store.js` / `memory-retrieve.js` / `mind-sea.js` 可单独取用；设计说明见其 `docs/how-to-plug-in.md`
- 两个上游目录都自带 `.codegraph` 索引：查它们的代码用 codegraph MCP 并带 `projectPath` 指向对应目录
