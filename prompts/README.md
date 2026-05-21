**这个文件才是工程开发真正的README.md文件，根目录的README.md文件只是为在vscode插件市场中展示的。**

# 目标
让 Windows、Linux 和 macOS 平台下 VS Code 的快捷键尽量贴近 Eclipse，尤其是 Eclipse Windows 快捷键习惯。

核心诉求是：在 macOS 上也尽量使用 `ctrl` 风格快捷键，减少 VS Code 默认 `cmd` 快捷键带来的肌肉记忆差异，让从 Eclipse 迁移到 VS Code 的用户可以少改习惯。

# 范围
本工程是一个 VS Code Keymap 插件，主要维护 `package.json` 中的 `contributes.keybindings`。

优先覆盖高频通用操作：

- 文件、编辑、查找、导航。
- Java 开发常用操作。
- 运行、调试、断点。
- 终端、视图切换等日常工作流。

不追求完整复刻 Eclipse 的所有快捷键。只有在 VS Code 中有稳定等价命令、且符合 Eclipse 用户常见习惯时，才加入映射。

# 参考资料
`references` 目录中保存了 Eclipse JEE 导出的快捷键列表：

- `eclipse-keys-windows.csv`：Windows 平台导出的 Eclipse 快捷键，作为主要参考基准。
- `eclipse-keys-macOS.csv`：macOS 平台导出的 Eclipse 快捷键，用于对比平台差异。

新增或调整快捷键时，应优先以 Windows 导出结果为准，再结合 VS Code 的命令能力和各平台冲突情况做取舍。

# 实施原则
- 三个平台尽量保持同一组按键，避免 macOS 单独切回 `cmd` 风格。
- 不覆盖 VS Code 中明显更关键、冲突风险更高的系统级快捷键。
- 每个 keybinding 都应有明确的 Eclipse 来源或迁移价值。
- `when` 条件要尽量收窄，避免在无关上下文中抢占快捷键。
- 不与 macOS 系统级快捷键冲突。
- 根目录 `README.md` 面向插件市场展示，不放开发决策细节。

# 产出目标
最终产出是一个简单、稳定、低干扰的 Eclipse Windows 风格 VS Code Keymap 插件。
