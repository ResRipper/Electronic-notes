# 热阻

热阻即物体对热量传递的阻碍能力，常见符号为 $\theta$ 或 $R$，单位为 $°C/W$。

## 热阻、温差与功率的变换

该公式被称作热欧姆定律（Thermal Ohm’s law）[^1]：

$$
    \theta = \frac{T_{1} - T_{2}}{P} = \frac{\Delta T}{P}
$$

热阻、温差与功率的定义和变换规律与电学中的对应概念类似，因此热阻网络常被作为电阻网络进行绘制和计算，热阻的串并联规律也与电阻相同：

- $\Delta T \rArr U$
- $\theta \rArr R$
- $P \rArr I$

## 热阻计算

### 稳态材料

对于不产热且热导均匀的材料，其热阻为

- $L$：物体在传递轴线上的长度
- $A$：传递路径的物体横截面积
- $\lambda$：材料热导

$$
    \theta = \frac{L}{\lambda \times A}
$$

与热欧姆定律结合：

$$
P = \lambda A \frac{\Delta T}{L}
$$

### 散热

/// admonition | 参考
    type: info

    参考[散热](./heat_dissipation.md)一节

///

将牛顿冷却定律与热欧姆定律结合，可以得到

$$
\theta = \frac{1}{hA}
$$

---

[^1]: [ROHM Co. Basics of Thermal Resistance and Heat Dissipation. Application Note 2021.](https://fscdn.rohm.com/en/products/databook/applinote/common/basics_of_thermal_resistance_and_heat_dissipation_an-e.pdf)
