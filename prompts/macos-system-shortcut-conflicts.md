# macOS 系统快捷键冲突整理

## 目标
记录 Eclipse Windows 快捷键移植到 VS Code Keymap 时，哪些按键在 macOS 上不能直接沿用，以及当前的处理方式。

实际生效文件是 `package.json` 的 `contributes.keybindings`。`keybindings.json` 只是参考导出文件，不作为插件生效来源。

## 判断口径
- 默认只写 `key`，让 Windows、Linux、macOS 共用同一映射。
- 只有遇到 macOS 系统级冲突或高风险平台惯例冲突时，才写 `mac` 覆盖。
- 不能用空 `mac` 禁用快捷键；VS Code 会回退到 `key`。
- 系统级冲突主要包括输入法、Mission Control/Spaces、键盘焦点、窗口平铺等 macOS 先处理的快捷键。
- 文本编辑惯例不全部避让；只有会破坏核心编辑体验或与已有 macOS 替代键互相挤占时才处理。

## 当前结论
当前配置不保留 `mac` 覆盖项，并移除 `ctrl+space` 的补全绑定。

项目决策：`cursorWordPartLeft` / `cursorWordPartRight` 以及它们的选择版都保留 `ctrl` 组合，不再单独为 macOS 改键。这样能在未开启 Spaces 时保持完整的 Eclipse Windows 风格；若用户启用了 Mission Control 的 Control+Arrow，是否接受系统接管由用户自行决断。

## 有意保留的冲突风险
| macOS 有效键 | 命令 | 说明 |
| --- | --- | --- |
| `ctrl+left` | `cursorWordPartLeft` | 保留 Eclipse Windows 的按键习惯；如果 macOS 启用了 Spaces 左切换，系统可能先处理。 |
| `ctrl+right` | `cursorWordPartRight` | 保留 Eclipse Windows 的按键习惯；如果 macOS 启用了 Spaces 右切换，系统可能先处理。 |
| `ctrl+shift+left` | `cursorWordPartLeftSelect` | 保留 Eclipse Windows 的按键习惯；如果 macOS 启用了 Spaces 左切换，系统可能先处理。 |
| `ctrl+shift+right` | `cursorWordPartRightSelect` | 保留 Eclipse Windows 的按键习惯；如果 macOS 启用了 Spaces 右切换，系统可能先处理。 |
| `alt+left` | `workbench.action.navigateBack` | 保留 Eclipse Windows 的导航后退行为。 |
| `alt+right` | `workbench.action.navigateForward` | 保留 Eclipse Windows 的导航前进行为。 |

## 已删除映射
| 原按键 | 原命令 | 处理 |
| --- | --- | --- |
| `ctrl+space` | `editor.action.triggerSuggest` | 删除，避免与 macOS 输入法切换冲突；保留 `alt+/` 作为 Content Assist。 |

## 后续维护规则
- 新增快捷键前，先确认 VS Code command id 有效。
- 新增 `mac` 覆盖前，必须说明具体冲突来源。
- 修改后检查同一平台下是否存在相同 `(key, when)` 重复绑定。
- 修改后检查 macOS 有效按键是否命中系统快捷键。

## 校验命令
查看当前平台覆盖项：

```bash
jq -r '.contributes.keybindings[] | select(has("mac") or has("win") or has("linux")) | [.key, (.mac//""), (.win//""), (.linux//""), .command, (.when//"")] | @tsv' package.json
```

查看本机已启用的 macOS symbolic hotkeys：

```bash
defaults export com.apple.symbolichotkeys - 2>/dev/null | plutil -convert json -o - - | jq '.AppleSymbolicHotKeys'
```

## 参考来源
- Apple: https://support.apple.com/en-us/102650
- Apple: https://support.apple.com/guide/mac-help/mh14112/mac
- Apple: https://support.apple.com/guide/mac-help/mac-window-tiling-icons-keyboard-shortcuts-mchl9674d0b0/mac
