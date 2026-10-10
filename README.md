# dsh-anip-icons

自用的 **ANIP 图标资源包**（[Android Notification Icon Project](https://github.com/BetterAndroid/android-notification-icon-project) 格式），
用于 Flyme 通知图标替换 / 支持 ANIP 自定义图标的模块（实测环境：FlymeStatusBarSizer / FlymeBarSizer）。

## 内容

- 共 **744** 张通知图标（45×66 px、纯白前景 + 透明背景、等比居中）
- 全部放在 `icons/app/` 下，配 `icons/app/manifest.json`（包名 → 图标规则）

> **为什么只有 `app/` 一棵树**：FlymeBarSizer 的 bundle 解析器只解包 `app/` 前缀
> （`AnipBundleStore.PREFIX = "app/"`），`game/`、`system/` 会被直接忽略。

## 尺寸校准（本版新增）

FlymeBarSizer 的渲染规则是「裁掉透明留白 → 等比 fit 到 18dp 方框」，所以**图标在通知栏里的可见大小只由图形的宽高比决定**：

- 以成品微信（54×50 px）与 QQ（48×54 px）为基准（渲染面积均值 ≈2565），
- 对宽高比偏离的 **364 张**做了**受限拉伸**（每条最多 15%，避免图形明显变形），
- 结果：渲染面积 <2000 px² 的图标从 136 张降到 73 张，平均面积偏差 348 → 266。

另外所有图标四周统一保留 **≥1px** 安全边距（在放大/加粗之前先留边），**不会有任何像素被画布边缘切掉**。

## 变更记录

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
