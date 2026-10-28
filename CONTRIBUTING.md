# 参与贡献

感谢你对小练 Daily 的关注！本项目欢迎 Issue、讨论与 Pull Request。

## 报告问题

- 提交前请先搜索现有 Issue，避免重复。
- 使用「Bug 报告 / 功能建议」模板，写清：复现步骤、期望行为、实际行为、环境信息（微信开发者工具版本 / Python 版本 / 设备型号）。

## 开发环境

- **小程序**：微信开发者工具导入 `miniprogram/`（AppID 可用测试号）。
- **模型服务**：Python ≥ 3.11，`cd backend && pip install -e ".[test]"`。
- **模型层**：按根目录 `requirements.txt` 安装；PyTorch 需按设备单独安装（CPU / CUDA 版本不同，`requirements.txt` 未固定）。

## 提交 Pull Request

1. Fork 并创建分支：`feat/xxx`、`fix/xxx`、`docs/xxx`。
2. 提交前本地跑通测试：
   ```bash
   node --test "test/*.test.js"
   cd backend && python -m pytest tests/
   ```
3. Commit message 遵循 [Conventional Commits](https://www.conventionalcommits.org/zh-hans/v1.0.0/)（`feat:` / `fix:` / `docs:` / `chore:` …）。
4. PR 说明写清：动机、改动点、测试结果、是否有破坏性变更。

## 代码约定

- **安全红线不可突破**：动作分 / 计数 / 标准度等数值只能出自模型或确定性规则；LLM/VLM 只负责产文案；任何模型故障必须可降级，服务不返回 5xx。
- 新增模型服务 API 必须附带 pytest 用例；新增小程序核心逻辑必须附带 `node --test` 用例。
- 不引入付费墙、定价与收费逻辑。
- 不提交大体积可再生产物（训练数据 `.npy`、视频、模型缓存等，见 `.gitignore`）；自训模型权重（`model/*.pth`）除外。

## 行为准则

尊重每一位参与者，对事不对人。违规行为由维护者按 GitHub 社区准则处理。
