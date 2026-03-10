---
name: analyze-douyin
description: 分析抖音博主视频内容，自动下载、转写并生成 Word 分类分析报告（Windows + FunASR GPU 版）
---

# 抖音博主视频分析 (Windows + FunASR)

分析目标博主的视频，使用 FunASR Paraformer-large 模型进行中文语音转写（GPU加速），生成按内容分类的 Word (.docx) 深度分析报告。

## 执行步骤

### 1. 解析参数

从用户输入中解析：
- **博主URL**（必填）：抖音博主主页链接
- **视频数量**（可选）：默认分析最近 10 个视频
- **滚动次数**（可选）：`--scroll-times`，视频多时需增大（每次滚动约加载3-5个视频，96个视频需约 `--scroll-times 80`）
- **输出格式**（可选）：默认生成 Word (.docx)，支持分类报告

### 2. 检查依赖

```bash
$USERPROFILE/tools/douyin-analyzer/.venv/Scripts/python.exe -c "from funasr import AutoModel; import torch; print(f'CUDA: {torch.cuda.is_available()}'); print('FunASR: OK')"
```

如果依赖缺失，手动部署（不要用 setup.ps1，有编码问题）：
```bash
cd $USERPROFILE/tools/douyin-analyzer
python -m venv .venv
.venv/Scripts/pip install playwright funasr modelscope onnxruntime
.venv/Scripts/playwright install chromium
```

**GPU 注意事项**：
- RTX 5090 (Blackwell/sm_120) 需要 PyTorch nightly cu128：
  ```bash
  .venv/Scripts/pip install --pre torch torchaudio --index-url https://download.pytorch.org/whl/nightly/cu128
  ```
- 其他 GPU 使用稳定版即可：`pip install torch torchaudio --index-url https://download.pytorch.org/whl/cu124`

### 3. 运行分析（采集 + 转写）

```bash
PYTHONIOENCODING=utf-8 $USERPROFILE/tools/douyin-analyzer/.venv/Scripts/python.exe $USERPROFILE/tools/douyin-analyzer/analyze.py --url "博主URL" --limit N --scroll-times S
```

- `--limit N`：最多处理 N 个视频
- `--scroll-times S`：页面滚动次数，视频多时增大（默认30，96个视频用80）
- 每个视频处理时间约10-20秒（含下载+转写），96个视频约20-30分钟
- **重要**：设置 `PYTHONIOENCODING=utf-8` 避免 GBK 编码错误

### 4. 读取结果

读取 `$USERPROFILE/tools/douyin-analyzer/output/result.json`，包含：
- 博主信息（昵称、粉丝数、简介等）
- 每个视频的元数据（标题、点赞、评论、发布时间）
- 语音转写文本（transcript）

### 5. 生成 Word 分析报告

使用 Node.js + docx 库生成 Word 报告。报告脚本位于：
- `$USERPROFILE/tools/douyin-analyzer/generate_report_grouped.cjs` — 分类报告（推荐）
- `$USERPROFILE/tools/douyin-analyzer/generate_report.cjs` — 逐视频报告

```bash
cd $USERPROFILE/tools/douyin-analyzer && node generate_report_grouped.cjs
```

**注意**：脚本必须用 `.cjs` 扩展名，因为上级目录 package.json 有 `"type": "module"`。

#### 5.1 深度内容分析引擎

报告生成脚本内置内容分析，对每个视频的转写文本提取：
- **主题**：基于标题 + 转写内容综合判断
- **核心观点**：通过意见标记词（"我觉得"、"建议"、"不值得"等）提取
- **风格**：口语化/书面化、直接/委婉等维度分析
- **关键论据**：通过因果推理模式（"因为...所以"、数字+单位等）提取
- **关键词**：高频实词提取
- **目标受众**：基于内容特征推断
- **内容类型**：教程/评测/分析/娱乐等

#### 5.2 内容分类

分析所有视频后，按内容相似度归类（通常 6-10 个大类），每个类别包含：
- 类别名称、描述、主题色
- 该类别下所有视频的完整分析

报告结构：封面 → 目录 → 数据概览（含分类汇总表）→ 各分类章节 → 每视频详细分析

### 6. 文档美化（OOXML）

使用 docx skill 的 unpack/pack 工具进行 OOXML 级别样式优化：

```bash
# 解包
python "<unpack.py路径>" "output/报告.docx" unpacked_dir

# 编辑 styles.xml, document.xml, header1.xml, footer1.xml
# - styles.xml：添加段落间距、标题边框、背景色
# - document.xml：段落左边框、转写文本灰色边框
# - header/footer：分隔线

# 重新打包
python "<pack.py路径>" unpacked_dir "output/报告.docx"
```

美化脚本参考：`$USERPROFILE/tools/douyin-analyzer/polish_grouped.py`

### 7. 输出到桌面

```bash
cp "$USERPROFILE/tools/douyin-analyzer/output/报告.docx" "D:/桌面/"
```

## 关键文件清单

| 文件 | 用途 |
|------|------|
| `analyze.py` | 主采集+转写脚本（Playwright + FunASR） |
| `generate_report_grouped.cjs` | 分类 Word 报告生成器（含深度分析引擎） |
| `generate_report.cjs` | 逐视频 Word 报告生成器 |
| `polish_grouped.py` | OOXML 文档美化脚本 |
| `output/result.json` | 采集+转写原始数据 |
| `output/*.docx` | 生成的 Word 报告 |
