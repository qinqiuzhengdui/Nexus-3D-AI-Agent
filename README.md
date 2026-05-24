# 🌟 Nexus 3D: AI 沙盒创世数字人系统

**Nexus 3D** 是一个强大的端到端（End-to-End）全自动化工作流平台。只需一句提示词或一段语音，即可实现从 AI 原画设计、3D 模型生成、格式转换、Unity 自动装配，到操控虚拟数字人进行实时智能对话与动作控制的完整体验。

---

## ✨ 核心功能

| 功能 | 说明 |
|---|---|
| 🧠 **AI 大脑** | 接入 DeepSeek V4 Pro，理解用户自然语言指令 |
| 🎙️ **语音识别** | 阿里云 DashScope `paraformer-realtime-v1` 实时转文字 |
| 🔊 **语音合成** | GPT-SoVITS 本地语音克隆，数字人开口说话 |
| 🏃 **动作控制** | 支持行走、跳跃、挥手、跳舞等动画指令 |
| 💬 **个人对话** | 每个数字人拥有独立对话面板，支持"回答"与"做动作"两种模式 |
| 🧊 **物理引擎** | React Three Cannon 物理碰撞，踢箱子、推物体等交互 |
| 🖼️ **场景生成** | AI 生成全景背景图，一句话换场景 |
| 🤖 **3D 模型生成** | 文字/图片生成 3D GLB 模型，拖入即用 |
| 🖨️ **3D 打印** | 自动推送到 Bambu Lab 打印机打印实体 |

---

## 🏗️ 系统架构

```
用户浏览器 (React + Three.js)
    ↕ WebSocket (ws://127.0.0.1:8765)
interactive_server.py (DeepSeek V4 Pro 大脑)
    ↕ HTTP (127.0.0.1:9880)
GPT-SoVITS (语音合成)
    ↕ HTTP (127.0.0.1:8000)
speech_provider.py (阿里云 DashScope 语音识别)
```

---

## 🚀 快速开始 (Quick Start)

### 环境准备 (Prerequisites)

* **Python 3.10+**
* **Node.js 18+**
* **FFmpeg** (需添加到系统 PATH，用于音频处理)
* **GPT-SoVITS** (本地启动，默认端口 `9880`)
* **API Keys**：
  * `DEEPSEEK_API_KEY`: DeepSeek 大模型密钥
  * `DASHSCOPE_API_KEY`: 阿里云语音识别密钥
  * `TRIPO_API_KEY`: （可选）图生 3D 模型生成

### 步骤 1：安装后端依赖

```bash
cd backend2
pip install -r requirements.txt
```

### 步骤 2：配置 API Keys

在 `backend2/interactive_server.py` 中填入你的 DeepSeek Key：
```python
DEEPSEEK_API_KEY = "your-deepseek-api-key"
```

在 `backend2/speech_provider.py` 中填入你的阿里云 Key：
```python
DASHSCOPE_API_KEY = "your-dashscope-api-key"
```

### 步骤 3：启动后端服务

**终端 1 — AI 大脑服务器（WebSocket）：**
```bash
cd backend2
python interactive_server.py
```
> 看到 `server listening on 127.0.0.1:8765` 即启动成功。

**终端 2 — 语音识别服务：**
```bash
cd backend2
python speech_provider.py
```
> 看到 `Uvicorn running on http://127.0.0.1:8000` 即启动成功。

**终端 3 — GPT-SoVITS（语音合成，可选）：**
按照 GPT-SoVITS 官方文档启动推理 API，确保端口 `9880` 可用。

### 步骤 4：启动前端界面

```bash
cd frontend2
npm install
npm run dev
```
打开浏览器访问 `http://localhost:5173`（或 Vite 提示的端口）。

---

## 🎮 使用指南

### 加载数字人

1. 将 `.glb` / `.gltf` / `.fbx` 文件拖入左侧上传区域，或输入提示词通过 AI 生成。
2. 数字人会出现在 3D 场景中，左侧同时自动生成对应的**专属控制面板**。

### 每个数字人的专属控制面板

加载数字人后，左侧滚动条中会为每个 Avatar 自动添加一张控制卡片：

| 控件 | 功能 |
|---|---|
| **编号圆圈** | 显示 Avatar 编号（Avatar 1, 2, 3...） |
| **做动作** 按钮 | 切换为动作模式，控制数字人行走、跳舞、踢东西 |
| **💬 回答** 按钮 | 切换为对话模式，数字人以语音回答你的问题 |
| **🎙️ 语音按钮** | 点击录音，识别后自动发送给该数字人 |
| **文字输入框** | 手动输入指令或问题，按 Enter 或点发送 |

### 交互示例

**做动作模式：**
- "向左走两步"
- "跳起来"
- "去踢那个橙色的箱子"
- "1号和2号分别向两边走"

**💬 回答模式：**
- "你叫什么名字？"
- "你喜欢什么？"
- "给我讲个笑话"

### 全局指令（左侧顶部面板）

用于控制整个场景：
- "生成一个赛博朋克街道场景"
- "生成一个机器人"

---

## 📁 目录结构

```
Nexus-3D-AI-Agent/
├── backend2/
│   ├── interactive_server.py   # AI 大脑 WebSocket 服务（DeepSeek V4 Pro）
│   ├── speech_provider.py      # 语音识别服务（阿里云 DashScope）
│   ├── agent_tools.py          # 3D 模型生成工具（Tripo3D）
│   ├── image_generator.py      # AI 图像生成工具
│   └── bambu_tool.py           # Bambu Lab 3D 打印机推送工具
├── frontend2/
│   ├── src/
│   │   ├── App.jsx             # 主界面（侧边栏、控制面板、WebSocket）
│   │   ├── components/
│   │   │   └── Web3DScene.jsx  # 3D 场景（Three.js + 物理引擎 + 数字人）
│   │   └── index.css           # 全局样式
│   └── package.json
├── 2Dto3D/                     # 2D 转 3D 辅助脚本
└── README.md
```

---

## 🛠️ 技术栈

| 层级 | 技术 |
|---|---|
| **前端框架** | React 18 + Vite |
| **3D 渲染** | Three.js + @react-three/fiber + @react-three/drei |
| **物理引擎** | @react-three/cannon |
| **后端通信** | Python WebSocket (websockets) + FastAPI (uvicorn) |
| **AI 大模型** | DeepSeek V4 Pro |
| **语音识别** | 阿里云 DashScope paraformer-realtime-v1 |
| **语音合成** | GPT-SoVITS（本地推理） |
| **3D 生成** | Tripo3D API |
| **图像生成** | Pollinations AI |

---

## ⚠️ 注意事项

- 语音识别需要**麦克风权限**，请在浏览器弹出权限请求时点击"允许"
- GPT-SoVITS 为可选组件；若未启动，数字人的回复将**只有文字，没有语音**
- 物理碰撞（踢箱子）依赖于 `@react-three/cannon`，数字人走到物体旁边会自动触发碰撞
- 目前语音识别仅支持中文（`paraformer-realtime-v1` 模型）

---

## 📄 License

MIT License
