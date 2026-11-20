# 云灯 · 守灯人七夜

一款单文件、零依赖的网页休闲游戏。云海之上的望潮镇，守灯人阿烬的灯油只剩七夜；每晚爬上屋顶接住坠落的星屑添进灯塔，让纸船找到回家的路。

> 在线试玩：https://pcni70hct3kq.doubao.link/app/app_17jc6zuxqsp

![标题画面](assets/title.jpg)

## 玩法

- 左右拖动（或 ← → / A D）移动提灯，接住落下的东西：
  - **碎光** — 加分
  - **心焰** — 加一根灯芯，延长燃灯
  - **记忆** — 解锁一段回忆
- 躲开 **黑雾**：碰到烧掉一根灯芯，灯芯耗尽这一夜就结束。
- 连击每 8 次升一档倍率（最高 ×3，燃灯期 ×4）；连击 ≥12 触发「燃灯」5.5 秒，光域变宽、黑雾减半。
- 每夜 42–60 秒；两夜之间用光屑升级 **灯油 / 灯罩 / 手速**（各 3 级，58 → 104 → 176）。
- 七夜打完进结局。

## 技术

- 原生 Canvas 2D，720×1280 虚拟坐标，`ctx.setTransform` 等比缩放适配任意屏幕。
- 单文件 66 KB：HTML + 两段 IIFE（`CORE` 管数值/存档/剧情/音频，`engine` 管渲染/输入/自检）。
- 零三方库、零构建、零接口；存档写 `localStorage`（键 `yundeng.save.v1`）。
- 手感细节：接光判定用「光域宽」，受伤判定只用「身体窄」——升级不会让你更容易死。

## 目录

```
index.html            游戏本体（双击即玩）
assets/title.jpg      标题插画
assets/ending.jpg     结局插画
docs/                 从零到上线的学习包（6 篇）
```

## 本地运行

直接双击 `index.html`。或起一个静态服务：

```bash
python -m http.server 8080
# 打开 http://localhost:8080
```

## 自检入口（开发时用）

| 地址参数 | 作用 | 结果输出 |
| --- | --- | --- |
| `?selftest=1` | 纯数值：七夜得分 / 灯芯 / 机器人胜负 | `<pre id="selftest">` |
| `?walk=1` | 交互链路：按钮 → 状态机 → 拖动换算 → 结算 → 结局 | `<pre id="walklog">` |
| `?probe=play:4` | 定格某一夜的画面，便于截图评审 | 截图 |
| `?probe=story:2` / `upgrade` / `result:4` / `ending` | 其余界面定格 | 截图 |

命令行验收（Windows PowerShell，无需开浏览器）：

```powershell
& 'C:\Program Files\Google\Chrome\Application\chrome.exe' --headless=new --disable-gpu `
  --virtual-time-budget=6000 --dump-dom "file:///.../index.html?walk=1" |
  Select-String 'RESULT'
```

## 部署

任意静态托管即可（Nginx / Cloudflare Pages / GitHub Pages / OSS / 对象存储）。
Nginx 最小配置：

```nginx
server {
    listen 80;
    server_name your.domain;
    root /var/www/yundeng;
    index index.html;
    location / { try_files $uri $uri/ /index.html; }
}
```

把 `index.html` 与 `assets/` 一起上传到 `root` 指向的目录即可，没有后端依赖。

## 已知边界

- 单条操作线 + 有限终点（七夜）＝ 体验向小品，不是留存向产品；想长期玩需另加「无尽夜」模式。
- 升级偏便宜：一条熟练路线到第五夜就能点满三轨。
- 移动端已做 `touch-action:none` / `overscroll-behavior:none`，未做横屏适配（建议竖屏）。

## License

MIT，见 [LICENSE](LICENSE)。插画与代码由本项目作者使用 AI 辅助生成并整理，可自由使用、修改、商用；转载保留本仓库地址即可。
