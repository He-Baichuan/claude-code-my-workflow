---
paths:
  - "**/*.do"
  - "Code/Stata/**"
---

# Stata 编程规范

**标准**: 高级数据分析师 + 博士研究员质量

---

## 1. 可复现性

### 必须项
- `set seed YYYYMMDD` 在文件顶部调用一次（使用日期格式）
- 所有路径相对于仓库根目录
- 每个 do 文件生成日志文件
- 明确的运行顺序（文件命名编号）

### 日志文件模式
```stata
* 开始日志
log using "Code/Stata/logs/01_data_cleaning.log", replace text

* [你的代码...]

* 关闭日志
log close
```

### 路径管理
```stata
* 设置项目根目录（相对路径）
global root "."
global data "$root/Data"
global code "$root/Code/Stata"
global output "$root/Data/Results"

* 使用相对路径
use "$data/Raw/pension_data.dta", clear
save "$data/Cleaned/analysis_ready.dta", replace
```

---

## 2. 文件结构

### 标准模板
```stata
/*******************************************************************************
* 文件名: 01_data_cleaning.do
* 作者: [姓名]
* 日期: YYYY-MM-DD
* 目的: 清洗养老保险参保数据
* 输入: Data/Raw/pension_data.dta
* 输出: Data/Cleaned/analysis_ready.dta
*******************************************************************************/

* 初始化
clear all
set more off
set seed 20260212

* 设置路径
global root "."
global data "$root/Data"

* 开始日志
log using "$root/Code/Stata/logs/01_data_cleaning.log", replace text

* 加载数据
use "$data/Raw/pension_data.dta", clear

* [数据清洗步骤...]

* 保存输出
save "$data/Cleaned/analysis_ready.dta", replace

* 关闭日志
log close
```

### 文件命名
- `01_描述性名称.do` - 按执行顺序编号
- `99_master.do` - 主控制脚本（按顺序运行所有脚本）

---

## 3. 代码风格

### 命名规范
- **变量名**: `snake_case`（如 `pension_enroll`, `age_group`）
- **全局宏**: `$ALLCAPS` 或 `$snake_case`
- **局部宏**: `snake_case`

### 注释
```stata
* 单行注释用星号

/*
   多行注释用这种格式
   解释复杂逻辑
*/

// 不推荐使用双斜杠（某些版本不支持）
```

### 代码块分隔
```stata
*===============================================================================
* 第一部分: 数据清洗
*===============================================================================

* --- 子部分: 处理缺失值 ---
[代码...]

* --- 子部分: 变量重编码 ---
[代码...]

*===============================================================================
* 第二部分: 描述统计
*===============================================================================
```

---

## 4. DID 分析规范

