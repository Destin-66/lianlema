# 第三方依赖与许可清单

> 整理日期：2026-10-27。许可信息来自 PyPI / npm 上游声明；如与上游最新声明不一致，以上游仓库为准。
> 本项目以 MIT 协议分发，下列依赖均为 MIT / BSD / Apache-2.0 等宽松协议，或弱 Copyleft（MPL-2.0，文件级）协议，与 MIT 兼容。

## 模型层（根目录 `requirements.txt`）

| 依赖 | 许可 | 用途 |
| --- | --- | --- |
| rtmlib | Apache-2.0 | RTMPose 姿态估计封装 |
| opencv-python | Apache-2.0 | 图像 / 视频处理 |
| onnxruntime-gpu | MIT | RTMO ONNX 模型推理 |
| numpy | BSD-3-Clause | 数值计算 |
| scipy / scikit-learn / pandas | BSD-3-Clause | 科学计算 / 机器学习 / 数据处理 |
| matplotlib | Matplotlib License（BSD 风格） | 可视化 |
| Cython | Apache-2.0 | 扩展编译 |
| Flask / Jinja2 / Werkzeug / MarkupSafe | BSD-3-Clause | Web Demo |
| ollama | Apache-2.0 | 本地 Ollama 客户端 |
| imageio-ffmpeg | BSD-2-Clause | 捆绑 ffmpeg |
| coloredlogs / humanfriendly | MIT | 日志 |
| tqdm | MPL-2.0（文件级弱 Copyleft） | 进度条 |
| protobuf / networkx / sympy / filelock 等 | 以上游声明为准 | 传递依赖 |

> **注意**：`src/` 与 `web_app.py` 依赖 PyTorch，但 `requirements.txt` 未固定 torch 版本——需按设备（CPU/CUDA）单独安装。PyTorch 本身为 BSD-3-Clause 风格许可（含部分第三方组件，见其随附的 THIRD_PARTY_LICENSES）。建议后续在 `requirements.txt` 中增加注释行说明，或将 torch 列为按平台可选的依赖组。

## 模型服务（`backend/pyproject.toml`）

| 依赖 | 许可 | 用途 |
| --- | --- | --- |
| fastapi / uvicorn / pydantic / pydantic-settings | MIT | Web 框架与校验 |
| sqlalchemy / alembic | MIT | ORM 与数据库迁移 |
| httpx | BSD-3-Clause | 异步 HTTP 客户端 |
| python-dotenv | BSD-3-Clause | 环境变量加载 |
| pytest / pytest-asyncio | MIT | 测试 |
| hypothesis | MPL-2.0（仅测试依赖） | 属性测试 |

## 小程序与 App（`app/package.json`）

| 依赖 | 许可 | 用途 |
| --- | --- | --- |
| expo / @expo/* | MIT | RN 构建与运行时 |
| react / react-dom / react-native | MIT | UI 框架 |
| @react-navigation/* | MIT | 导航 |
| zustand / zod | MIT | 状态管理 / 校验 |
| react-native-health | MIT | 健康数据（预留） |
| typescript | Apache-2.0 | 开发依赖 |

## 模型与数据

- **自训权重**：`model/*.pth`、`model/*.onnx` 为本仓库基于公开数据集自训产生，随本仓库以 MIT 协议分发。
- **训练数据来源**：动作识别模型训练数据包含公开数据集（如 MM-Fit）与自采数据；使用公开数据集时请遵循其上游许可与引用要求。随仓库发布的 `.npy` 中间产物可由 `tools/` 与 `src/` 中的管线再生成，已不纳入版本追踪（见 `.gitignore`）。
- **模型输出**：LLM/VLM 走 OpenAI 兼容协议，按所接入服务商（Qwen / DeepSeek / GLM / Ollama）的条款使用。

## 合规自查结论

- 未发现 GPL / AGPL 等强 Copyleft 依赖被引入本仓库分发物。
- 发现两处需在使用文档中明示的事项：PyTorch 未固定版本（见上）；公开数据集的上游许可需在模型卡中进一步说明（见上）。
