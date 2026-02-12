---
paths:
  - "**/*.f90"
  - "**/*.f95"
  - "**/*.f"
  - "Code/Fortran/**"
---

# Fortran 编程规范

**标准**: 现代科学计算 + 数值分析质量

---

## 1. 语言标准

### 使用 Fortran 90/95 现代语法
```fortran
! 推荐: 现代声明
real(8) :: x, y, z
integer :: i, j, k

! 避免: 老式声明
real*8 x, y, z
integer i, j, k
```

### 精度控制
```fortran
! 方法 1: 显式字节数
integer, parameter :: dp = selected_real_kind(15, 307)
real(dp) :: x

! 方法 2: 简化（双精度）
real(8) :: x

! 一致性: 项目中选定一种方法并坚持使用
```

---

## 2. 模块化结构

### 使用 module 组织代码
```fortran
module pension_model
    implicit none
    private
    public :: solve_pension_problem, utility_function

    ! 模块级参数
    integer, parameter :: dp = 8
    real(dp), parameter :: beta = 0.96_dp  ! 贴现因子

contains

    function utility_function(c) result(u)
        real(dp), intent(in) :: c
        real(dp) :: u

        u = log(c)
    end function utility_function

    subroutine solve_pension_problem(param, result)
        real(dp), intent(in) :: param(:)
        real(dp), intent(out) :: result(:)

        ! [求解逻辑...]
    end subroutine solve_pension_problem

end module pension_model
```

### 主程序
```fortran
program main
    use pension_model
    implicit none

    real(8), allocatable :: parameters(:), results(:)

    ! [初始化和调用...]

end program main
```

---

## 3. 变量声明和意图

### 始终使用 implicit none
```fortran
program main
    implicit none  ! 强制显式声明所有变量

    real(8) :: x, y
    integer :: i

end program main
```

### 子程序参数意图
```fortran
subroutine compute_welfare(c, a, u)
    ! 清晰标注参数意图
    real(8), intent(in) :: c, a      ! 输入
    real(8), intent(out) :: u        ! 输出

    u = log(c) - 0.5_8 * a**2
end subroutine compute_welfare
```

---

## 4. 数值方法最佳实践

### 迭代求解模板
```fortran
subroutine value_function_iteration(V, policy, tol, maxiter)
    real(8), intent(inout) :: V(:)
    real(8), intent(out) :: policy(:)
    real(8), intent(in) :: tol
    integer, intent(in) :: maxiter

    integer :: iter
    real(8) :: diff, V_new(size(V))

    do iter = 1, maxiter
        ! 更新价值函数
        call update_value(V, V_new)

        ! 计算误差
        diff = maxval(abs(V_new - V))
        V = V_new

        ! 检查收敛
        if (diff < tol) then
            write(*,*) "收敛于第", iter, "次迭代，误差 =", diff
            exit
        end if

        ! 报告进度（每100次迭代）
        if (mod(iter, 100) == 0) then
            write(*,*) "迭代", iter, "误差 =", diff
        end if
    end do

    if (diff >= tol) then
        write(*,*) "警告: 未收敛！最终误差 =", diff
    end if

    ! 计算策略函数
    call compute_policy(V, policy)
end subroutine value_function_iteration
```

### 数值精度验证
```fortran
! 检查数值稳定性
if (abs(x) < 1.0e-10_8) then
    write(*,*) "警告: 数值接近零"
end if

! 检查 NaN 和 Inf
if (x /= x) then
    write(*,*) "错误: 检测到 NaN"
    stop
end if
```

---

## 5. 结构估计工作流

### 读取 Stata 数据（通过 CSV）
```fortran
subroutine read_empirical_moments(filename, moments)
    character(len=*), intent(in) :: filename
    real(8), intent(out) :: moments(:)

    integer :: unit, ios, i
    character(len=256) :: line

    open(newunit=unit, file=filename, status='old', action='read')

    ! 跳过表头
    read(unit, *)

    ! 读取数据
    do i = 1, size(moments)
        read(unit, *, iostat=ios) moments(i)
        if (ios /= 0) exit
    end do

    close(unit)
end subroutine read_empirical_moments
```

### 输出结果供 Stata 使用
```fortran
subroutine write_estimates(filename, estimates, se)
    character(len=*), intent(in) :: filename
    real(8), intent(in) :: estimates(:), se(:)

    integer :: unit, i

    open(newunit=unit, file=filename, status='replace')

    ! 写入表头
    write(unit, '(A)') "parameter,estimate,std_error"

    ! 写入数据
    do i = 1, size(estimates)
        write(unit, '(A,I0,2(",",F12.6))') "param_", i, estimates(i), se(i)
    end do

    close(unit)
end subroutine write_estimates
```

