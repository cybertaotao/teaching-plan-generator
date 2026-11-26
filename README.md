# 智能教案制作助手 · teaching-plan-generator V1.0

面向支持 `SKILL.md` 的 AI Agent（如具有文件读图和 Python 执行能力的编码助手）的便携技能工程。不是独立的桌面应用，不内置大模型 API，不需要 API Key；学习资料的理解与转写由运行技能的 AI 完成。

## 快速使用（推荐：让 AI 执行）

1. 解压整个 `teaching-plan-generator` 文件夹，放入你的 Agent 可读取的技能目录或项目工作目录。具体技能目录与自动触发机制取决于平台；本包不会自动安装到任何账户。
2. 新对话上传学习资料，指定使用该 Skill；默认采用附带的莺湖教育模板（若上传新模板，以新模板为准）。
3. 提示示例：

> 使用 teaching-plan-generator Skill，基于附件中的学习资料制作教案，从笔试题开始。保留所有原题、选项、划线、配图及空白，不填写答案。采用默认教案模板，保持每页边框和教学过程竖栏，逐页验收后输出 Word。

4. AI 阅读 `SKILL.md`，生成 JSON 计划，调用脚本并逐页渲染检查。若 Agent 没有技能发现机制，直接让它读取该文件及其引用文件即可。

## 本地运行脚本（Windows / Linux / macOS）

需要 Python 3.10+、LibreOffice（`soffice` 可在 PATH 找到），以及中文/英文字体（宋体、Times New Roman 或可用等价字体；不分发字体文件）。在目录中执行：

```bash
python -m pip install -r requirements.txt
python scripts/extract_material.py "你的资料.pdf" work/source
# AI 读取 work/source/material.json 和每页 PNG，生成 work/plan.json
python scripts/fill_template.py examples/english-unit1-4.json output/教案.docx
python scripts/render_preview.py output/教案.docx output/preview
python scripts/validate_docx.py examples/english-unit1-4.json output/教案.docx --pdf output/preview/preview.pdf --report output/qa.json
python -m unittest discover -s tests -v
```

上面的 fill 命令演示历史英语样例；制作新教案时将两个命令的 JSON 路径改成 `work/plan.json`。PowerShell 下同样可逐行执行。没有 LibreOffice 时结构检查可运行，但不得宣称完成视觉验收。

## 目录

```text
teaching-plan-generator/
├── SKILL.md                        # Agent 入口、触发场景、强制流程
├── README.md                       # 安装及使用
├── requirements.txt
├── assets/default-template.docx    # 默认三页型空白模板
├── references/                    # 原题、教学、模板、计划、验收规范
├── scripts/                       # 提取、填充、渲染、结构校验
├── examples/                      # 120 题完整英语回归计划 + 最小格式示例
└── tests/                         # 自动回归及测试说明
```

## 能力与边界

- 已支持默认模板及相同三页型结构的适配模板；不自动识别任意机构 Word 模板。模板源于用户原模板的边框修正版；保留修正版的页眉页脚、边框与页边距。不是任意模板的通用排版引擎。
- PDF、图片、Word 可作为学习资料；扫描页由 AI 看图，不内置 OCR 或模型调用。旧 `.doc` 学习材料需 LibreOffice 转 PDF；旧 `.doc` 模板须先转换并准备页型，不能直接传给填充脚本。
- 支持可编辑文本、普通内嵌表格、段落下划线和图片；复杂公式、图形、合并内嵌表需专项适配，不能保证全可编辑。正文结构检查不能保证源文准确，必须人工/AI 核对原始附件。
- 页数与版式受字体及 Word/WPS/LibreOffice 渲染差异影响，需实际打印前预览。脚本不会自动决定分页或自动重试；Agent 根据渲染结果调整计划后再运行。
- 附带机构标志和联系电话、历史练习题仅用于本人的备课与回归测试；公开发布工程前确认授权或换用脱敏资源。

## 版本

V1.0：英语教案场景实测；固定模板填充、题目 ID 对账、结构验收与渲染检查。多学科专项与学生历史分析留待后续版本。
