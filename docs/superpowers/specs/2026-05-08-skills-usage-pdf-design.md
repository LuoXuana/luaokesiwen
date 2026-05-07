# Skills 使用指南 PDF — 设计文档

## 目标

生成一份关于 Agent Skills 使用的 PDF 实战手册，输出为 `E:\claude\Codex_概述.pdf`。

## 定位

**实战手册，非百科全书。** 以用户视角展开，每个章节围绕具体场景。

## 结构（5 章）

### 1. 认识 Skill
- 核心问题：为什么装了 Skill 的 AI 和没装的是两个东西？
- 内容：Skill 是什么（类比：给 AI 装专家应用）、核心价值（专业/按需/共享/跨平台）
- 篇幅：约 1 页

### 2. Skill 怎么工作
- 核心问题：触发→加载→执行，背后发生了什么？
- 内容：SKILL.md 文件结构、Frontmatter 触发机制、加载流程
- 篇幅：约 1 页

### 3. 上手实战
- 核心问题：3 个场景串讲，每一步哪个 Skill 在干活
- 场景 1：写 PPT（brainstorming → pptx）
- 场景 2：改 Bug（systematic-debugging → 相关领域 Skill）
- 场景 3：审代码（requesting-code-review → receiving-code-review）
- 篇幅：约 2-3 页

### 4. 构建自己的 Skill
- 核心问题：从想法到可用 Skill 的完整流程
- 内容：skill-creator 六步法、Skill 文件模板、高质量 Skill 要点
- 篇幅：约 1-2 页

### 5. 常见问题 & 进阶技巧
- Skill 没触发怎么办？
- 多个 Skill 冲突怎么办？
- 流程型 vs 实现型的区分
- 保持 Skill 更新
- 篇幅：约 1 页

## 技术约束（严格 PDF Skill 范式）

- 只用 `SimpleDocTemplate` + `story` 列表
- 样式基于 `getSampleStyleSheet()` 微调（fontName、fontSize）
- 只用 `Paragraph`、`Spacer`、`PageBreak`、`Table`、`HRFlowable`
- 不用自定义 PageTemplate / canvas 装饰
- 字体：SimHei（粗体）、SimSun（正文）、KaiTi（引用）
- 目标页数：8-10 页 A4

## 文件组织

- 生成脚本：`E:\claude\output\generate_skills_pdf.py`（全新文件，不基于旧脚本）
- 输出 PDF：`E:\claude\Codex_概述.pdf`

## 验收标准

- [ ] PDF 可正常打开，中文无乱码
- [ ] 5 章内容完整，风格实战导向
- [ ] 生成脚本只使用 PDF Skill 中示范的模式
- [ ] 代码清晰，无自定义 PageTemplate 等高级 hack
