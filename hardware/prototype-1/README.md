Prototype #1
============

[中文版](zh.md)

### Hardware

![](/img/prototype1.png)

The hardware includes:

+ Raspberry Pi 3B
+ ReSpeaker 4 Mic Linear Array
+ 45mm Speaker
+ Laser-Cut Paper Case
+ 2 Nylon Rivets (R3035 or R3045), The hole is ф3

  ![](/img/nylon_rivet.png)


Raspberry Pi and ReSpeaker 4 Mic Linear Array can be found at Seeed Studio, and we get the 45mm speaker and nylon rivets from Alibaba or Taobao.


### Design a Paper Case
To use a laser cutter, we need a CAD design file. I learned how to use a CAD software at college, but have not used CAD for many years.
Fortunately, it is easy to design a paper case. We only have to know how draw lines and circels.
Commercial CAD tools are quite expensive, but for this simple task, the open source CAD tool - [LibreCAD](https://librecad.org/) is enough to get it done. You can design a unique paper case based on [my work](prototype1.dxf).

![](/img/prototype1_librecad.png)

If you have not used any CAD software, it will take some time to be familiar with a CAD software like LibreCAD.
You can also use Python script to genrate the CAD DXF file. [Manfred Moitzi's handy Python library - ezdxf](https://github.com/mozman/ezdxf) will help us to do the design work. For example, to draw an array of speaker holes, using Python script is much easier.

![](/img/speaker_hole.png)

The Python script is like:
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
The output is:

![](/img/cad_speaker_hole.png)

We can use both LibreCAD and Python script to make a complex design.

### Software
For a smart speaker, the software is the key point.
The easiest way is using Alexa Voice Service or Google Assistant SDK.
If you want to know how a voice assistant works, you should try MyCroft.
If you prefer to offline voice assistant, Snips.ai is a good option.
You can find a full list of resources at [voice-engine/make-a-smart-speaker](https://github.com/voice-engine/make-a-smart-speaker).