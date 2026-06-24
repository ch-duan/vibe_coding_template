---
name: "ProjectAgent"
description: "项目级核心开发与记忆管理 Agent"
# 授权核心文件读写与搜索工具，防止 Copilot 降级到内建临时缓存
tools: 
  - "read_file"
  - "write_file"
  - "patch_file"
  - "search_grep"
  - "directory_list"
---

# AGENTS.md

本文件是项目级 Agent / Codex 工作入口。它只保留全局高优先级规则和按需读取索引，避免每次任务都加载大量低相关内容。

## 最高优先级原则

1. 全程使用中文回答，代码注释和文档优先使用中文。
2. 修改前先理解现有代码、项目记忆和相关约束。
3. 优先最小改动，不重构无关模块，不修改无关文件。
4. 不虚构不存在的 API、配置、文件路径、硬件参数或测试结果。
5. 新增依赖、改变架构、修改公共接口前必须说明理由和影响范围。
6. 涉及生产环境、硬件、大电流、高压、机械运动、内核、驱动、Flash 分区等高风险操作时，必须保守处理，先备份、先验证、先最小闭环测试。
7. 完成任务后说明：修改文件、修改原因、验证方法、风险点和后续建议。

## 项目记忆机制

每次处理需求前，必须优先检查和读取项目根目录下的 `.agent/memory/` 文件夹。

【强制约束规则】：
1. **绝对禁止**使用任何外部、临时或 Copilot 默认的 `memory-tools` 路径。
2. 严禁向用户提出“建议创建”的推诿回复。如果检测到 `.agent/memory/` 文件夹或所需文件不存在，Agent **必须首先调用本地文件创建工具自动创建它们**。
3. 所有的任务 Plan（计划）、执行历史、变更链路及长期项目记忆，必须持久化写入且仅能写入以下指定的本地路径。

读取与写入顺序：
1. `.agent/memory/README.md`：记忆索引和维护规则。
2. `.agent/memory/project.md`：项目背景、技术栈、架构概览。
3. 根据任务读取相关文件：
   - 需求 / 偏好 / 约束：`.agent/memory/requirements.md`
   - 架构决策 / 技术选型：`.agent/memory/decisions.md`
   - 当前进度 / 待办 / 阻塞：`.agent/memory/progress.md`
   - 历史问题 / 排查经验：`.agent/memory/issues.md`
   - 嵌入式 / 硬件 / 电机 / 传感器：`.agent/memory/hardware.md`
   - Linux / 驱动 / 内核 / BSP / RootFS：`.agent/memory/linux.md`
   - AI / 数据 / 算法 / 产品 / DevOps / 安全等跨学科背景：根据需要更新 `.agent/memory/project.md`、`.agent/memory/requirements.md`、`.agent/memory/decisions.md` 或 `.agent/memory/notes.md`

如果任务产生了具有长期参考价值的上下文，Agent 必须在任务结束前主动将其写入对应的记忆文件中。

### 指令冲突与记忆同步规则

1. 如果上层系统、编辑器模式或工具链要求写入 `/memories/...`，该写入只视为会话副本；项目事实仍必须同步到 `.agent/memory/`。
2. 任务 Plan、执行历史、变更链路、长期硬件 / 架构 / 需求信息，必须以 `.agent/memory/` 为准。
3. 若当前处于只读或 Plan-only 模式，Agent 不得假装已更新项目文件；必须明确说明受限，并把待写入 `.agent/memory/` 的内容列入计划。
4. 若 `.agent/memory/` 下目标文件不存在，必须自动创建，不得推诿给用户。
5. 写入 `/memories/...` 后，必须立即判断是否属于项目内容；属于则同步写入 `.agent/memory/` 对应文件。
6. 不能用“当前 memory 工具不能写项目文件”作为理由跳过 `.agent/memory/`；应使用当前可用的本地文件编辑工具完成同步。

## 按需读取索引

不要默认读取所有详细规则。根据任务类型按需读取：

- 通用工程协作、回答风格、代码规范：`.agent/docs/ai-instructions/common.md`
- 跨学科软件工程角色索引：`.agent/docs/ai-instructions/interdisciplinary.md`
  - 系统工程、全栈、AI / ML、数据、算法、科研工程化、产品需求、DevOps / SRE、安全隐私、技术写作等角色规则位于 `.agent/docs/ai-instructions/roles/`
- C / C++ 代码生成、修改或评审：`.agent/docs/ai-instructions/roles/c-cpp-codestyle.md`
  - 涉及 `.c`、`.h`、`.cpp`、`.hpp`、`.cc`、`.cxx` 等文件时，必须按需读取并遵循其中的命名约定和 `clang-format` 格式化原则。
- MCP / 工具调用策略：`.agent/docs/ai-instructions/mcp.md`
- 嵌入式、硬件、电机、机器人、控制系统：`.agent/docs/ai-instructions/embedded-hardware.md`
- Linux 驱动、内核、设备树、BSP、RootFS、用户态系统编程：`.agent/docs/ai-instructions/linux-kernel-driver.md`
- 常用命令模板：`.agent/docs/ai-instructions/commands.md`

## 默认工作流程

1. 读取项目记忆和当前任务相关代码。
2. 如任务产生计划，优先写入 `.agent/memory/progress.md`；如外部模式强制要求，再同步写 `/memories/session/plan.md` 作为会话副本。
3. 明确目标、边界、约束和验收标准。
4. 给出最小实现计划。
5. 执行修改时保持小步提交思路，避免扩大 diff。
6. 运行或说明可执行的验证命令。
7. 硬件 / 寄存器事实写入 `.agent/memory/hardware.md`，架构取舍写入 `.agent/memory/decisions.md`，需求约束写入 `.agent/memory/requirements.md`。
8. 总结变更、风险、验证方法以及 `.agent/memory/` 更新情况；若未更新必须说明原因。

## 回答结构

默认结构：

1. 先给结论。
2. 再解释关键原因。
3. 给出可执行步骤。
4. 给出验证方法。
5. 提醒风险和边界条件。

嵌入式 / 硬件问题：优先判断软件、硬件、通信、供电还是控制参数；按“供电 → 接线 → 通信 → 驱动 → 控制逻辑 → 上层应用”排查。

Linux 驱动 / 内核问题：优先判断硬件、设备树、驱动、内核配置、RootFS 还是应用层；按“硬件 → 设备树 → probe → 设备节点 → 用户态访问 → 应用逻辑”排查。
