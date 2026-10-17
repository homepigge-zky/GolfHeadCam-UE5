# GolfHeadCam · UE5 高尔夫挥杆与头戴相机仿真

可编辑的 Unreal Engine **5.8.3** 工程，游戏运行逻辑采用原生蓝图。包含简化挥杆、头骨绑定相机、右肩第三人称、鼠标环绕、相机控制台、镜头畸变，以及实时计算的球运动和实际位置轨迹。

**本公开仓库按要求排除所有视频和视频逐帧渲染序列。** 报告、演示 PPT、讲稿、参数、静态对照图和验证记录均已整理在内。

## 一次下载工程与实验报告

**[下载「UE5 工程＋实验报告」完整压缩包](https://github.com/homepigge-zky/GolfHeadCam-UE5/releases/latest/download/GolfHeadCam_UE5_Project_Report.zip)**

完整解压后，顶层只有两项：

```text
GolfHeadCam/             UE5 工程文件夹
  GolfHeadCam.uproject   用 UE5.8.3 打开此文件
  Content/              蓝图、动画、材质、人物和关卡
  Config/               工程配置
  ...                   说明、PPT、参数和验证记录
实验报告.pdf             当前版本的 5 页实验报告
```

请保留整个工程文件夹；`.uproject` 需要同级 `Content` 和 `Config`。本交付需要 UE5 编辑器，符合可打开、可编辑的工程要求。

也可单独查看 [最新实验报告](实验报告.pdf)，或在 [Releases](https://github.com/homepigge-zky/GolfHeadCam-UE5/releases/latest) 下载报告与 SHA-256 校验文件。

## 交付入口

| 内容 | 文件／目录 |
| --- | --- |
| UE5 可编辑工程 | [GolfHeadCam.uproject](GolfHeadCam.uproject)、[Content](Content)、[Config](Config) |
| 5 页实验报告（当前版本） | [实验报告.pdf](实验报告.pdf) |
| 早期报告（历史资料） | [实践报告.pdf](Deliverables/实践报告.pdf)；只保留在源码仓库，整合下载包不重复收录 |
| 10 页演示 PPT（基础版） | [演示讲解_10页.pptx](Deliverables/演示讲解_10页.pptx) |
| 约 10 分钟讲稿 | [演示讲稿](Deliverables/演示讲稿_约10分钟.txt) |
| 当前交付范围与验收入口 | [公开交付说明](Deliverables/公开交付说明.md)、[原任务验收清单](Deliverables/验收清单.txt) |
| 相机内参、畸变与导出参数 | [相机与导出参数.json](Deliverables/相机与导出参数.json) |
| 操作、蓝图、资产及复现说明 | [Docs](Docs)、[详细 README](README.txt) |
| 畸变静态对照图 | [Calibration](Output/Validation/Calibration) |
| 当前实时球运动验收 | [运行验收](Output/Validation/RealtimeTrajectory/Validation.json)、[资产验收](Output/Validation/RealtimeTrajectory/Asset_Validation.json) |
| 全部公开文件的 SHA-256 | [交付包内容清单.json](Deliverables/交付包内容清单.json) |

## 启动

1. 推荐下载上面的「UE5 工程＋实验报告」压缩包并完整解压；也可克隆仓库或选择 **Code → Download ZIP**。
2. 使用 UE **5.8.3** 打开 `GolfHeadCam.uproject`，等待资源和着色器加载完成。
3. 打开 `/Game/GolfHeadCam/Maps/L_GolfPractice`，点击编辑器 **Play**。
4. 初始为右肩后方视角，动作保持待命；移动鼠标可直接环绕人物。按 **Tab** 显示鼠标，再点击左下控制台的“播放”。

需要自行安装 UE；仓库不包含引擎、打包 EXE、缓存或临时作者脚本。使用的插件均为引擎内置插件，详见 [资产与依赖](Docs/资产与依赖.txt)。

## 操作

| 输入 | 功能 |
| --- | --- |
| 鼠标移动 | 默认直接环绕人物；人物根位置与朝向保持不变 |
| Tab | 显示鼠标操作 UI／返回鼠标环绕 |
| W / S、A / D、Q / E | 相机前后、左右、世界下降／上升；Shift 加速 |
| Home | 恢复右肩后方第三人称机位 |
| Space / R | 从头播放，球和轨迹同时复位 |
| P | 暂停／继续挥杆与球的运动 |
| V | 切换第三人称／头戴相机 |
| T | 显示／隐藏实际轨迹，默认关闭 |
| B / G | 头戴镜头畸变／标定网格 |
| F | 开关自由相机导航 |
| Esc | 结束编辑器运行 |

控制台提供播放、暂停、重新开始、视角、相机微调、镜头预设、网格和轨迹按钮。调整相机不改变动画播放时间。头戴相机绑定 `head` 骨骼，球杆绑定 `hand_r`。

## 实时球运动与版本范围

- 动作时间到 **2.25 秒**时发射一次，默认初速度 `(650, 0, 380) cm/s`。
- 原生 `ProjectileMovementComponent` 根据重力、场景碰撞、反弹和摩擦实时更新球；轨迹记录球已经走过的世界位置，没有预生成的未来曲线。
- 暂停控制动画与球，即使挥杆结束后球仍在飞行也可暂停／继续；隐藏轨迹保留历史，重新开始清空轨迹。
- 当前记录包含 **28 项运行检查和 25 项资产检查，均通过**。这些是源工程已执行的验证记录；本次上传验证了文件完整性与排除范围，没有重新执行全部运行测试。
- 挥杆是简化关键姿势动画；初速度和发射时刻可调，未实现空气阻力、自旋升力或球杆碰撞冲量求解。

根目录的 **实验报告.pdf 已更新为当前版本**，包括实时球运动、实际轨迹、肩后相机与继承 UI。`Deliverables/实践报告.pdf`、PPT 和原始任务验收清单保留早期基础版范围，其中的预设弹道和视频描述属于历史版本。当前蓝图功能以本 README、最新实验报告、公开交付说明和 `Output/Validation/RealtimeTrajectory` 为准。`BallTrajectory` 等更早的验证目录保留为历史证据；`RealtimeTrajectory/Session1` 是已标注作废的尝试，最终验收依据后续有效会话。

## 视频排除与回放

本仓库没有 MP4 或其他视频，也没有主视频的 720 张渲染帧。视频检查 JSON、文件名和静态截图属于文档／验证记录，予以保留；PDF 无嵌入附件，PPT 仅有静态图片。

“查看已保存视频”按钮和蓝图媒体资产保留，但仅下载本仓库时没有视频可播放。若需要回放，把本地两段视频分别放入：

```text
Content/Movies/GolfHead.mp4
Content/Movies/GolfThird.mp4
```

这些文件已加入 `.gitignore`。重新导出可参考 [复现视频与相机参数](Docs/复现视频与相机参数.txt)。历史视频不是当前实时运动版本的录像。

## 资产来源

人物、骨架及部分模板资源来自 Epic 官方 Third Person 模板；本工程编排了挥杆、场景、相机、界面、球运动及材质。动画为原生 Animation Sequence，畸变由原生材质节点实现；没有自定义 C++ 游戏模块或运行时 Python。

Epic 资源及引擎继续适用其各自许可，本仓库未为这些资源重新授予开源许可。来源和作者工具边界见 [资产与依赖](Docs/资产与依赖.txt)。
