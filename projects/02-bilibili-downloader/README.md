# BibiliDown

> B 站视频下载工具，Electron + React + TypeScript 重写自 Java Swing 版；浏览器与桌面共用一套代码，当前 v7.7.4。
>
> 独立仓库：<https://github.com/Luo-Feng6/bibilidown>

## 背景与目标

- 场景：需要把 B 站视频保存到本地，离线看、反复看
- 原来的做法：原项目 [nICEnnnnnnnLee/BibiliDown](https://github.com/nICEnnnnnnnLee/BibiliDown) 基于 Java Swing，功能完整；这一版是按原项目思路的完全重写
- 目标：换成 Electron + React + TypeScript，保留原项目的核心设计；同一套 React 代码同时支持浏览器和桌面两种形态

## 我做了什么

- 架构：Electron 主进程 + preload（contextBridge）+ React 渲染进程；服务层 8 个、Zustand store 8 个、组件 20 个、页面 3 个
- 解析：支持 7 种输入（BV/av、ep、ss、md、ml、b23.tv 短链、完整 URL）；处理 B 站 WBI 签名（img_key + sub_key → MD5）
- 下载：DASH 音视频分离下载 + FFmpeg 合并；FLV 单流；多 P 自动拆分；浏览器端走 Blob 下载，Electron 端走 IPC 由主进程流式写盘
- 登录：QR 码 / 密码（含 GeeTest 滑块）/ 短信三种方式，Cookie 持久化
- 体验：亮色 / 暗色双主题 + 5 色强调色；下载方案预设（保存 7 项配置快照）；历史记录上限 500 条，支持 JSON 导入导出
- 工程：55 个单元测试（3 skipped），`tsc --noEmit` 零错误；LoginPanel 从 1131 行拆成 6 个文件（主文件 184 行）

## 结果

- 版本 v7.7.4，Apache-2.0 协议，代码在 GitHub 公开
- 测试 55 passed / 3 skipped，TypeScript 零错误
- `src` 下 TypeScript 源码 13,175 行（含测试，`wc -l` 统计）
- CHANGELOG 记录了 9 个版本，时间跨度 2026-07-01 至 2026-07-22
- 实际用户数、下载量：未统计

> 合规说明：仅供个人离线学习使用，不提供成品下载服务，不鼓励批量传播。

## 截图

截图：待补（主界面 / 解析结果 / 下载队列 / 设置页）

## 技术栈

- Electron 43 + React 18 + TypeScript 5（strict）
- Vite 5 + Tailwind CSS 3
- Zustand 5（状态管理与持久化）
- Vitest 4 + Testing Library（单元测试）
- electron-builder 26（Windows NSIS / 便携版，macOS dmg，Linux AppImage）
- Phosphor Icons、spark-md5、jsencrypt、qrcode

## 如何运行

```bash
git clone https://github.com/Luo-Feng6/bibilidown.git
cd bibilidown
npm install

# 浏览器模式，打开 http://localhost:5173
npm run dev

# Electron 桌面模式
npm run dev:electron

# 运行测试
npm run test
```

- 环境要求：Node.js ≥ 18；FFmpeg 可选，桌面版会自动检测
- 不想配环境的话，可以直接从仓库的 Releases 页面下载安装包
