# 🏮 敲击感应小灯 (NeoPixel 2026)

## 你的目标 @unplugged
敲一下，就变亮，再敲一下就灭了！
![效果演示：敲击变色](https://raw.githubusercontent.com/jenzenng-max/neopixel2026/master/docs/static/neopixel2026_demo.gif)
加上自己的创意，动手做个类似趣味小灯吧！

## Step 2: 连接灯带

我们要把灯带和 Micro:bit 连接起来：

- :hand lizard: ![电路原理图：GND接GND，VCC接3V，DIN接P0](/static/neopixel2026_sch.png "3242 ")
说明：
    - 灯带的 **GND** 连到 Micro:bit 的 **GND**。
    - 灯带的 **VCC** (电源) 连到 Micro:bit 的 **3V**。
    - 灯带的 **DIN** (信号) 连到 Micro:bit 的 **P0** 引脚。


```blocks

```
---
![h实物接线图：请仔细核对线序](https://raw.githubusercontent.com/jenzenng-max/neopixel2026/master/docs/static/neopixel2026_wire.gif "3242 ")



## Step 2: 初始化灯带

首先，我们需要告诉 Micro:bit 我们连接了灯带。

在 `||basic:当开机时||` 积木中：

1. 从 `||neopixel:Neopixel||` 栏拖入`||neopixel: strip 设为引脚...||` 积木。
2. 设置引脚为 **P0**，灯珠数量为 **10** 颗。
3.  `||neopixel:Neopixel||` 栏拖入`||neopixel: strip 显示颜色红||`积木。

```blocks
let strip = neopixel.create(DigitalPin.P0, 10, NeoPixelMode.RGB)
strip.showColor(neopixel.colors(NeoPixelColors.Red))

```

## Step 3: 感知震动与开关逻辑

我们需要一个变量 `||variables:isOn||` (是否开灯) 来记录状态。

* 如果灯现在是 **关** (false)，拍一下就变 **开** (true)。
* 如果灯现在是 **开** (true)，拍一下就变 **关** (false)。

我们利用加速度传感器来检测敲击（力量 > 1100）。

```blocks
let isOn = false
basic.forever(function () {
    if (input.acceleration(Dimension.Strength) > 1100) {
        isOn = !isOn
    }
})

```

## Step 4: 控制开灯

现在让灯根据 `||variables:isOn||` 的状态开关灯。

* 如果是 **开**，就显示红色。
* 如果是 **关**，就清空颜色 (灭灯)。
* **注意**：最后加一个 500 毫秒的暂停，防止一次敲击被误判为多次。

```blocks
let isOn = false
let strip = neopixel.create(DigitalPin.P0, 10, NeoPixelMode.RGB)
basic.forever(function () {
    if (input.acceleration(Dimension.Strength) > 1100) {
        isOn = !isOn
        if (isOn) {
            strip.showColor(neopixel.colors(NeoPixelColors.Red))
        } else {
            strip.clear()
            strip.show()
        }
        basic.pause(500)
    }
})

```

## 恭喜完成！ @unplugged

恭喜你！现在把 Micro:bit 塞进纸杯底座，套上画有“2026”/“新年快乐”的 小纸杯
对着桌子轻轻一拍——你的新年小灯亮了吗？

快去给爸爸妈妈展示你的科技年货吧！

```package
neopixel=github:microsoft/pxt-neopixel

```
