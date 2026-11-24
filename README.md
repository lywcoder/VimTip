# VimTip

VimTip 是一款面向 Windows 的轻量键盘工具，适合经常在 Vim、终端和中文输入之间切换的人。**轻点 CapsLock 发送 Esc 并尝试切到英文；按住它，就能像按住左 Ctrl 一样操作。** 两种用法共用一个键，左手可以留在主键区。

例如，在 WSL 的 Neovim 里写完一行中文注释，轻点 CapsLock 就能退出插入模式。回到 Windows 编辑器，按住同一个键再按 C 或 V，就能复制或粘贴。VimTip 驻留在 Windows 托盘，WSL 内无需安装组件。

## 安装与上手

适用系统为 **Windows 10 / 11 x64**。安装版自带 AutoHotkey 运行时。

1. 双击 `VimTip-1.1.1-Setup-x64.exe`。如果拿到的是发行压缩包，先解压。程序按当前用户安装，可勾选创建桌面快捷方式。
2. 从开始菜单打开 **VimTip**。在「键位与测试」的试打区选中一段文字，按住 CapsLock，依次按 C、V，再松开 CapsLock。
3. 单独轻点 CapsLock。页面上方会显示类似 **CapsLock → Esc　143 ms** 的结果，下一行显示触发时的应用和输入法反馈。
4. 希望每天登录后自动运行，可以在「常规」勾选「登录 Windows 时启动」。关闭设置窗口后映射继续生效。双击托盘里的绿色 **V** 可再次打开设置，右键可暂停映射或退出程序。

## 默认键位

| 操作 | 结果 |
| --- | --- |
| 单独轻点 CapsLock | 在短按阈值内松开时发送 Esc，并尝试切到英文输入模式。默认阈值为 260 ms，包含 260 ms。 |
| 按住 CapsLock，再按键或点击鼠标 | 从按下的一刻起提供左 Ctrl，支持快速组合和 Ctrl + 鼠标点击。即使在 260 ms 内完成复制，松手也不会补发 Esc。 |
| 单独按住 CapsLock 超过阈值，再松开 | 释放模拟的左 Ctrl。 |
| 先按 Shift，再按 CapsLock | 等组合键全部松开后切换系统大小写，屏幕显示 **A** 或 **a**。 |
| 先按 CapsLock，再按 Shift | 组成 Ctrl + Shift。需要这个组合时，请留意按键顺序。 |
| 单独按左 Shift 或右 Shift | 由当前输入法处理切换。VimTip 随后读取状态，成功时显示 **中** 或 **英**。 |
| 按物理 Esc | Esc 照常传给当前应用，同时尝试切到英文输入模式。 |

## 设置与屏幕提示

| 页面 | 可以调整什么 |
| --- | --- |
| **键位与测试** | 查看最近一次识别结果，试打并调节短按阈值（50–1000 ms）。轻点 CapsLock 和按物理 Esc 后是否切英文，可分别设置。 |
| **屏幕提示** | 分别开关「中／英」和「A／a」。点击对应的预览按钮，所有显示器中央都会出现提示。默认停留 650 ms，可调范围为 100–5000 ms。 |
| **常规** | 管理登录自启、暂停或恢复映射、查看配置路径、恢复默认参数，以及还原已备份的旧脚本启动项。 |

修改阈值、提示时长或切换选项后，点击 **应用设置** 保存；下一次操作就会使用新参数。登录自启选项立即生效。个人配置位于 `%APPDATA%\VimTip\settings.ini`。

**为什么按 Shift 后偶尔看不到「中／英」？** 先在当前应用里确认输入法已把单按 Shift 配置为中英文切换键。不同输入法对 Windows 状态查询的支持有所差异。无法确认时，测试页会显示「未知」或「未确认切换」；状态未知时，VimTip 会跳过对应的屏幕提示。反馈问题时，注明应用和输入法有助于复现。

## 升级与卸载

再次运行新版安装程序即可覆盖更新，默认安装位置是 `%LOCALAPPDATA%\Programs\VimTip`。从便携版升级时，按提示松开组合键并退出旧实例。已有设置会沿用，已开启的 VimTip 自启快捷方式会更新为安装版路径。

