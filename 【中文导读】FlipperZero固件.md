# Flipper Zero 固件（flipperzero-firmware）学习导读

> 目录：`04-项目实战/flipperzero-firmware-FlipperZero固件`
> 来源：https://github.com/flipperdevices/flipperzero-firmware （约 16.7k star）

## 这是什么

Flipper Zero 是那台"海豚"多协议工具机：RFID/NFC/红外/SubGHz/蓝牙/GPIO/BadUSB 全能。官方固件**工程化程度在开源嵌入式项目里是顶流**：自研构建系统 fbt（SCons）、应用 .fam 清单机制、场景化服务架构（scene_manager/view_dispatcher）、完整的 FAP（Flipper Application Package）第三方 App 加载体系、精密的单元测试与 CI。**想学"一个大团队如何做嵌入式工程"，直接读它。**

⚠️ 硬件：STM32WB55（主控，含 BLE）+ CC1101（SubGHz）+ 各种射频前端；也有免费模拟器（`fbt fap_dist` / 官方桌面模拟器）。

## 目录导读

| 目录/文件 | 内容 |
| --- | --- |
| `applications/main/` | 主功能 App：`subghz/`（SubGHz 收发）、`nfc/`、`lfrfid/`、`infrared/`、`bad_usb/`、`u2f/`... |
| `applications/services/` | **系统服务**：`gui/`、`cli/`、`bt/`、`storage/`、`furi/`（Furi 是它的"操作系统层"）、`desktop/`... |
| `lib/` | 公共库（驱动、工具、协议解析） |
| `targets/f7/` | 硬件抽象（STM32WB55 平台） |
| `fbt/` | 自研构建系统（SCons 脚本） |
| `applications_user/` | 用户 App 示例 |
| `documentation/` | 官方开发文档（App 开发指南必读） |
| `assets/` | 动画/图标资源（dolphin 动画） |

## 学习路径（建议）

1. 读 `documentation/applications.md` 与官方 docs.flipper.net 开发者章节，理解 **FAP 模块化应用** 思想（应用=独立 ELF，按需加载）。
2. 读 `applications/services/gui/`：View/ViewDispatcher/SceneManager 三件套——**嵌入式 GUI 架构的教科书面孔**（页面栈、输入分发、ViewPort 合成）。
3. 选一个功能 App 深读：推荐 `applications/main/infrared/`（红外，协议简单）或 `subghz/`（协议丰富但复杂）。
4. 读 `furi/`：线程、事件队列、record 注册机制（服务间用字符串名共享句柄）。
5. 动手：用 `fbt` 写一个自己的 FAP（模板在 applications_user），在模拟器或真机跑。

## 学习要点

1. **服务化架构**：GUI/BT/Storage 都是后台服务线程，App 通过 FURI record 拿句柄——大项目的"微内核"思路。
2. **场景管理**：每个界面一个 scene，`scene_manager_next_scene` 压栈/弹栈——写复杂菜单不会乱。
3. **fbt 构建**：SCons + 应用 .fam 清单自动生成依赖——对比 CMake 的另一种工程组织。
4. **协议解析库**：`lib/` 里 RFID/NFC/IR 协议如何分层抽象与注册。

## 建议购买/准备

- **零成本**：官方桌面模拟器（Windows/Mac/Linux）可跑全部 App 与自定义 FAP，学习架构完全够用。
- **真机**：Flipper Zero（官方 $169 / 国内代理 ¥1300-1600）。**不买真机也能完成 90% 的学习**。
- 替代路线：只想学它的架构 → 读代码 + 模拟器；想要硬件折腾 → 与其买 Flipper 不如买 Pico/ESP32 自己复刻红外+RFID 小工具。
- 若玩 SubGHz/红外研究：433MHz 天线、红外接收发射模块（¥5-15）可配真机使用。

## 学习目标（自测）

- [ ] 在模拟器跑通自己的第一个 FAP
- [ ] 画出 View/ViewDispatcher/SceneManager 的协作关系
- [ ] 说清 FURI record 机制解决什么问题
- [ ] 从红外 App 里追踪一次"学习遥控器信号"的代码路径
