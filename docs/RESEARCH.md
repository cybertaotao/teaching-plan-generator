# 竞品、开源与技术可行性调研

归档日期：2026-11-26。整理自会话中的公开资料调研，不代表已购买竞品实测或全面审阅源码；未确认项不是“不支持”。产品、价格、许可证会变化，开发前需再次核对官方最新版本。

## 1. 教育与办公竞品

| 产品 | 已知方向 | 可借鉴 |
|---|---|---|
| [MagicSchool AI](https://www.magicschool.ai/) | 面向教师的教案、教学活动、测验等 AI 工具 | 按教学任务拆解 |
| [Eduaide.AI](https://www.eduaide.ai/) | 教学资源生成、编辑、资料加工 | 教学工作台 |
| [Diffit](https://web.diffit.me/) | 已有资料加工、差异化阅读及练习 | 资料导入与难度调整 |
| [WPS AI](https://ai.wps.cn/) | 办公文档内的 AI 辅助 | 贴合教师现有文档工作流 |

固定机构 Word 模板的逐页保真、原题独立对账、每页边框自动验收等能力在此次竞品调研中均未完成专项实测。差异化方向：原题保真、机构标准化和真实文档交付，不与大厂竞争通用教学生成。

## 2. 开源及二次开发

| 项目 | 适用范围 | 决策 |
|---|---|---|
| [Lesson Loom](https://github.com/michael-borck/lesson-loom) | 教案生成、模型配置、多端部署与历史 | 研究设计；未确认许可证，不直接复制源码 |
| [Lesson Plan Compiler](https://github.com/Teaching-Works-Lab/lesson-plan-compiler) | 高校教案 Skill，材料、来源及 QA | 仅核实公开索引说明，仓库与授权需复核 |
| [Dify](https://github.com/langgenius/dify) | 可视化模型工作流 | 可用于 PoC；不作为最终框架，核对多租户及品牌限制 |
| [PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR) | 本地 OCR、版面、表格与公式 | 优先 PoC |
| [MinerU](https://github.com/opendatalab/MinerU) | PDF/图片复杂版面结构化 | 备选 PoC，审查许可证与权重 |
| [python-docx](https://github.com/python-openxml/python-docx) | Word 结构读取与填充 | 直接复用现有 V1.0 |
| [docxtpl](https://github.com/elapouya/python-docx-template) | 模板变量替换 | 可选辅助 |
| [pypdfium2](https://github.com/pypdfium2-team/pypdfium2) | PDF 渲染与提取 | 商业闭源时评估替代 PyMuPDF |
| [LibreOffice](https://www.libreoffice.org/) | DOCX 转 PDF 预览 | 复用 |
| [PDF.js](https://github.com/mozilla/pdf.js) | 浏览器预览 | 推荐 |
| [Ollama](https://github.com/ollama/ollama) | 本地模型 | 后续支持 |

许可证调研要点：PaddleOCR Apache-2.0、python-docx MIT、pypdfium2 Apache-2.0/BSD-3-Clause、docxtpl LGPL-2.1、PyMuPDF AGPL 或商业授权；MinerU 与 Dify 含附加条款，需按实际使用版本核验。所有模型权重、第三方字体、教材资料和模板版权也需独立审查。

**建议：组件复用 + 自研业务引擎，而非整体二开竞品。**

## 3. 模型接入与 API Key

| 方案 | Key | 适用 |
|---|---|---|
| Agent Skill | 由宿主提供模型能力，通常无需额外 Key | 自用、验证 |
| BYOK | 用户配置供应商 API Key、Base URL、模型 | 独立 MVP |
| 平台统一 Key | 平台服务端配置，用户无需自己申请 | SaaS |
| 本地 Ollama | 本机通常无需 Key；对外需鉴权 | 离线、私有部署 |

ChatGPT 订阅不等于 API 额度；独立应用不得将 Plus 账号当免费 API。模型方向：千问视觉/OCR 用于图文识别，DeepSeek 等文本模型用于教学分析，GPT/Claude/Gemini 多模态作为可选高质量方案。具体模型 ID、可用地区与价格须在接入时查看官方文档。

官方资料：
- [阿里云百炼模型定价](https://help.aliyun.com/zh/model-studio/model-pricing)
- [OpenAI API 价格](https://openai.com/api/pricing/)
- [Ollama 文档](https://docs.ollama.com/)

采用 OpenAI 兼容接口 + 供应商能力适配；文字模型不能假定支持图片。首版不训练模型、不使用 RAG/向量库或多 Agent。模型只处理必要教学内容；学生姓名、机构联系方式等不应默认传至云端。

## 4. 成本预算示例

非实际模型报价。假设一份教案总输入 20,000 tokens，输出 10,000 tokens，输入/输出单价分别 ¥2/¥8 每百万 tokens，额外识别复核 ¥0.05/份：
- 输入 ¥0.04 + 输出 ¥0.08 + 额外 ¥0.05 = ¥0.17/份。
- 1,000 份/月约 ¥170，仅此假设下的模型与额外识别成本。
- 不包括服务器、OCR 本机硬件、存储、带宽、重试之外的费用及运维；正式定价前测量真实 token、图片计费及失败率。

## 5. 产品形态及商业验证

Skill 开发成本最低但教师门槛较高；桌面端隐私和本地文件体验好但分发复杂；Web 使用门槛低；Word/WPS 插件受宿主兼容与发布限制。建议先本地 Web，再按实际需求 Tauri 封装，最后考虑云端 SaaS。教师订阅 ¥19–39/月、买断 ¥99–199 仅为待验证商业假设，不是调研得出的市场价格。

待验证：5–10 名教师访谈，30 份资料测试，OCR 对照、模板输出测试、付费意愿、许可证全面审核。
