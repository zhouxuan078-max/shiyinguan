# 拾音馆 Shi Yin Guan

一款用粤语声调讲故事的视觉小说。从一间收着旧唱片的铺子出发，经市场街、电车区、旧城、河岸与戏棚，把整座城的声音一寸寸拾回来。

- 引擎：Unity 6000.0.85f1 / URP 17.0.4
- 平台：Windows 10 / 11 · x64（开发试玩 v0.5）；另有浏览器版
- 规模：36 段情境任务 · 13 个区域 · 150 汉字 / 161 读音 · 2040 音节

## 线上地址

| 用途 | 地址 |
|---|---|
| 主站（Cloudflare Pages） | https://shiyinguan.pages.dev |
| 在线试玩（浏览器直接玩） | https://shiyinguan.pages.dev/play/ |
| 备用站（GitHub Pages，仅介绍页） | https://zhouxuan078-max.github.io/shiyinguan/ |

`index.html` + `assets/` 是介绍页，`play/` 是 WebGL 版；整个 `dist/` 直接静态托管，无构建步骤。

## 浏览器版是怎么塞进去的

Unity WebGL 打出来的数据文件 **194 MB**，而 Cloudflare Pages 免费版单文件上限 **25 MiB**，直接放放不进去。这里的做法：

1. `Build/WebGL.data.unityweb` 按 20 MB 切成 10 片，放在 `play/chunks/`；
2. `play/sw.js` 注册成 Service Worker，把 `/play/Build/WebGL.data.unityweb` 这个请求拦下来，按顺序取分片，用 `ReadableStream` 边取边喂给 Unity；
3. 对引擎来说，它请求的仍然是那个原始文件名，URL 与扩展名都没变，所以 gzip 解压回退照常工作。

好处是不用额外服务、同源、无 CORS、无单文件限制；代价是首次进入要下满 194 MB（之后走浏览器缓存）。

想换成 Cloudflare R2（无单文件上限），把 `play/chunks/*` 换成 R2 对象、给桶配 CORS 即可，`sw.js` 可整个删掉 —— R2 需要在控制台点一次「启用 R2」才能建桶，当前账号还没开。

## 本地发布

```bash
# 介绍页 + 试玩页一起发
npx wrangler pages deploy dist --project-name=shiyinguan --branch=main
```

重新产出 WebGL 包（改了 Unity 工程之后）：

```bash
# 需要先装 WebGL Build Support 模块
D:/Unity/Editor/6000.0.85f1/Editor/Unity.exe -batchmode -nographics \
  -projectPath D:/GameDev/YueProject \
  -executeMethod Yue.Editor.YueBuild.BuildWebGL \
  -logFile D:/GameDev/YueProject/build-webgl.log
```

注意：构建前必须 `unset NODE_OPTIONS`。否则 WorkBuddy 注入的 `--require=node-language-shim.cjs` 会让 Emscripten 自带的旧版 node 直接崩掉（`Cannot find module ... node-language-shim.cjs`），构建会在 emcc 链接阶段失败。

`YueBuild.BuildWebGL` 会顺带调 `YueWebGLTextures.Apply(1024)`，只给 WebGL 平台加纹理降采样覆盖，Windows 构建与编辑器里看到的都不变。

## 下载

[Release v0.5-2026-10-28](https://github.com/zhouxuan078-max/shiyinguan/releases/tag/v0.5-2026-10-28) 里的 `ShiYinGuan-win64.zip`（198 MB），解压后双击 `ShiYinGuan.exe`。便携版，无需安装。

## 操作

鼠标为主。← → 切换地点，Enter / E 进入；Tab 拾音册，Esc 设置。启动加 `--yue-3d` 回到旧的 3D 行走版。

## 说明

当前美术为待替换开发稿，粤语内容仍需母语审校；音高播放为纯音示意，暂无录音采集与发音评分。剧情与时刻表为虚构教学情境。浏览器版的存档写在 IndexedDB 里，换设备不会跟过去。
