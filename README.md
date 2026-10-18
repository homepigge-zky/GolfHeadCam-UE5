# GolfHeadCam：UE5 高尔夫挥杆与头戴相机仿真

**v1.1.0 完整交付**：可编辑蓝图工程、双视角演示视频、5 页实验报告、10 页 PPT、约 10 分钟讲稿、参数与验收证据。开发和验证环境为 **Unreal Engine 5.8.3 / Windows**。

- [完整下载包（工程 + 视频 + 报告）](https://github.com/homepigge-zky/GolfHeadCam-UE5/releases/download/v1.1.0/GolfHeadCam_UE5_Project_Report.zip)
- [头戴演示视频](https://github.com/homepigge-zky/GolfHeadCam-UE5/releases/download/v1.1.0/Golf_HeadCamera.mp4) · [第三人称演示视频](https://github.com/homepigge-zky/GolfHeadCam-UE5/releases/download/v1.1.0/Golf_ThirdPerson.mp4)
- [实验报告 PDF](https://github.com/homepigge-zky/GolfHeadCam-UE5/releases/download/v1.1.0/Experiment_Report.pdf) · [10 页演示 PPT](https://github.com/homepigge-zky/GolfHeadCam-UE5/releases/download/v1.1.0/Presentation_10_Slides.pptx)
- [公开 Release 与 SHA-256 校验](https://github.com/homepigge-zky/GolfHeadCam-UE5/releases/tag/v1.1.0)

两段视频由当前工程重新渲染，均为 1920×1080、标称 60 fps、360 帧、约 6 秒，无音轨。1 秒准备、4 秒完整挥杆、1 秒收杆保持；显示实际球运动与金色历史轨迹。视频使用无畸变基线，畸变对照见报告与 Calibration 图像。

## 打开工程

完整解压 ZIP，用 UE5.8.3 打开 `GolfHeadCam/GolfHeadCam.uproject`。保留同级 `Config` 和完整 `Content`；不能只下载单个 `.uproject`。直接克隆本仓库也可打开根目录的工程入口。

加载 `/Game/GolfHeadCam/Maps/L_GolfPractice` 并点击 Play。每次启动默认右肩后方视角，动作时间为 0，等待手动播放。移动鼠标直接环绕人物，人物根位置不随输入移动。

| 操作 | 功能 |
|---|---|
| Tab | 显示界面鼠标 / 返回鼠标环绕 |
| 控制台播放、暂停 | 显式开始 / 暂停或继续挥杆及球运动 |
| Space / R；P | 从头播放；暂停或继续 |
| WASD / Q、E / Shift | 移动相机 / 下降、上升 / 加速 |
| Home / F | 恢复肩后机位 / 切换自由相机微调面板 |
| V | 第三人称与头戴视角切换 |
| T | 显示或隐藏实际轨迹，隐藏保留历史，重播清空 |
| B / G | 头戴畸变开关 / 标定网格 |
| UI 镜头预设 | 无畸变、桶形、枕形、任务参数 |
| 查看已保存视频 | 选择两段视频并播放、暂停、重播或关闭 |

先点击运行窗口并使用英文输入法，确保快捷键得到输入。头戴及视频模式屏蔽自由相机输入。相机移动不重置或暂停动画；视频面板打开时暂停实时场景，关闭后需手动继续。

## 当前实现

- `AS_GolfSwing`：简化 4 秒原生骨骼动画；球杆附着 `hand_r`，相机附着 `head`。
- `BP_GolfGolfer`：蓝图控制动作、相机、球运动及轨迹。球由原生 ProjectileMovement 按初速度、重力、扫掠碰撞、反弹和摩擦实时更新，轨迹采样实际位置。
- `WBP_GolfCameraConsole` 真实继承 `WBP_CameraControl`，提供鼠标操作及独立媒体回放。
- 镜头使用原生 Brown-Conrady 后处理材质，无 Custom HLSL；内参和畸变系数是明确给出的仿真假设。
- 运行不需要作者 Python 脚本或自定义 C++ 模块。人物、骨架复用 Epic 官方模板，动作不是真人动作捕捉。暂未计算杆面冲量、自旋升力、空气阻力或真实传感器噪声。

## 文档与证据

- [当前实验报告](实验报告.pdf)；`Deliverables/实践报告.pdf` 是同一版本的副本。
- [PPT](Deliverables/演示讲解_10页.pptx)、[约 10 分钟讲稿](Deliverables/演示讲稿_约10分钟.txt)、[验收清单](Deliverables/验收清单.txt)、[相机与导出参数](Deliverables/相机与导出参数.json)。
- [相机控制台说明](Docs/相机控制台使用说明.txt)、[视频复现与成像参数](Docs/复现视频与相机参数.txt)、[资产与依赖](Docs/资产与依赖.txt)。
- `Output/Validation/FullRelease`：本次视频 19 项、资产 25 项、PIE 定向回归 8 项检查通过。两段视频各 360 帧完整解码，并检查运行遥测、固定头戴安装和真实轨迹。
- `Output/Validation/RealtimeTrajectory/Validation.json`：此前 28 项实际交互检查通过。其余目录保留对应历史版本范围，不能将全部数字合并为本次完整重测；定向回归调用蓝图控制函数，不冒充键鼠操作。
- `Deliverables/交付包内容清单.json`：逐文件大小和 SHA-256。Release ZIP 内清单以 ZIP 根目录为基准。

`Output/FirstPerson`、`Output/ThirdPerson` 保存正式 MP4；`Content/Movies` 保存相同视频供原生 UI 回放。项目包含两段独立视频、四个 MP4 文件。缓存、引擎、临时作者工具和主视频全部逐帧 PNG 不随包交付；选取的实帧和静态畸变对照保留。

交付形式是 UE5 可打开的工程，需已安装兼容引擎。重渲染使用保存的 Level Sequence 与 Movie Render Queue。v1.0.0 为历史不含视频版本，当前使用 v1.1.0。
