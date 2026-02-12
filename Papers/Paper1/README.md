# Paper1 - 社会保障与养老金研究

## 文件

- `manuscript.docx` - Word 主稿件
- `manuscript.tex` - LaTeX 版本（如需）
- `slides.tex` - Beamer 幻灯片
- `figures/` - 本文专用图
- `tables/` - 本文专用表

## 编译

### Word
直接编辑 `manuscript.docx`

### LaTeX 幻灯片
```bash
cd Papers/Paper1
xelatex slides.tex
bibtex slides
xelatex slides.tex
xelatex slides.tex
```

## 相关代码

- 数据分析: `Code/Stata/`
- 数值求解: `Code/Fortran/`

## 图表索引

| 图/表 | 文件 | 生成脚本 |
|-------|------|----------|
| 图1 | `figures/figure1.png` | `Code/Stata/...` |
| 表1 | `tables/table1.tex` | `Code/Stata/...` |
