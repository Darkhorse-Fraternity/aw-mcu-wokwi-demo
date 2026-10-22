# 双耳闹钟 Wokwi 演示

这是 `aw-mcu` Arduino + 共享 Rust 核心的仿真资产，不是实物刷机包。
固件使用合成 PCM、易失 RAM，不连接后台，不含账号凭据。

[直接打开 Wokwi 演示](https://wokwi.com/experimental/viewer?diagram=https%3A%2F%2Fraw.githubusercontent.com%2FDarkhorse-Fraternity%2Faw-mcu-wokwi-demo%2Ffbfe1eafa021f498e8efbd4fed9142f4d389f3f5%2Fdiagram.json&firmware=https%3A%2F%2Fraw.githubusercontent.com%2FDarkhorse-Fraternity%2Faw-mcu-wokwi-demo%2Ffbfe1eafa021f498e8efbd4fed9142f4d389f3f5%2Ffirmware.bin)

使用 Wokwi 官方 Viewer 加载本仓库的 `diagram.json` 与 `firmware.bin`，
即可在浏览器操作，不需要安装工具链或上传本地文件。

新版屏幕移植了原型的椭圆眼睛、笑嘴和眨眼：中文状态、大号计时/进度、响铃动画与双耳提示。
调试输出统一放在串口。初始熄屏正常，按耳朵会亮屏，15 秒无交互再熄屏。

串口按行输入：

- `down 30`：准备 30 秒倒计时，再按右耳开始；右耳短按暂停/继续，长按约 1 秒结束。
- `up`：准备正向计时。新计时前先长按右耳结束旧计时。
- `remind 3`：3 秒后响铃；右短按贪睡 5 分钟，右长按结束本次。
- `advance 300000`：加速 5 分钟，方便检查贪睡。
- 左耳短按：开始/提交模拟指令；处理期间输入 `reply query` 返回模拟查询结果。
- 指令期间右耳短按：取消指令并暂停当前活动。
- `journal`：选择模拟日记，再按右耳看笑脸和周期眨眼；短按暂停/继续，15 秒熄屏后录音灯仍亮。
- `reset`：清空本次仿真数据。
- `test`：执行 25 项交互断言，成功输出 `AW_TEST_PASS`。

模拟板：ESP32-S3、4 MiB Flash、SSD1306 显示占位、双耳按键、录音灯、蜂鸣器。
每次采音租约只注入一帧合成 PCM；这不是语音识别、真实录放音、物理持久化或续航验证。

固定构建使用 ESP-IDF 5.5.4、Arduino-ESP32 3.3.12 和共享 Rust Xtensa 核心。
`firmware.bin` SHA-256：

```text
c0fd617a322710c3b3bd88e2ca2251df8ae5c071c22986622a241f50e8eab7bd
```
