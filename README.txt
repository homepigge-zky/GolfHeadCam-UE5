【GitHub 公开交付版说明｜2026-10-17】
本仓库按要求不收录任何视频文件，也不收录主视频逐帧渲染序列。
下文中“视频已交付”“完整压缩包”等描述属于本地完整交付或历史验证范围，并不表示视频在此仓库中。
报告、PPT 和原始验收清单保留基础版范围；当前功能以 README.md、公开交付说明及 RealtimeTrajectory 验证记录为准。
已保存视频回放需自行补回 Content/Movies/GolfHead.mp4 和 GolfThird.mp4；工程实时运行不依赖这两段视频。
文档和验证记录中的本机路径已用 <PROJECT_DIR>、<UE_ROOT>、<AUTHOR_HOME>、<AUTHOR_TEMP> 代称。

GolfHeadCam｜高尔夫挥杆与头戴相机仿真
操作说明更新｜2026-10-17（原基础版：2026-10-16）｜Unreal Engine 5.8.3

快速启动

1. 使用 Unreal Engine 5.8.3 打开同目录的 GolfHeadCam.uproject。
2. 默认演示地图为 /Game/GolfHeadCam/Maps/L_GolfPractice。
   如未自动打开，在内容浏览器进入 GolfHeadCam/Maps，双击 L_GolfPractice。
3. 等资源和着色器加载完成，点击编辑器 Play／运行，显示左下角“相机控制台”。
   每次启动均为角色右肩后方的第三人称、动作时间 0 秒待命；鼠标环绕默认开启，畸变、网格及视频面板关闭，不自动播放。
4. 按 Tab 显示鼠标，再点击控制台“播放”才开始挥杆；“暂停”后点“播放”继续当前位置，“重新开始”重置动作和球。
5. 运行后移动鼠标即可环绕人物，不需要右键或 F；WASDQE 可直接移动相机。
   Tab 在环绕与界面鼠标间切换。“自由相机”与 F 仍可开关导航；微调面板的 10 个按钮每次平移 25 cm、转向 5°。
   点击“复位第三机位”恢复本次启动时的第三相机位置和朝向；调机位不会改变动画时间、速率或暂停状态。
6. 在控制台选择实时视角、镜头预设或标定网格；点击“查看已保存视频”打开两段成片的独立回放面板。
   关闭视频后实时场景保持暂停，点击控制台“播放”继续。原 Space/R/V/P/B/G 及自由相机快捷键仍可使用。

键盘操作

Space / R：从头播放，球同时复位；清除暂停状态。
V：切换第三人称观察视角与头戴视角；切换时退出自由模式。
P：暂停／恢复挥杆、实时球运动与轨迹记录；暂停时仍可切换视角。
B：打开／关闭头戴镜头畸变。该效果仅加在 HeadCamera 上。
G：打开／关闭校验网格；网格仅在头戴视角显示。
W/S：相机前后；A/D：相机左右；Q/E：世界下降／上升。直接按键即可移动，Shift 为 3 倍速度。
移动鼠标：默认环绕人物，无需按住右键。Tab：显示鼠标操作界面／返回环绕。
F：进入／退出自由模式；退出时同时停止鼠标环绕。
Home：第三人称下恢复本次启动的肩后机位，普通第三人称也可使用。
Esc：结束编辑器运行。Tab 为工程内界面入口；Shift+F1 仍可释放编辑器鼠标。

如果按键没有响应：先点击运行画面，关闭遮挡画面的浮动窗口，并切换至英文输入法。
头戴相机跟随头骨，不使用鼠标自由转向，也不通过追踪球的位置来修正朝向。

实现内容

- 4 秒、60 fps 原生骨骼动画：准备、上杆、下杆、击球、收杆。
- HeadCamera 固定绑定 head 骨骼；球杆固定绑定 hand_r；第三相机默认从角色右肩后方开始，鼠标环绕人物，WASDQE 移动机位。
- 蓝图实时球运动：动作时间 2.25 秒发射，按初速度、重力和场景碰撞飞行，落地弹跳、摩擦减速。
- 头戴相机内参和 Brown–Conrady 径向／切向畸变，以及无畸变／有畸变对照网格。
- 两条原生 Level Sequence、Movie Render Queue 预设和队列；当前工程重新渲染会采用实时球运动，已有双视频保留基础版内容。
- 原生 UMG 相机控制台：实时播放／暂停、双视角、自由相机与鼠标机位微调、四种镜头预设、网格和既有视频浏览。

