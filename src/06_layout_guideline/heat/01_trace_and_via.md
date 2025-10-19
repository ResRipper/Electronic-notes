# 线宽与热过孔

在过去，走线宽度、电流和温升的关系是由 [IPC-2221](https://shop.ipc.org/ipc-2221/ipc-2221-standard-only) 标准指定的，但其所用公式基本上是从老旧的 MIL-STD-275 标准抄来的，没有考虑如底部铺铜、内/外层走线、导线厚度等影响，现已被基于实验测定的 [IPC-2152](https://shop.ipc.org/ipc-2152/ipc-2152-standard-only) 标准替代，因此本文主要基于 IPC-2152 与部分论文及报告编写，不考虑 IPC-2221 中的内容。

需要注意的是，标准测试使用的环境较为理想化，实际中必须进行测试以检查效果。

<image src='./assets/01_trace_and_via/IPC-2152_reality.webp' alt='Credit: Würth Elektronik' width='600'/>

## 环境假设

IPC-2152 中进行了一些假设：

- 走线相对电路板面积较小，且走线外的电路板面积达 `76.2 x 76.2 mm`
- 电路板厚度在 `1.52 ~ 1.79 mm` 范围内，材质为 `FR4`
- 没有对制造过程中造成的线宽误差进行补偿
- 临近走线在 `25.4 mm` 外，否则需要使用线宽和与电流和

## 走线宽度

标准中提供了大量不同环境与情况下的测试结果图表，但没有提供任何公式，使用时需通过查表找出对应的值。但在实际设计过程中，可以使用各个 EDA 自带的计算工具，也可以使用比较知名的 [Saturn PCB Toolkit](https://saturnpcb.com/saturn-pcb-toolkit/) 的 Conductor Properties 模式，无需手动计算。

> KiCAD 提供的计算器仍在使用 IPC-2221 标准，相关 [issue](https://gitlab.com/kicad/code/kicad/-/issues/2260) 在 2018 年就已提出，但尚未实现，因此建议使用其它工具。

通常而言我们会以 **10℃** 温升作为理想目标，**20℃** 作为绝对目标。但据主导该标准工作的 Mike Jouppi [所说](https://www.youtube.com/watch?v=A4o2MzFRL_U)，根据该标准设计的走线值通常会比实际需要更宽，因此可以在一定范围内缩减线宽，以节约板上空间。

如果无法在单层走线下满足线宽目标，有以下几种办法：

- 通过多层板并联走线，并使用多个过孔平衡各层电流
    - 使用过孔会从各层削掉一部分铜，因此大量使用过孔有可能会导致走线的等效宽度降低，使用时需控制数量，或是使用过孔塞铜等工艺（但会显著增加成本）
- 使用焊锡或是铜片/铜排增加导线厚度
    - 需注意使用环境，防止短路或触电
    - 有大量关于焊锡加厚导线的效果和性价比的争论，建议预先进行测试和计算
    - 可能导致成本飙升

## 热过孔（Thermal Via）

热过孔其实就是普通的过孔，但其作用为通过孔壁铜层快速将热量传导到电路板的背面，协助降低整块 PCB 的热量。在放置热过孔时，需尽量将其放置在地平面，借助内外层的铺铜区快速将热量扩散开，并尽可能靠近热源。

根据一项[研究](https://ieeexplore.ieee.org/document/8706634)，热过孔设计可参考以下几点：

- 在过孔排布上，使用交叉排列可以比矩阵排列增加约 $15.5\%$ 的过孔数量，热阻降为矩阵排列的约 $86.6\%$

    <p align="center">
    <image src='./assets/01_trace_and_via/thermal_via_l1.webp' alt='Array style' width='300'/>
    <image src='./assets/01_trace_and_via/thermal_via_l2.webp' alt='Crossover style' width='300'/>
    </p>

- 过孔间距越小，传热越快
    - 需参考厂商可制造性
    - 避免孔距过小造成断裂
    - 避免过度掏空，造成路径电阻增加

- 对于跨板传热，过孔对热量传导的影响最大，板厚、铜厚和层数基本不影响热传导。在长宽方向过孔数一致时，简化的归一化过孔热阻为：

    - $t_{PTH}$：孔壁铜厚
    - $s$：过孔边到边间距
    - $k$：材质的导热系数，单位为 $W/mK$（瓦/米·度）
        - 空气：0.026 W/m·K @ 25°C
        - 铜：393 W/m·K @ 25°C
        - FR4（跨平面）：0.29 W/m·K @ 25°C
        - 塞铜工艺使用的铜浆不是纯铜，需参考厂商提供的值

    $$
    \Theta \approx \frac{2\sqrt{3}(s + \phi)^{2} k_{FR4}}{4 \pi k_{Cu} t_{PTH} (\phi - t_{PTH}) + \pi k_{filler}(\phi - 2 t_{PTH})^{2}}
    $$

    设孔边间距为 0.2mm，$t_{PTH} \approx 18 \times 10^{-6} m$，可以看到过孔尺寸存在一最佳值，约为 0.236mm：

    <p align="center">
    <image src='./assets/01_trace_and_via/thermal_via_resistance.webp' alt='Via size' width='600'/>
    </p>

## 参考

- [Altium - Using an IPC-2152 Calculator: Designing to Standards](https://resources.altium.com/p/using-ipc-2152-calculator-designing-standards)
- [Lazar Rozenblat - Calculation of PCB trace width based on IPC-2152](https://www.smps.us/pcb-calculator.html)
- [Thermal Modeling and Design Optimization of PCB Vias and Pads](https://ieeexplore.ieee.org/document/8706634)
