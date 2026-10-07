# WingChat 桌面端发布

这个仓库只放 WingChat 桌面端（bots 平台客户端）的**安装包**和说明，不放源代码。每个版本都是一个 GitHub Release，tag 形如 `desktop/vX.Y.Z`。

## 下载

1. 打开 [Releases](https://github.com/siliwave/bots-desktop-releases/releases)，选最新的一个版本。
2. 按你的 Mac 选包：
   - Apple 芯片（M 系列）：`WingChat_<版本>_arm64.dmg`
   - Intel 芯片：`WingChat_<版本>_x64.dmg`
   - 不想用 dmg 时，可以下同名的 `.zip`，解压后得到 `WingChat.app`。
3. 想核对下载完整性：同一 Release 里的 `SHA256SUMS.txt` 列出了每个文件的 sha256，可以用 `shasum -a 256 <文件>` 对照。

目前只出 macOS 包（arm64 与 x64 各一份，不出 universal）。

## 未签名构建：首次打开要多一步

现在的安装包**还没有** Apple Developer ID 签名和公证（在做），这类 Release 会标成 **Pre-release**，说明的第一段也会提示。macOS 的 Gatekeeper 会在第一次打开时拦下，提示「无法打开」或「无法验证开发者」。任选一种办法放行：

- **Finder 里右键打开**：把 `WingChat.app` 拖进「应用程序」，在 Finder 里**右键（或按住 Control 点按）→ 打开**，在弹窗里再点「打开」。之后正常双击即可。
- **终端去掉隔离属性**：

  ```sh
  xattr -dr com.apple.quarantine /Applications/WingChat.app
  ```

另外，未签名构建里系统通知不可用。签名和公证就绪后，新版本会去掉 Pre-release 标记，以上步骤就不再需要。

## 应用内的更新提示

桌面端启动约 30 秒后会检查一次更新，之后每 24 小时检查一次。数据来源就是这个仓库的 Releases：

```
GET https://api.github.com/repos/siliwave/bots-desktop-releases/releases
```

- 匿名读取，不需要登录 GitHub，也不带任何 token。
- 只认 `desktop/v*` 的 tag，按版本号和本机已装版本比较；有更新的版本就在窗口右下角提示，给出本机对应的下载链接。
- 应用**只提示，不自动下载、不自动安装**；点「下载」会用浏览器打开对应的安装包链接。
- 可以选「忽略此版本」，之后同一版本不再提示，更新的版本照常提示。
- 网络不通或 GitHub 不可达时静默，不打扰。

## 这个仓库怎么更新

Release 由 bots 平台的 CI 在打 `desktop/v*` tag 时自动发布，说明末行注明了对应的源码提交。请不要手动往这里提交文件或上传资产。
