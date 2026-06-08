---
title: 微星Wake-on-LAN(Wol)网络唤醒
published: 2026-03-20
image: api
category: 折腾
tags: [BIOS, 网络唤醒]
---

# 微星Wake-on-LAN(Wol)网络唤醒

经过多次尝试设置无果，且关机网口连接灯不亮（最后成功WOL唤醒，灯也是不亮的）。成功唤醒后发现，最最重要的是一定要用MSI LIVE UPDATE里面的网卡驱动，Win 10 自带的网卡驱动无法实现WOL。

## 🔧BIOS设置

1. 高级–整合周边设备–网卡ROM启动，设置为允许
2. 高级–电源管理设置–ErP Ready (老主板:Eup 2013)，设置为禁止
3. 高级–唤醒事件设置–PCIE设备唤醒，设置为允许

## 💻Windows 系统设置

1. **打开设备管理器**：右键点击“开始”菜单，选择“设备管理器”。
2. **配置网络适配器**：
   - 展开“网络适配器”，右键点击您的网卡，选择“属性”。
   - 在“高级”选项卡中，确保以下选项设置为“启用”：
     - `Wake on Magic Packet`（魔术封包唤醒）
     - `Shutdown Wake-On-Lan`（关机唤醒）
   - 在“电源管理”选项卡中，勾选以下选项：
     - `允许此设备唤醒计算机`
     - `仅允许魔术封包唤醒计算机`（如果可用）

## ⌨️Linux安装发送唤醒包工具

1. **安装 `wakeonlan` 工具**

`wakeonlan` 是一个 Perl 脚本，也可用于发送 WOL 魔术包。

大多数主流 Linux 发行版都可以直接通过包管理器安装：

对于 **Debian / Ubuntu** 系：

```bash
sudo apt update
sudo apt install wakeonlan
```

对于 **RHEL / CentOS / Rocky / AlmaLinux** 系：

默认仓库中没有，建议使用 `cpan` 安装：

```bash
sudo yum install perl
sudo cpan Net::Wake
```

或者先安装 `epel-release` 然后尝试搜索包：

```bash
sudo yum install epel-release
sudo yum install wakeonlan
```

对于 **Arch Linux / Manjaro**：

```bash
sudo pacman -S wakeonlan
```

------

2. **手动安装（如果包管理器没有）**

你也可以手动下载 Perl 脚本：

```bash
# github: github.com/jpoliv/wakeonlan
wget https://raw.githubusercontent.com/jpoliv/wakeonlan/master/wakeonlan
chmod +x wakeonlan
sudo mv wakeonlan /usr/local/bin/
```

------

3. **使用方式**

使用非常简单，只需要目标设备的 MAC 地址：

```bash
wakeonlan AA:BB:CC:DD:EE:FF
```

------

## ⚠️ 注意事项

- **网卡驱动**：确保安装了最新的官方网卡驱动程序。使用 MSI 提供的驱动程序，而非 Windows 自带的驱动程序，以确保 WOL 功能的正常运行。
- **快速启动**：在 Windows 中关闭“快速启动”功能，以避免影响 WOL 的效果。
- **wakeonlan**：使用手动安装更加方便。