### 平行趋势检验
```stata
* 事件研究设计 - 检验平行趋势
gen rel_time = year - treatment_year
replace rel_time = -5 if rel_time < -5  // 截断前期
replace rel_time = 5 if rel_time > 5    // 截断后期

* 生成时期虚拟变量（省略 t=-1 作为基准期）
qui tab rel_time, gen(time_)
forval i = 1/10 {
    local t = `i' - 6  // 调整为 -5 到 +5
    if `t' != -1 {
        rename time_`i' time_`t'
    }
    else {
        drop time_`i'  // 删除基准期
    }
}

* 估计事件研究模型
reghdfe outcome time_* controls, absorb(id year) vce(cluster id)

* 绘制事件研究图
coefplot, keep(time_*) vertical yline(0) ///
    title("事件研究: 平行趋势检验") ///
    xtitle("相对时间（年）") ytitle("处理效应")
```

### DID 估计
```stata
* 标准 DID
reghdfe outcome i.treated##i.post controls, ///
    absorb(id year) vce(cluster id)

* 报告结果
estimates store did_baseline

* 稳健性: 不同控制变量
reghdfe outcome i.treated##i.post alt_controls, ///
    absorb(id year) vce(cluster id)
estimates store did_robust

* 输出表格
esttab did_baseline did_robust using ///
    "Tables/did_results.tex", replace ///
    se star(* 0.1 ** 0.05 *** 0.01) ///
    keep(_cons) ///
    mtitles("基准" "稳健性")
```

### 常见陷阱
| 陷阱 | 问题 | 预防 |
|------|------|------|
| 未检验平行趋势 | 识别假设可能不满足 | 事件研究图必须先做 |
| 标准误未聚类 | t 统计量偏大 | 使用 `vce(cluster id)` |
| 处理时间异质性 | 经典 TWFE 有偏 | 考虑 `csdid` 等新方法 |

---

## 5. 结构估计配套 Stata 代码

### 准备矩条件数据
```stata
* 计算实际数据矩
collapse (mean) moment1=var1 moment2=var2, by(group)
save "$output/empirical_moments.dta", replace

* 导出为 CSV 供 Fortran 读取
export delimited using "$output/empirical_moments.csv", replace
```

### 读取 Fortran 输出
```stata
* 导入 Fortran 估计结果
import delimited "$output/fortran_estimates.csv", clear

* 计算标准误（如使用 bootstrap）
* [bootstrap 代码...]
```

---

## 6. 表格和图形输出

### 描述统计表
```stata
* 使用 estout 生成 LaTeX 表格
estpost summarize var1 var2 var3 var4
esttab using "Tables/descriptive_stats.tex", replace ///
    cells("mean(fmt(2)) sd(fmt(2)) min max count") ///
    label title("描述统计")
```

### 回归结果表
```stata
* 多个模型并排
esttab model1 model2 model3 using "Tables/regression.tex", ///
    replace se star(* 0.1 ** 0.05 *** 0.01) ///
    keep(_cons var1 var2) ///
    order(var1 var2) ///
    label title("回归结果") ///
    addnotes("注: 聚类稳健标准误在括号中")
```

### 图形
```stata
* 保存高分辨率 PNG（300 DPI）
graph export "Figures/figure1.png", replace width(2400)

* 或导出 PDF
graph export "Figures/figure1.pdf", replace
```

---

## 7. 性能优化

### 大数据集处理
```stata
* 使用 compress 减少内存
compress

* 保留需要的变量
keep id year outcome treatment controls

* 对大型合并使用 merge
merge 1:1 id year using "$data/other.dta", keep(3) nogen
```

### 循环优化
```stata
* 使用 forval 而非 foreach（如果是数字序列）
forval i = 1/100 {
    [代码...]
}

* 使用 levelsof 遍历唯一值
levelsof id, local(ids)
foreach id in `ids' {
    [代码...]
}
```

---

## 8. 常见错误

| 错误 | 原因 | 解决方案 |
|------|------|----------|
| `no observations` | 数据筛选过度 | 检查 if/in 条件 |
| `varlist required` | 变量名拼写错误 | 使用 `describe` 检查 |
| `insufficient memory` | 数据集过大 | 使用 `compress`, `keep` |
| 结果不可复现 | 未设置种子 | `set seed` |

---

## 9. 代码质量检查清单

提交前确认：
- [ ] `set seed` 已设置
- [ ] 所有路径为相对路径
- [ ] 日志文件已启用
- [ ] 变量名清晰有意义
- [ ] 关键步骤有注释
- [ ] 结果可从头到尾复现
- [ ] 表格图形已输出到正确位置
- [ ] 文件头部有清晰文档

---

## 10. 质量门槛评分标准

| 维度 | 优秀 (95+) | 良好 (80-94) | 不足 (<80) |
|------|-----------|-------------|-----------|
| 可复现性 | 完整种子、日志、路径 | 部分遗漏 | 无法复现 |
| 代码风格 | 清晰注释、结构良好 | 基本可读 | 混乱 |
| 结果正确性 | 逻辑正确、无错误 | 小错误 | 重大错误 |
| 文档 | 完整头部、注释充分 | 基本文档 | 无文档 |
