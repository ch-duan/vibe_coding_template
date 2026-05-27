# 跨学科角色索引

本文件只作为跨学科能力的按需读取索引。不要把所有角色规则一次性加载；根据任务类型读取对应文件。

## 通用判断原则

1. 先判断任务主要属于哪个角色或哪些角色。
2. 优先读取最相关的 1-3 个角色文件，避免无关规则增加上下文开销。
3. 如果任务跨多个领域，先读取 `roles/systems-engineer.md` 作为统筹视角，再读取专项角色文件。
4. 涉及长期项目背景、偏好或历史决策时，同时查看 `.agent/memory/` 中相关记忆文件。

## 角色文件

- 系统工程师：`docs/ai-instructions/roles/systems-engineer.md`
- 全栈软件架构师：`docs/ai-instructions/roles/fullstack-architect.md`
- AI / 机器学习工程顾问：`docs/ai-instructions/roles/ai-ml-engineer.md`
- 数据工程与分析顾问：`docs/ai-instructions/roles/data-engineer-analyst.md`
- 算法与数值计算工程师：`docs/ai-instructions/roles/algorithm-numerical-engineer.md`
- 科研工程化顾问：`docs/ai-instructions/roles/research-engineering.md`
- 产品与需求分析伙伴：`docs/ai-instructions/roles/product-requirements.md`
- DevOps / SRE 顾问：`docs/ai-instructions/roles/devops-sre.md`
- 安全与隐私工程顾问：`docs/ai-instructions/roles/security-privacy.md`
- 技术写作与知识管理顾问：`docs/ai-instructions/roles/technical-writing-knowledge.md`

## 组合读取建议

- Web / 平台功能开发：`fullstack-architect.md` + `product-requirements.md`
- AI 应用 / RAG / 模型部署：`ai-ml-engineer.md` + `data-engineer-analyst.md` + `devops-sre.md`
- 数据平台 / 指标系统：`data-engineer-analyst.md` + `fullstack-architect.md`
- 仿真 / 优化 / 数值算法：`algorithm-numerical-engineer.md` + `research-engineering.md`
- 论文复现 / 原型工程化：`research-engineering.md` + `algorithm-numerical-engineer.md`
- 需求梳理 / MVP 设计：`product-requirements.md` + `systems-engineer.md`
- 生产故障 / 稳定性治理：`devops-sre.md` + `systems-engineer.md`
- 权限 / 隐私 / 供应链风险：`security-privacy.md` + `devops-sre.md`
- 文档沉淀 / 故障复盘：`technical-writing-knowledge.md` + `systems-engineer.md`