所有游戏运行逻辑保存在可编辑蓝图中。动画是原生 Animation Sequence；畸变使用原生材质节点，
没有 Custom HLSL、自定义 C++ 游戏模块、运行时 Python 或外部插件。
挥杆为简化关键姿势动作；球用原生运动组件实时处理重力和碰撞。发射时刻与初速度可调，未包含空气阻力、自旋或球杆冲量模型。
相机参数是明确给出的仿真假设，不是实体相机的测量标定结果。

从哪里继续

- Docs/自由第三人称相机说明.txt：默认鼠标环绕、Tab 界面入口、WASDQE 移动、鼠标微调与机位复位。
- Docs/相机控制台使用说明.txt：界面入口、按钮、四种镜头系数和保存视频浏览。
- Docs/挥杆与相机说明.txt：蓝图入口、动作时间、球轨迹、绑定与可调参数。
- Docs/复现视频与相机参数.txt：无需 Python 的 MRQ 操作流程、时间轴、内参与畸变约定。
- Docs/资产与依赖.txt：引擎／插件、资产来源、作者工具边界与工程迁移说明。
- Deliverables/：正式报告、演示讲解材料、最终验收清单及交付入口。
- Output/FirstPerson/LS_Golf_HeadCamera_HeadCamera.mp4：头戴视角视频。
- Output/ThirdPerson/LS_Golf_ThirdPerson_ThirdCamera.mp4：第三人称视频。
- Output/Validation/Calibration/：Grid/Scene 的无畸变／有畸变 PNG 对照。
- Output/Validation/：验证记录与参数文件。每份记录的适用版本和检查范围以其内容为准。

核心资产

界面入口：/Game/GolfHeadCam/UI/WBP_GolfCameraConsole（由 BP_GolfHUD 在运行时创建并加入视口）
界面父类：/Game/GolfHeadCam/UI/WBP_CameraControl。WBP_GolfCameraConsole 是其真实子 Widget Blueprint，继承原有布局和控制逻辑。
媒体资产：/Game/GolfHeadCam/UI/Media（MP_SavedGolf、MT_SavedGolf、MS_GolfHead、MS_GolfThird、M_UI_SavedGolf）
比较预设：/Game/GolfHeadCam/Materials/MI_HeadLens_Barrel、MI_HeadLens_Pincushion
角色与运行逻辑：/Game/GolfHeadCam/Blueprints/BP_GolfGolfer
挥杆动画：/Game/GolfHeadCam/Animations/AS_GolfSwing
演示地图：/Game/GolfHeadCam/Maps/L_GolfPractice
双视角序列：/Game/GolfHeadCam/Sequences/LS_Golf_HeadCamera、LS_Golf_ThirdPerson
输出预设：/Game/GolfHeadCam/RenderPresets/MRQ_Golf_1080p60
输出队列：/Game/GolfHeadCam/RenderPresets/Q_GolfViews

版本与交付验证

UE 5.8.3，工程关联版本 5.8；内置 MovieRenderPipeline 插件用于导出，ElectraPlayer 用于视频回放。
界面和媒体资产使用引擎内置 UMG、Media/MediaAssets 模块，无外部插件。
原模板的 GameplayStateTree 和仅编辑器使用的 ModelingToolsEditorMode 继续保留。
基础版验证（原正式视频与报告对应版本）：四个蓝图均为 0 错误、0 警告；
动作／绑定／球运动的 11 项运行检查通过。以下媒体和 271 文件副本数字属于基础版。
两个视频均为 H.264、1920×1080、360 帧，标称 60 fps，完整解码验证通过。
实际时长均为 5.999967 秒，平均帧率约 60.0001667；与名义 6 秒的差异为编码尾时间戳的 33 微秒舍入。
四张镜头对照已输出；网格畸变与独立公式重采样的图像比较通过。
基础版干净工程副本的 271 个核心文件哈希一致；使用已安装引擎的独立 -game 模式启动，
未启用作者 bridge、未传入运行时 Python 脚本，Space/R/V/P/B/G 及收杆、网格视角限制均已实际检查。
证据见 Output/Validation 下的 Final_Blueprint_Validation.json、Delivery_Runtime_Validation.json、
Lens_Render_Validation.json、Video_Validation.json、Standalone_Validation.json。

