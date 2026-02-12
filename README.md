# 经济学博士论文 Claude Code 工作流

> **项目**: 论文1 - 社会保障与养老金研究
> **学校**: 东北财经大学
> **工具**: Stata + Fortran + Word + LaTeX (幻灯片)
> **方法**: DID + 结构估计

基于 Claude Code 的经济学论文研究工作流。规划、执行、审查、验证——全自动化。

---

## 快速开始

### 1. 克隆仓库

```bash
git clone https://github.com/YOUR_USERNAME/claude-code-my-workflow.git dissertation
cd dissertation
```

### 2. 启动 Claude Code

```bash
claude
```

或在 VS Code 中打开 Claude Code 面板。

### 3. 开始工作

描述你的任务，Claude 会：
1. 进入计划模式，制定详细方案
2. 等待你批准
3. 自主执行：编写代码 → 审查质量 → 修复问题 → 验证结果
4. 呈现总结

---

## 工作流程

### 承包商模式

你描述任务 → Claude 规划 → 你批准 → Claude 自主完成

**示例任务**:
- "用 Stata 做 DID 分析，检验平行趋势，输出回归表"
- "写 Fortran 程序求解生命周期模型，读取 Stata 数据"
- "审查我的手稿结构和论证逻辑"
- "搜索养老金与储蓄的最新文献并综合"

### 专业代理

- **stata-reviewer**: Stata 代码质量审查（可复现性、DID 方法、代码风格）
- **fortran-reviewer**: Fortran 数值代码审查（现代化、数值稳定性、算法正确性）
- **proofreader**: 手稿语言和格式审查
- **domain-reviewer**: 经济学方法和实质内容审查

### 质量门槛

| 分数 | 门槛 | 含义 |
|------|------|------|
| 80 | 提交 | 可以保存 |
| 90 | PR | 准备发表 |
| 95 | 卓越 | 理想目标 |

---

## 项目结构

```
dissertation/
├── CLAUDE.md              # 项目配置（Claude 每次都读）
├── .claude/               # 规则、技能、代理
├── Papers/                # 论文
│   └── Paper1/
│       ├── manuscript.docx    # Word 主稿件
│       ├── slides.tex         # Beamer 幻灯片
│       └── figures/tables/    # 图表
├── Code/
│   ├── Stata/             # 数据分析
│   ├── Fortran/           # 数值求解
│   ├── Matlab/            # 可选
│   └── R/                 # 可选（图表）
├── Data/
│   ├── Raw/               # 原始数据（只读）
│   ├── Cleaned/           # 清洗后数据
│   └── Results/           # 结果（.dta, .csv）
├── Figures/               # 共享图
├── Tables/                # 共享表
└── explorations/          # 研究沙盒（快速原型）
```

---

## 核心功能

### 1. Stata 分析

**特性**:
- 可复现性检查（随机种子、相对路径、日志）
- DID 方法检查（平行趋势、稳健性、异质性）
- 代码风格审查
- 自动生成 LaTeX 表格

**示例**:
```stata
* Claude 会帮你写这样的代码:
set seed 20260212
global data "Data"

log using "Code/Stata/logs/01_clean.log", replace

use "$data/Raw/pension.dta", clear

* 平行趋势检验（事件研究）
reghdfe outcome time_*, absorb(id year) vce(cluster id)
coefplot, keep(time_*) yline(0)

* DID 估计
reghdfe outcome treated_post, absorb(id year) vce(cluster id)

* 输出表格
esttab using "Tables/did_results.tex", replace
```

### 2. Fortran 数值求解

**特性**:
- 现代 Fortran 90/95 语法
- 数值稳定性检查（收敛诊断、NaN 检测）
- 与 Stata 数据接口（CSV）
- 结构估计算法验证

**示例**:
```fortran
! Claude 会帮你写这样的代码:
module pension_model
    implicit none
    real(8), parameter :: beta = 0.96_8

contains
    subroutine solve_value_function(V, tol, maxiter)
        real(8), intent(inout) :: V(:)
        real(8), intent(in) :: tol
        integer, intent(in) :: maxiter

        integer :: iter
        real(8) :: diff, V_new(size(V))

        do iter = 1, maxiter
            call update_value(V, V_new)
            diff = maxval(abs(V_new - V))
            V = V_new

            if (diff < tol) exit
        end do

        if (diff >= tol) write(*,*) "警告: 未收敛"
    end subroutine
end module
```

