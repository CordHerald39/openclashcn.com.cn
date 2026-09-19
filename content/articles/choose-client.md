---
title: "OpenClash 安装前：OpenWrt 固件与依赖检查"
category: "tutorials"
description: "先核对路由器环境，避免把插件当成桌面客户端。"
date: "2026-09-19"
updated: "2026-09-19"
author: "OpenClash 中文指南编辑部"
draft: false
---

## 确认软件层级

OpenClash 是用于 OpenWrt 的客户端插件，通常通过 LuCI 进行管理。它不是给 Windows 或 Android 双击安装的软件。

## 核对固件与空间

记录固件版本、包管理方式、CPU 架构与可用存储。依赖及包格式可能随固件变化，按项目 Wiki 与当前发行说明选择。

## 保留管理入口

安装或调整网络前备份配置，并确保仍能进入路由器管理界面。不要在不清楚回退方式时同时变更 DNS、网关和防火墙。

## 参考来源

- [原始项目仓库](https://github.com/vernesong/OpenClash)
- [OpenClash 项目 Wiki](https://github.com/vernesong/OpenClash/wiki)
