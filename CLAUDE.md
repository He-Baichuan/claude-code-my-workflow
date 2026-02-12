# CLAUDE.MD -- 经济学博士论文研究项目

**项目:** 论文1 - 社会保障与养老金研究
**学校:** 东北财经大学
**分支:** main

---

## 核心原则

- **先规划后执行** -- 非琐碎任务前进入计划模式；计划保存至 `quality_reports/plans/`
- **执行后验证** -- 每个任务结束后编译/运行并确认输出
- **单一真实来源** -- 避免内容重复；数据和代码溯源清晰
- **质量门槛** -- 低于 80/100 不提交
- **[LEARN] 标签** -- 被纠正时，保存 `[LEARN:类别] 错误 → 正确` 到 MEMORY.md

---

## 文件夹结构

```
economics-dissertation/
├── CLAUDE.md                    # 本文件
├── .claude/                     # 规则、技能、代理、钩子
├── Bibliography.bib             # 集中式文献库
├── Papers/                      # 各篇论文/章节
│   ├── Paper1/
│   │   ├── manuscript.docx      # Word 主稿件
│   │   ├── manuscript.tex       # LaTeX 版本（如需）
│   │   ├── slides.tex           # Beamer 幻灯片
│   │   ├── figures/             # 本文专用图
│   │   └── tables/              # 本文专用表
│   └── Paper2/
├── Code/                        # 所有分析代码
│   ├── Stata/                   # 数据分析脚本
│   │   ├── 01_data_cleaning.do
│   │   ├── 02_did_analysis.do
│   │   └── utils/               # 通用函数
│   ├── Fortran/                 # 数值求解程序
│   │   ├── structural_model.f90
│   │   └── Makefile
│   ├── Matlab/                  # 数值方法（偶尔使用）
│   └── R/                       # 可选：发表级图表
├── Data/
│   ├── Raw/                     # 原始数据（只读）
│   ├── Cleaned/                 # 清洗后数据
│   └── Results/                 # 分析结果（.dta, .csv, .rds）
├── Figures/                     # 共享图片
├── Tables/                      # 共享表格
├── Templates/                   # LaTeX/Word 模板
├── Preambles/                   # LaTeX 导言
├── explorations/                # 研究沙盒（见规则）
├── quality_reports/             # 计划、会话日志、合并报告
├── scripts/                     # 实用脚本
└── master_supporting_docs/      # 参考文献和现有材料
```

---

## 常用命令

```bash
# Stata (批处理模式，带日志)
stata -b do Code/Stata/01_data_cleaning.do

# Fortran (编译和运行)
cd Code/Fortran && make && ./structural_model

# Matlab (批处理模式)
matlab -nodisplay -nosplash -r "run('Code/Matlab/script.m'); exit"

# LaTeX 幻灯片编译 (XeLaTeX, 3遍)
cd Papers/Paper1 && xelatex slides.tex && bibtex slides && xelatex slides.tex && xelatex slides.tex

# Word 转 LaTeX（如需）
pandoc manuscript.docx -o manuscript.tex

# LaTeX 转 Word
pandoc manuscript.tex -o manuscript.docx --bibliography=../../Bibliography.bib

# 质量评分
python scripts/quality_score.py Papers/Paper1/manuscript.docx
```

---

## 质量门槛

| 分数 | 门槛 | 含义 |
|------|------|------|
| 80 | 提交 | 可以保存 |
| 90 | PR | 可以部署 |
| 95 | 卓越 | 理想目标 |

---

## 技能快速参考

| 命令 | 功能 |
|------|------|
| `/run-stata [script]` | 执行 Stata do 文件并记录日志 |
| `/run-fortran [program]` | 编译运行 Fortran 程序 |
| `/run-matlab [script]` | 执行 Matlab 脚本 |
| `/compile-slides [file]` | 3遍 XeLaTeX + bibtex 编译幻灯片 |
| `/check-replication [script]` | 验证代码可复现结果 |
| `/manuscript-review [file]` | 手稿多维度审查 |
| `/export-to-word [tex]` | LaTeX 转 Word (pandoc) |
| `/table-to-latex [data]` | 生成发表级表格 |
| `/validate-bib` | 交叉引用文献 |
| `/commit [msg]` | 暂存、提交、PR、合并 |
| `/lit-review [topic]` | 文献搜索与综合 |
| `/research-ideation [topic]` | 研究问题与策略生成 |
| `/interview-me [topic]` | 交互式研究访谈 |
| `/review-paper [file]` | 手稿审阅 |
| `/data-analysis [dataset]` | 端到端数据分析 |

