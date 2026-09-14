# Snore Detector 更新通道 / release channel

这里只放**已签名的安装包和版本清单**，不含任何源代码。
App 内「我的 → 检查更新」读取 `version.json`，需要更新时下载 Release 上的 APK，
再交给系统安装器完成升级。

## 文件

- `version.json`：当前可升级到的版本（`versionCode` 必须大于手机上的数字才会提示更新）。
- Releases：每个版本一个 tag，附带 `snore-detector-v<版本>.apk`。

## 发布新包

在开发机上依次执行：

```powershell
# 1. 改 build.gradle.kts 里的 versionCode / versionName
# 2. 编译并校验签名
pwsh -File build-apks.ps1
# 3. 发布到本通道
pwsh -File publish-release.ps1 -Notes "本次更新说明"
```

清单必须保持 UTF-8 JSON；`sha256` 只是防下载损坏，防篡改靠的是签名证书一致性：
换证书的 APK 无法覆盖安装，系统安装器会直接拒绝。

本仓库必须保持 public：匿名可读的 raw.githubusercontent.com 是 App 唯一的取清单方式，
改成 private 就必须在 APK 里内置 token。