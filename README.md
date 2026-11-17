# 烽火 HG6145D / HG6145D1 工具与固件

配套文章：[烽火 HG6145D 光猫：开 Telnet、取超密与改省份](https://xiaozou123.cn/fh-hg6145d-telnet-super-password/)

HG6145D 是烽火给移动、联通、电信做的定制光猫，D1 是量最大的那批。它的入口不是 exe，而是一个 GET 请求：用 MAC 拼出地址，打开 Telnet，再进工厂模式把超管密码读出来。仓库只放工具、固件和原始教程。

## 目录

- `tools/tftpd64.exe`：TFTP 服务端，导配置用
- `tools/一键获取烽火光猫超密.exe`：界面化的读超密工具
- `tools/烽火光猫密码解算工具.exe`：解 U 盘导出的加密配置
- `firmware/MUT_eng.exe`：英文版整机固件的刷写工具
- `firmware/multi_image_nvram0x2.ini` / `.md5` / `.sha1` / `conf.dat`：镜像的元数据与组播参数，**刷机时必须和 bin 放在同一目录**
- `docs/烽火HG6145D获取管理员密码.txt`：论坛原帖存档，含移动/联通两套取超密和改省份流程
- `docs/HG6145D刷英文和改码教程.doc`：厂商的批量刷机说明
- `multi_image_nvram0x2.bin`：52,039,314 字节的英文版整机固件，**体积原因放在 Releases**

## 文件与 SHA256

| 文件 | 字节 |
|---|---|
| `tools/tftpd64.exe` | 146,656 |
| `tools/一键获取烽火光猫超密.exe` | 50,176 |
| `tools/烽火光猫密码解算工具.exe` | 262,144 |
| `firmware/MUT_eng.exe` | 778,240 |
| `firmware/multi_image_nvram0x2.ini` | 310 |
| `firmware/multi_image_nvram0x2.md5` | 59 |
| `firmware/multi_image_nvram0x2.sha1` | 67 |
| `firmware/conf.dat` | 106 |
| `docs/烽火HG6145D获取管理员密码.txt` | 5,105 |
| `docs/HG6145D刷英文和改码教程.doc` | 124,928 |
| `multi_image_nvram0x2.bin`（Release） | 52,039,314 |

```
988292db5e71fa50b090ce2bfe66813791d944cde64c19b7f0ecda8892392a15  tools/tftpd64.exe
787848aa26c91eaa1c689fc8e9ffe8dc11af64e51c87d1d5797d0df2e75c9286  tools/一键获取烽火光猫超密.exe
30a9cdeaa43e329e137555499f5e498ad275eef3391b6eea3bf08f25aa4531ba  tools/烽火光猫密码解算工具.exe
cc64efa842d3c44e545ad236c24d9d8d82a81698e18935491092d118783722c8  firmware/MUT_eng.exe
bda58b83a8b4e216d482cb53a205d0ba77622938b7d6138eeddc19376e91873e  firmware/multi_image_nvram0x2.ini
806a7a7e123ca71b421a1e78a93711a26aaeaf67b07294fbaf3d4a30cdbb5821  firmware/multi_image_nvram0x2.md5
7b98f9032a510ac00af7c57b9205e028f18d4a224d74fd9ae4d8dde255843848  firmware/multi_image_nvram0x2.sha1
2ccfeccd480bb4bad5f87015a8754c8958c7a1ef1a65887afea84008c91c59a6  firmware/conf.dat
7b3218e1459fd8095b29ad2278d2a8a493aa3f1dceb70ccd964e7d2732c0a52e  docs/烽火HG6145D获取管理员密码.txt
fe93ba5d2d93fec9c07259855cc1b42009d13449be9a34e7ed6761ec1657adeb  docs/HG6145D刷英文和改码教程.doc
74e92412b881cc918d6bb10825555ced93ace47ac04e797b750b939e8cd345d4  multi_image_nvram0x2.bin
```

## 使用顺序

1. 电脑接 LAN1，网卡设 `192.168.1.x`，执行 `arp -a 192.168.1.1` 拿到 MAC。
2. 浏览器打开 `http://192.168.1.1/cgi-bin/telnetenable.cgi?telnetenable=1&key=<MAC>`（移动）或 `http://192.168.1.1/telnet?enable=1&key=<MAC>`（联通）。
3. Telnet 登录：移动 `admin`、联通 `root`，密码 `Fh@` 加 MAC 后 6 位。
4. `load_cli factory` → `show admin_name` → `show admin_pwd`。
5. 要改省份再执行 `load preconfig <省份全拼>`，重启后按文章里的步骤补 VLAN。

## 关于这个固件

`multi_image_nvram0x2.bin` 是英文版整机镜像，元数据里 `FIRMWARE_VERSION=0`、`PACKAGE_ID=fnt-HGW`，属于产线和维修用的裸包，不是网页升级能用的格式。刷写要用 `MUT_eng.exe` 走组播，多台并联；包里只有一个英文版镜像，**没有回退到中文版的包**。

## 免责声明

工具与固件均来自公开维修资料，仅供自有设备维修与学习使用。刷机、改 SN/MAC、改省份都有变砖和断网风险，操作前先备份，后果自负。仓库不含任何真实设备的配置备份。
