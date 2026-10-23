# 小熊双耳闹钟 · 两键与对话效果演示

[直接打开 Wokwi 演示](https://wokwi.com/experimental/viewer?diagram=https%3A%2F%2Fraw.githubusercontent.com%2FDarkhorse-Fraternity%2Faw-mcu-wokwi-demo%2Ff95ec92700b474a1cab922c5a29659b5d0ef531a%2Fdiagram.json&firmware=https%3A%2F%2Fraw.githubusercontent.com%2FDarkhorse-Fraternity%2Faw-mcu-wokwi-demo%2Ff95ec92700b474a1cab922c5a29659b5d0ef531a%2Ffirmware.bin)

本版使用 Wokwi 原生 ESP32-S3 与横向 320×240 ILI9341 TFT：
表情与眨眼、20 px 中文、72 px 计时数字、提醒铃铛，新增圆形火柴人动画区。
这是矩形屏内的圆形内容预览，未虚构 Wokwi 不支持的圆屏控制器。

左侧显示电池供电接线示意：带保护 3.7V / 2500mAh 锂电池 → BQ24074 充电/电源切换
→ 3.3V 升降压 → S3 / 单麦 / 显示；USB-C 5V 接充电板输入。
**该图是方案注释，没有电池元件接入仿真供电网络。当前仍由模拟器供电，
电池接入与充放电尚未完成；电压状态注入不能算作电池供电。**

器件图仅保留左右耳两个按键，取消额外动作键、测试旋钮和测试开关。
动画演示：左耳短按发起对话 → 再按左耳提交 → 串口输入 `reply wave` 回车。
也可输入 `reply walk` 或 `reply jump`；每段约 3 秒，完成后停住。
每次需要新的有效对话请求；取消、超时或重复注入的回复不启动动作。
双耳操作返回业务界面，提醒和指令优先显示；动画帧不延长 15 秒亮屏时间。

串口 `power battery 3200` 注入 3.20V，范围 3000–4200mV；
`power usb 1` / `power usb 0` 注入 USB 接入/拔出。电压和 BAT/USB 在屏幕显示。
无 USB 且低于 3.4V 时提示低电并停止动作，业务记录和计时继续。
这是状态注入，不模拟电池化学、充电电流、真实插拔供电和续航；不显示伪造的电量百分比。

打开后如未运行，按绿色启动键。初始黑屏是预期行为。
右耳短按开始记录／暂停／继续，15 秒无操作后屏幕清黑，录音灯保持独立。
左耳短按开始／提交模拟指令；指令期间右耳短按取消。
长按右耳约 1 秒结束当前活动或本次响铃。

串口输入一行后回车：

- `down 30`：准备 30 秒倒计时，再按右耳开始；短按暂停／继续，长按结束。
- `up`：准备正向计时；新计时前先长按右耳结束旧计时。
- `remind 3`：3 秒后提醒；右耳短按贪睡 5 分钟，长按结束本次。
- `advance 300000`：加速 5 分钟，检查贪睡后再次响铃。
- `journal`：选择日记，再按右耳看表情与模拟记录。
- `reply query`：左耳提交后注入模拟查询结果。
- `reply wave` / `reply walk` / `reply jump`：提交对话后注入模拟动画结果。
- `reset`：清空本次仿真数据。
- `test`：执行 25 项核心交互断言，通过输出 `AW_TEST_PASS`。

这是 Arduino + 共享 Rust 核心的无凭据仿真镜像，不连接后台。
GPIO 4/5 是双耳，13 为采音灯，14 为蜂鸣器；
TFT CS/MOSI/SCK/D-C/RST 分别为 10/11/12/16/17。
电路图未接入额外动作键或电源测试控件。这些不是实体板接线承诺。
Wokwi 不模拟背光和 RST，DISPOFF 后画布仍保留，固件先清黑再发关闭命令；
黑屏不能证明真实面板或背光已断电。

本版浏览器验证同时检查两键交互、有效对话回复后的动画像素、过期/取消/重放隔离和电源状态注入。
`reply walk/wave/jump` 是模拟器测试语法；真实云端动作字段与协议尚未接入此演示。
每次采音只注入一帧合成 PCM，存储为易失 RAM；
不代表真实录放音、WakeNet 接入、周期提醒日历、掉电可靠性或一周续航已经验收。
原工程及账号配置不在此仓库。本镜像仅供仿真，不刷入实体 S3。

固件 SHA-256：`d193d33eb2130effe1c7c689e59d1b512bff4a187087d605ffc26854db8f2924`。
工具链：ESP-IDF 5.5.4、Arduino-ESP32 3.3.12、Rust Xtensa 1.94.0.2。

字体使用 Noto Sans SC 固定字形子集，原字体来自
[google/fonts](https://github.com/google/fonts/tree/a85815a42757630ce188fdad368c2dfc444d4773/ofl/notosanssc)。
字体文件 SHA-256：`a3041811a78c361b1de50f953c805e0244951c21c5bd412f7232ef0d899af0da`。
下面保留字体版权与 SIL Open Font License，字体子集仍遵循该许可。

```text
Copyright 2014-2021 Adobe (http://www.adobe.com/), with Reserved Font Name 'Source'

This Font Software is licensed under the SIL Open Font License, Version 1.1.
This license is copied below, and is also available with a FAQ at:
https://scripts.sil.org/OFL


-----------------------------------------------------------
SIL OPEN FONT LICENSE Version 1.1 - 26 February 2007
-----------------------------------------------------------

PREAMBLE
The goals of the Open Font License (OFL) are to stimulate worldwide
development of collaborative font projects, to support the font creation
efforts of academic and linguistic communities, and to provide a free and
open framework in which fonts may be shared and improved in partnership
with others.

The OFL allows the licensed fonts to be used, studied, modified and
redistributed freely as long as they are not sold by themselves. The
fonts, including any derivative works, can be bundled, embedded, 
redistributed and/or sold with any software provided that any reserved
names are not used by derivative works. The fonts and derivatives,
however, cannot be released under any other type of license. The
requirement for fonts to remain under this license does not apply
to any document created using the fonts or their derivatives.

DEFINITIONS
"Font Software" refers to the set of files released by the Copyright
Holder(s) under this license and clearly marked as such. This may
include source files, build scripts and documentation.

"Reserved Font Name" refers to any names specified as such after the
copyright statement(s).

"Original Version" refers to the collection of Font Software components as
distributed by the Copyright Holder(s).

"Modified Version" refers to any derivative made by adding to, deleting,
or substituting -- in part or in whole -- any of the components of the
Original Version, by changing formats or by porting the Font Software to a
new environment.

"Author" refers to any designer, engineer, programmer, technical
writer or other person who contributed to the Font Software.

PERMISSION & CONDITIONS
Permission is hereby granted, free of charge, to any person obtaining
a copy of the Font Software, to use, study, copy, merge, embed, modify,
redistribute, and sell modified and unmodified copies of the Font
Software, subject to the following conditions:

1) Neither the Font Software nor any of its individual components,
in Original or Modified Versions, may be sold by itself.

2) Original or Modified Versions of the Font Software may be bundled,
redistributed and/or sold with any software, provided that each copy
contains the above copyright notice and this license. These can be
included either as stand-alone text files, human-readable headers or
in the appropriate machine-readable metadata fields within text or
binary files as long as those fields can be easily viewed by the user.

3) No Modified Version of the Font Software may use the Reserved Font
Name(s) unless explicit written permission is granted by the corresponding
Copyright Holder. This restriction only applies to the primary font name as
presented to the users.

4) The name(s) of the Copyright Holder(s) or the Author(s) of the Font
Software shall not be used to promote, endorse or advertise any
Modified Version, except to acknowledge the contribution(s) of the
Copyright Holder(s) and the Author(s) or with their explicit written
permission.

5) The Font Software, modified or unmodified, in part or in whole,
must be distributed entirely under this license, and must not be
distributed under any other license. The requirement for fonts to
remain under this license does not apply to any document created
using the Font Software.

TERMINATION
This license becomes null and void if any of the above conditions are
not met.

DISCLAIMER
THE FONT SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO ANY WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT
OF COPYRIGHT, PATENT, TRADEMARK, OR OTHER RIGHT. IN NO EVENT SHALL THE
COPYRIGHT HOLDER BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY,
INCLUDING ANY GENERAL, SPECIAL, INDIRECT, INCIDENTAL, OR CONSEQUENTIAL
DAMAGES, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF THE USE OR INABILITY TO USE THE FONT SOFTWARE OR FROM
OTHER DEALINGS IN THE FONT SOFTWARE.
```
