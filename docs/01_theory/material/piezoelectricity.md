# 压电效应

在对压电材料施加压力时，材料会出现电极化并在两侧呈现一个电场，这一现象被称为`压电效应`，材料拉伸一侧会呈现正电压，压缩一侧呈现负电压。

反过来，在对压电材料施加电压时，其会出现形变，这一现象被称为`逆压电效应`。

电场强度与压力和形变间的关系如下：

$$
E = \frac{gX}{L}
$$

- $E$：电场强度（$V$）
- $g$：压电电压常数（$V \cdot m/N$）
- $X$：压力（$N$）
- $L$：形变长度（$m$）

$$
S = dE
$$

- $S$：形变量（$m$）
- $E$：电场强度（$V$）
- $d$：压电应变常数（$m/V$）
    - 有时其单位为 $C/N$，这两种单位对应的数值理论上相同，但实际上 $C/N$ 通过压电效应测得，$m/V$ 通过逆压电效应测得，两种方式测得的值可能会有相当大差别，因此最好区分使用。

通常文档中提供的 $g$ 和 $d$ 都带有下标（将其记为 $ij$），其意义为：

- $i$：电场极化方向，
    - 范围：1~3
        - 分别表示三维坐标的 $X, Y, Z$ 方向
- $j$：应力方向
    - 范围：1~6
        - 1~3：按顺序代表三维坐标的 $XX$，$YY$，$ZZ$ 方向，被称为正应力（Normal stress），受力方向与施力方向垂直；
        - 4~6：按顺序代表三维坐标的 $YZ$，$XZ$，$XY$ 方向，被称为剪应力（Shear stress），受力方向与施力方向平行

一般使用上只关心 $d_{33}$ 和 $g_{33}$，即电场极化方向与施力方向相同。

## 常见材料

以下为几种常见的压电材料：

- 石英晶体，通常用在振荡电路中
- 锆钛酸铅（PZT）陶瓷，使用量高，但含铅
- 聚偏二氟乙烯（PVDF），常用作柔性压电薄膜，价格相对 PZT 较高

## 应用场景

- 麦克风/扬声器
- 振动传感器
- 高压生成（打火机）
- 压力传感器（容易漂移，因此不常见）
- 免维护风扇

## 注意

- 压电材料本身没有提供电流的能力（阻抗近似正无穷），作为传感器使用时需先接入放大器

## 参考

- [Fundamentals of Piezoelectricity](https://application.wiley-vch.de/books/sample/3527345124_c01.pdf)
- [Piezoelectric ceramics for transducers](https://doi.org/10.1533/9780857096302.1.70)
- [Is there any difference between the piezoelectric coefficient values expressed in pm/V and pC/N?](https://www.researchgate.net/post/Is_there_any_difference_between_the_piezoelectric_coefficient_values_expressed_in_pm_V_and_pC_N)