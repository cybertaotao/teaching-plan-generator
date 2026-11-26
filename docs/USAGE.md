# 使用教程 · Teaching Plan Generator V1.0

适合初次使用的教师，也适合通过 AI 编码助手或本地 Python 继续开发的用户。

## A. 首先确认文件齐全

本仓库目前归档文档和 Skill 入口。要运行完整 V1.0，需同时拥有历史交付的 `teaching-plan-generator-v1.0.zip`（不是只下载 GitHub 的当前源码 ZIP）。原始工程包含：

```text
teaching-plan-generator/
├── SKILL.md
├── README.md
├── requirements.txt
├── assets/default-template.docx
├── references/
├── scripts/
├── examples/
└── tests/
```

如果缺少 scripts/fill_template.py 或 assets/default-template.docx，请先完成下方「同步原始工程」。不要假定 README 中提到的待同步文件已经在远端。

## B. AI Agent 使用方式（推荐）

1. 解压完整工程，放在 Agent 可访问的本地项目或技能目录。不同平台技能发现机制不同；没有自动加载时，让 Agent 直接读取 SKILL.md 及其引用文件。
2. 上传学习资料（PDF、图片、Word）。可另外上传教案模板、参考教案，明确哪份是学习资料、哪份是模板。用户当次指定模板优先；否则使用内置固定模板。
3. 复制以下指令：
   > 使用 teaching-plan-generator Skill，根据附件中的学习资料生成教案，从笔试题开始，完整保留原题、题号、选项、划线、配图和答题空白，归纳对应知识点，不填写答案。沿用指定或默认教案模板，保证每页完整边框、左侧教学过程栏、页眉页脚和审核区域。逐页验收后输出可编辑 Word 文档。
4. AI 解析资料、归纳知识点、编制 plan.json、调用 Python 填充及渲染脚本；教师核对原题、预览及最终 Word。
5. 下载 DOCX，用 Word/WPS 打开、编辑、打印。若发现跨页溢出或漏题，反馈具体页码/题号，重新生成对应分页计划。

在支持文件读取、视觉和 Python 执行的 Agent 中使用现有模型能力，通常不必另配 API Key；若平台不支持这些能力，则需使用本地执行方案。ChatGPT Plus 与独立 API 计费不是一回事。此版本不是独立 OCR/LLM 服务。

## C. 本地 Python 使用方式

准备 Python 3.10+、LibreOffice（soffice 加入 PATH），安装中英文可用字体。Windows 建议 PowerShell，其他系统亦可。以下命令须在完整工程目录执行：

```bash
python -m pip install -r requirements.txt
python -m unittest discover -s tests -v
python scripts/fill_template.py examples/english-unit1-4.json output/教案.docx
python scripts/render_preview.py output/教案.docx output/preview
python scripts/validate_docx.py examples/english-unit1-4.json output/教案.docx --pdf output/preview/preview.pdf --report output/qa.json
```

预期历史样例：120 个题目 ID、8 个计划页及实际渲染页（其他字体环境可能变化）。查看 output/preview/page-*.png 和 qa.json，结构通过不代表原题准确或视觉验收通过。

处理新资料：
```bash
python scripts/extract_material.py "新练习.pdf" work/source
# 让 AI 逐页阅读 work/source 的 material.json 与 PNG，生成 work/plan.json
python scripts/fill_template.py work/plan.json output/新教案.docx
python scripts/render_preview.py output/新教案.docx output/preview
python scripts/validate_docx.py work/plan.json output/新教案.docx --pdf output/preview/preview.pdf --report output/qa.json
```

计划 JSON 格式见完整包 references/plan-format.md 及 examples/minimal.json。扫描件没有文字层时由 AI 看图，V1.0 不内置 OCR。新模板需先适配三页型结构，再给填充与校验脚本同时传相同的 --template 参数。

## D. 常见问题

| 问题 | 处理 |
|---|---|
| 找不到 requirements 或 scripts | 完整 ZIP 尚未同步；先按下节操作 |
| 找不到 soffice | 安装 LibreOffice 并配置 PATH，重开终端 |
| 模板结构不匹配 | 先适配首页/续页/末页；不可自动套用其他机构格式 |
| 计划页数与预览不同 | 检查文档网格、字体、内容溢出；移动完整题块到新续页再验收 |
| 题目缺字、歧义或扫描不清 | 对照源图修正，保留待核标记；不要猜测 |
| Key 不需要吗 | Agent 模式通常不额外配置；独立云模型应用需要 API Key |
| 学生答案被填入 | 默认学生版禁止填写答案；检查计划内容后重新生成 |

## E. 同步原始工程到 GitHub（一次性）

保留本仓库新 README、AGENTS.md、docs/；从原始 V1.0 ZIP 只复制缺失工程目录。以下 PowerShell 示例假设 ZIP 已下载到 Downloads，且尚未同步任何源码目录：

```powershell
git clone https://github.com/cybertaotao/teaching-plan-generator.git
Set-Location teaching-plan-generator
$zip = Join-Path $HOME "Downloads\teaching-plan-generator-v1.0.zip"
$tmp = Join-Path ([System.IO.Path]::GetTempPath()) ("teaching-plan-" + [guid]::NewGuid())
Expand-Archive -LiteralPath $zip -DestinationPath $tmp
$src = Join-Path $tmp "teaching-plan-generator"
foreach ($name in @("assets","references","scripts","examples","requirements.txt")) {
    if (!(Test-Path $name)) {
        Copy-Item (Join-Path $src $name) -Destination . -Recurse
    } else { Write-Host "已存在，请人工比较，不覆盖: $name" }
}
New-Item -ItemType Directory -Force tests | Out-Null
foreach ($name in @("test_regression.py","regression-report.json","TEST-REPORT.md")) {
    $dest = Join-Path "tests" $name
    if (!(Test-Path $dest)) { Copy-Item (Join-Path "$src/tests" $name) -Destination $dest }
}
git status
# 确认未包含学生信息、Key、无授权资料及临时输出
git add assets references scripts examples requirements.txt tests
git commit -m "feat: sync original Skill V1.0 implementation"
git push origin main
```

如已有本地 clone，先确保工作区干净并 git pull --ff-only。若已存在部分目录，逐文件对照原包补齐，不使用覆盖或强推。私有仓库需登录有写权限的 GitHub 账号。同步成功并复跑测试后，更新 README 的仓库状态及 docs/ROADMAP.md。

## F. 日常开发

先读 AGENTS.md、README、路线图；创建 feature 分支，修改代码并复跑测试，确认原题和模板约束未破坏后提交。不要把 API Key、用户附件、output 或临时文件提交。新需求按 PRD 与路线图记录，保持 README 只展示项目定位、启动方式、文档导航与真实进度。
