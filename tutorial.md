# 🏮 敲击感应小灯 (NeoPixel 2026)

## 🏮 敲击感应小灯 @unplugged

![效果演示：敲击变色](https://raw.githubusercontent.com/jenzenng-max/neopixel2026/master/docs/static/neopixel2026_demo.gif)
敲一下，就变亮，再敲一下就灭了！
加上自己的创意，动手做个类似趣味小灯吧！

## Step 1: 连接灯带

我们要把灯带和 micro:bit 的扩展板连接起来：

- :hand lizard: ![电路原理图：GND接GND，VCC接3V，DIN接P0](https://raw.githubusercontent.com/jenzenng-max/neopixel2026/master/docs/static/neopixel2026_sch.png "GND接GND，VCC接3V，DIN接P0 ")
```blocks

```
---
![实物接线图：请仔细核对线序](https://raw.githubusercontent.com/jenzenng-max/neopixel2026/master/docs/static/neopixel2026_wire.gif "请仔细核对 ")



## Step 2: 初始化灯带

首先，我们需要告诉 micro:bit 我们连接了灯带。

在 `||basic:当开机时||` 积木中：

1. 从 `||neopixel:Neopixel||` 栏拖入`||neopixel: strip 设为引脚...||` 积木。
2. 设置引脚为 **P0**，灯珠数量为 **10** 颗。
3.  `||neopixel:Neopixel||` 栏拖入`||neopixel: strip 显示颜色红||`积木。

然后把代码刷入micro:bit,查看灯带是否亮起。

亮起则接线正确且接触良好；

若不亮，请检查接线和代码。

```blocks
let strip = neopixel.create(DigitalPin.P0, 10, NeoPixelMode.RGB)
strip.showColor(neopixel.colors(NeoPixelColors.Red))

```

## Step 3-1: 感知震动与开关逻辑1

现在来描述我们想要看到的效果：

我们敲一下，就变亮，再敲一下就灭了！

那么micro:bit执行的事情是：

* 如果micro:bit的加速度计感受到震动（我们敲一下），
    * 灯的状态就从 **开** (true)变成 **关** (false)。
    * 灯的状态就从 **关** (false)变成 **开** (true)。

灯的状态就变成**相反** 的状态！


```
## Step 3-2: 感知震动与开关逻辑2

所以我们要使用输入变量：加速度值`||input:加速度值||`；输出变量：灯的状态`||variables:LED||`；

然后按照micro:bit执行的事情从自然语言转化为机器代码；

你来尝试一下吧！遇到问题可以尝试查看提示！
tips: 感受到敲击，灯的状态就变成**相反** 的状态！
```blocks
let LED = false

basic.forever(function () {
    if (input.acceleration(Dimension.Strength) > 1100) {
        LED = !LED
    }
})

```
## Step 4: 控制开灯

最后让灯根据 `||variables:LED||` 的状态开关灯。

* 如果是 **开**，就显示红色。
* 如果是 **关**，就清空颜色 (灭灯)。
* **注意**：最后加一个 500 毫秒的暂停，防止一次敲击被误判为多次。

尝试写一下代码！

写完就下载代码到micro:bit,你的敲击小灯就做完了！

```blocks
let LED = false

let strip = neopixel.create(DigitalPin.P0, 10, NeoPixelMode.RGB)
basic.forever(function () {
    if (input.acceleration(Dimension.Strength) > 1100) {
        LED = !LED
        if (LED) {
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

恭喜你！现在把 micro:bit 塞进纸杯底座，套上画有“2026”/“新年快乐”的 小纸杯
对着桌子轻轻一拍——你的新年小灯亮了吗？
![效果演示：敲击变色](https://raw.githubusercontent.com/jenzenng-max/neopixel2026/master/docs/static/neopixel2026_demo.gif)
快去给爸爸妈妈展示你的科技年货吧！

```package
neopixel=github:microsoft/pxt-neopixel

```