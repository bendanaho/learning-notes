# WebRTC

- **学习时间**：2026-10-14 ~ 2026-10-16，2026-11-10 归档
- **主要材料**：[WebRTC for the Curious](https://webrtcforthecurious.com/)（第 1、8 章）、MDN [WebRTC API](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API) 与 [Signaling and video calling](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/Signaling_and_video_calling)
- **目标**：先掌握"功能 + 怎么用"（把 WebRTC 当黑箱用起来），协议实现细节之后视需要再学

## 学到哪里

| 部分 | 内容 | 状态 |
|---|---|---|
| A 能做什么 | 两层身份（API + 协议）、四步（信令 / 连接 / 加密 / 通信）、NAT 与 ICE、三种能力与五个卖点、应用场景、拓扑（Mesh / SFU / MCU） | ✅ 已完成 |
| B 怎么用 | B1 核心 API 与四步的对应 | 已讲，检查点未做 |
| | B2 Offer/Answer 与候选交换、B3 完整应用的组成 | 未开始（"候选逐个发送 / Trickle ICE"已提前讲过） |
| C 动手 | 跑官方 webrtc samples + `chrome://webrtc-internals` | 未开始 |
| E 横向对比 | E1 传消息/数据：HTTP 轮询 / SSE / WebSocket / WebTransport / DataChannel | ✅ 已完成 |
| | E2 传音视频：RTMP / HLS·DASH / RTSP / WebRTC | 未开始 |
| F 模拟提问 | 覆盖 A、B、E 的自检问答 | 未开始 |
| 实现细节层 | 书第 2~7 章：SDP、NAT/ICE、DTLS/SRTP、RTP/RTCP、拥塞控制、SCTP | 未开始 |

**下次接着学的顺序**：E2 → B1 检查点 → B2 剩余部分 → B3 → C → F。

## 文件说明

| 文件 | 内容 |
|---|---|
| [学习笔记.md](学习笔记.md) | 自己整理的笔记（A 部分 + E1），由 `original/WebRTC学习笔记.docx` 转换 |
| [学习清单.md](学习清单.md) | 完整学习路线：第一层（功能与用法）+ 实现细节层，每部分读什么、验收标准 |
| [学习档案-WebRTC.md](学习档案-WebRTC.md) | 学习进度档案：已确认的知识、待讲的黑箱、踩过的坑 |
| [学习档案-网络分层.md](学习档案-网络分层.md) | 前置知识：计算机网络分层、TCP/UDP、socket |
| `images/` | 笔记中的图：ICE 流程、拓扑选型、数据传输技术对比表 |
| `original/` | 原始 Word 笔记 |

## 最容易出错的几个点（复习时先看）

- **TURN 是解药，不是病因**：直连不通又没配 TURN 才会连不上；配了 TURN 能连上，代价是延迟和服务器流量。
- **ICE 是总指挥，STUN / TURN 是它的工具**：三类候选同时收集，TURN 只是选路优先级最低，不是"失败了才去找"。
- **实时 ≠ 可以丢**：判断能否用不可靠传输，要看丢掉的数据能不能被后面的新数据替代（摇杆角度可以丢，弹幕、AI 逐字输出不能丢）。
- **四步不是严格的时间先后**：信令里的候选地址来自 ICE 收集，现在主流做法是 Trickle ICE，即找到一个候选就通过信令发一个。
