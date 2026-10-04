---
title: "OpenClash 小闪存模式：为什么重启后又要下载内核？"
category: "tutorials"
description: "核对 0.47.156 的小闪存文件路径，区分闪存不足、内存不足与重启后的下载依赖，给出启用及恢复检查。"
date: "2026-10-04"
updated: "2026-10-04"
author: "OpenClash 中文指南编辑部"
draft: false
---

小闪存模式把部分运行文件搬到临时目录，适合需要节省持久存储的设备。但如果路由器重启后必须靠代理才能下载内核，就可能形成启动依赖：内核尚未准备好，代理也无法先工作。启用前需要验证下载路径。

## 先分清两种容量

在路由器的 SSH 会话中，可用以下只读命令查看当前容量；不要在自己的 Windows 电脑上运行它们。

```sh
df -h / /tmp /etc/openclash
free -m
```

根文件系统剩余空间、临时目录容量和可用内存不是同一个数。把文件放入 `/tmp` 并不增加 RAM；在常见 OpenWrt 环境中，临时目录会占用内存且不跨重启保留。应结合设备实际挂载结果判断。

## 正式版把哪些文件搬走

核验日期为 2026-10-04。0.47.156 的模式设置提供 `small_flash_memory` 开关；固定版本启动脚本在启用时使用以下临时路径，并为部分数据文件建立链接。

| 文件 | 启用后的目标位置 |
| --- | --- |
| Mihomo 内核 | `/tmp/etc/openclash/core/clash_meta` |
| IP 数据库 | `/tmp/etc/openclash/Country.mmdb` |
| 域名分类数据 | `/tmp/etc/openclash/GeoSite.dat` |
| GeoIP Dat | `/tmp/etc/openclash/GeoIP.dat` |

看到 `/etc/openclash` 中仍有同名链接，不代表真正的数据还存在。重启后要核对链接目标与插件下载日志，不能只检查文件名是否显示。

## 启用前完成一个可恢复的试验

1. 备份当前插件配置，保留无需代理也能打开的路由器管理入口。
2. 在“服务 → OpenClash → 插件设置 → 模式设置”确认小闪存开关。先记录原值，再只修改这一项并应用。
3. 查看启动日志，确认内核和实际使用的数据文件已准备好；用一台终端建立新请求。
4. 选择可现场恢复的时段重启路由器，观察下载是否能在代理尚未启动时完成。分别记录“下载完成”和“内核启动”的时间。

此流程是建议的验收方法，本站没有在你的路由器上完成试验。第一次启动成功而重启失败，优先保留重启后的下载错误与临时目录状态，避免重复开关多个网络选项。

## 什么时候应退出小闪存模式

若临时目录或内存余量不足，或重启后下载长期失败，重新评估设备资源和持久存储。关闭开关前先确认根文件系统有足够空间；0.47.156 启动脚本包含把临时文件迁回 `/etc/openclash` 的处理，迁移后仍需核对日志及实际文件。

不要手工删除链接、内核和数据库来“清空间”。先保留当前能工作的配置与下载来源，再通过插件设置恢复。下载和文件准备都正常但终端访问异常时，继续阅读[局域网终端排查](/articles/openclash-lan-check/)。

## 来源与边界

- [OpenClash 0.47.156 正式发布页](https://github.com/vernesong/OpenClash/releases/tag/v0.47.156)
- [0.47.156 模式设置源码](https://github.com/vernesong/OpenClash/blob/v0.47.156/luci-app-openclash/luasrc/model/cbi/openclash/settings.lua)
- [0.47.156 启动与文件迁移源码](https://github.com/vernesong/OpenClash/blob/v0.47.156/luci-app-openclash/root/etc/init.d/openclash)

文件路径据固定版本源码核对；容量判断和试验顺序为本站整理，未声称硬件实测或性能提升。
