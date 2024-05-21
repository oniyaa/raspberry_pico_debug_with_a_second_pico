# 树莓派 Pico 调试另一个 Pico

## 1. 环境安装
-----

###### 1. 下载安装脚本

```
wget https://raw.githubusercontent.com/raspberrypi/pico-setup/master/pico_setup.sh
```

###### 2. 更改为可执行文件

```
chmod +x pico_setup.sh
```

###### 3. 执行脚本

```
./pico_setup.sh
```

###### 4.安装成功后，重新启动
```
sudo reboot
```

## 2.按图连接条线 （左 Pico A，右 Pico B）
-------------
![alt text](PICO_A_TO_PICO_B.png)

## 3.烧录 Pico A
----------------
1. 下载 [debugprobe_on_pico.uf2](https://github.com/raspberrypi/debugprobe/releases/download/debugprobe-v2.0.1/debugprobe_on_pico.uf2) 文件
2. 拷贝`debugprobe_on_pico.uf2`到`Pico A`
* 按住 `BOOTSEL` 按钮，将连接电脑`USB`线插入`Pico A`
* 出现`RPI-RP2`盘符，将`debugprobe_on_pico.uf2`文件拖拽到`RPI-RP2`盘符
3. `Pico A` 自动重启
## 4.调试 Pico B
----------------
###### 1. 编译`blink.c`

```
cd ~/pico/pico-examples/
rm -rf build
mkdir build
cd build
export PICO_SDK_PATH=../../pico-sdk
cmake -DCMAKE_BUILD_TYPE=Debug ..
cd blink
make -j4
```

###### 2. 烧录到 `Pico B`
```
sudo openocd -f interface/cmsis-dap.cfg -f target/rp2040.cfg -c "adapter speed 5000" -c "program blink.elf verify reset exit"
```

###### 命令参数说明

| 参数 | 功能 |
|---|---|
| cmsis-dap.cfg | 调用的类型文件（Pico A） |
| rp2040.cfg | 调用的类型文件（Pico B） |
| adapter speed 5000 | 速率 |
| program blink.elf | 程序 blink.elf |
| verify | 校验 |
| reset | 重置 |
| exit | 退出 |

###### 3. 查看灯是否闪亮  

<details>
<summary>更多例子</summary>

###### cd 到 ~/pico/pico-examples/build 目录下，编译全部实例
```
cd ~/pico/pico-examples/build
make -j4
```
* ###### 烧录 blink 目录下 blink.elf 文件
```
sudo openocd -f interface/cmsis-dap.cfg -f target/rp2040.cfg -c "adapter speed 5000" -c "program blink/blink.elf verify reset exit"
```
* ###### 烧录 hello_world 目录下 serial 下的 hello_serial.elf 文件
```
sudo openocd -f interface/cmsis-dap.cfg -f target/rp2040.cfg -c "adapter speed 5000" -c "program hello_world/serial/hello_serial.elf verify reset exit"
```
</details>  


###### 更多参考PDF [getting-started-with-pico.pdf](getting-started-with-pico.pdf) , [openocd.pdf](openocd.pdf)文档  
