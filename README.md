# 拾音馆 Shi Yin Guan

一款用粤语声调讲故事的视觉小说。从一间收着旧唱片的铺子出发，经市场街、电车区、旧城、河岸与戏棚，把整座城的声音一寸寸拾回来。

- 引擎：Unity 6000.0.85f1 / URP 17.0.4
- 平台：Windows 10 / 11 · x64（开发试玩 v0.5）
- 规模：36 段情境任务 · 13 个区域 · 150 汉字 / 161 读音 · 2040 音节

## 线上地址

| 用途 | 地址 |
|---|---|
| 主站（Cloudflare Pages） | https://shiyinguan.pages.dev |
| 备用站（GitHub Pages） | https://zhouxuan078-max.github.io/shiyinguan/ |

`index.html` 与 `assets/` 就是线上页面本体，纯静态，无构建步骤。
发布到 Cloudflare Pages：

```bash
npx wrangler pages deploy dist --project-name=shiyinguan --branch=main
```

（`dist/` 由 `index.html` + `assets/` 组成，不含 `.git`。）

## 下载

[Release v0.5-2026-10-28](https://github.com/zhouxuan078-max/shiyinguan/releases/tag/v0.5-2026-10-28) 里的 `ShiYinGuan-win64.zip`（198 MB），解压后双击 `ShiYinGuan.exe`。便携版，无需安装。

直链：`https://github.com/zhouxuan078-max/shiyinguan/releases/download/v0.5-2026-10-28/ShiYinGuan-win64.zip`

## 操作

鼠标为主。← → 切换地点，Enter / E 进入；Tab 拾音册，Esc 设置。启动加 `--yue-3d` 回到旧的 3D 行走版。

## 说明

当前美术为待替换开发稿，粤语内容仍需母语审校；音高播放为纯音示意，暂无录音采集与发音评分。剧情与时刻表为虚构教学情境。

## 关于浏览器版

Unity WebGL 构建需要额外安装 WebGL Build Support 模块，且当前资源体量（`ShiYinGuan_Data` 约 339 MB）远超 Cloudflare Pages 的单文件 25 MiB 上限。若要做真正可玩的网页版，需要先把美术资源压到十分之一以下，或把大文件放到 R2 / COS 再分片加载。目前线上只做介绍与下载。
