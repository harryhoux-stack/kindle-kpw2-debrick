# Kindle Paperwhite 2 救砖实录与工具包

一台变砖的 Kindle Paperwhite 2（KPW2）被成功救活的完整记录。

## 背景

- 设备：Kindle Paperwhite 2（开发代号 `wario`，序列号 905A 开头）
- 故障：此前刷 CrackDroid 安卓系统失败，设备变砖
- 症状：USB 可被电脑识别，但磁盘显示 **0 字节、无分区（RAW）、无媒体**
- 结论：底层引导（U-Boot）完好，eMMC 上的分区表/系统被写坏 —— 属于"半砖"，无需换主板

## 修复方式（全程不拆机）

1. 设备进入 Android fastboot 模式（USB ID `18D1:4E40`）
2. 安装 Google USB 驱动（`android_winusb.inf`），让 Windows 识别 fastboot 设备
3. 从 kdroid.club 网页存档获取 CrackDroid 官方 KPW2 刷机包（`kpw2.191115.zip`，约 466MB）
4. 运行刷机包内 `fastdownload.exe`，选择 **选项 4「转换为原始系统(Kindle)」**
5. 工具自动写入原生系统镜像 `system.bin` 与内核 `kernel.bin`，设备自动重启
6. 重启后恢复为 Kindle 原生系统：MBR 分区、3.37GB FAT32、卷标 "Kindle"

## 修复后状态

| 项目 | 修复前 | 修复后 |
| --- | --- | --- |
| 分区表 | RAW（损坏） | MBR（正常） |
| 容量 | 0 字节 | 3.37GB 可读写 |
| 文件系统 | 无媒体 | FAT32（Kindle） |
| 系统版本 | 无 | Kindle 5.12.2 |
| 系统运行 | 无法启动 | MTP/WPD 便携设备正常识别 |

## 目录结构

```
├── README.md                    # 本文件
├── docs/
│   ├── 救砖指南.md              # 完整救砖步骤（方案 C 为实测成功路径）
│   ├── 修复结果确认.md          # 本次修复结果核验记录
│   └── 会话关键信息.md          # 本次排障全过程要点与命令
├── tools/                       # 救砖工具（均已上传，可直接下载）
│   ├── 软件清单.md              # 每个工具的用途、来源与获取方式
│   ├── zadig-2.9.exe            # 备用驱动安装工具（5.1MB）
│   ├── drivers/
│   │   └── usb_driver_r13-windows.zip   # Google USB 驱动，识别 fastboot（8.3MB）
│   ├── fastboot/
│   │   └── platform-tools.zip   # fastboot / adb 命令行工具（7.7MB）
│   └── mfgtool/
│       └── popcorn.7z           # MfgTool 备用引导工具（5.2MB）
└── reference/
    └── 测试点参考/              # 主板测试点照片（串口/短接方案用）
```

## 工具获取

- **已上传**：`tools/` 目录内的驱动与工具小于 GitHub 100MB 限制，可直接从仓库下载。
- **需在线下载**：CrackDroid 刷机包（约 466MB，含 `fastdownload.exe` 和系统镜像）与官方固件（约 227MB）超出 GitHub 单文件限制，下载链接见 [`tools/软件清单.md`](tools/软件清单.md) 第 2 节。

## 重要提醒

- 大型二进制（官方刷机包 466MB、官方固件 227MB）**不包含在本仓库**（超出 GitHub 单文件限制），获取方式见 `tools/软件清单.md`
- fastboot 会话有约 5 秒超时：若 `fastboot devices` 能看到设备但命令超时，需重新让设备进入 fastboot 后立即执行
- 本仓库仅用于技术学习与个人设备维护，操作前请确认你清楚自己在做什么

## 参考来源

- ZephRay《Kindle Paperwhite 2 强行救砖》系列文章
- MobileRead 论坛 CrackDroid 恢复经验帖
- kdroid.club（CrackDroid 官方站，部分页面已下线，经网页存档获取）
