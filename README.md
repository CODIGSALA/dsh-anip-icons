# dsh-anip-icons

自用的 **ANIP 图标资源包**（[Android Notification Icon Project](https://github.com/BetterAndroid/android-notification-icon-project) 格式），
用于 Flyme 通知图标替换 / 支持 ANIP 自定义图标的模块。

## 内容

- 共 **834** 张通知图标（45×66 px、纯白前景 + 透明背景）
  - `icons/app/` 730 张（第三方 App）
  - `icons/game/` 12 张
  - `icons/system/coloros/` 49 张
  - `icons/system/mios/` 40 张
  - `icons/system/common/` 3 张
- 每组带 `manifest.json`（包名 → 图标规则）

## 怎么用（ANIP 自定义地址）

在支持 ANIP 的模块里填 **目录地址**（SDK 会自动拼接 `anip-release.json`）：

```
https://github.com/CODIGSALA/dsh-anip-icons/releases/latest/download/
```

## 数据来源

图标来自 ANIP 官方资源库，并按需替换为自己整理的白化版本。
