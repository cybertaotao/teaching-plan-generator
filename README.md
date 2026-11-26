# 智能教案制作助手 · Teaching Plan Generator

> 将教师已有的教材、试卷、PDF、图片加工为 **原题完整、知识点清晰、符合机构模板、可编辑可打印的 Word 教案**。

![阶段](https://img.shields.io/badge/阶段-Skill_V1.0-blue) ![方向](https://img.shields.io/badge/下一步-Web_MVP-orange)

## 1. 项目是做什么的？

面向中小学教师、兼职辅导老师与教培机构，解决资料录题、知识点整理、固定教案排版与打印返工问题。区别于通用 AI 教案生成器，本项目强调 **原题保真 + 机构模板保护 + 逐页质量验收**。

```mermaid
flowchart LR
  A[上传试卷 / 教材] --> B[提取原题]
  B --> C[原题确认]
  C --> D[AI 教学分析]
  D --> E[结构化教案 JSON]
  E --> F[Word 模板填充]
  F --> G[分页与质量验收]
  G --> H[导出 DOCX]
```

## 2. 快速使用

- **已有完整 V1.0 压缩包：** 解压后让支持文件读取、视觉和 Python 的 AI Agent 阅读 [SKILL.md](SKILL.md)，上传学习资料即可开始。
- **想了解详细操作：** [使用教程（Agent / 本地 Python / Git 同步）](docs/USAGE.md)。
- **想继续开发：** 阅读 [开发路线图](docs/ROADMAP.md) 和 [技术架构](docs/ARCHITECTURE.md)。

可复制给 AI：

> 使用 teaching-plan-generator Skill，根据附件制作教案，从笔试题开始。完整保留原题、题号、选项、划线、配图和答题空白，归纳核心知识点，不填写答案。沿用机构教案模板，每页保留外框和教学过程栏，逐页检查后输出 Word。

**当前仓库状态（2026-11-26）：** Skill 入口、使用教程、产品需求、调研、架构、路线图、历史测试报告已归档。原始 V1.0 ZIP 中的 Python 脚本、模板及测试数据尚未完整同步到 GitHub；仅克隆本仓库还不能运行生成脚本。请按教程中的「同步原始工程」补齐，注意不要覆盖新版 README。V1.0 是 Agent 技能，不是已上线的 Web 平台。

## 3. 项目文档导航

| 文档 | 内容 |
| --- | --- |
| [SKILL.md](SKILL.md) | AI 执行入口与强制工作流 |
| [使用教程](docs/USAGE.md) | 环境、安装、调用、示例、排错 |
| [产品需求 PRD](docs/PRD.md) | 项目背景、用户痛点、功能优先级、验收 |
| [竞品与可行性调研](docs/RESEARCH.md) | 竞品、开源复用、模型 API 与成本 |
| [技术架构](docs/ARCHITECTURE.md) | 技术栈、数据协议、模型和文档引擎 |
| [路线图](docs/ROADMAP.md) | 当前进度、MVP 边界、迭代任务 |
| [V1.0 原始说明](docs/SKILL-V1-README.md) | 保留首次交付的 README |
| [历史测试报告](tests/TEST-REPORT.md) | 120 道小题、8 页回归案例与测试限制 |

## 4. 当前成果与下一步

| 阶段 | 状态 |
| --- | --- |
| 英语教案真实案例验证 | 已完成 |
| Skill V1.0 完整工程 | 已在此前会话生成，本仓库部分同步 |
| V1.0 回归案例 | 历史测试：120 个题目 ID、8 页、4 项自动测试 |
| 本地 OCR + 模型 API 验证 | 待开发 |
| Web 教案工作台 MVP | 待开发 |
| 多学科、桌面端、机构管理 | 后续规划 |

首版优先实现：上传 PDF/图片/Word、原题解析与人工确认、知识点生成、固定模板填充、逐页预览、Word 导出。暂不做完整在线 Word 编辑器、在线题库、学生管理、模型训练或多 Agent 系统。

## 5. 推荐技术栈

Vue 3 + TypeScript + Vite + Element Plus / FastAPI + Python / SQLite / PaddleOCR（或 MinerU）/ python-docx / LibreOffice / PDF.js。独立应用优先支持 OpenAI 兼容模型接口，个人使用可继续借助已有 Agent 而无需另配 API Key；云 API 与 ChatGPT 订阅分别计费。

## 6. 项目原则

- 不漏题、不擅自改题、不默认填写答案。
- 不重绘机构外表，不删除每页边框、教学过程竖栏、页眉页脚及审核区域。
- 模板或识别异常时明确提示，不伪造验收结果。
- 保持结构化内容与 Word 排版分离，先复用已有脚本，再增加界面。
- 当前私有仓库：原机构模板含 Logo 和联系方式，公开前需脱敏及确认授权；不得提交 API Key、学生个人信息或无授权资料。

下次继续开发时，先读 [AGENTS.md](AGENTS.md) 与 [路线图](docs/ROADMAP.md)，避免重复分析。
