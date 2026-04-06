# Open Assistant Hub

> Multi-room voice and LLM orchestration for Home Assistant.

一个面向智能家居场景的开源语音中枢框架。
它将 **语音终端**、**大模型 API**、**Home Assistant** 与 **设备生态** 连接在一起，帮助开发者快速构建可本地部署、可远程访问、可扩展演进的智能家居语音控制系统。

---

## 项目愿景

Open Assistant Hub 旨在解决这样一个问题：

在家庭自动化场景中，设备接入、语音入口、模型能力、权限边界和远程控制通常分散在不同系统中，导致部署复杂、扩展困难、维护成本高。

本项目希望提供一个清晰、模块化、面向工程落地的中枢层，让开发者能够：

* 快速接入 Home Assistant 作为设备与自动化底座
* 快速接入外部大模型 API 作为语义理解与对话能力
* 快速接入多房间语音终端，例如旧 Android 手机、ESP32、Wyoming 卫星
* 通过统一协议实现房间感知、上下文对话、场景编排与受控执行
* 在安全策略约束下完成自然语言到家居动作的映射

---

## 核心定位

Open Assistant Hub **不是** 一个新的智能家居平台，也不是为了替代 Home Assistant。
它更像是一个运行在 Home Assistant 之上的 **智能控制层 / 编排层**。

### Home Assistant 负责

* 设备接入
* 状态管理
* 自动化执行
* 场景与服务调用
* 实体与区域建模

### Open Assistant Hub 负责

* 语音与文本请求接入
* 多轮对话上下文管理
* 大模型 API 调度
* 意图解析与动作生成
* 权限边界与确认机制
* 多房间、多终端统一编排

---

## 目标特性

* **多房间语音控制**：支持每个房间独立语音端点
* **LLM 解耦**：模型通过 API 接入，不绑定特定硬件或特定模型厂商
* **设备解耦**：通过 Home Assistant 接入多种智能家居生态
* **统一动作协议**：将自然语言请求转换为结构化动作
* **安全优先**：高风险设备支持确认机制与白名单控制
* **快速部署**：支持 Docker Compose 与 Home Assistant App 形态
* **易于开源演进**：模块化设计，便于扩展新端点、新模型、新设备桥接

---

## 典型使用场景

### 1. 家庭语音控制增强

用户在客厅说：

> 我准备睡觉了。

系统可结合当前房间、设备状态和用户习惯，自动执行：

* 关闭客厅主灯
* 关闭电视插座
* 关闭窗帘
* 将卧室空调设置为指定温度
* 返回一段自然语言确认

### 2. 多房间上下文感知

用户在书房说：

> 打开这里的灯。

系统可根据当前语音端点所在房间，自动解析“这里”为书房，而无需用户明确说出设备名。

### 3. 远程语音与查询

用户在外出时可通过手机语音或文本询问：

* 家里还有哪些灯没关？
* 我到家前把客厅空调打开。
* 如果客厅没人，30 分钟后关闭电视和落地灯。

---

## 系统架构

```text
Voice Endpoints
  ├─ Android phones
  ├─ ESPHome voice nodes
  └─ Wyoming satellites
          │
          ▼
Open Assistant Hub
  ├─ Conversation Context
  ├─ Intent Orchestrator
  ├─ LLM Adapter
  ├─ Policy Engine
  └─ Home Assistant Adapter
          │
          ▼
Home Assistant
  ├─ Entities
  ├─ Services
  ├─ Areas
  ├─ Automations
  └─ Integrations (Xiaomi / ESPHome / Matter / ...)
          │
          ▼
Smart Home Devices
```

---

## 设计原则

### 1. 控制与推理解耦

模型负责理解，Home Assistant 负责执行。

### 2. 设备与厂商解耦

优先基于设备能力抽象，而不是绑定某个品牌。

### 3. 房间优先

将“房间”作为自然语言理解和设备控制的重要上下文单位。

### 4. 安全优先于炫技

高风险操作需要明确的确认机制与策略控制。

### 5. 协议先于实现

优先稳定输入协议、动作协议和策略协议，保证系统具备长期演进能力。

---

## 核心模块规划

### `core-orchestrator`

负责会话管理、意图编排、LLM 调度与动作生成。

### `ha-adapter`

负责与 Home Assistant 进行 API 交互，包括状态查询、服务调用和场景执行。

