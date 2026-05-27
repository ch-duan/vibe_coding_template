# Optimized AGENTS Package

这是从原始大体积 `AGENTS.md` 拆分出来的优化版本。


## 全局配置

本包新增根目录 `agents_global.md`，用于用户级 / 工具级全局配置。建议：

1. 将 `agents_global.md` 放入全局 instructions 或全局 Agent 配置。
2. 将本包其余内容放入具体项目根目录。
3. 全局配置只负责稳定偏好和索引；项目内 `AGENTS.md`、`docs/ai-instructions/`、`.agent/memory/` 保存项目专用规则和上下文。

这样可以避免把嵌入式、Linux 驱动、AI、数据、DevOps、安全等长规则全部塞进全局上下文。

## 使用方式

1. 将本压缩包解压到项目根目录。
2. 保留根目录 `AGENTS.md` 作为 Agent 入口。
3. 根据项目情况补充 `.agent/memory/*.md`。
4. 详细规则放在 `docs/ai-instructions/`，由 `AGENTS.md` 按需引用。

## 结构

```text
agents_global.md
AGENTS.md
docs/ai-instructions/
  common.md
  mcp.md
  embedded-hardware.md
  linux-kernel-driver.md
  commands.md
.agent/memory/
  README.md
  project.md
  requirements.md
  decisions.md
  progress.md
  issues.md
  hardware.md
  linux.md
  notes.md
```

## 设计目标

- 降低每次任务加载 `AGENTS.md` 的 token 开销。
- 保留全局高优先级规则。
- 将嵌入式、硬件、Linux 驱动等长规则拆成按需读取文件。
- 将项目长期上下文放入 `.agent/memory/`。

## 本次补充

新增 `docs/ai-instructions/interdisciplinary.md`，用于覆盖嵌入式、硬件、Linux 驱动和内核之外的跨学科软件工程能力，包括：

- Web / 后端 / 平台工程
- AI / 机器学习 / 大模型应用
- 数据工程与分析
- 算法、仿真与数值计算
- 科研原型工程化
- 产品、需求与项目管理
- DevOps / SRE / 可观测性
- 安全、隐私与合规
- 技术写作与知识沉淀


## 跨学科角色拆分

跨学科能力已按角色拆分到 `docs/ai-instructions/roles/`：

- `systems-engineer.md`：系统工程师
- `fullstack-architect.md`：全栈软件架构师
- `ai-ml-engineer.md`：AI / 机器学习工程顾问
- `data-engineer-analyst.md`：数据工程与分析顾问
- `algorithm-numerical-engineer.md`：算法与数值计算工程师
- `research-engineering.md`：科研工程化顾问
- `product-requirements.md`：产品与需求分析伙伴
- `devops-sre.md`：DevOps / SRE 顾问
- `security-privacy.md`：安全与隐私工程顾问
- `technical-writing-knowledge.md`：技术写作与知识管理顾问

`docs/ai-instructions/interdisciplinary.md` 现在只作为角色索引和组合读取建议，避免一次性加载所有跨学科规则。