### 3. Word 手稿审查

**特性**:
- 结构检查（引言、文献、方法、结果、结论）
- DID/结构估计特定内容检查
- 格式规范（图表、公式、引用）
- 语言质量审查

### 4. 文献综述

**技能**: `/lit-review [topic]`

自动搜索最新文献、提取关键发现、识别研究缺口。

---

## 常用命令

```bash
# Stata (批处理)
stata -b do Code/Stata/01_clean.do

# Fortran (编译和运行)
cd Code/Fortran && make && ./structural_model

# LaTeX 幻灯片
cd Papers/Paper1 && xelatex slides.tex

# Word/LaTeX 互转
pandoc manuscript.docx -o manuscript.tex
pandoc manuscript.tex -o manuscript.docx --bibliography=../../Bibliography.bib
```

---

## 技能列表

| 技能 | 功能 |
|------|------|
| `/lit-review [topic]` | 文献搜索与综合 |
| `/research-ideation [topic]` | 研究问题生成 |
| `/interview-me [topic]` | 交互式研究访谈 |
| `/review-paper [file]` | 手稿审阅 |
| `/validate-bib` | 文献交叉检查 |
| `/commit [msg]` | 暂存、提交、PR |

*注: 技能会根据项目需求持续添加*

---

## DID 和结构估计重点

### DID 必做检验
- ✅ 平行趋势检验（事件研究图）
- ✅ 稳健性检验（控制组、时间窗口、聚类）
- ✅ 异质性分析
- ✅ 处理交错 DID（如适用）

### 结构估计必做步骤
- ✅ 模型设定清晰
- ✅ 参数识别说明
- ✅ 数值收敛诊断
- ✅ 模型拟合验证
- ✅ 反事实分析
- ✅ Bootstrap 标准误

---

## 前置要求

| 工具 | 用途 | 安装 |
|------|------|------|
| [Claude Code](https://code.claude.com) | 一切 | `npm install -g @anthropic-ai/claude-code` |
| Stata | 数据分析 | 商业软件 |
| gfortran | Fortran 编译 | `brew install gcc` (macOS) |
| XeLaTeX | 幻灯片 | [TeX Live](https://tug.org/texlive/) |
| pandoc | Word/LaTeX 转换 | [pandoc.org](https://pandoc.org) |
| R (可选) | 图表 | [r-project.org](https://www.r-project.org) |

---

## 研究沙盒

`explorations/` 文件夹用于快速原型：
- 降低质量门槛（60/100）
- 简化审查流程
- 成熟后"毕业"到主代码库

详见 `.claude/rules/exploration-folder-protocol.md`

---

## 工作流原则

1. **先规划后执行** - 非琐碎任务进入计划模式
2. **执行后验证** - 编译/运行确认输出
3. **单一真实来源** - 避免重复，保持溯源
4. **质量门槛** - 低于 80 不提交
5. **学习记忆** - 纠正后保存到 MEMORY.md

---

## 自定义

### 1. 填写知识库
编辑 `.claude/rules/econometrics-knowledge-base.md` 添加你的：
- 特定数据集约定
- 常用变量定义
- 领域特定陷阱

### 2. 调整代理
编辑 `.claude/agents/domain-reviewer.md` 自定义经济学审查维度

### 3. 更新 MEMORY.md
记录项目特定的决策和纠正：
```
[LEARN:Stata] 我的数据集使用 person_id 而非 id
[LEARN:模型] 贴现因子 β 默认为 0.96
```

---

## 来源

改编自 [pedrohcgs/claude-code-my-workflow](https://github.com/pedrohcgs/claude-code-my-workflow)，原为 Beamer/Quarto 讲义工作流，现适配经济学论文研究。

---

## 许可

MIT License. 自由用于研究和学习。
