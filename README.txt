GolfHeadCam｜完整交付 v1.1.0｜2026-10-18

用 Unreal Engine 5.8.3 打开 GolfHeadCam.uproject，保留同级 Content 和 Config。
主地图 /Game/GolfHeadCam/Maps/L_GolfPractice，点击 Play 后默认肩后第三人称、0 秒待命。
鼠标直接环绕人物；Tab 显示/隐藏鼠标；控制台点击“播放”开始。
WASD 前后左右，Q 下降、E 上升，Shift 加速，Home 复位，F 自由相机微调。
Space/R 从头播放，P 暂停/继续，V 切换视角，T 实际轨迹开关，B 畸变，G 网格。
相机操作不移动人物根位置，不改变播放时间。头戴或视频面板中不接受自由相机输入。

当前正式资料：实验报告.pdf；Deliverables 下的报告副本、10 页 PPT、约 10 分钟讲稿、参数 JSON、验收清单。
两段演示 MP4：Output/FirstPerson 和 Output/ThirdPerson。
界面回放副本：Content/Movies/GolfHead.mp4、GolfThird.mp4，已随包提供。
新视频为当前实时碰撞与实际位置轨迹版本，均为 1080p、60 fps、360 帧、约 6 秒、无音轨。
视频面板支持选择、播放、暂停、重播；关闭后实时场景仍暂停。镜头预设不会改变已有视频。

详细复现见 Docs；本次视频、资产和定向回归见 Output/Validation/FullRelease。
此前 28 项实际交互验收位于 RealtimeTrajectory。其他验证目录按各自历史范围保留。
运行逻辑为原生蓝图，动画和材质为 UE 资产。编辑器作者工具不是运行依赖。
项目不包含引擎或缓存，交付可编辑 .uproject，需要已安装 UE5.8.3。
完整下载：https://github.com/homepigge-zky/GolfHeadCam-UE5/releases/tag/v1.1.0