### `llm-adapter`

负责适配不同的大模型 API 提供方。

### `voice-endpoints`

负责接入不同语音终端，例如 Android、ESPHome、Wyoming。

### `policy-engine`

负责动作白名单、确认机制、风险等级与策略校验。

### `vendor-bridges`

负责厂商生态示例桥接，例如 Xiaomi Home。

---

## 目录建议

```text
open-assistant-hub/
├─ apps/
│  └─ ha-agent-app/
├─ compose/
│  ├─ docker-compose.yml
│  └─ .env.example
├─ packages/
│  ├─ core-orchestrator/
│  ├─ ha-adapter/
│  ├─ llm-adapter/
│  ├─ voice-protocol/
│  ├─ policy-engine/
│  └─ vendor-bridges/
│     └─ xiaomi-home/
├─ mobile/
│  └─ android-client-config/
├─ examples/
│  ├─ minimal-home/
│  ├─ xiaomi-house/
│  └─ multi-room/
├─ docs/
│  ├─ architecture.md
│  ├─ quick-start.md
│  ├─ security-model.md
│  └─ api-reference.md
├─ schemas/
│  ├─ action.schema.json
│  ├─ context.schema.json
│  └─ policy.schema.json
└─ README.md
```

---

## 快速开始（规划中）

未来版本将支持以下部署方式：

### 方式一：Docker Compose

适用于开发者、本地服务器、NAS 或 Linux 主机。

### 方式二：Home Assistant App

适用于希望通过 Home Assistant 面板快速安装和配置的用户。

---

## 动作协议示意

```json
{
  "reply": "好的，已为你切换到睡眠模式。",
  "actions": [
    {
      "domain": "light",
      "service": "turn_off",
      "target": "area:living_room"
    },
    {
      "domain": "cover",
      "service": "close_cover",
      "target": "cover.bedroom_curtain"
    },
    {
      "domain": "climate",
      "service": "set_temperature",
      "target": "climate.bedroom_ac",
      "data": {
        "temperature": 24
      }
    }
  ],
  "requires_confirmation": false
}
```

---

## 安全模型

Open Assistant Hub 默认遵循以下原则：

* 模型不直接拥有底层设备全权限
* 所有可执行动作应经过策略引擎校验
* 高风险设备默认需要确认
* 管理类操作默认不开放给模型
* 远程访问优先建议通过 VPN 等安全通道完成

### 高风险设备示例

* 门锁
* 安防系统
* 摄像头隐私模式
* 车库门
* 加热类设备

---

## 路线图

### v0.1

* 单房间基本通路
* 文本命令接入
* Home Assistant 动作执行
* Android 语音入口示例

### v0.2

* 多房间上下文
* 安全策略与白名单
* Docker Compose 部署
* Xiaomi Home 示例桥接

### v0.3

* Home Assistant App 版本
* 远程访问模板
* 多模型后端切换
* 日志与追踪能力

### v0.4

* Web 配置页面
* 策略可视化编辑
* 插件机制
* 更多设备生态支持

---

## 当前状态

项目目前处于早期设计阶段，README 主要用于明确目标、范围与架构方向。
后续将逐步补充：

* 最小可运行示例
* API 文档
* 部署脚本
* 配置模板
* 开发指南

---

## 适合谁

Open Assistant Hub 适合以下用户：

* 已经在使用 Home Assistant 的开发者
* 希望将外部大模型接入智能家居控制链路的工程师
* 想用旧手机、ESP32 等低成本硬件搭建全屋语音端点的极客用户
* 希望构建多房间语音控制与家庭自动化能力的开源爱好者

---

## 贡献

欢迎通过 Issue、Discussion、Pull Request 一起参与项目建设。
当前阶段尤其欢迎以下方向的贡献：

* 架构设计建议
* 动作协议与策略模型讨论
* Android / ESPHome / Wyoming 端点适配
* Xiaomi Home 等生态桥接示例
* Docker / Home Assistant App 部署方案

---

## License

建议采用 `MIT` 或 `Apache-2.0`，后续根据项目发展方向确定。

---

## 一句话总结

**Open Assistant Hub 是一个面向智能家居场景的开源语音与 LLM 编排中枢，让开发者能够基于 Home Assistant 快速构建多房间、可控、安全、可扩展的智能家庭助手。**