### 矩估计框架
```fortran
function objective_function(theta, emp_moments) result(obj)
    real(8), intent(in) :: theta(:)      ! 参数
    real(8), intent(in) :: emp_moments(:) ! 实际矩
    real(8) :: obj

    real(8) :: sim_moments(size(emp_moments))

    ! 模拟矩
    call simulate_model(theta, sim_moments)

    ! 计算目标函数（如 GMM）
    obj = sum((sim_moments - emp_moments)**2)
end function objective_function
```

---

## 6. Makefile 模板

```makefile
# Fortran 编译器
FC = gfortran

# 编译选项
FFLAGS = -O3 -march=native -ffast-math -Wall
DEBUG_FLAGS = -g -fbounds-check -fbacktrace

# 目标文件
TARGET = structural_model

# 源文件
SOURCES = pension_model.f90 main.f90

# 对象文件
OBJECTS = $(SOURCES:.f90=.o)

# 默认目标
all: $(TARGET)

# 编译可执行文件
$(TARGET): $(OBJECTS)
	$(FC) $(FFLAGS) -o $@ $^

# 编译对象文件
%.o: %.f90
	$(FC) $(FFLAGS) -c $<

# 调试版本
debug: FFLAGS = $(DEBUG_FLAGS)
debug: $(TARGET)

# 清理
clean:
	rm -f $(TARGET) $(OBJECTS) *.mod

# 运行
run: $(TARGET)
	./$(TARGET)

.PHONY: all clean run debug
```

---

## 7. 注释和文档

### 文件头部
```fortran
!===============================================================================
! 文件名: structural_model.f90
! 作者: [姓名]
! 日期: YYYY-MM-DD
! 目的: 求解养老金结构模型
!
! 输入: Data/Results/empirical_moments.csv
! 输出: Data/Results/fortran_estimates.csv
!
! 方法: 值函数迭代 + Nelder-Mead 优化
!===============================================================================
```

### 函数/子程序文档
```fortran
!-------------------------------------------------------------------------------
! 函数: solve_pension_problem
!
! 目的: 求解家庭养老金最优化问题
!
! 参数:
!   beta  (in)  - 贴现因子 (0 < beta < 1)
!   gamma (in)  - 风险规避系数 (gamma > 0)
!   V     (out) - 值函数向量
!
! 算法: 值函数迭代直至收敛（容差 1e-6）
!-------------------------------------------------------------------------------
subroutine solve_pension_problem(beta, gamma, V)
    [代码...]
end subroutine solve_pension_problem
```

---

## 8. 常见陷阱

| 陷阱 | 问题 | 预防 |
|------|------|------|
| 数组越界 | 段错误 | 使用 `-fbounds-check` 调试 |
| 未初始化变量 | 结果随机 | 显式初始化所有变量 |
| 整数除法 | `3/2 = 1` | 使用 `3.0_8/2.0_8` |
| 精度损失 | 混用单/双精度 | 统一使用 `real(8)` |
| 未收敛 | 无限循环 | 设置 `maxiter` 上限 |

---

## 9. 性能优化

### 编译器优化
```bash
# 基本优化
gfortran -O2 -o program source.f90

# 激进优化（发布版本）
gfortran -O3 -march=native -ffast-math -o program source.f90

# 调试版本
gfortran -g -fbounds-check -fbacktrace -o program source.f90
```

### 循环优化
```fortran
! 推荐: 列优先访问（Fortran 默认）
do j = 1, n
    do i = 1, m
        A(i, j) = A(i, j) + B(i, j)
    end do
end do

! 避免: 行优先访问（缓存不友好）
do i = 1, m
    do j = 1, n
        A(i, j) = A(i, j) + B(i, j)
    end do
end do
```

---

## 10. 代码质量检查清单

提交前确认：
- [ ] `implicit none` 在所有程序/模块中
- [ ] 所有参数有 `intent` 声明
- [ ] 数值精度统一（`real(8)` 或自定义）
- [ ] 收敛标准和最大迭代次数已设置
- [ ] 文件头部有清晰文档
- [ ] Makefile 可正确编译
- [ ] 输出结果验证正确
- [ ] 与 Stata 的数据接口测试通过

---

## 11. 质量门槛评分标准

| 维度 | 优秀 (95+) | 良好 (80-94) | 不足 (<80) |
|------|-----------|-------------|-----------|
| 现代化 | 全部 F90/95 | 部分老式语法 | 大量 F77 风格 |
| 数值稳定性 | 收敛诊断完整 | 基本检查 | 无检查 |
| 文档 | 完整注释和说明 | 基本文档 | 无文档 |
| 性能 | 优化良好 | 可接受 | 效率低下 |
| 可维护性 | 模块化、清晰 | 基本结构 | 混乱 |