本次交付包名：GolfHeadCam_集成控制台版.zip，位于本工程文件夹的上一级。
旧 GolfHeadCam_自由相机版.zip、GolfHeadCam_相机控制台版.zip 与基础版 GolfHeadCam_完整交付.zip 均保留。
新版包含完整 Content（含 UI/Media、两种 MI 和 Movies 视频副本）、Config、uproject、Docs、README、Deliverables、正式 MP4、校验 PNG 和验证证据。
两条视频共 720 张逐帧 PNG 保留在本地原工程，压缩包不收录这些大体积帧图。
交付为可编辑 UE 工程，需要已安装的 UE5.8.3；没有制作或验证打包 EXE，也不包含引擎安装、缓存和临时作者环境。

[相机控制台升级说明 BEGIN]
相机控制台 UI 升级（上一版功能与记录）

运行 L_GolfPractice 后，左下角新增原生蓝图界面：播放／暂停／重新开始、实时双视角、
无畸变／桶形／枕形／任务参数四项镜头预设，以及会自动切到头戴视角的网格按钮。
暂停后“播放”继续当前位置；“重新开始”重置动作和球。原实时快捷键保留。

“查看已保存视频”在中央面板浏览两段既有 MP4。视频有独立播放／暂停／重新播放按钮；
关闭视频返回仍暂停的实时场景。成片是无畸变基线，实时镜头选择不会重编码或覆盖它们。
本次升级没有新增录像功能；重新导出仍使用原 MRQ 流程。

界面资产：/Game/GolfHeadCam/UI/WBP_CameraControl。
媒体副本：Content/Movies/GolfHead.mp4、GolfThird.mp4，使用工程内相对路径。
新增依赖：Epic 内置 ElectraPlayer 插件。运行逻辑仍全部由蓝图实现。
详细按钮、四种系数和边界说明见 Docs/相机控制台使用说明.txt。

上一版 UI 升级不改写原正式视频、报告和 PPT。其验证与基础版分开记录，以下不是本次自由相机的验收：
上一版新增或修改的 WBP_CameraControl、BP_GolfHUD、BP_GolfGolfer 三个蓝图均为 0 错误、0 警告；
37 项 UI 检查通过、0 失败、0 未观察。干净副本的 281 个核心文件哈希全部一致；
通过已安装 UE 的原生 -game 模式实际验证双视频显示／播放／暂停及关闭返回暂停场景，无作者 bridge。
证据位于 Output/Validation/UI：UI_Validation.json、UI_Asset_Validation.json、UI_Standalone_Validation.json。
这是可编辑工程及原生 -game 副本验证，不是 cook 或打包 EXE 验证。
[相机控制台升级说明 END]

[自由第三人称相机说明 BEGIN]
自由第三人称与集成控制台

每次启动使用第三人称，动作时间为 0 秒待命；默认鼠标环绕，镜头畸变、网格和视频面板关闭。
按 Tab 显示鼠标，点击“播放”开始动作；相机操作本身不会启动、暂停或重置挥杆。
主卡“自由相机”按钮和 F 仍可显式开关导航，退出时停止环绕；Tab 可返回环绕。
“复位第三机位”恢复本次启动时的第三相机位置和朝向。

自由模式下右下角显示“机位微调”：前进／后退／左移／右移／升高／降低六个移动按钮，
向左看／向右看／抬头／低头四个转向按钮。每次点击移动 25 cm 或转向 5°，无需按住右键。
W/S 相机前后、A/D 相机左右、Q 世界下降、E 世界上升，均无需按住右键；移动鼠标即可环绕人物。
Shift 为 3 倍速度。默认移动速度 150 cm/s，鼠标敏感度 2.0；Tab 显示鼠标点击界面，再按 Tab 返回环绕。
Home 在普通第三人称和自由模式下均可恢复启动肩后机位；退出再进入保留本次运行机位，不跨结束运行保存。

自由相机只修改 ThirdCamera，不改变动画时间、速率或暂停状态。打开已保存视频会退出自由模式、
隐藏微调面板、阻止相机输入，并暂停实时场景；关闭后仍暂停并可操作界面，在第三人称下按 Tab 返回环绕。
全部运行逻辑为原生蓝图。运行界面 WBP_GolfCameraConsole 真实继承 WBP_CameraControl，
父蓝图保留共同布局和按钮逻辑；子蓝图作为界面入口。详细说明见 Docs/自由第三人称相机说明.txt。

