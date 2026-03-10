# 项目记忆

## 高频踩坑点

### createPortal 弹窗渲染（反复出现）
所有 `fixed` 定位的弹窗/抽屉/模态框 **必须** 用 `createPortal(jsx, document.body)` 渲染。
不用会导致按钮被父容器裁剪、看不到等 UI 问题。此问题已多次出现。
新增或修改弹窗时第一件事就检查有没有 portal。

已全部修复，之前缺少的文件已补上 createPortal：
- `src/views/AnchorDataView.jsx` ✅
- `src/views/BusinessView.jsx` ✅
- `src/views/TeamStructureView.jsx` ✅
- `src/views/TeamManagementView.jsx` ✅
- `src/views/recruitment/OnlineRecruitmentPanel.jsx` ✅

## NAS 部署

- NAS 路径: `//172.16.35.44/docker/miracle-pro-cs/`
- Docker 容器名: `miracle-pro-cs`
- 容器内路径: `/app/server`
- canvas 模块需要系统依赖: `apt-get install build-essential libcairo2-dev libjpeg-dev libpango1.0-dev libgif-dev librsvg2-dev`
- reportService.js 中 canvas 做了懒加载，缺失不会崩溃

## 钉钉图片推送

- catbox.moe 图床偶尔不稳定，uploadImage 已加3次重试
- 钉钉 webhook 不支持 base64 图片，必须用 URL

## 术语规范（2026-03-03 统一）

已在 CLAUDE.md 中写入完整术语规范，新增模块必须遵循：
- 运营（禁止：运营经纪人/经纪人/负责人员）
- 产品（禁止：推广项目/推广产品/游戏）
- 待审批/已拒绝/已封禁（禁止：待审核/已驳回/已停用）
- 团队成员（禁止：普通用户）
- 三国:冰河时代（半角冒号）
- 6个产品：途游斗地主、富豪麻将、三国:冰河时代、捕鱼大作战、途游休闲捕鱼、电玩捕鱼
- bizConfig/BatchImporter 等数据兼容层的旧术语不能改（Excel导入需要）

### 待完成：数据库迁移（B类）
等用户备份数据库后执行：
1. 风控 gameName: '三国冰河时代'→'三国:冰河时代', '途游斗地主（比赛版）'→'途游斗地主'
2. 风控表单默认值: '途游斗地主（比赛版）'→'途游斗地主'
3. 编导工作流 project: 棋牌线下/线上→新产品名
4. 投放数据产品名对齐（可选）
