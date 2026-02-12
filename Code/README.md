# 代码文件夹

## 运行顺序

### 数据清洗和分析 (Stata)
1. `Stata/01_data_cleaning.do` - 数据清洗
2. `Stata/02_descriptive_stats.do` - 描述统计
3. `Stata/03_did_analysis.do` - DID 分析
4. `Stata/04_robustness.do` - 稳健性检验

### 数值求解 (Fortran)
1. `Fortran/structural_model.f90` - 结构模型求解
   - 编译: `cd Fortran && make`
   - 运行: `./structural_model`

### 可选工具
- **Matlab/**: 数值方法（偶尔使用）
- **R/**: 发表级图表生成（可选）

## 编程规范

详见：
- `.claude/rules/stata-conventions.md`
- `.claude/rules/fortran-conventions.md`

## 可复现性

所有脚本应：
- 使用相对路径
- 设置随机种子
- 记录运行日志
- 可从头到尾运行
