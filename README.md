# Open CMD and PowerShell 7 Here

中文 | [English](#english)

一个简单的 Windows 注册表小工具，用来在文件夹和文件夹空白处的右键菜单中添加：

- 在此处打开 CMD
- 在此处打开 PowerShell 7

适合不想折腾复杂安装器、只想双击一个 `.reg` 文件就完成配置的人。

## 中文

### 功能

- 在文件夹右键菜单中添加 `在此处打开 CMD`
- 在文件夹空白处右键菜单中添加 `在此处打开 CMD`
- 在文件夹右键菜单中添加 `在此处打开 PowerShell 7`
- 在文件夹空白处右键菜单中添加 `在此处打开 PowerShell 7`
- 安装和卸载分别由独立的 `.reg` 文件完成

### 文件说明

- [install_open_shell_here.reg](./install_open_shell_here.reg)：安装右键菜单项
- [uninstall_open_shell_here.reg](./uninstall_open_shell_here.reg)：卸载右键菜单项

### 系统要求

- Windows
- 如果要使用 `在此处打开 PowerShell 7`，请先安装 [PowerShell 7](https://github.com/PowerShell/PowerShell)

脚本中的命令直接调用 `pwsh.exe`（依赖 PATH），兼容以下两种安装方式：

- MSI 安装：`pwsh.exe` 位于 `C:\Program Files\PowerShell\7\`，由安装器加入 PATH
- Microsoft Store / `winget` 安装：通过 `%LocalAppData%\Microsoft\WindowsApps\` 下的 App Execution Alias 提供

菜单图标使用 MSI 路径 `C:\Program Files\PowerShell\7\pwsh.exe`。若通过 Store 安装（实际路径为 `C:\Program Files\WindowsApps\Microsoft.PowerShell_*_x64_*\pwsh.exe`，版本号会随更新变化），图标可能显示为默认图标，但菜单项仍可正常工作。如需精确图标，可编辑注册表脚本中的 `Icon` 值指向本机实际的 `pwsh.exe` 路径。

### 安装方法

1. 下载或保存本项目文件到本地。
2. 双击 `install_open_shell_here.reg`。
3. 在弹出的注册表导入提示中选择“是”。
4. 重新打开资源管理器窗口后生效。

### 卸载方法

1. 双击 `uninstall_open_shell_here.reg`。
2. 在弹出的注册表导入提示中选择“是”。

### 实现说明

这个工具写入的是当前用户注册表：

```text
HKEY_CURRENT_USER\Software\Classes\
```

优点：

- 不影响其他用户
- 不需要修改系统级配置
- 安装和卸载都很直接
- 通常不需要管理员权限

`CMD` 菜单项使用的是与 Windows 内置项一致的调用方式：

```text
cmd.exe /s /k pushd "%V"
```

`PowerShell 7` 菜单项使用：

```text
pwsh.exe -NoExit -WorkingDirectory "%V"
```

这样可以更稳定地在目标目录中打开终端。

### 说明

- 本项目只处理“目录”和“目录背景”右键菜单，不会添加到文件右键菜单。
- 如果导入后没有立刻显示，可以重启资源管理器或重新登录系统。
- 如果系统策略阻止导入 `.reg` 文件，需要使用管理员允许的方式执行。

### License

MIT

---

## English

This is a small Windows Registry utility that adds the following context menu entries for folders and folder backgrounds:

- Open CMD Here
- Open PowerShell 7 Here

It is intended for people who want a minimal, double-clickable `.reg` solution without packaging or installers.

### Features

- Adds `Open CMD Here` to the folder context menu
- Adds `Open CMD Here` to the folder background context menu
- Adds `Open PowerShell 7 Here` to the folder context menu
- Adds `Open PowerShell 7 Here` to the folder background context menu
- Separate install and uninstall scripts

### Files

- [install_open_shell_here.reg](./install_open_shell_here.reg): installs the context menu entries
- [uninstall_open_shell_here.reg](./uninstall_open_shell_here.reg): removes the context menu entries

### Requirements

- Windows
- [PowerShell 7](https://github.com/PowerShell/PowerShell) if you want the PowerShell 7 menu entry

The command invokes `pwsh.exe` directly via PATH, so both installation methods are supported:

- MSI install: `pwsh.exe` lives in `C:\Program Files\PowerShell\7\`, which the installer adds to PATH.
- Microsoft Store / `winget` install: `pwsh.exe` is exposed via the App Execution Alias in `%LocalAppData%\Microsoft\WindowsApps\`.

The menu icon uses the MSI path `C:\Program Files\PowerShell\7\pwsh.exe`. For Store installs (the actual binary is at `C:\Program Files\WindowsApps\Microsoft.PowerShell_*_x64_*\pwsh.exe`, a versioned path that changes on upgrade), Windows may fall back to a generic icon but the menu entry still works. If you want a precise icon, edit the `Icon` value in the registry script to point to your actual `pwsh.exe`.

### Installation

1. Download or save the project files locally.
2. Double-click `install_open_shell_here.reg`.
3. Click `Yes` when Windows asks for registry import confirmation.
4. Reopen File Explorer windows if the menu does not appear immediately.

### Uninstallation

1. Double-click `uninstall_open_shell_here.reg`.
2. Click `Yes` when Windows asks for registry import confirmation.

### How it works

The scripts write entries under the current user registry hive:

```text
HKEY_CURRENT_USER\Software\Classes\
```

Benefits:

- No system-wide changes
- No effect on other users
- Easy to install and remove
- Usually does not require administrator privileges

The `CMD` entry uses the same invocation style as the built-in Windows command prompt entry:

```text
cmd.exe /s /k pushd "%V"
```

The `PowerShell 7` entry uses:

```text
pwsh.exe -NoExit -WorkingDirectory "%V"
```

This makes both entries open more reliably in the target directory.

### Notes

- This project only targets folders and folder backgrounds, not individual files.
- If the menu does not appear right away, restart File Explorer or sign out and sign back in.
- Some environments may block `.reg` imports due to policy restrictions.

### License

MIT
