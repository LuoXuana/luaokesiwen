# Skills 使用指南 PDF — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 生成一份 5 章、8-10 页的 Agent Skills 实战手册 PDF，严格使用 PDF Skill 范式。

**Architecture:** 单个 Python 脚本，用 reportlab 的 SimpleDocTemplate + story 列表模式构建 PDF。不自定义 PageTemplate，只用 Paragraph/Spacer/PageBreak/Table/HRFlowable。

**Tech Stack:** Python 3, reportlab, SimHei/SimSun/KaiTi 中文字体

---

## File Structure

| 文件 | 职责 |
|------|------|
| `output/generate_skills_pdf.py`（新建） | PDF 生成脚本，5 章内容全部内联 |
| `Codex_概述.pdf`（覆盖） | 最终输出 PDF |

---

### Task 1: 编写 PDF 生成脚本

**Files:**
- Create: `output/generate_skills_pdf.py`

**代码策略：** 严格使用 PDF Skill 示范的 platypus 模式——SimpleDocTemplate + Paragraph + Spacer + PageBreak + Table + HRFlowable。样式基于 getSampleStyleSheet 扩展（只改 fontName/fontSize/textColor/alignment/leading）。不写自定义 PageTemplate 类。

- [ ] **Step 1: 编写并保存完整脚本**

