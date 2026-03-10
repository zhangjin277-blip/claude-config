# 抖音分析工具 - 项目记忆

## 工具位置
- 部署目录: `C:\Users\Administrator\tools\douyin-analyzer\`
- Python venv: `.venv\Scripts\python.exe`
- Skill 文件: `C:\Users\Administrator\.claude\skills\analyze-douyin-win.skill.md`

## 踩坑记录
- **RTX 5090**: 需要 PyTorch nightly cu128（sm_120/Blackwell 架构）
- **setup.ps1**: UTF-8 BOM 编码问题，建议手动部署
- **PYTHONIOENCODING=utf-8**: 必须设置，否则 GBK 编码报错
- **Node.js .cjs**: 上级目录有 `"type": "module"` 的 package.json，脚本必须用 `.cjs`
- **--scroll-times**: 视频多时需增大，默认30不够96个视频，用80

## 用户偏好
- 输出 Word (.docx) 而非 JSON/Markdown
- 每个视频都要单独的深度分析（主题、观点、风格、论据）
- 报告按内容分类组织
- 输出文件复制到桌面 `D:\桌面\`
- 用 OOXML 美化文档样式（docx skill unpack/pack 流程）

## 关键脚本
- `analyze.py` — Playwright 抓取 + FunASR 转写
- `generate_report_grouped.cjs` — 分类 Word 报告（含 NLP 分析引擎）
- `polish_grouped.py` — OOXML 样式美化
