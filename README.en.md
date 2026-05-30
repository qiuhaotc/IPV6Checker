# IPv6 Network Checker

English | [中文](README.md)

**Live demo:** [https://qiuhaotc.github.io/IPV6Checker/ipv6-check.html](https://qiuhaotc.github.io/IPV6Checker/ipv6-check.html)

---

## Overview

A pure front-end single-page tool that detects whether the current network supports IPv6 using two independent methods. No backend required.

---

## How It Works

### Method 1: Public IPv6 Connectivity Test

Tries the following **IPv6-only** endpoints in sequence (passes if any succeeds):

| Endpoint | Notes |
|----------|-------|
| `https://v6.ident.me` | IPv6-only, returns plain-text public IP |
| `https://ipv6.icanhazip.com` | IPv6-only Cloudflare node |
| `https://ipv6.lookup.test-ipv6.com/ip/` | Official test-ipv6.com API |

Each request times out after **7 seconds**. A response is only considered valid if it contains a colon (`:`), confirming it is an IPv6 address.

### Method 2: WebRTC ICE Candidate Collection

Uses the browser `RTCPeerConnection` API to create a data channel and gather ICE candidates, then extracts local IPv6 addresses from the candidate strings. Google STUN servers (`stun.l.google.com:19302`) are used to assist. IPv4-mapped addresses (`::ffff:x.x.x.x`) are filtered out; link-local addresses (`fe80::`) are labeled separately. Collection times out after **6 seconds**.

### Verdict Logic

```
Public connectivity OR local IPv6 found → IPv6 supported
Both methods fail                        → IPv6 not supported
```

---

## Tech Stack

- **Vue 3** (CDN, `vue.global.prod.js`)
- **Element Plus** (CDN, component library + CSS)
- Single HTML file, no build step required

---

## Caveats

- WebRTC address collection requires browser support; private/incognito mode or strict security policies may limit results.
- External API tests require public internet access; firewalls or proxies may affect results.
- Public connectivity only confirms access to the IPv6 internet, not that all local interfaces have IPv6.

---

## Deployment

Served via GitHub Pages (legacy mode) directly from the `main` branch root directory. No build pipeline needed.
