# dsh-anip-icons

自用的 **ANIP 图标资源包**（[Android Notification Icon Project](https://github.com/BetterAndroid/android-notification-icon-project) 格式），
用于 Flyme 通知图标替换 / 支持 ANIP 自定义图标的模块（实测环境：FlymeStatusBarSizer / FlymeBarSizer）。

## 内容

- 共 **745** 张通知图标（45×66 px 画布、纯白前景 + 透明背景；图形**包围盒宽高比统一为 41:36**，与 DSH 同档，见「尺寸校准」）
- 全部放在 `icons/app/` 下，配 `icons/app/manifest.json`（包名 → 图标规则）

> **为什么只有 `app/` 一棵树**：FlymeBarSizer 的 bundle 解析器只解包 `app/` 前缀
> （`AnipBundleStore.PREFIX = "app/"`），`game/`、`system/` 会被直接忽略。

## 尺寸校准

FlymeBarSizer 的渲染规则是「裁掉透明留白 → 等比 fit 到 18dp 方框」，所以**图标在通知栏里的可见大小只由图形的宽高比决定**。

**当前版本（2026-10-16 起）：全库 745 张的图形包围盒（bbox）宽高比统一为 41:36（DSH 档）**——模块先裁透明留白、再按长边等比 fit，所以通知栏里的高度只由 bbox 宽高比决定：**511 张**偏高的（含 QQ 32×36、MT 管理器 36×36 这类纵向/方形图，他们渲染满格 14dp）在图形左右各补 1px 宽 **α=9 的隐形像素**把 bbox 撑到 41×36，渲染高度精确等于 DSH（12.29dp）；**63 张**本来已是 41:36、**171 张**横向图（bw > bh，如华为 WeLink 43×12、开黑啦 43×21）**原样不动**——横向图在方形框里几何上提不上去，除非裁掉内容或拉伸变形。实体图形（α>16）逐点校验零差异，形状、明暗层次、纯白单色全部保留。

历史做法（已废弃）：曾以成品微信（54×50 px）与 QQ（48×54 px）为基准，对宽高比偏离的 **364 张**做**受限拉伸**（每条最多 15%，避免图形明显变形），结果渲染面积 <2000 px² 的图标从 136 张降到 73 张、平均面积偏差 348 → 266，但尺寸仍然参差。

新增/重做的图标按其素材本身比例出片；统一 bbox 只动「隐形像素」（α=9，模块的可见阈值是 >8 才计入包围盒），**不改任何可见像素、不改形状**。

另外所有图标四周统一保留 **≥1px** 安全边距（在放大/加粗之前先留边），**不会有任何像素被画布边缘切掉**。

## 变更记录

- **以 DSH 为标准统一包围盒宽高比（本次）**：按哥的要求「以 dsh 为标准改宽高比，只改高的或低的，横向无法提升的不改」，511 张偏高的图标左右各补 1px **α=9 隐形边**把 bbox 撑到 41×36，通知栏显示高度与 DSH / 京东同档，不再比 QQ、MT 管理器高一截；171 张横向图与 63 张已达标者原样不动
- ~~全库统一为 Via 尺寸 43×36~~（**已撤回**）：曾把 745 张全部拉伸到 43×36，导致 41 张（横排文字型 / 竖长型）比例变化明显，哥要求改规则
- **全库纵向高度统一为 36px**（本次）：按哥的新规则，只统一纵向高度到 Via 标准 **36px**——高度 > 36 的 **609 张**按原比例**等比缩小**到高 36（宽度随之变小、绝不拉伸），画布内居中；高度 ≤ 36 的 **136 张原样跳过**。上一版的极端变形问题由此消除
- **高德地图重做：源改回 Flyme 主题原图，只改大小到 43×36**（本次）：源 = 哥指认的原样式文件 `/data/customizecenter/theme/com.android.systemui` 内 `mz_stat_sys_gaode.png`（45×66 画布、图形 35×34），**不再用库内另一版剪影**——那版形状不同，哥一眼看出「样式变了」。本次只做尺寸变换：图形重采样到 **43×36**（与 Via 完全同尺寸，通知栏渲染宽 18dp / 高 15.1dp），纯白单色、不重绘、不分档，画布位置与 Via 相同（`x1..43 y15..50`）
- **Via 与 Google Play 商店改为灰阶层次版**：参考库内「飞书」图标的做法，不再通体死白，改用多档白/灰 alpha 做纹理——Via 三段 `0.66 / 0.86 / 1.00`（左暗右亮，呈折纸丝带感）；Play 商店官方 monochrome 矢量四块 `0.60 / 1.00 / 0.74 / 0.88`（上亮下暗，保留官方折痕缝）。同时把合成算法改为**覆盖率加权**（`Σ hits_i/area × a_i`），块交界不再出现抗锯齿暗线
- 新增「Via」`mark.via` 与「Google Play 商店」`com.android.vending`：两者都取自应用自身的矢量素材再自行光栅化——Via 用 APK 内 `assets/logo.svg` 的三段品牌 V 路径；Play 商店用 APK 内 adaptive-icon 的 **monochrome 单色矢量层**（官方单色三角）。成品 43×36 / 43×48 等比居中，与微信（43×40）同档
- **再次撤掉**「移动热点」`com.android.networkstack.tethering`（哥实机确认：换成控制中心同款形态后依然不生效；根因是模块对系统应用的 `hasLauncherEntry` 判定，与图标形态无关）
- 重新加入「移动热点」`com.android.networkstack.tethering`（本次）：源用 SystemUI 的 `ic_control_hotspot`（中心点 + 三道同心弧，与 Flyme 控制中心/状态栏热点样式一致），裁留白后 43×36 等比居中
- 撤掉「移动热点」`com.android.networkstack.tethering`——ANIP 条目对 Flyme 热点通知不生效（Flyme 自有一套通知图标机制），不保留
- 撤掉 8 个原生 Android 系统图标（`android` / 蓝牙 / 浏览器 / 电子邮件 / NFC / 下载 / 设置 / 录音机）——实测对 Flyme 通知图标无实际影响，不保留
- 新增「移动热点」`com.android.networkstack.tethering`
- （曾短暂加入的）恢复 8 个被误删的系统图标（`android` / 蓝牙 / 浏览器 / 电子邮件 / NFC / 下载 / 设置 / 录音机）+ 新增「移动热点」`com.android.networkstack.tethering`
- 新增「信息」`com.android.mms`（哥提供成品）与「搜狗输入法」`com.sohu.inputmethod.sogou.meizu`（原文件名 `inputnethod` 系笔误已修正）
- 移除 92 张 Android 系统相关图标（原 `game/`、`system/` 分组：ColorOS 49 + MIUI 40 + 通用 3）
- 恢复 `com.jd.jdlite`（京东特价版，与 `com.jingdong.app.mall` 是不同应用）
- DSH 图标重做：图形 43×38，渲染 54×48，与微信同档；四边各留 1px，不再裁到像素
- 全量尺寸校准（见上）

## 怎么用（ANIP 自定义地址）

```
https://cdn.gh-proxy.org/https://github.com/CODIGSALA/dsh-anip-icons/releases/latest/download
```

备用（jsDelivr，注意不要带 `@main`）：

```
https://cdn.jsdelivr.net/gh/CODIGSALA/dsh-anip-icons/dist
```

模块会自动拼接 `anip-release.json`；更新后重启 App / SystemUI 生效。

## 数据来源

图标来自 ANIP 官方资源库，并按需替换为自己整理的白化版本（Lanczos3 + USM 锐化）。
