# dtsgrep - 设备树递归搜索工具

!https://img.shields.io/badge/Python-3.6%2B-blue
!https://img.shields.io/badge/License-MIT-green
!https://img.shields.io/badge/Platform-Linux%20%7C%20macOS%20%7C%20WSL-lightgrey

`dtsgrep` 是一个强大的命令行工具，专为设备树开发者设计，用于在复杂设备树结构中递归搜索指定字段。它可以解析 `.dts` 和 `.dtsi` 文件中的包含关系，并在所有相关文件中精确查找匹配内容，提供详细的定位信息。

## 功能亮点

🔍 **智能递归搜索** - 自动解析设备树文件中的引用关系，支持嵌套 `#include` 和 `/include/` 语法  
📂 **路径智能处理** - 自动处理相对路径，支持跨目录引用  
⚡ **正则表达式支持** - 支持完整的正则表达式模式匹配  
🛠️ **增强搜索选项**：
- `-i`/`--ignore-case` - 忽略大小写
- `-w`/`--word-regexp` - 整词匹配
- `-v`/`--invert-match` - 反向匹配（显示不匹配的行）

## 使用场景

- 快速定位设备树中的特定配置项
- 调试复杂的设备树包含结构
- 验证设备树修改的影响范围
- 学习大型设备树项目结构

## 安装方法

1. 下载dtsgrep到~/.local/bin
2. 给予执行权限
```bash
chmod +x ~/.local/bin/dtsgrep
```

3. 确保 `~/.local/bin` 在您的 PATH 环境变量中：
```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

## 使用示例

### 基本搜索
```bash
dtsgrep myboard.dts 'gpio-controller'
```

输出示例：
```
在3处找到'gpio-controller':

[1] ./common.dtsi:45
    gpio: gpio-controller@0 {

[2] ./soc.dtsi:128
    gpio2: gpio-controller@d401 {

[3] ./myboard.dts:201
    my-device {
        compatible = "gpio-controller";
    }
```

### 高级搜索选项
```bash
# 整词匹配 + 忽略大小写
dtsgrep -iw main.dts 'SPI'

# 反向匹配（显示不包含'status = "disabled"'的行）
dtsgrep -v board.dts 'status = "disabled"'

# 多模式搜索
dtsgrep top.dts 'clock-frequency' 'pinctrl_[a-z0-9_]+'
```

### 使用绝对路径
```bash
dtsgrep /path/to/project/device-trees/main.dts 'interrupt-controller'
```

## 技术原理

`dtsgrep` 使用两步处理流程：
1. **递归依赖解析** - 深度优先搜索解析设备树文件的包含关系：
   - 支持 `#include "file.dtsi"` 和 `/include/ "file.dtsi"` 语法
   - 智能路径解析（优先相对当前文件目录）
   - 避免重复处理和循环引用

2. **正则模式匹配**：
   - 整词匹配使用 `\b` 边界符实现精确搜索
   - 多模式并行处理提高搜索效率
   - 统一处理不同操作系统的换行符（LF/CRLF/CR）

## 许可证

本项目使用 LICENSE，允许自由使用、修改和分发。