从原始 `config.ahk` 迁移时，如果启动文件夹中有直接指向用户主目录下该脚本的快捷方式，在「常规」开启自启后可按提示备份并切换。需要切回时，选择「还原旧版登录启动项」，下次登录起生效。

卸载入口在 Windows **设置 → 应用** 的应用列表中。卸载会清理应用和快捷方式，保留个人设置。若检测到旧脚本的启动项备份，可以在卸载窗口勾选还原。

## 从源码运行与构建

日常修改设置页或键位逻辑，只需安装 **AutoHotkey v2**，从托盘退出正在运行的 VimTip，然后运行 [src/VimTip.ahk](src/VimTip.ahk)。这是调试界面最快的入口。

| 位置 | 用途 |
| --- | --- |
| [src/Core.ahk](src/Core.ahk) | CapsLock、Shift、Esc 的按键判定，模拟 Ctrl 的释放，以及输入法状态读写 |
| [src/Panel.ahk](src/Panel.ahk)、[src/Hud.ahk](src/Hud.ahk) | 托盘菜单、设置窗口和多显示器屏幕提示 |
| [src/Settings.ahk](src/Settings.ahk)、[src/Startup.ahk](src/Startup.ahk) | 默认参数、配置持久化、登录启动项迁移与还原 |
| [installer/](installer/) | C# WinForms 安装与卸载程序、图标和构建脚本 |
| [tests/](tests/) | 原脚本行为对照和安装流程检查 |

**构建发布版。** 准备 Windows x64、Python 3 和 .NET Framework 4.x 的 C# 编译器。要复现发布版的绿色 V 图标和版本资源，请将 [runtime-source.zip](runtime-source.zip) 解压到项目根目录下的 `runtime`，按其中的 `BUILD.md` 编译 AutoHotkey 2.0.21 运行时。随后在项目根目录运行：

```powershell
$runtime = (Resolve-Path '.\runtime\bin\AutoHotkey64.exe').Path
.\installer\build.ps1 -Runtime $runtime
```

构建产物位于 `build\`，包括主程序 `VimTip.exe` 和当前版本安装程序 `VimTip-1.1.1-Setup-x64.exe`。

**验证修改。** 构建后可运行应用自测。下面的命令等待程序退出，并读取本次测试结果：

```powershell
$testDir = Join-Path $env:TEMP ("VimTip-test-" + [guid]::NewGuid())
$app = Start-Process '.\build\VimTip.exe' -ArgumentList "--self-test `"$testDir`"" -Wait -PassThru
Get-Content (Join-Path $testDir 'result.txt')
if ($app.ExitCode -ne 0) { throw "VimTip 自测失败，退出码 $($app.ExitCode)" }
```

如果改动了按键判定，并且手头有原始 `config.ahk`，还可用下面的命令对照 **22 组模拟输入场景**。将第二个路径替换为原脚本的位置。涉及输入法的改动，建议再用实际输入法和编辑器走一遍。

```powershell
python .\tests\compare.py $runtime 'C:\path\to\config.ahk'
```

## 参与改进

欢迎提交 Issue 或 Pull Request。报告按键问题时，最有帮助的是提供 **VimTip 版本、Windows 版本、应用、输入法、按下和松开的顺序、预期与实际结果**。例如：「在 Windows Terminal 的 Neovim 中，先按住 Shift，再按下并松开 CapsLock，最后松开 Shift」。界面显示问题请附 Windows 缩放比例和截图。

如果有功能建议，欢迎描述你正在做什么、哪一步打断了手上的操作。VimTip 后续迭代会继续关注三个日常体验：按键时序可靠、中文输入切换可理解、托盘常驻轻量。

## 第三方许可

VimTip 使用 AutoHotkey v2 运行时。随附的运行时许可见 [AutoHotkey-LICENSE.txt](AutoHotkey-LICENSE.txt)，运行时源码包中也包含相关组件的许可说明。
