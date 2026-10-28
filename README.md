# 小练 Daily · lianlema

> 每天动一动，AI 教练在身边。

**开源 AI 微训练教练**：端侧姿态 AI 打底、云端大模型增强的随身 AI 教练。手机摄像头就是 AI 教练的眼睛——它在晨起、午间、久坐、睡前主动推荐 1-3 分钟微训练，看着你训练并实时纠错，用连续打卡习惯环陪你把运动变成日常，而不只待在聊天框里。

第四届开放原子大赛 · 2026 开源行业解决方案创新赛参赛作品（赛道一：开源基础软件与解决方案）。

## 三层 AI 架构（核心）

| 层 | 能力 | 实现 |
|----|------|------|
| L1 端侧 AI | 姿态识别 + 实时动作纠错（视频不出设备） | `miniprogram/` VKSession + 确定性角度规则 |
| L2 云端视觉模型 | 自研 ST-GCN 动作识别（11 类）+ 确定性角度规则 + VLM 快照评述 | `backend/app/providers/vision/` |
| L3 LLM 教练大脑 | 对话 / 计划解释 / 组末复盘（OpenAI 兼容：Qwen/DeepSeek/GLM/Ollama） | `backend/app/providers/coach_llm/` |

**安全红线**：动作分、计数、标准度等数值永远出自模型或确定性规则；LLM/VLM 只消费结构化事实产文案；任何模型故障自动降级，服务永不 5xx。

## 技术栈（全部构建于开源技术体系）

微信 VisionKit（端侧关键点）· RTMPose / RTMO（姿态估计）· 自研 ST-GCN（PyTorch，MM-Fit 等数据自训）· FastAPI + SQLAlchemy + Alembic（模型服务）· OpenAI 兼容协议（Qwen / DeepSeek / GLM / Ollama 可插拔）。第三方依赖与许可清单见 [THIRD_PARTY_LICENSES.md](THIRD_PARTY_LICENSES.md)。

## 目录

| 目录 | 说明 |
| --- | --- |
| `miniprogram/` | 微信小程序（7 页面：计划/AI 教练对话/训练/报告/记录/我的/评估） |
| `backend/` | FastAPI 模型服务（analyze / chat / plan_explain / usage / status + 用量计量） |
| `src/`、`model/` | 模型层：RTMPose + ST-GCN（自训权重与训练管线） |
| `app/`、`doc/` | 基座仓库的 RN App 与模型文档（本定制款未改动，保留供参考） |
| `test/`、`tests/`、`backend/tests/` | 测试：小程序核心层 + 模型服务 |

## 快速开始

### 1. 小程序（零配置可跑）

微信开发者工具导入 `miniprogram/` 目录，AppID 选「测试号」，编译即运行。未连接云端时，所有 AI 入口自动降级并明确标注（AI 教练页走本地模板回复）。

### 2. 云端模型服务

```bash
cd backend
cp .env.example .env      # 填 LLM_API_KEY（Qwen/DeepSeek/GLM/Ollama 任一 OpenAI 兼容端点）
pip install -e ".[test]"  # 或按 pyproject 安装依赖
uvicorn app.main:app --port 8000
```

### 3. 连接两者

`miniprogram/config.js`：

```js
MODEL_SERVICE_BASE_URL: 'http://<电脑局域网IP>:8000',  // 真机需同 WiFi
MODEL_SERVICE_API_KEY: '',                              // backend 配置 MODEL_SERVICE_API_KEYS 后填写
```

打开训练页的「☁️ 云端 AI 增强」开关即可体验云端大模型增强链路；`ENABLE_STGCN=1`（需健康 torch 环境）开启自研 ST-GCN 识别，详见 `backend/.env.example`。

## 测试

```bash
node --test "test/*.test.js"          # 小程序核心层 50 条（计划/姿态/权益/打卡/微训练/重排）
cd backend && python -m pytest tests/ # 模型服务 70 条（API/降级/计量/鉴权/角度规则）
```

## 能力开放与商业化边界

不设付费墙、不做定价与收费，所有能力对用户完整开放。模型服务已内置 API-key 鉴权 + SQLite 用量计量（按调用次数与 token），构成对外开放的计量数据面；后续计费售卖由平台/厂商在生态侧接入（如 API-key 对接计费网关），作品侧零改动。

## 路线图

- **v0.1（当前）**：小程序 7 页面 + 模型服务 5 组 API + 自研 ST-GCN 11 类动作识别，120 条测试全部通过。
- **v0.2**：动作库扩充（面向久坐办公人群）；端云延迟双轨对比演示；微信运动步数联动触发微训练推荐。
- **v0.3**：训练数据集与模型卡整理发布；贡献者指南与 Issue 模板落地；RN App 分支合并主线。
- **长期**：姿态模型 API 对外开放；与高校实验室共建动作数据集；探索企业健康管理的行业落地。

## 与基座的关系

本仓库从 [Health-525/lian_le_ma](https://github.com/Health-525/lian_le_ma)（本人作品，MIT）克隆，保留其模型层（`src/`、`model/`）作为 L2 的模型来源；`miniprogram/` 基线同步自同仓库的微信大赛参赛版。此后两分支产品定位、功能集与代码完全独立（本版：AI 服务 + 智能日常；微信版：健身工具 + 效率类目）。品牌差异：小练 Daily / 珊瑚红（回切开关见 `miniprogram/app.wxss` 首行注释）。

## 开源治理

- **License**：[MIT](LICENSE)
- **贡献指南**：[CONTRIBUTING.md](CONTRIBUTING.md)
- **第三方依赖许可**：[THIRD_PARTY_LICENSES.md](THIRD_PARTY_LICENSES.md)
- **Issue / PR**：欢迎通过 Issues 报告问题、提出建议；PR 流程见贡献指南
