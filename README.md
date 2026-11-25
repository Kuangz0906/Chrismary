Merry Christmas · 互动粒子圣诞树
一个零构建、单文件的浏览器 3D 圣诞树项目。使用 Three.js 构建香槟金粒子树、金框照片墙与 Bloom 辉光，使用 Google MediaPipe 实现单手手势控制。所有 HTML、CSS 和 JavaScript 均位于 christmas_tree.html 中，无需后端或本地资源文件。

功能特性
约 1500 个主体粒子与 2500 个尘埃粒子。
金色/墨绿色方块、金色/红色球体与程序化红白糖果手杖。
TREE、SCATTER、FOCUS 三种状态，使用 lerp 连续形变。
默认显示 JOYEUX NOEL 照片，支持上传本地照片并自动添加金色相框。
最多保留 30 张照片，新照片从树顶飘落至环绕位置。
捏合聚焦、张手散开、握拳聚合、手掌移动旋转。
根据手腕到中指指尖的距离连续控制镜头缩放。
摄像头不可用时自动降级为拖拽、滚轮、触摸与照片点击交互。
响应式布局、移动端 DPR 限制、页面隐藏时暂停渲染和推理。
技术栈
技术	版本/用途
Three.js	0.160.0，通过 importmap 从 jsDelivr 加载
@mediapipe/tasks-vision	0.10.3，通过 importmap 从 jsDelivr 加载
HandLandmarker	VIDEO 模式，GPU delegate，单手 21 关键点
EffectComposer	RenderPass + UnrealBloomPass + OutputPass
RoomEnvironment	PMREM 环境反射
Google Fonts	Cinzel；无网络时降级为 serif
项目不使用 React、Vue、打包器或后端服务。

文件结构
Chrismary/
├── christmas_tree.html    # 完整应用：HTML、CSS、ES Module JavaScript
├── README.md              # 项目说明
├── 圣诞树网页提示词.md      # 原始需求与实现提示词
└── douyin_dump.html       # 参考页面文件

快速开始
确保电脑可以访问互联网，以加载 CDN、字体、WASM 和手势模型。
使用 Chrome 或 Edge 打开 christmas_tree.html。
等待 LOADING HOLIDAY MAGIC 加载遮罩淡出。
浏览器请求摄像头时点击“允许”。
底部状态显示 Camera on… 后，即可使用手势。
部分企业浏览器策略会限制 file:// 访问摄像头。如果遇到这种情况，需要将页面部署到静态 HTTPS 环境，或通过 localhost 打开。

操作说明
手势
操作	动作	效果
捏合	拇指尖与食指尖接触，保持约 0.2 秒	聚焦照片；再次捏合切换下一张
张手	五指张开，保持约 0.2 秒	粒子散开为星云，并根据手掌大小拉近镜头
握拳	四指握紧，保持约 0.2 秒	粒子恢复树形，并推远镜头
移动手掌	手掌保持在画面内，左右/上下移动	旋转整个 3D 场景
改变手掌大小	逐渐握拳或张开	连续调整相机距离；照片聚焦时锁定缩放
手势识别包含连续两帧确认和 400ms 冷却。持续捏合不会重复触发；切换下一张前应先完全松手，等待约 0.5 秒再捏合。

鼠标与触摸
拖拽：旋转场景。
滚轮：缩放场景。
触摸单指拖拽：旋转场景。
触摸双指缩放：调整相机距离。
点击照片：进入照片聚焦。
聚焦状态下点击空白区域：切换下一张照片。
Esc：退出照片聚焦，返回树形。
H：隐藏或显示全部 UI。
上传照片
点击右上角 ADD MEMORIES。
选择一张或多张本地图片。
照片会在浏览器本地读取，不会上传到服务器。
新照片从树顶飘落到树体中上部的环绕位置。
照片总数达到 30 张后，新照片会替换最早上传的非默认照片。
状态机
TREE：主体粒子按螺旋圆锥公式排列，整棵树缓慢自转。
SCATTER：粒子散布在半径 8～20 的球壳内，自转、漂移并提高 Bloom 强度。
FOCUS：随机选择一张照片移动到相机前方并放大，其余粒子退到背景形成光晕。
所有模式切换均通过逐帧 Vector3.lerp 完成，不会瞬间跳变。

摄像头与故障降级
摄像头未授权、设备不存在、被其他程序占用、MediaPipe 模型加载失败或推理异常时，页面不会中断，而是显示：

Camera off · drag to rotate

将鼠标悬停在该提示上，可以查看具体失败原因。常见处理方法：

关闭微信、钉钉、Zoom、Teams、OBS 等占用摄像头的程序。
在 Chrome/Edge 隐私设置中允许摄像头访问。
刷新页面并重新授权。
确保手部完整入镜、距离约 30～60cm、光线充足。
确认底部状态为 Camera on… 后再测试手势。
如果 file:// 环境禁止摄像头，改用 HTTPS 或 localhost。
性能与兼容性
主体粒子使用 InstancedMesh 减少绘制调用。
移动端像素比上限为 1.5，桌面端上限为 2。
移动端进一步降低 Bloom 中间渲染目标分辨率。
MediaPipe 推理限制为约 30fps，并跳过重复视频帧。
visibilitychange 时暂停渲染与 CV 推理，恢复后继续。
WebGL 上下文丢失、CDN 加载失败和运行时异常均有统一提示与降级处理。
已知限制
首次打开需要联网加载 CDN、WASM 和 HandLandmarker 模型。
摄像头效果取决于设备、驱动、浏览器权限和光线条件。
iOS Safari 的 MediaPipe/GPU 支持可能不稳定，此时优先使用拖拽与触摸操作。
建议上传 JPG/PNG，避免一次选择过多高分辨率原图。
手势缩放控制的是页面中的 3D 场景，不会缩放浏览器窗口或网页 UI。
