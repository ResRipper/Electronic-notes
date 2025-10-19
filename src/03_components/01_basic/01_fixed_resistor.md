# 定值电阻

## 原理

任何材料都存在一个电阻率。对于单一材料且横截面均匀的物体，可通过以下公式求得其电阻：

$$
R=\rho\frac{l}{A}
$$

- $\rho$：材料的电阻率
- $l$：材料长度
- $A$：材料横截面积

因此，有以下几种主要方式调控电阻值：

- 使用沉积等方法调节导体厚度
- 使用激光切割调节横截面宽度（也常见于半导体元件中）
- 使用合金、陶瓷等不同材质的导体
- 通过导体长度进行控制

## 阻值

参考[无源元件标准尺寸与值](./passive_component_size.md)。

### 色环电阻标记

插件电阻通常使用 IEC 60062 中定义的色环标识自身阻值、误差及温度系数：

<image src="./assets/01_resistor/resistor_color_code.webp" alt="Credit: KiCad PCB Calculator" width=500px>

### 贴片电阻标记

表面贴装电阻通常使用编号标记自身阻值，其中又分为多个标准：

- EIA-96

    由电子工业联盟（EIA，现合并到电子行业协会 ECIA）制定，现合并到 IEC 60062:2016 标准中，常用于 1% 精度的电阻上，由两位数的编号加上单字母的指数标记组成：

    - 编号

        <image src="./assets/01_resistor/EIA-96.webp" alt="Credit: Bourns" width=700px>

    - 指数编号

        | 编号 | 对应值    |
        | ---- | --------- |
        | Y/R  | $10^{-2}$ |
        | X/S  | $10^{-1}$ |
        | A/Z  | $1$       |
        | B/H  | $10$      |
        | C    | $10^{2}$  |
        | D    | $10^{3}$  |
        | E    | $10^{4}$  |
        | F    | $10^{5}$  |

    示例：

    - 15C

        - 编号 15 对应值为 140
        - C 的对应值为 $10^{2}$
        - 15C $= 140 \times 10^{2} = 14k\Omega$

- 3/4 位编码

    由数字和作为小数点的 `R` 字符组成，其中最后一位为指数标记。

    示例：

    - 4R7
        - $4.7\Omega$
    - 471
        - $47 \times 10^{1} = 470\Omega$
    - 5233
        - $523 \times 10^{3} = 523k\Omega$
    - R010
        - $0.01 \times 10^{0} = 10m\Omega$

    注意，如果电阻值为个位数毫欧，可能会出现如下标记：

    - R005
        - $0.005 = 5m\Omega$
    - R001
        - $0.001 = 1m\Omega$

- 小于 $1m\Omega$

    通常有两种方式进行标记：

    - 使用 `m` 字母作为小数点

        - 0m25 $= 0.25m\Omega$
        - 0m05 $= 0.05m\Omega$

    - 直接打上料号

        这类低阻值的电阻通常尺寸都很大，有充足的空间标明其阻值、精度、厂商和料号等信息。

## 特殊类型

> 与传感器及电路保护相关的电阻请参考对应章节

### 排阻

顾名思义，即多个电阻形成的单一元件。通常有两种类型：

- 在单块基板上制成的多个独立电阻

    与使用多个电阻没有本质区别，但可以降低 BOM count 和贴片耗时，常作为上/下拉电阻使用。

- 精密匹配电阻网络

    该类电阻通常使用 MSOP 等 IC 封装，其单个电阻的精度可能非常糟糕，但各电阻间的比率受严格控制。以 ADI LT5401 为例，其单个电阻的公差在 $15\%$，但电阻间匹配精度和温漂都非常优秀。

    该类电阻常用在放大器和分压网络等需要精确电压输出比率的场景，但成本高昂。

### 检流电阻（shunt resistor）

专门用于测量电流大小，特性为低阻值且高精度。其原理为测量电阻两端的压降大小，然后通过欧姆定律计算具体的电流值。

使用时需要通过开尔文连接走线到电压测量端，以降低电压测量端到电阻的走线阻值差异，避免对测量端的放大器产生影响；还需要参考厂商文档的焊盘及走线方式，以绕过电阻端子材质的温漂影响。比较常见的检流电阻有两种：

- 两端点电阻

    最常见，价格较低。

    <image src="./assets/01_resistor/shunt_2.webp" alt="Credit: Bourns" width=400px>

- 四端点电阻

    测量电流与主电流路径分离，且可以通过端子和电阻材质的温度系数互相进行补偿，以提高测量精度。

    <image src="./assets/01_resistor/shunt_4.webp" alt="Credit: Bourns" width=400px>

## 选型要点

### 温度系数

温度系数指电阻值在不同温度下的偏差大小，单位为 ppm/℃（ppm 即 parts-per-million，$10^{-6}$）。

根据 IEC 60115-1 标准，标称电阻值是在 25℃ 下测得的，变化后的值可通过以下公式求出：

- $T_{1}：25℃$
- $T_{2}：目标温度$
- $R_{t}：目标温度下的阻值$
- $C：温度系数$

$$
R_{t} = R [1 + C(T_{2} - T_{1}) \times 10^{-6}]
$$

如果一个电阻的标称值为 $10k\Omega$，温度系数为 $100ppm/℃$，那么在 100℃ 下其阻值为

$$
R_{100℃} = 10 \times 10^{3} \times [1 + 100 \times (100 - 25) \times 10^{-6}] = 10.075k\Omega
$$

### 精度

一般情况使用 5% 精度即可。

对精度有一定要求时使用 1%，还需注意温度系数应尽量满足 $\le100ppm/℃$

### 功率

电阻的额定功率通常仅覆盖到 70℃，如果电阻的工作环境温度较高，需参考厂商数据手册中的图表，检查衰减后功率是否满足使用条件。

### 耐压

有时候数据表里仅会给出最高工作电压，这时需通过以下公式得出额定工作电压：

$$
U = \sqrt{P \times R}
$$

原理：

$$
U = R \times I, P = U \times I \\
P = \frac{U^{2}}{R}
$$

变换后即可得出上面的工作电压公式。如果得出的电压值超出了数据表中的最高工作电压，需以表中的值为准（通常由材料和电阻结构决定）。

## 参考

- [Analog - 改进低值分流电阻的焊盘布局，优化高电流检测精度](https://www.analog.com/media/cn/analog-dialogue/volume-46/number-2/articles/optimize-high-current-sensing-accuracy_cn.pdf)
- [Bourns - Using Current Sense Resistors forAccurate Current Measurement](https://www.bourns.com/docs/technical-documents/technical-library/current-sense-pulse-power-high-power-resistors/application-notes/bourns_n1702_current_sense_accurate_measurement_appnote.pdf)
