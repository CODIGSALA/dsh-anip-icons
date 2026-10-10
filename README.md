# dsh-anip-icons

自用的 **ANIP 图标资源包**（[Android Notification Icon Project](https://github.com/BetterAndroid/android-notification-icon-project) 格式），
用于 Flyme 通知图标替换 / 支持 ANIP 自定义图标的模块（实测环境：FlymeStatusBarSizer / FlymeBarSizer）。

## 内容

- 共 **833** 张通知图标（45×66 px、纯白前景 + 透明背景、等比居中不拉伸）
- 全部放在 `icons/app/` 下，配 `icons/app/manifest.json`（包名 → 图标规则）
- 含 DSH（`com.dsharnessmobile.shell`，label `DSH`）、百度（`com.baidu.searchbox`）、常用应用与游戏
  （原神 / 崩坏：星穹铁道 / 王者荣耀 / 和平精英 / 英雄联盟手游 / 碧蓝航线 / 光·遇 …）

> **为什么只有 `app/` 一棵树**：FlymeBarSizer 的 bundle 解析器只解包 `app/` 前缀
> （`AnipBundleStore.PREFIX = "app/"`），`game/`、`system/` 会被直接忽略。
> 早期版本按官方库分了 5 棵树，结果 `game/`（12 张）和 `system/`（92 张）在模块里搜不到也生效不了。

## 变更记录

- `mz_stat_sys_baidu.png` → **`com.baidu.searchbox.png`**（百度，Flyme 主题资源名改为正式包名）
- 删除重复：`mz_stat_sys_gaode.png`（已有 `com.autonavi.minimap.png`）、
  `net.evecom.android.mztapp .png`（尾随空格坏名，已有 `net.evecom.android.mztapp.png`）
- 834 → 833 张：上两条去重后即为当前数量

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
