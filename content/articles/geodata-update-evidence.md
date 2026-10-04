---
title: "OpenClash GEO 数据更新失败：分清 MMDB、GeoIP 与 GeoSite"
category: "tutorials"
description: "对应 0.47.156 的数据库更新脚本，判断 HTML 响应、文件过小、无变化与更新成功，核对规则实际使用的数据类型。"
date: "2026-10-04"
updated: "2026-10-04"
author: "OpenClash 中文指南编辑部"
draft: false
---

订阅节点更新成功，并不表示 GEO 数据库也更新成功。两者的下载内容和用途不同。数据库更新报错时，应先确认具体文件与实际使用的规则，而不是反复重新导入节点订阅。

## 先确认更新的是哪类文件

| 数据文件 | 用途与核对重点 |
| --- | --- |
| `Country.mmdb` | IP 地理数据；核对是否采用 MMDB 路径 |
| `GeoIP.dat` | Dat 形式的 IP 分类；与 MMDB 是不同格式 |
| `GeoSite.dat` | 域名分类数据；核对需要的分类是否由该数据源提供 |
| `ASN.mmdb` | ASN 数据；不要把它当作 GeoSite 域名列表 |

不能通过改扩展名把 MMDB 变成 Dat。即使同名 `GeoSite.dat` 下载成功，也不代表不同发布者提供完全相同的分类标签。应对照运行配置中的 GEO 相关规则及内核加载设置，确认文件类型与分类名称相匹配。

## 在插件中只更新目标数据库

本文于 2026-10-04 核验 OpenClash 0.47.156。进入“服务 → OpenClash → 插件设置 → GEO 更新”，记录对应数据库的来源和更新时间。修改自定义来源前保留原地址与可用文件，按该项的更新按钮刷新，随后查看插件日志。

自定义 URL 应指向实际的数据文件，而不是仓库首页、登录页或下载介绍页。订阅服务返回的节点配置也不能填到 GEO 下载地址中。检查来源时不要把带访问令牌的地址粘贴到公开反馈里。

## 如何阅读固定版本更新结果

0.47.156 的 `openclash_geo.sh` 不只判断 HTTP 下载有没有完成。它会检查响应内容、文件大小，并比较新旧文件。

| 日志结果 | 能得出的结论 | 下一步 |
| --- | --- | --- |
| HTML Response Detected | 开头被识别为 HTML | 检查是否下载到登录页、错误页或网页链接 |
| File Size Too Small | 文件小于脚本的最低检查值 | 核查下载是否截断，或来源返回了短错误文本 |
| No Change | 没有发现需要替换的内容 | 不必把未重启视作失败，继续核对现有文件与规则 |
| Update Successful | 数据已替换，脚本设置重启标志 | 等待服务恢复，再验证相关规则 |
| Update Error | 下载流程未满足成功条件 | 保存错误时间、目标类型与访问现象 |

大小检查不是完整的格式验证，更不是证明分类内容适合你的规则。文件替换成功后仍要确认内核启动正常，相关分类能够加载。

## 更新后的验收

先确认运行中的配置仍是原先那一份，再选一个原有规则应覆盖的目标建立新请求，核对命中的规则和出口。若日志出现分类不存在或数据库加载错误，记录分类名与数据源版本，不用增加一条兜底规则来掩盖加载失败。

启用小闪存模式时还需确认实际数据所在的临时目录；文件路径的含义见[小闪存与重启依赖](/articles/small-flash-reboot/)。本次排查只修改一个数据库来源；若需恢复，按已保存的来源和文件准备回退，避免同时更新插件、内核与全部数据。

## 来源

- [0.47.156 GEO 更新实现](https://github.com/vernesong/OpenClash/blob/v0.47.156/luci-app-openclash/root/usr/share/openclash/openclash_geo.sh)
- [0.47.156 GEO 设置页](https://github.com/vernesong/OpenClash/blob/v0.47.156/luci-app-openclash/luasrc/model/cbi/openclash/settings.lua)
- [Mihomo 通用配置：地理数据选项](https://wiki.metacubex.one/config/general/)

日志分支依据源码整理，例表不代表已运行你的数据库更新。本站未进行路由器或节点实测。
