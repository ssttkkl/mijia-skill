# Mijia Skill

小米米家智能家居设备控制 Skill for OpenClaw。

## 功能

- 📱 列出所有米家设备
- 🔍 查询设备状态和属性
- ⚡ 控制设备开关和设置
- 🎯 执行设备动作
- 🏠 支持多种设备类型： heater、洗衣机、灯具、插座、风扇、空气净化器等

## 安装

```bash
cd ~/.openclaw/workspace/skills/mijia
uv sync
```

## 快速开始

### 列出所有设备

```bash
uv run mijiaapi --list_devices
```

### 查询设备属性

```bash
uv run mijiaapi get --did <设备ID> --prop_name on
```

### 控制设备开关

```bash
# 打开设备
uv run mijiaapi set --did <设备ID> --prop_name on --value true

# 关闭设备
uv run mijiaapi set --did <设备ID> --prop_name on --value false
```

### 查询设备型号信息

```bash
uv run mijiaapi --get_device_info mibx5.washer.32
```

## 支持的设备类型

| 设备类型 | 示例 | 说明 |
|---------|------|------|
| 暖风机 | xiaomi.heater.ma8 | 石墨烯暖风机 |
| 洗衣机 | mibx5.washer.32 | 超净洗滚筒 10kg |
| 智能香薰机 | bwj.diffuser.s5 | SO 智能香薰机 S5 |
| 智能手环 | hmpace.watch.v7nfc | 小米手环 7 NFC 版 |

## 详细用法

更多用法请参考 [SKILL.md](./SKILL.md)。

## 依赖

- Python 3.9+
- [mijiaAPI](https://github.com/Do1e/mijia-api) - 第三方米家 API 库
- uv (推荐) / pip

## License

MIT License