---

## Stata 编程约定

| 约定 | 说明 |
|------|------|
| 文件命名 | `01_描述性名称.do` (编号 + 描述) |
| 随机种子 | `set seed YYYYMMDD` 在顶部一次 |
| 日志文件 | 每个 do 文件自动记录日志 |
| 相对路径 | 所有路径相对于仓库根目录 |
| 注释 | 解释"为什么"而非"是什么" |

---

## Fortran 编程约定

| 约定 | 说明 |
|------|------|
| 标准 | Fortran 90/95 现代语法 |
| 模块化 | 使用 module 组织代码 |
| 精度 | 使用 `real(8)` 或 `selected_real_kind(15,307)` |
| 注释 | 算法说明和参数含义 |
| Makefile | 包含编译选项和优化标志 |

---

## Word 手稿标准

| 标准 | 要求 |
|------|------|
| 模板 | 使用期刊/学位论文模板 |
| 引用 | Zotero/Mendeley 管理，或手动编号 |
| 图表 | 嵌入高分辨率图片（300 DPI） |
| 公式 | MathType 或 Word 内置公式编辑器 |
| 追踪修订 | 使用修订模式记录变更 |

---

## LaTeX 幻灯片环境

Beamer 自定义环境（如使用）：

| 环境 | 效果 | 使用场景 |
|------|------|----------|
| `alertblock` | 红色强调块 | 重要结论 |
| `exampleblock` | 绿色示例块 | 案例说明 |
| `block` | 蓝色标准块 | 一般内容 |

*根据您的 Beamer 主题自定义此表*

---

## 计量方法重点

### DID (双重差分)

- **平行趋势检验** - 事前趋势图 + 安慰剂检验
- **稳健性检验** - 不同控制组、时间窗口、聚类标准误
- **异质性分析** - 分组回归、三重差分

### 结构估计

- **参数识别** - 矩条件、排除性约束
- **数值求解** - 精度验证、收敛诊断
- **标准误** - Bootstrap 或渐进方差
- **模型验证** - 模拟匹配实际矩、过度识别检验

---

## 当前项目状态

| 论文 | 主稿件 | 幻灯片 | 代码 | 关键内容 |
|------|--------|--------|------|----------|
| Paper1 | `Papers/Paper1/manuscript.docx` | `Papers/Paper1/slides.tex` | `Code/Stata/01_*.do` | [待填写] |

*随项目进展更新此表*

---

## 数据溯源

每个数据文件应记录：
- **来源** - 原始数据来源（机构、网站、API）
- **获取日期** - 下载或提取日期
- **清洗脚本** - 从 Raw → Cleaned 的脚本
- **变量定义** - 变量字典或 codebook

保存在 `Data/README.md` 或各子文件夹的 README 中。

---

## 可复现性清单

每次提交代码前检查：
- [ ] 随机种子已设置（Stata/Matlab/R）
- [ ] 所有路径相对于仓库根目录
- [ ] 数据文件在 `.gitignore` 中（如过大）
- [ ] README 说明运行顺序
- [ ] 结果可从头到尾重现

---

## 研究沙盒（explorations/）

快速原型和探索性分析使用 `explorations/` 文件夹：
- 降低质量门槛（60/100）
- 简化工作流（不需多轮审查）
- 成熟后"毕业"到主代码库或归档

详见 `.claude/rules/exploration-folder-protocol.md`

---

## 注意事项

- **数据安全** - 敏感数据不上传 GitHub，使用加密或本地存储
- **版本控制** - 大数据文件用 Git LFS 或仅保存清洗脚本
- **计算密集任务** - 结果保存为中间文件，避免重复运行
- **文献管理** - 集中在 `Bibliography.bib`，所有论文共享

---

*这是一个活文档。随着项目演进持续更新。Claude 每次会话都会读取此文件。*
