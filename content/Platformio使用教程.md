#### Arduino IDE 非常难用，所以使用 platformio 替代，但是 vsc 插件版本的 platformio 安装速度，创建速度都有很大问题，所以使用 cli 形式工具链代替，无法使用gui，此处以 linux 为例
---

##### **tags:**  #软件教程 
##### **2026-03-17**

---
## 下载
- 使用 [[UV使用教程|UV]] 下载，也可以直接使用 `pip`，但是污染行为不作为讲解
- `uv tool install platformio`
- 为工具链补充一个 `pip`，防止报错
```
~/.local/share/uv/tools/platformio/bin/python -m ensurepip
```

## 使用
- 创建指定 `board` 以及 `framework` 的工程
```
pio project init --board xxx --project-option "xxx"
```
- **为项目生成 `vsc` 的代码补全文件**，非常重要，不生成无法使用代码补全
```
pio project --ide vscode
```
- 测试是否安装成功 `pio --version`
- 查看端口：`pio device list`
- 编译：`pio run`
- 烧录/使用指定端口烧录：`pio run -t upload/pio run -t upload --upload-port /dev/ttyACMx`
- 开启监视：`pio device monitor`
- 指定波特率监视：`pio device monitor -b xxx`
- 一键编译烧录开启监视：`pio run -t upload -t monitor`
- 端口，波特率以及各种设置是可以直接固定在 `platformio.ini` 配置文件中的
```
monitor__speed=115200
```

## 示例
- 以 `esp32s3` 环境搭建为例
- 生成项目
```
pio project init --board esp32-s3-devkitc-1 --project-option "framework=arduino"
```
- 修改配置文件，开启 cdc，使用 usb 模拟 uart 串口输出
```
[env:esp32-s3-devkitc-1]
platform = espressif32
board = esp32-s3-devkitc-1
framework = arduino

build_flags = 
    -D ARDUINO_USB_CDC_ON_BOOT=1
    
monitor_speed=115200
upload_port=/dev/ttyACM*
```
- 启动：`pio run -t upload -t monitor`