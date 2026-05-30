# IPv6 网络检测工具

[English](README.en.md) | 中文

**在线地址：** [https://qiuhaotc.github.io/IPV6Checker/ipv6-check.html](https://qiuhaotc.github.io/IPV6Checker/ipv6-check.html)

---

## 简介

一个纯前端的单页工具，综合两种方式检测当前网络是否支持 IPv6，无需后端服务。

---

## 检测原理

### 方式一：公网 IPv6 连通性测试

依次尝试连接以下 **IPv6-only** 外部接口（任一成功即通过）：

| 接口 | 说明 |
|------|------|
| `https://v6.ident.me` | IPv6-only，返回纯文本公网 IP |
| `https://ipv6.icanhazip.com` | IPv6-only Cloudflare 节点 |
| `https://ipv6.lookup.test-ipv6.com/ip/` | test-ipv6.com 官方 API |

每个请求超时 **7 秒**。返回结果含冒号（`:`）才视为有效 IPv6 地址。

### 方式二：WebRTC ICE 候选地址采集

通过浏览器 `RTCPeerConnection` API 创建数据通道并收集 ICE 候选，从候选字符串中提取本地 IPv6 地址。使用 Google STUN 服务器（`stun.l.google.com:19302`）辅助。过滤掉 IPv4-mapped 地址（`::ffff:x.x.x.x`），链路本地地址（`fe80::`）单独标注。采集超时 **6 秒**。

### 综合判断逻辑

```
公网连通 OR 存在本地 IPv6 → 支持 IPv6
两者均失败                 → 不支持 IPv6
```

---

## 技术栈

- **Vue 3**（CDN，`vue.global.prod.js`）
- **Element Plus**（CDN，组件库 + CSS）
- 纯 HTML 单文件，无构建步骤

---

## 注意事项

- WebRTC 地址采集需浏览器权限支持，隐私模式或严格安全策略可能限制结果。
- 外部 API 检测需访问公网，防火墙或代理可能影响结果。
- 公网连通性仅代表能访问 IPv6 互联网，不代表本地所有接口都有 IPv6。

---

## 部署

通过 GitHub Pages（legacy 模式）从 `main` 分支根目录直接提供静态文件，无需构建流程。
