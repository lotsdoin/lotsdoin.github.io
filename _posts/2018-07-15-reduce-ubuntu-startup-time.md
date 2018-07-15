---
layout: post
title: 加快 ubuntu 启动速度
categories: [Linux]
tags: 系统
---

## 查看 Linux 启动时间消耗
```bash
$ systemd-analyse blame
         25.080s dev-sda3.device
         17.040s docker.service
         14.322s dev-loop8.device
         13.788s proftpd.service
         12.728s plymouth-quit-wait.service
         10.380s systemd-fsck@dev-disk-by\x2duuid-64046202\x2d735b\x2d419b\x2d8436\x2de4509642a4af.service
         10.097s systemd-fsck@dev-disk-by\x2duuid-a22c885a\x2dd6f9\x2d4063\x2d87b3\x2d8db0bd571df4.service
          6.676s NetworkManager-wait-online.service
          6.624s snapd.seeded.service
          6.210s mysql.service
          6.053s snapd.service
          5.575s ModemManager.service
```

## 查看 Linux 开机启动服务
```bash
$ systemctl list-unit-files --type=service | grep enable
```

## 停止开机启动服务
```bash
$ sudo systemctl stop bluetooth.service
$ sudo systemctl disable bluetooth.service
# 验证是否成功
$ systemctl status bluetooth.service
# 停用的服务进程仍然能够被另外一个服务进程启动。如果你真的想在任何情况下系统启动时都不启动该进程，无需卸载该它，只需要把它掩盖起来就可以阻止该进程在任何情况下开机启动。
$ sudo systemctl mask bluetooth.service
# 通过命令 journalctl -b -1 可以复审前一次启动，journalctl -b -2 可以复审倒数第 2 次启动，以此类推。
# 当前启动
$ journalctl -b
```
* accounts-daemon.service 是一个潜在的安全风险。它是 AccountsService 的一部分，AccountsService 允许程序获得或操作用户账户信息。我不认为有好的理由能使我允许这样的后台操作，所以我选择掩盖mask该服务进程。
* avahi-daemon.service 用于零配置网络发现，使电脑超容易发现网络中打印机或其他的主机，我总是禁用它，别漏掉它。
* brltty.service 提供布莱叶盲文设备支持，例如布莱叶盲文显示器。
* debug-shell.service 开放了一个巨大的安全漏洞（该服务提供了一个无密码的 root shell ，用于帮助 调试 systemd 问题），除非你正在使用该服务，否则永远不要启动服务。
* ModemManager.service 该服务是一个被 dbus 激活的守护进程，用于提供移动宽频broadband（2G/3G/4G）接口，如果你没有该接口，无论是内置接口，还是通过如蓝牙配对的电话，以及 USB 适配器，那么你也无需该服务。
* pppd-dns.service 是一个计算机发展的遗物，如果你使用拨号接入互联网的话，保留它，否则你不需要它。
* rtkit-daemon.service 听起来很可怕，听起来像是 rootkit。 但是你需要该服务，因为它是一个实时内核调度器real-time kernel scheduler。
* whoopsie.service 是 Ubuntu 错误报告服务。它用于收集 Ubuntu 系统崩溃报告，并发送报告到 https://daisy.ubuntu.com 。 你可以放心地禁止其启动，或者永久的卸载它。
* wpa_supplicant.service 仅在你使用 Wi-Fi 连接时需要。

## 修改虚拟内存交换效率
```bash
$ cat /proc/sys/vm/swappiness
60
$ sudo vim /etc/sysctl.conf
# vm.swappiness = 10
```

## 修改 grub2 等待时间
```bash
# 本人双系统, 减少系统确认时间
$ sudo vim /etc/default/grub
```
## 多核启动
```bash
$ sudo vim /etc/init.d/rc
# CONCURRENCY=makefile
```
