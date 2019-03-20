原型 #1
=======

![](/img/prototype1.png)

### 硬件
所用到的硬件包括:

+ 树莓派 3B 或 3B+
+ ReSpeaker 4 Mic Linear Array
+ 45mm 带音腔的喇叭
+ 激光切割的纸壳 （后面尝试不借助激光切割机 DIY 纸壳）
+ 2个尼龙铆钉 (R3035 或 R3045)，见下图，定位孔是ф3的，用螺丝螺母也可以，但尼龙铆钉使用起来更方便

  ![](/img/nylon_rivet.png)


树莓派、ReSpeaker 4 Mic Linear Array、喇叭、铆钉都可以在淘宝上买到，激光切割的纸壳可以在当地的创客空间里制作，很多创客空间都激光切割机、3D打印机。在深圳的话，可以去柴火创客空间，里面有各种各样的工具。

### 设计一个纸壳
激光切割纸壳需要一份CAD设计图，CAD倒是在大学的《机械工程制图》课中学过，已经好几个年头没有用过了，好在设计一个简单纸壳只需要画线、画圆即可，就是数字化尺规作图。
商业的CAD软件都很贵，但对于设计纸壳这个任务，用开源的[LibreCAD](https://librecad.org/)就可以搞定。

![](/img/prototype1_librecad.png)

如果从来没有接触过CAD制图，上手CAD有一定的门槛，可以Python设计CAD图纸，这里用到[Manfred Moitzi写的一个很好用的Python库ezdxf](https://github.com/mozman/ezdxf)，我们把纸壳的尺寸算清楚，就可以在Python里面画线、画圆了。
用Python设计一些有规律的结构可能比用CAD更快速，比如说设计阵列喇叭孔。

![](/img/speaker_hole.png)

用Python代码设计这样的：

```python
import ezdxf

dwg = ezdxf.new('R2007')
msp = dwg.modelspace()
d = 40.
n = int(40 / 1.5)

delta = d / n

start = (100, 100)

msp.add_circle(start, 20)

for x in range(-n, n):
    for y in range(-n, n):
        if y & 1:
            offset = 0.5
        else:
            offset = 0
        rx = ((x + offset)**2 + y**2)**0.5
        if rx <= (n/2 + 0.1):
            r = 0.35 # (0.25 * delta * (n/2 - rx) / (n/2) + 0.15 * delta)
            msp.add_circle((start[0] + (x + offset) * delta, start[1] + y * delta), r)

dwg.saveas('speaker_hole.dxf')
```
这段代码输出的图形是：

![](/img/cad_speaker_hole.png)

可以将CAD和Python代码结合，进行更复杂的设计。

### 软件
实现语音交互智能音箱的功能，最简单的方法是用各大厂商提供的语音助手服务，Amazon、Google、百度都提供了多种编程语言SDK的支持，百度甚至推出兼容Amazon语音服务的API；像更深层次的折腾，可以尝试MyCroft和wukong-robot；像要离线的使用，可以尝试snips.ai的一套语音交互解决方案。
这里资源的链接都可以在 [voice-engine/make-a-smart-speaker](https://github.com/voice-engine/make-a-smart-speaker) 上找到。