```python
# -*- coding: utf-8 -*-
"""Agent Skills 使用指南 PDF 生成脚本
严格按照 PDF Skill 工作流：SimpleDocTemplate + story 列表 + 标准 flowable。
不自定义 PageTemplate，只使用 Paragraph/Spacer/PageBreak/Table/HRFlowable。
"""
from reportlab.lib.pagesizes import A4
from reportlab.lib.styles import getSampleStyleSheet, ParagraphStyle
from reportlab.lib.colors import HexColor
from reportlab.lib.units import mm
from reportlab.lib.enums import TA_CENTER, TA_JUSTIFY, TA_LEFT
from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, PageBreak, Table, TableStyle
from reportlab.platypus.flowables import HRFlowable
from reportlab.pdfbase import pdfmetrics
from reportlab.pdfbase.ttfonts import TTFont

# ---------- 字体 ----------
pdfmetrics.registerFont(TTFont('SimHei', 'C:/Windows/Fonts/simhei.ttf'))
pdfmetrics.registerFont(TTFont('SimSun', 'C:/Windows/Fonts/simsun.ttc', subfontIndex=0))
pdfmetrics.registerFont(TTFont('KaiTi', 'C:/Windows/Fonts/simkai.ttf'))

# ---------- 样式（基于 getSampleStyleSheet 扩展） ----------
styles = getSampleStyleSheet()

styles.add(ParagraphStyle(
    'CNTitle', fontName='SimHei', fontSize=24, leading=32,
    textColor=HexColor('#1a5276'), spaceAfter=16, alignment=TA_CENTER))

styles.add(ParagraphStyle(
    'CNH1', fontName='SimHei', fontSize=16, leading=24,
    textColor=HexColor('#1a5276'), spaceBefore=18, spaceAfter=10))

styles.add(ParagraphStyle(
    'CNH2', fontName='SimHei', fontSize=12, leading=18,
    textColor=HexColor('#2980b9'), spaceBefore=12, spaceAfter=6))

styles.add(ParagraphStyle(
    'CNBody', fontName='SimSun', fontSize=10.5, leading=18,
    textColor=HexColor('#2c3e50'), spaceAfter=6, alignment=TA_JUSTIFY))

styles.add(ParagraphStyle(
    'CNQuote', fontName='KaiTi', fontSize=10, leading=16,
    textColor=HexColor('#7f8c8d'), leftIndent=16, rightIndent=16,
    spaceBefore=6, spaceAfter=10))

styles.add(ParagraphStyle(
    'CNFooter', fontName='SimSun', fontSize=8, leading=10,
    textColor=HexColor('#95a5a6'), alignment=TA_CENTER))

# ---------- 辅助函数 ----------
def h1(text):
    """一级标题"""
    return Paragraph(text, styles['CNH1'])

def h2(text):
    """二级标题"""
    return Paragraph(text, styles['CNH2'])

def body(text):
    """正文"""
    return Paragraph(text, styles['CNBody'])

def quote(text):
    """引用"""
    return Paragraph(text, styles['CNQuote'])

def hr():
    return HRFlowable(width="100%", thickness=1, color=HexColor('#2980b9'))

def make_table(data, col_widths, header_color='#1a5276'):
    """创建带样式的表格"""
    t = Table(data, colWidths=col_widths)
    style_cmds = [
        ('FONTNAME', (0, 0), (-1, -1), 'SimSun'),
        ('FONTSIZE', (0, 0), (-1, -1), 9),
        ('VALIGN', (0, 0), (-1, -1), 'MIDDLE'),
        ('GRID', (0, 0), (-1, -1), 0.5, HexColor('#bdc3c7')),
        ('TOPPADDING', (0, 0), (-1, -1), 5),
        ('BOTTOMPADDING', (0, 0), (-1, -1), 5),
        ('LEFTPADDING', (0, 0), (-1, -1), 6),
        ('RIGHTPADDING', (0, 0), (-1, -1), 6),
        ('TEXTCOLOR', (0, 0), (-1, -1), HexColor('#2c3e50')),
    ]
    if header_color:
        style_cmds += [
            ('BACKGROUND', (0, 0), (-1, 0), HexColor(header_color)),
            ('TEXTCOLOR', (0, 0), (-1, 0), HexColor('#ffffff')),
            ('FONTNAME', (0, 0), (-1, 0), 'SimHei'),
        ]
    t.setStyle(TableStyle(style_cmds))
    return t

# ---------- 构建文档 ----------
OUTPUT = "Codex_概述.pdf"

doc = SimpleDocTemplate(OUTPUT, pagesize=A4,
                        leftMargin=25*mm, rightMargin=25*mm,
                        topMargin=20*mm, bottomMargin=20*mm,
                        title='Agent Skills 使用指南',
                        author='Skills Guide')

story = []

# ========== 封面 ==========
story.append(Spacer(1, 50*mm))
story.append(Paragraph("Agent Skills", styles['CNTitle']))
story.append(Spacer(1, 6*mm))
story.append(Paragraph("实战使用指南",
    ParagraphStyle('Sub', parent=styles['CNTitle'], fontSize=16, leading=22,
                   textColor=HexColor('#2980b9'))))
story.append(Spacer(1, 12*mm))
story.append(Paragraph("如何用 Skill 将 AI 编程助手变成专家团队",
    ParagraphStyle('Tagline', fontName='KaiTi', fontSize=11, leading=18,
                   textColor=HexColor('#7f8c8d'), alignment=TA_CENTER)))
story.append(Spacer(1, 20*mm))
story.append(Paragraph("2026 年 5 月  |  v1.0",
    ParagraphStyle('Date', fontName='SimSun', fontSize=9, leading=12,
                   textColor=HexColor('#95a5a6'), alignment=TA_CENTER)))
story.append(PageBreak())

# ========== 第一章：认识 Skill ==========
story.append(h1("一、认识 Skill"))
story.append(hr())
story.append(body(
    "如果你用过的 AI 编程助手感觉\"还行但不够好\"，大概率是因为它没有加载 Skill。"
    "Skill 之于 AI，就像 App 之于手机——没有 App 的手机只能打电话发短信，"
    "没有 Skill 的 AI 只能做最基本的代码补全。"))
story.append(body(
    "<b>Agent Skill（智能体技能）</b> 是一种可安装的专业化能力模块。每个 Skill 封装了"
    "特定领域的知识、工作流程和工具脚本——PDF 处理、幻灯片制作、Bug 调试、代码审查……"
    "当你的任务匹配到某个 Skill 时，AI 会自动加载对应的\"专家大脑\"，以最佳方式完成任务。"))
story.append(body(
    "Skill 系统的核心设计理念：<b>按需加载，用完即走</b>。不需要一次性把所有知识塞进提示词"
    "（那样既烧 Token 又降低质量），而是在正确的时刻激活正确的专家。"))

story.append(h2("Skill 的四大价值"))
values = [
    ["价值", "说明"],
    ["专业化", "每个 Skill 在特定领域深耕，包含行业最佳实践"],
    ["按需激活", "只在匹配任务时加载，不占用不必要的上下文窗口"],
    ["可共享", "通过 GitHub 分发，团队共用一套 Skill 体系"],
    ["跨平台", "同一套 Skill 可用于 Claude Code、Codex、Copilot CLI 等"],
]
story.append(make_table(values, [28*mm, 112*mm]))
story.append(Spacer(1, 4*mm))
story.append(PageBreak())

# ========== 第二章：Skill 怎么工作 ==========
story.append(h1("二、Skill 怎么工作"))
story.append(hr())
story.append(body(
    "每个 Skill 在磁盘上就是一个 Markdown 文件（SKILL.md），结构极其简单："))
story.append(body(
    "<b>1. YAML Frontmatter（元数据）</b>——声明 name（名称）和 description（触发条件描述）。"
    "description 是整个 Skill 最关键的一行，AI 系统通过它来判断\"这个 Skill 是否适用于当前任务\"。"))
story.append(body(
    "<b>2. Markdown 正文</b>——包含该领域的完整工作流程、工具说明、代码模板等。"
    "可以附带 scripts/（脚本）、templates/（模板）、references/（参考文档）等配套资源目录。"))

story.append(h2("触发加载流程"))
story.append(body(
    "整个过程对用户几乎透明，你只需要正常提问："))

flow_data = [
    ["步骤", "发生什么"],
    ["1. 你提问", "\"帮我写一份周报 PPT\""],
    ["2. 系统匹配", "检测到 PPT 相关任务 → 匹配 pptx Skill"],
    ["3. 加载 Skill", "Skill 工具把完整 SKILL.md 内容注入 AI 上下文"],
    ["4. AI 执行", "AI 按照 Skill 指导的专业流程完成 PPT 制作"],
]
story.append(make_table(flow_data, [16*mm, 124*mm]))
story.append(Spacer(1, 4*mm))
story.append(quote(
    "\"即使只有 1% 的可能 Skill 适用，也必须先激活 Skill 再行动。"
    "这是不可协商的规则。\" —— Superpowers 核心原则"))
story.append(PageBreak())

# ========== 第三章：上手实战 ==========
story.append(h1("三、上手实战"))
story.append(hr())
story.append(body(
    "理论够了，来看三个真实场景，看看 Skill 在每一步是怎么干活的。"))

# --- 场景 1 ---
story.append(h2("场景 1：写一份 PPT"))
story.append(body("<b>你说：</b>\"帮我做一份 Q2 季度汇报 PPT。\""))
story.append(body(
    "→ 系统自动激活 <b>brainstorming</b> Skill：先搞清楚 PPT 的受众是谁、要覆盖哪些数据、什么风格。"
    "不会一上来就写代码，而是先问清楚需求。"))
story.append(body(
    "→ 设计确认后，激活 <b>pptx</b> Skill：按照专业 PPT 制作流程生成 .pptx 文件，"
    "包含封面、目录、数据图表、总结页。"))

# --- 场景 2 ---
story.append(h2("场景 2：改一个 Bug"))
story.append(body("<b>你说：</b>\"这个登录功能报 500 错误，帮我看看。\""))
story.append(body(
    "→ 系统自动激活 <b>systematic-debugging</b> Skill：不是瞎猜，而是走系统化调试流程——"
    "复现 Bug → 看日志 → 定位根因 → 写测试复现 → 修复 → 验证。"))
story.append(body(
    "→ 如果 Bug 涉及数据库操作，还可能激活 <b>claude-api</b> 或其他领域 Skill 辅助排查。"))

# --- 场景 3 ---
story.append(h2("场景 3：审查代码"))
story.append(body("<b>你说：</b>\"我刚写完这个模块，帮我看看有没有问题。\""))
story.append(body(
    "→ 激活 <b>requesting-code-review</b> Skill：系统逐文件审查代码质量、安全性、"
    "性能、可维护性，给出结构化反馈。"))
story.append(body(
    "→ 你收到 Review 意见后，激活 <b>receiving-code-review</b> Skill："
    "不是每条意见都照改——先做技术验证，有道理的就改，存疑的就讨论。"))

story.append(Spacer(1, 6*mm))
story.append(body(
    "<b>关键要点：</b>每个场景都是\"流程 Skill 先上 → 领域 Skill 后上\"。"
    "先决定<b>怎么做</b>（brainstorming / debugging），再决定<b>做什么</b>（pptx / 具体代码）。"))
story.append(PageBreak())

# ========== 第四章：构建自己的 Skill ==========
story.append(h1("四、构建自己的 Skill"))
story.append(hr())
story.append(body(
    "除了使用现成的 Skill，你还可以把自己团队的专属工作流封装成 Skill。"
    "整个过程由 <b>skill-creator</b> Skill 指导。"))

story.append(h2("六步创建法"))

steps_data = [
    ["阶段", "做什么"],
    ["1. 捕捉意图", "明确 Skill 要解决什么问题，边界在哪"],
    ["2. 编写草稿", "用 Markdown 写 SKILL.md，Frontmatter 写好 name 和 description"],
    ["3. 创建测试", "写几个典型提示词，在启用 Skill 的情况下跑一遍"],
    ["4. 评估结果", "检查输出质量——Skill 是否被正确触发？生成的代码/文档是否符合预期？"],
    ["5. 迭代优化", "根据评估反复修改 SKILL.md，直到效果满意"],
    ["6. 扩展测试", "用更多样化的输入验证 Skill 的稳定性和触发准确率"],
]
story.append(make_table(steps_data, [26*mm, 114*mm]))
story.append(Spacer(1, 4*mm))

story.append(h2("SKILL.md 文件模板"))
story.append(body(
    "最简单的 Skill 就长这样："))

template = (
    "---\n"
    "name: my-skill\n"
    "description: 当用户需要做 XX 时使用此 Skill。\n"
    "---\n\n"
    "# 技能名称\n\n"
    "## 工作流程\n\n"
    "1. 步骤一：做什么\n"
    "2. 步骤二：做什么\n"
    "3. 步骤三：做什么\n\n"
    "## 注意事项\n\n"
    "- 重要规则\n"
    "- 常见陷阱"
)
story.append(Paragraph(
    f'<pre>{template}</pre>',
    ParagraphStyle('CodeBlock', fontName='Courier', fontSize=7.5, leading=10,
                   textColor=HexColor('#2c3e50'), backColor=HexColor('#f4f6f8'),
                   leftIndent=8, rightIndent=8, spaceAfter=8,
                   borderWidth=0.5, borderColor=HexColor('#d5dbdb'), borderPadding=6)))

story.append(h2("写好 description 的诀窍"))
story.append(body(
    "description 决定了 Skill 什么时候被触发。写得太宽泛会误触发，写得太窄会不触发。"
    "好例子：\"当用户要求创建、读取、编辑 Word 文档（.docx）时使用...\""
    "—— 有明确的触发词（.docx / Word 文档）和排除条件（非 PDF / 非 Google Docs）。"))
story.append(PageBreak())

# ========== 第五章：常见问题 & 进阶技巧 ==========
story.append(h1("五、常见问题 &amp; 进阶技巧"))
story.append(hr())

story.append(h2("Q: Skill 没有被触发？"))
story.append(body(
    "检查三点：(1) 你的提问是否包含 Skill description 中的关键词；"
    "(2) description 是否写得不够精确导致匹配不上；"
    "(3) Skill 文件路径是否在系统的扫描范围内。"))

story.append(h2("Q: 多个 Skill 同时匹配怎么办？"))
story.append(body(
    "<b>流程型优先于实现型</b>。brainstorming、debugging、writing-plans 这类\"决定方法\""
    "的 Skill 总是最先加载。然后才是 pdf、pptx、frontend-design 这类\"执行工作\"的 Skill。"
    "如果同一类型的多个 Skill 都匹配，选最精准的那个。"))

story.append(h2("Q: 流程型 vs 实现型怎么区分？"))
story.append(body(
    "<b>流程型：</b>定义\"怎么做事\"（brainstorming、debugging、TDD）。先激活。"
    "<br/><b>实现型：</b>定义\"怎么做出具体东西\"（pdf、pptx、frontend-design）。后激活。"
    "<br/>记一个口诀：<b>\"先问怎么干，再问干出啥\"</b>。"))

story.append(h2("Q: 如何保持 Skill 最新？"))
story.append(body(
    "Skill 会持续演进。如果安装来源是 GitHub 仓库，定期 git pull 即可。"
    "也可以用 skills-lock.json 锁定版本，确保团队环境一致。"))

story.append(Spacer(1, 10*mm))
story.append(h2("进阶：必知的 6 个核心 Skill"))

core_data = [
    ["Skill", "用途", "何时必用"],
    ["using-superpowers", "Skill 系统入口", "每次对话启动"],
    ["brainstorming", "需求探索与设计", "任何创造性工作前"],
    ["systematic-debugging", "系统化调试", "遇到任何 Bug"],
    ["test-driven-development", "测试驱动开发", "实现功能前"],
    ["writing-plans", "任务规划", "多步骤任务前"],
    ["verification-before-completion", "完工验证", "声称\"完成\"前"],
]
story.append(make_table(core_data, [38*mm, 54*mm, 48*mm]))
story.append(Spacer(1, 6*mm))

# ========== 结尾 ==========
story.append(Spacer(1, 12*mm))
story.append(hr())
story.append(Spacer(1, 4*mm))
story.append(Paragraph(
    "Agent Skill 系统是将 AI 从\"通用助手\"升级为\"专家团队\"的关键基础设施。"
    "每个 Skill 都可能为你省下数小时的试错时间。善用 Skill，让 AI 真正成为你的生产力倍增器。",
    styles['CNBody']))
story.append(Spacer(1, 10*mm))
story.append(Paragraph("—— 全文完 ——", styles['CNFooter']))

# ---------- 生成 ----------
doc.build(story)
print(f"PDF 已生成: {OUTPUT}")
```

