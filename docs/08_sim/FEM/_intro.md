<!--
 Copyright 2026 ResRipper.
 SPDX-License-Identifier: Apache-2.0
-->

# 有限元素法介绍

有限元法（Finite Element Method，FEM）是最常用的仿真方法之一，常用于机械、流体和电磁场等的分析，还可用于涉及多种物理现象时的仿真。

## 原理

描述物理现象的公式通常都是偏微分方程，仿真的目的为求解在指定边界条件下的方程解。FEM 将模型拆解为简单单元，对单个单元的解进行描述，然后整合所有单元和边界条件获得整体的待求解方程组，然后使用迭代法等方法进行求解。该方法得出的值为近似值，但可通过网格收敛性、误差分析、迭代求解时是否收敛和实际验证来确保结果可信。

为确保计算结果准确性和精度，在使用时需重点关注网格质量，模型问题和数学模型准确性。

## 预处理

### 材质

为了进行计算，需要为进行仿真的各组件分配材质。这一步骤实际上是将预设的材质参数分配给了组件，并在求解时作为常量使用。常见的一些材质参数有：

- 电导率
- 相对介电常数
- 密度
- 粘度

### 约束/边界条件

边界条件提供了求解时的固定参考，否则将无法确定最终结果。常见的边界条件有：

- 温度
- 电势
- 流速
- 力载荷

### 网格化

网格化也就是将建模分割为独立单元的过程，请参考[网格](./mesh.md)一文。

## 参考

- [胡茂彬 - 10.4 有限元法的基本原理和解题步骤](http://staff.ustc.edu.cn/~humaobin/course/cht/ppt/10.4.pdf)
- [Comsol - An Introduction to the Finite Element Method](https://www.comsol.com/multiphysics/finite-element-method)
- [CSC - ElmerSolver Manual](https://www.nic.funet.fi/index/elmer/doc/ElmerSolverManual.pdf#chapter.1)