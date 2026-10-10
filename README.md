# dsh-anip-icons

自用的 **ANIP 图标资源包**（[Android Notification Icon Project](https://github.com/BetterAndroid/android-notification-icon-project) 格式），
用于 Flyme 通知图标替换 / 支持 ANIP 自定义图标的模块（实测环境：FlymeStatusBarSizer / FlymeBarSizer）。

## 内容

- 共 **832** 张通知图标（45×66 px、纯白前景 + 透明背景、等比居中不拉伸）
- 全部放在 `icons/app/` 下，配 `icons/app/manifest.json`（包名 → 图标规则）
- 含 DSH（`com.dsharnessmobile.shell`，label `DSH`）、百度（`com.baidu.searchbox`）、高德（`com.autonavi.minimap`）等

> **为什么只有 `app/` 一棵树**：FlymeBarSizer 的 bundle 解析器只解包 `app/` 前缀
> （`AnipBundleStore.PREFIX = "app/"`），`game/`、`system/` 会被直接忽略 → 全部图标并入 `app/`，才能被搜到、生效。

## 变更记录

- **DSH 重新设计**：图形 45×37（原 45×34），通知栏按模块规则渲染为 **54×44 px**，
  与微信图标（54×44）完全等高；轮廓只向外加粗 1px（保留眼睛/嘴部镂空），视觉分量与微信/QQ 一致。
- `com.autonavi.minimap`（高德）、`com.baidu.searchbox`（百度）替换为哥提供的新版本。
- 移除 `com.jd.jdlite`（源目录已删除）。

## 怎么用（ANIP 自定义地址）

在模块里把「通知图标来源」设为**自定义地址**：

```
https://cdn.gh-proxy.org/https://github.com/CODIGSALA/dsh-anip-icons/releases/latest/download
```

备用（jsDelivr，注意不要带 `@main`）：

```
https://cdn.jsdelivr.net/gh/CODIGSALA/dsh-anip-icons/dist
```

模块会自动拼接 `anip-release.json`；更新后**必须重启 App 或 SystemUI**（图标库只在进程启动时加载一次）。

## 数据来源

图标来自 ANIP 官方资源库，并按需替换为自己整理的白化版本（Lanczos3 + USM 锐化 + 去雾）。
