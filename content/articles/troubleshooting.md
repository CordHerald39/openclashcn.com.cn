---
title: "OpenClash DNS 排错：先画清楚解析路径"
category: "tutorials"
description: "区分终端、路由器与上游解析环节。"
date: "2026-09-19"
updated: "2026-09-19"
author: "OpenClash 中文指南编辑部"
draft: false
---

## 记录 DNS 来源

确认终端通过 DHCP、手动设置还是应用自己的加密 DNS 获得解析服务。不同路径会导致同一域名出现不同表现。

## 避免解析循环

检查路由器 DNS 服务与插件 DNS 的上游指向，确保请求不会在两者之间循环。修改之前记录原始端口与地址。

## 逐步恢复验证

一次只修改一处 DNS 设置，先验证域名解析，再检查规则匹配和连接。必要时按事先备份恢复，不盲目叠加多个教程的配置。

## 参考来源

- [原始项目仓库](https://github.com/vernesong/OpenClash)
- [OpenClash 项目 Wiki](https://github.com/vernesong/OpenClash/wiki)
