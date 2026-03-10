# 用户信息
- 奇迹互娱游戏公司，直播部部门负责人
- 主导开发了 miracle-pro3 中台管理系统（React 19 + Express + SQLite，V9.5.0），管理主播、内容、运营、数据等
- 个人也在做三国题材卡牌游戏（Cocos Creator，目前定位单机）

# 用户偏好

## 文档生成
- 生成的文档文件名需要加版本号（如 V1、V2），方便追踪修改历史
- 对外文档措辞要接地气，不用"贵方/我方"，用具体名称（如"项目组"、"长沙基地"）
- 对外文档不写太满，留谈判空间（约70%），运营成本细节不暴露

## 沟通风格
- 始终使用中文
- 讨论问题时一个一个问，不要一次性抛出太多

## Cocos Creator 开发原则（第一原则）
- **必须使用场景化方案**：UI 都搭在 scene 文件中，不用程序化创建
- **生成占位图片**：用 Python 脚本生成 PNG 占位图，方便后续美术替换
- **绑定好图片和按钮**：在 scene JSON 中直接引用图片 UUID 和绑定 @property
- 脚本只负责逻辑（按钮事件、面板切换等），不负责 UI 构建

## 业务方案
- 主播评估体系（初试+入职后），详见 [anchor-evaluation.md](anchor-evaluation.md)
- 面试扫码填表+自动打印功能，详见 [interview-form-plan.md](interview-form-plan.md)

## 项目信息
- 项目路径: /Users/zhangjin/Desktop/NewProject_1/NewProject_1/NewProject_1/
- 场景: Scene.scene(登录), Main.scene(主界面), Formation.scene(布阵), Recruit.scene(招募)
- LoginPanelController 脚本类型 ID: 4155djlpV9Kdp5HbaIFnNK5
- 详细项目笔记见 [cocos-project.md](cocos-project.md)

## 视频生成平台
- 正在搭建 SaaS 视频生成平台，详见 [video-saas-plan.md](video-saas-plan.md)
- 部署在 M1 Mac，NAS 负责存储
- 第一个模板：SOP 培训视频（基于三冰签约转化SOP）
- Remotion 视频风格：不要浮动抖动，出场动画后保持静止，沉稳专业
- 配音：macOS TTS 和 Edge TTS 效果不好，用户偏好剪映AI配音

# currentDate
Today's date is 2026-03-09.
