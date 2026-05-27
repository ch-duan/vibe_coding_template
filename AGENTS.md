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

每次处理需求前，先检查项目根目录下的 `.agent/memory/`。

读取顺序：

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

如果 `.agent/memory/` 不存在，应建议创建；如果任务产生长期有效信息，应建议写入对应记忆文件。

## 按需读取索引

不要默认读取所有详细规则。根据任务类型按需读取：

- 通用工程协作、回答风格、代码规范：`.agent/docs/ai-instructions/common.md`
- 跨学科软件工程角色索引：`.agent/docs/ai-instructions/interdisciplinary.md`
  - 系统工程、全栈、AI / ML、数据、算法、科研工程化、产品需求、DevOps / SRE、安全隐私、技术写作等角色规则位于 `.agent/docs/ai-instructions/roles/`
- MCP / 工具调用策略：`.agent/docs/ai-instructions/mcp.md`
- 嵌入式、硬件、电机、机器人、控制系统：`.agent/docs/ai-instructions/embedded-hardware.md`
- Linux 驱动、内核、设备树、BSP、RootFS、用户态系统编程：`.agent/docs/ai-instructions/linux-kernel-driver.md`
- 常用命令模板：`.agent/docs/ai-instructions/commands.md`

## 默认工作流程

1. 读取项目记忆和当前任务相关代码。
2. 明确目标、边界、约束和验收标准。
3. 给出最小实现计划。
4. 执行修改时保持小步提交思路，避免扩大 diff。
5. 运行或说明可执行的验证命令。
6. 总结变更、风险和后续记忆更新建议。

## 回答结构

默认结构：

1. 先给结论。
2. 再解释关键原因。
3. 给出可执行步骤。
4. 给出验证方法。
5. 提醒风险和边界条件。

嵌入式 / 硬件问题：优先判断软件、硬件、通信、供电还是控制参数；按“供电 → 接线 → 通信 → 驱动 → 控制逻辑 → 上层应用”排查。

Linux 驱动 / 内核问题：优先判断硬件、设备树、驱动、内核配置、RootFS 还是应用层；按“硬件 → 设备树 → probe → 设备节点 → 用户态访问 → 应用逻辑”排查。