- [ ] **Step 2: 运行脚本生成 PDF**

```powershell
& "C:\Users\14680\AppData\Local\Programs\Python\Python314\python.exe" "E:\claude\output\generate_skills_pdf.py"
```

Expected: 输出 `PDF 已生成: Codex_概述.pdf`，无报错。

- [ ] **Step 3: 验证 PDF 内容**

```powershell
& "C:\Users\14680\AppData\Local\Programs\Python\Python314\python.exe" -c "
from pypdf import PdfReader
reader = PdfReader('E:/claude/Codex_概述.pdf')
print(f'页数: {len(reader.pages)}')
for i, page in enumerate(reader.pages):
    text = page.extract_text() or ''
    if text.strip():
        print(f'--- 第{i+1}页 ({len(text)}字) ---')
        print(text[:200])
        print()
"
```

Expected: 5-10 页，中文无乱码，5 章标题完整。

- [ ] **Step 4: 确认文件大小合理**

```powershell
(Get-Item -LiteralPath "E:\claude\Codex_概述.pdf").Length
```

Expected: 50-200 KB。

- [ ] **Step 5: Commit**

```powershell
git add "E:\claude\output\generate_skills_pdf.py" "E:\claude\Codex_概述.pdf"
git commit -m "add: Agent Skills 使用指南 PDF（Skill 范式重写）"
```