本次包名 GolfHeadCam_集成控制台版.zip；旧自由相机版、相机控制台版及基础版包均保留。
历史自由相机验证记录位于 Output/Validation/FreeCamera，以下数字对应本次集成控制台升级前的版本：
两个修改蓝图均为 0 错误、0 警告；自由相机组合验收 84 项通过，含真实左 Shift 150→450 cm/s 补测。
281 个核心文件干净副本哈希一致，原生 -game 机位与动画操作通过。证据见 Validation.json、
Asset_Validation.json、Standalone_Validation.json；并非打包 EXE 测试。
这些历史数量不作为本次默认待命、鼠标微调及继承子界面的验证数量。
[自由第三人称相机说明 END]

[集成控制台验证记录 BEGIN]
本次集成控制台验证记录

本次集成控制台正式交互验收采用 Session3、Session4 两轮实际操作：75 项通过，0 失败，0 未观察。
覆盖默认第三人称／0 秒待命、显式播放、自由相机、10 个鼠标微调按钮、复位、继承界面及既有控制；
子界面的 87 个继承控件均已确认存在。正式结果见 Output/Validation/IntegratedConsole/Validation.json 和 Validation.txt。
首次测试受到编辑器作者桥“Executing Python Script”通知窗口遮挡，失败结果保留为同目录下的
Initial_Obstructed_Validation.json／.txt。更换编辑器测试启动方式后完成上述正式两轮验收；
排除该外部测试遮挡时没有修改运行蓝图。本段只记录正式交互验收，不替代资产检查或独立运行验证。
本次交付包名为 GolfHeadCam_集成控制台版.zip；原报告、PPT 和基线成片继续保持各自版本范围。
[集成控制台验证记录 END]

[球轨迹可视化说明 BEGIN]
实时球飞行与历史轨迹（2026-10-17）

运行后先按 T 开启轨迹，或按 Tab 显示鼠标后点击“显示实时轨迹（T）”。轨迹默认关闭。
击球前没有预览曲线；球飞到哪里，金色轨迹就延伸到哪里，落地弹跳和滑动也会记录。
轨迹读取 GolfBall 每帧的实际世界位置，不用发射参数预先生成整条曲线。
隐藏轨迹只改变显示，后台继续记录本次击球；再次显示能看到已飞过的部分。重新开始／复位时清空旧轨迹。
轨迹线半径默认 0.8 cm，最多保留 2048 段；超出时移除最早一段。线段本身无碰撞。
球使用原生 ProjectileMovementComponent，根据初速度、世界重力、场景碰撞、反弹和摩擦实时更新。
播放／暂停同时控制挥杆和球；即使四秒挥杆结束后球仍在运动，也能在界面暂停、继续，继续不会重发一球。
打开视频面板会暂停球并隐藏轨迹；关闭后保持暂停。已有 MP4 仍是先前版本，不是本次实时运动的录像。
鼠标环绕、Tab、WASDQE、Home 和人物骨骼附着保持原有作用，不因轨迹开关改变。
实现保存在原生蓝图、运动组件、ISM 和材质中，没有自定义运行时 Python 或 C++。
当前验收见 Output/Validation/RealtimeTrajectory；以前 BallTrajectory 的预测路径检查仅是历史记录。
[球轨迹可视化说明 END]

[肩后相机直接导航说明 BEGIN]
肩后机位与鼠标环绕参数（2026-10-17）

启动机位相对角色根位置 (-65,-240,170) cm，旋转 (Pitch=-14,Yaw=85,Roll=0)°，水平 FOV=80°；首次接管鼠标保持该画面。
鼠标默认围绕角色根部局部 (0,0,115) cm 的上身参考点环绕；人物整体位置和朝向不随相机输入改变，挥杆动画可独立播放。
Tab 显示鼠标以操作界面，再按 Tab 返回环绕；WASDQE、Shift 和 Home 保留。头戴、视频及电影模式不接受环绕输入。
机位调整不改变动画时间、暂停状态、球运动或轨迹开关；既有报告、MP4 与历史验收数字保留原范围。
[肩后相机直接导航说明 END]
