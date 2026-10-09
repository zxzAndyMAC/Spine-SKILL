# Spine 4.3 完整功能手册（中文）

核对日期：2026-10-09。对象：Esoteric Software 的 Spine 2D 骨骼动画编辑器与官方运行时。以 **4.3 正式版本、Professional 完整能力** 为主线，同时说明 Essential 差异。

本手册同时说明 **4.3 新能力和此前已有能力**。第 1—26 章用于建立概念和查找功能；第 27—30 章逐项展开参数、操作、组合方式与限制；第 31 章按官方 User Guide 的 48 个专题、693 个标题核对覆盖位置。标题包含分类、示例和视频入口，不能当作 693 种独立功能。它是中文参考手册，不是官方资料的逐字翻译；示例中的制作场景是对功能的应用说明。

**证据规则：** 常规功能依据各专题官方手册；4.3 变化依据版本日志、发布说明、4.3 源码和官方人员答复，并核对本机 4.3.26 的部分界面与快捷命令。部分在线手册尚未同步 4.3，新旧描述冲突处单独标记。未公开的算法、未核实的数值硬范围和编辑器工厂默认值明确保留，不能从运行时初始化值推断。没有在本机逐项执行全部功能。

## 目录

1. [版本、产品范围与授权差异](#1-版本产品范围与授权差异)
2. [Spine 的数据结构与工作方式](#2-spine-的数据结构与工作方式)
3. [工程管理、界面与工作区](#3-工程管理界面与工作区)
4. [骨架与骨骼](#4-骨架与骨骼)
5. [插槽、颜色、混合模式与绘制顺序](#5-插槽颜色混合模式与绘制顺序)
6. [图片与附件通用功能](#6-图片与附件通用功能)
7. [全部六种附件类型](#7-全部六种附件类型)
8. [网格编辑、软选择与链接网格](#8-网格编辑软选择与链接网格)
9. [绑定与权重](#9-绑定与权重)
10. [皮肤、换装与组合](#10-皮肤换装与组合)
11. [全部五类约束](#11-全部五类约束)
12. [动画、关键帧与可动画属性](#12-动画关键帧与可动画属性)
13. [Dopesheet 与 Graph](#13-dopesheet-与-graph)
14. [事件、音频与图片序列](#14-事件音频与图片序列)
15. [所有工作面板](#15-所有工作面板)
16. [导入与 PSD 管线](#16-导入与-psd-管线)
17. [全部导出类型与设置](#17-全部导出类型与设置)
18. [图集打包与解包](#18-图集打包与解包)
19. [命令行与自动化](#19-命令行与自动化)
20. [设置、快捷键与数值表达式](#20-设置快捷键与数值表达式)
21. [运行时、引擎与 Web](#21-运行时引擎与-web)
22. [性能指标与优化功能](#22-性能指标与优化功能)
23. [4.3 新增及强化功能总表](#23-43-新增及强化功能总表)
24. [升级与容易误解的边界](#24-升级与容易误解的边界)
25. [按需求查功能](#25-按需求查功能)
26. [官方功能覆盖索引与资料](#26-官方功能覆盖索引与资料)
27. [装配、附件、权重与约束逐项参考](#reference-rigging)
28. [动画、关键帧、曲线、声音与序列逐项参考](#reference-animation)
29. [工作区、资源、导入导出、CLI 与设置逐项参考](#reference-pipeline)
30. [运行时、Player 与 Web Components 参考](#reference-runtime)
31. [官方小节覆盖核对与验证范围](#reference-coverage)

## 1. 版本、产品范围与授权差异

### 1.1 当前版本事实

| 项目 | 已核实信息 |
| --- | --- |
| 4.3 首个稳定版 | 4.3.00，官方日志日期 2026-05-14 |
| 4.3 发布博客 | 官方博客列表日期 2026-05-28，与首次稳定构建日期不同 |
| 当前最新已发布补丁 | 4.3.26，2026-09-07 |
| 尚未发布的条目 | 4.3.27，标记 Unreleased，不计入已交付能力 |
| 版本兼容要求 | 编辑器导出数据和 runtime 的 major.minor 必须匹配；4.3 数据应使用 4.3 runtime |

来源：[编辑器更新日志](https://esotericsoftware.com/spine-changelog)、[官方博客列表](https://esotericsoftware.com/blog)、[版本管理](https://esotericsoftware.com/spine-versioning)。上述“当前”仅指核对日。

### 1.2 Spine 提供的三个层次

| 层次 | 做什么 | 产物或接口 |
| --- | --- | --- |
| 编辑器 | 装配图片、制作骨架、绑定、关键帧、约束、换装、预览 | `.spine` 可编辑工程 |
| 导出工具 | 生成运行数据、图集、图片、视频、HTML，支持 CLI 批处理 | JSON、binary、atlas、图像和预览文件 |
| Spine Runtimes | 在游戏、应用、网站中实时播放、混合、修改和渲染 | 各语言库、引擎组件、Player、Web Components |

Spine 核心是 2D 骨骼动画。所谓转头、转身、透视和厚度效果通常由网格变形、图片替换、缩放、层级切换组合完成。某个引擎把 Spine 角色放进 3D 场景，并不使编辑器变成三维建模软件。来源：[入门](https://esotericsoftware.com/spine-getting-started)、[附件](https://esotericsoftware.com/spine-attachments)。

### 1.3 Essential / Professional / Trial

| 能力 | Essential 4.3 | Professional 4.3 |
| --- | --- | --- |
| 基础骨骼、图片附件、关键帧和基础动画 | 支持 | 支持 |
| 保存工程、支持格式的导出 | 支持 | 支持 |
| 普通皮肤换装 | 支持 | 支持 |
| IK 约束 | **4.3 已开放** | 支持 |
| Skin bones / skin constraints | **4.3 已开放**，不代表所有高级约束均开放 | 支持 |
| 网格、权重和网格变形 | 不包含 Professional 网格能力 | 支持 |
| Path / Transform / Physics constraints | 官方专题仍列为 Professional 范围 | 支持 |
| Slider constraint | 不支持 | 支持 |
| Events 与事件音频 | 不支持 | 支持 |

**特别注意：Essential 的 IK 与皮肤骨骼信息发生了变化。** 4.3.78-beta 日志已允许 Essential 使用 IK 和皮肤骨骼／约束；官方工作人员随后确认 Essential 可正常使用、导出这些能力。旧 IK、Skins 手册和旧对照表中的“不支持”不能直接套到 4.3。来源：[4.3 日志](https://esotericsoftware.com/spine-changelog)、[官方答复，帖 #56](https://ru.esotericsoftware.com/forum/d/30212-spine-43-released/27)。

Trial 可以体验编辑器功能，但不能保存和导出工程。Professional 包含全部编辑器功能；Enterprise 是企业授权类型，不是一套另有建模工具的产品。具体授权条件以购买页和协议为准；本手册不做价格或合同分析。来源：[购买与版本](https://esotericsoftware.com/spine-purchase)、[Trial 说明](https://esotericsoftware.com/spine-getting-started)。

### 1.4 首次激活、启动与版本选择

首次启动输入授权页提供的 activation code 并 Submit。激活和首次下载编辑器版本需要联网；完成后可离线启动已下载版本，需要时在 Launcher 配置代理。激活码是授权凭据，不应随工程交付。

| Launcher 功能 | 操作与结果 |
| --- | --- |
| Language | 在启动编辑器前选择界面语言 |
| Start | 使用所选版本启动编辑器 |
| Start automatically | 下次自动启动；启动画面出现时点击可中断自动启动以改设置 |
| Latest stable | 选择当前稳定版；生产工程宜记录并固定具体版本 |
| Latest beta | 有开发中的 beta 时选择预览版本；runtime 可能尚未全部支持 |
| 已下载的具体版本 | 直接选择该版本；适合继续既有工程 |
| Other... | 输入需要的已发布版本号并下载 |

**建议操作顺序：** 新项目先确定运行平台和 runtime 的数据版本，再选匹配编辑器；已有项目先记录版本和另存副本，再升级。Launcher 自身版本与编辑器版本不同，本机 Launcher 显示 4.2.03 不等于实际编辑器仍为 4.2。

学习入口包括 Welcome 的示例与 Learn、User Guide、Animating with Spine 和官方论坛；工程问题可准备最小示例，授权问题走官方联系入口。来源：[Getting started](https://esotericsoftware.com/spine-getting-started)、[Versioning](https://esotericsoftware.com/spine-versioning)。

## 2. Spine 的数据结构与工作方式

### 2.1 七个核心对象

| 对象 | 作用 | 实例 |
| --- | --- | --- |
| Skeleton 骨架 | 一个可动画角色或对象的完整容器 | 角色、怪物、旗帜、UI 装饰 |
| Bone 骨骼 | 保存平移、旋转、缩放、错切，形成父子层级 | 上臂、前臂、武器控制骨 |
| Slot 插槽 | 连接骨骼与附件，保存显示、颜色和绘制层级 | 右手武器槽、眼睛槽 |
| Attachment 附件 | 实际图片、网格、路径或辅助几何 | 一把剑的图片、碰撞多边形 |
| Skin 皮肤 | 组织可替换附件及可选骨骼／约束 | 发型、衣服、装备组合 |
| Constraint 约束 | 按规则修改骨骼或应用动画 | IK、物理、Slider |
| Animation 动画 | 一组属性时间轴与关键帧 | idle、run、attack、blink |

层级关系通常是：工程 → 骨架 → 骨骼 → 插槽 → 附件。皮肤、动画、约束和事件是骨架下的其他组织结构。骨骼父子关系负责运动传递；插槽列表负责图片先后绘制，这两种顺序相互独立。来源：[骨架](https://esotericsoftware.com/spine-skeletons)、[骨骼](https://esotericsoftware.com/spine-bones)、[插槽](https://esotericsoftware.com/spine-slots)。

### 2.2 Setup 与 Animate

| 模式 | 主要任务 | 改动影响 |
| --- | --- | --- |
| Setup | 建立骨骼、装配附件、网格与绑定、设置皮肤与约束 | 改变共同基础结构和初始姿态 |
| Animate | 在动作中调整姿态、设置关键帧、曲线、事件 | 改变所选动画中的属性随时间变化 |

Setup pose 是动画的基础，不是一个必须播放的动作。未被某条动画控制的属性，可能由 setup pose、低层动画、Slider 或程序决定。动画混合时，是否为某个属性打帧会影响其他动画还能否控制它。来源：[界面模式](https://esotericsoftware.com/spine-ui)、[关键帧](https://esotericsoftware.com/spine-keys)。

### 2.3 制作与实时播放

制作流程：拆件 → 装配 → 骨骼 → 网格与绑定（按需要）→ 约束 → 动画 → 预览 → 导出 → runtime 集成。

图片／视频导出得到已经渲染好的画面；JSON／binary 导出得到可以实时计算的动画数据。后者可以在运行中改变 IK 目标、混合动作、换皮肤、读取事件；前者适合只需成片或序列帧的用途。来源：[导出](https://esotericsoftware.com/spine-export)、[运行时文档](https://esotericsoftware.com/spine-runtimes-guide)。

## 3. 工程管理、界面与工作区

### 3.1 工程功能

| 功能 | 说明 |
| --- | --- |
| 新建／打开／保存／另存 | 管理 `.spine`；可通过菜单、标题栏、快捷键或文件拖放操作 |
| 最近工程 | 快速切换近期工程；文件对话框保留不同任务的最近路径 |
| 多骨架工程 | 一个工程容纳多个骨架，适合参考、对照和组合场景 |
| 工程导入 | 从其他工程引入骨架或动画 |
| 撤销／重做 | 大多数编辑操作可回退；用于试改绑定和姿态 |
| 自动备份 | 通过设置控制备份；备份不等于完整素材归档 |
| 图片路径 | 每个骨架指向自己的图片目录，配合相对路径移动工程 |
| Package Project【4.3】 | 将工程和图片收集为 ZIP，便于交接与问题复现 |
| 非导出参考对象 | 将参考骨架／附件等 Export 关闭，留在工程中辅助制作 |

来源：[界面与工程操作](https://esotericsoftware.com/spine-ui)、[文件设置](https://esotericsoftware.com/spine-settings)、[4.3 发布说明](https://esotericsoftware.com/blog/Spine-4.3-released)。

### 3.2 工作区与导航

- **Viewport：** 操作骨骼、附件、控制柄和网格；支持平移、缩放、恢复 100%、适配骨架。
- **Tree：** 浏览结构，选择对象，并在属性区修改名称、父级、颜色和其他属性。
- **Views：** 打开、关闭、最小化、调整尺寸，拖动标签排列或叠为标签页。
- **智能选择：** 不需先切到独立选择工具；点击、拖动、多选和框选配合当前工具工作。
- **选择历史与选择组：** 回到近期选择，或保存常用身体部位的对象集合。
- **可选择开关：** 让背景／辅助对象在视口中无法误选，仍可在 Tree 选中。
- **显示开关：** 暂时隐藏骨骼、插槽或整个骨架，减少操作干扰。
- **面包屑【4.3】：** 显示当前选择的结构路径，便于定位复杂工程。

视图布局通常在 Spine 窗口内调整；不要把多显示器支持理解为所有面板均能像独立系统窗口任意浮出。来源：[Views](https://esotericsoftware.com/spine-views)、[Tools](https://esotericsoftware.com/spine-tools)、[Tree](https://esotericsoftware.com/spine-tree)。

### 3.3 Welcome 与 Problems

Welcome 提供示例工程、最近项目、学习入口、新闻、更新日志和操作提示。示例可以用来学习绑定实现。

**Problems【4.3 新增】** 把缺图、附件、约束、权重及工程警告集中展示；选择问题定位对应对象，部分问题有自动修复按钮。自动修复是一种检查辅助，修复后仍需查看变形、皮肤和导出是否正确。来源：[Welcome](https://esotericsoftware.com/spine-welcome-screen)、[更新日志](https://esotericsoftware.com/spine-changelog)。

## 4. 骨架与骨骼

### 4.1 骨架功能

- 一个工程可新建多个 Skeleton。
- 每个骨架有自己的骨骼、插槽、附件、皮肤、约束和动画。
- 骨架之间可调整整体绘制先后；编辑器通常完整画完一个骨架再画下一个。
- **Export 开关：** 决定是否进入数据、图片和视频输出。
- **隐藏骨架：** 减少视口及相关面板中的内容；隐藏不等于禁止导出。
- **Reference scale：** 为物理等依赖距离的效果提供尺度基准。

如果两个角色需要反复交叉遮挡各自的局部，单纯交换两个骨架的整体顺序不足以表达。可使用一个骨架、拆分骨架，或由 runtime 分段渲染。来源：[Skeletons](https://esotericsoftware.com/spine-skeletons)。

### 4.2 骨骼变换与结构

| 能力 | 作用及注意点 |
| --- | --- |
| Rotate | 围绕骨骼原点旋转；局部旋转可用于多圈动作 |
| Translate X/Y | 移动骨骼；可分离 X/Y 时间轴 |
| Scale X/Y | 缩放及拉伸；负缩放用于镜像 |
| Shear X/Y | 改变轴方向关系，实现倾斜／透视感 |
| 父子层级 | 父骨骼影响后代；适合肢体链、道具与附件组织 |
| 变换继承 | 控制旋转、缩放、反射的继承；继承模式可打帧 |
| Local / Constrained / Parent / World 操作轴 | Local 输入、Constrained 约束后局部结果、Parent 和 World 空间；不意味着键存储改成世界坐标 |
| 零长度骨 | 常用于控制柄；仍具备正常变换能力 |
| Length | 对 IK、路径及自动权重有实际作用，其余多用于显示 |
| Set Parent | 改变父级，重新组织结构 |
| Split | 分成多段，可形成嵌套链或同级骨；支持等长或 Fibonacci 长度分布 |
| 图标、颜色、名称、可选性 | 区分控制骨与形变骨，减少误选 |
| 图标尺寸与旋转【4.3】 | 改善控制柄表达与视口阅读 |
| Bone Order Up / Down【4.3】 | 调整控制骨在视口的显示先后，附件遮挡仍由插槽绘制顺序决定 |
| Add to Skin | 骨骼只在相关皮肤启用时活动 |

用途示例：上臂／前臂形成基础 FK 链；另设手部控制骨用于 IK；身体伸缩与剪切制造夸张动作；武器控制骨用于方向和机械联动。来源：[Bones](https://esotericsoftware.com/spine-bones)、[Tools](https://esotericsoftware.com/spine-tools)、[4.3 更新日志](https://esotericsoftware.com/spine-changelog)。

## 5. 插槽、颜色、混合模式与绘制顺序

### 5.1 Slot 的完整作用

一个 Slot 属于一个 Bone，是附件容器；同一时刻可显示一个附件或不显示。要同时显示皮肤和衣服两张图片，应使用不同插槽。插槽没有自身可动画的独立位置，其附件跟随所属骨骼。

| 功能 | 说明 |
| --- | --- |
| Attachment key | 在动画中切换显示附件／皮肤占位符，或设为无附件 |
| Color RGBA | 给附件染色、控制透明度，可打关键帧 |
| Separate color and alpha | 将 RGB 与 Alpha 分开控制 |
| Tint black | 双颜色染色，用于阴影和颜色表现；renderer 支持需核对 |
| Blend mode | Normal、Additive、Multiply、Screen；不同 renderer 支持可能不同 |
| Draw order | 改变前后遮挡，独立于骨骼父子结构 |
| Slot folders | 组织大量插槽与层级 |
| Slot 隐藏 | 帮助编辑，也不绘入图片／视频导出；数据仍保留，不可作为动画隐藏键 |

**隐藏区别：** 把 Alpha 设为 0 可以淡出，但常仍需处理／绘制几何；若只要完全关闭显示，Attachment 设为无通常更合适。Attachment 自身的 setup color 和 Slot 动画颜色是两个层次，不能混为一种可打帧属性。来源：[Slots](https://esotericsoftware.com/spine-slots)、[Attachments](https://esotericsoftware.com/spine-attachments)。

### 5.2 Draw order 与 4.3 文件夹关键帧

旧有整体 draw order key 可以在不同时间更改整个插槽列表。**4.3 支持文件夹独立关键帧**：分别给手臂、武器等分组调整内部层级，多个动作轨道不必互相覆盖整个骨架的层级。

使用场景：武器动画改变武器部件的前后顺序，同时转身动画调整身体部件。分组必须合理，才能让互不相关的动作各自控制自己的部分。裁剪依赖 draw order，改变层级后同时检查裁剪范围。来源：[绘制顺序](https://esotericsoftware.com/spine-slots)、[4.3 Draw order folder keying](https://esotericsoftware.com/blog/Spine-4.3-released)。

## 6. 图片与附件通用功能

### 6.1 图片资源

- 通过 Images path 指定骨架的图片根目录。
- 附件名称或 Image path 决定查找哪张图片，可包含子目录。
- 名称与图片路径可分离，多个附件可以复用同一张图片。
- 将图片拖到骨骼／插槽创建 Region attachment，再装配到 setup pose。
- 外部图像软件修改资源后，编辑器的文件监控／刷新机制用于更新素材。
- 素材已在 atlas 中时，可先解包；编辑器制作通常使用独立图片。
- PSD 和图像软件导出脚本可保留部件相对位置，减少手工拼装。
- **4.3：** Images 节点直接管理 PSD，附件可显示来源 PSD；图片路径可通过文件选择器选取。

Spine 不负责绘制原画。适合拆出独立移动的部件，并为遮挡后的关节补足图片。来源：[Images](https://esotericsoftware.com/spine-images)、[Import PSD](https://esotericsoftware.com/spine-import-psd)。

### 6.2 所有附件的公共属性

| 属性／操作 | 作用 |
| --- | --- |
| Name 勾选项 | 控制附件名称标签是否在视口显示；与 Rename 改逻辑名称不同 |
| Rename／附件逻辑名称 | 附件身份；Region／Mesh 未指定 Path 时也用于图片查找 |
| Set Parent | 移至不同插槽或骨骼；Tree 拖放也可组织 |
| Select | 禁止视口误选，Tree 中仍可选 |
| Export | 决定是否导出附件及相关动画引用 |
| Color | 图片／网格用于 setup 染色，辅助几何用于编辑器区分 |
| Duplicate / Rename / Delete | 复制、命名和清理附件 |
| 显示切换 | 通过 Slot 的当前附件状态控制；每槽最多一个 |

辅助几何的编辑器颜色不会使其在游戏中变成可见图片。取消附件 Export 可能连带移除其关键帧；链接网格还依赖源网格。来源：[Attachments](https://esotericsoftware.com/spine-attachments)。

## 7. 全部六种附件类型

### 7.1 Region attachment：矩形图片

最基础的图像附件，使用图片矩形或 atlas region。适合硬质部件、替换眼睛／嘴型、道具与简单角色。

- Setup 中设置位置、旋转、缩放和图片路径。
- 运动主要由所属 Bone 的变换控制。
- 可转为 Mesh；可配置图片序列。
- 附件自身的 setup transform 不是独立动画 transform 时间轴；需要独立运动通常增加骨骼。
- 奇数尺寸图片严格对齐像素中心时，应注意半像素偏移。

来源：[Region attachments](https://esotericsoftware.com/spine-regions)。

### 7.2 Mesh attachment：带纹理的多边形

由顶点、边、三角形及 UV 构成。可以压缩透明空白的绘制范围，也能用骨骼权重或 deform keys 弯曲图片。适合皮肤、肌肉、衣服、长发、布料和柔软道具。

- 手工编辑形状与拓扑。
- 自动 Trace 外轮廓与 Generate 内部顶点。
- 绑定多骨骼，为各顶点分配权重。
- 在动画中直接编辑顶点形成 deform timeline。
- Linked mesh 复用源网格结构、绑定及可选变形时间轴。
- 不规则网格可降低透明区 overdraw；顶点过多又增加 CPU/GPU 工作。

**边界：** 网格可以凹，但不能直接形成带洞的拓扑；洞可用透明像素或拆成多个网格表现。来源：[Meshes](https://esotericsoftware.com/spine-meshes)。

### 7.3 Bounding box attachment：包围多边形

为命中、选取、触发和碰撞区域提供形状数据，本身不渲染图片。

- 创建、增删和移动多边形顶点。
- 通过骨骼、权重和顶点变形跟随角色。
- 用插槽附件关键帧在不同动作阶段启用不同区域。
- runtime 可以计算世界顶点，并用于点／线段测试，或接入目标引擎碰撞系统。

“定义了攻击框”不等于已经实现伤害、碰撞响应或自动生成所有引擎物理组件，这些属于游戏集成。来源：[Bounding boxes](https://esotericsoftware.com/spine-bounding-boxes)、[runtime API](https://esotericsoftware.com/spine-api-reference)。

### 7.4 Clipping attachment：裁剪形状

控制一段 draw order 内图片／网格的可见区域。可用于局部揭示、容器中的液体、窗口、遮罩和切口。

- 创建、修改多边形；可绑定和打 deform keys。
- **End slot** 设置裁剪结束的插槽；裁剪作用范围按绘制顺序确定。
- 用附件切换启停裁剪，用 draw order 改变哪些图像受裁剪。
- **Inverse【4.3】：** 隐藏形状内部，保留外部，适合挖空与擦除式效果。
- **Convex hull【4.3】：** 提供更高效的凸包处理；凸包会改变凹形表达，应检查画面。

避免多边形自相交；传统裁剪范围不能任意重叠当成无限嵌套遮罩。裁剪消耗 CPU，复杂形状和大量被裁三角形需在目标设备测试。来源：[Clipping](https://esotericsoftware.com/spine-clipping)、[4.3 裁剪变化](https://esotericsoftware.com/blog/Spine-4.3-released)。

### 7.5 Path attachment：贝塞尔路径

用于 Path constraint，不是直接可见的描边图片。

- Knot 与 handles 定义路径形状；可增删节点、调整切线。
- 开放或闭合路径，控制连接与连续性。
- 路径可以跟随骨骼，绑定多个骨骼，或用 deform keys 改变形状。
- 通过骨骼链沿路径排列制作绳索、触手、尾巴、轨道和循环传送效果。
- 路径自身与其目标插槽中的可见附件状态共同决定约束行为。

来源：[Paths](https://esotericsoftware.com/spine-paths)、[Path constraints](https://esotericsoftware.com/spine-path-constraints)。

### 7.6 Point attachment：带方向的点

保存位置与旋转，作为游戏端的挂接或生成参考，不是图片。

- 配置点的坐标、方向和编辑器颜色。
- 跟随所属 Bone。
- runtime 读取世界位置与方向。
- 用于枪口、粒子出生点、剑尖、投掷点、UI 标记或目标锚点。

若点需独立运动，可给它独立骨骼；粒子系统和弹道仍由应用创建。来源：[Points](https://esotericsoftware.com/spine-points)。

## 8. 网格编辑、软选择与链接网格

### 8.1 网格工具逐项说明

| 工具／选项 | 功能 |
| --- | --- |
| Edit Mesh | 进入顶点与 UV／拓扑编辑状态 |
| Modify | 移动现有顶点，精修轮廓和内部结构 |
| Create | 增加顶点与手工边，控制三角剖分 |
| Delete | 删除顶点或边；支持多选 |
| New | 重新定义轮廓，适合从头铺网格 |
| Reset vertices | 回到基础矩形顶点布局 |
| Generate | 自动添加内部顶点，改善可变形区域的分布 |
| Trace | 按透明度轮廓自动建网格外边界 |
| Trace 参数 | Detail、Concavity、Refinement、Alpha threshold、Padding；4.3 加入 Uniform 并强化批量描边 |
| Triangles | 显示自动三角剖分，判断弯折会发生在哪里 |
| 手工 Edges | 限制自动三角形连接，保护鼻子、眼睛、关节等局部 |
| Edge loop selection | 连续选中一条边线上的相邻边 |
| Dim / Isolate | 暗化当前纹理／隔离其他附件，便于编辑 |
| Deformed | 不只改变观察：勾选时修改实际顶点与 UV；关闭时仅修改 UV |
| Wireframe | 未选中时也显示线框，辅助放置骨骼 |
| Freeze | 将目前的顶点状态作为基准，使显示用旋转／缩放归零 |
| Reset mesh | 顶点重新匹配 UV，涉及权重重建及移除 deform；4.3 保留绑定骨并重新 Auto，不能当作原精调权重未变 |

**顺序建议：** 先完成轮廓与内部拓扑，再绑定、刷权重、制作 deform。Reset、Trace、Generate 等重建操作会影响原精调权重和 deform；4.3 移除权重时保留绑定骨并重新自动计算，Edit Mesh 的 Auto 延迟到退出编辑时。绑定骨仍在不代表旧比例与动画仍然相同，执行前保留工程副本，之后复验。来源：[Meshes 编辑模式](https://esotericsoftware.com/spine-meshes)、[4.3.62-beta](https://esotericsoftware.com/spine-changelog#v4-3-62-beta)、[4.3.01](https://esotericsoftware.com/spine-changelog#v4-3-01)。

### 8.2 Mesh Tools 的软选择

选中一个顶点时，让周围顶点按距离衰减一起移动，而不是只拉出一个尖角。

- **Size：** 影响半径。
- **Feather：** 影响衰减程度。
- **Hull vertices：** 决定是否影响网格外边界；关闭时适合保持脸部轮廓、只做内部透视变形。
- 与旋转、平移、缩放及部分权重调整配合。
- 不只支持 Mesh，也作用于 Path、Bounding box、Clipping 的顶点。

来源：[Mesh Tools](https://esotericsoftware.com/spine-mesh-tools)。

### 8.3 Linked meshes

链接网格共享 source mesh 的几何／绑定，使相同结构换图时不用重做整套权重。可决定是否继承变形时间轴；4.3 也改进了 sequence 时间轴继承。

**4.3 允许源网格位于不同 Slot。** 这扩展了皮肤、分层附件和不同装配结构中的复用能力，不再只能在同一插槽内链接。共享意味着源网格改动会影响链接者；需要独立拓扑时，应改用真正复制的网格。来源：[Linked meshes](https://esotericsoftware.com/spine-meshes#Linked-meshes)、[4.3 链接网格](https://esotericsoftware.com/blog/Spine-4.3-released)。

## 9. 绑定与权重

### 9.1 基本能力

权重规定每个顶点受哪些骨骼影响、影响比例是多少。适用于 Mesh、Path、Bounding box、Clipping；Region 不使用这类多顶点骨骼混合。

| 功能 | 说明 |
| --- | --- |
| Bind | 将骨骼与附件关联；首次绑定可自动计算权重 |
| Bones list | 查看参与骨骼、当前顶点权重，移除不需要的绑定 |
| Direct | 精确调整选中顶点对选中骨骼的数值 |
| Add / Remove / Replace | 笔刷增加、减少或替换权重 |
| Strength / Size / Feather | 笔刷力度、范围与衰减 |
| Pies | 顶点饼图显示各骨骼影响比例 |
| Overlay | 将骨骼权重以颜色覆盖到附件 |
| Selected | 聚焦选中骨骼／顶点的权重显示 |
| Copy / Paste | 在匹配数量的顶点之间复制权重 |
| Auto | 根据几何和骨骼关系生成自动权重 |
| Smooth | 平滑邻近顶点的权重过渡 |
| Prune | 去除很小／多余的影响，降低顶点变换成本 |
| Weld | 让接近或重合顶点的权重一致，减少接缝 |
| Swap | 交换两个已有绑定骨骼对应的权重列；不是通用替换绑定骨骼 |
| Lock | 锁定特定骨骼权重，防止刷权重时被自动分配改变 |
| Update bindings | 调整绑定参考，处理 setup 中的骨骼／附件变化 |
| Triangle order | 借助骨骼在列表中的顺序处理同一网格自重叠的三角形前后 |

权重通常归一到总和 100%。修改一个骨骼的比例会重新分配其他未锁定骨骼的比例。自动权重是起点，需要检查极端姿态和关节收缩，不能仅因自动计算成功就认定绑定正确。来源：[Weights](https://esotericsoftware.com/spine-weights)。

### 9.2 4.3 的批量权重工作流

可以同时处理多个网格的权重绘制、Bind、Weld、Smooth、Auto、Prune。对跨衣服、身体和阴影层的同一关节，减少重复操作。

不要把“多网格刷权重”自动理解成任意对象数量和拓扑之间都能一键正确粘贴全部绑定；复制粘贴仍需满足对应条件。来源：[4.3 多网格权重](https://esotericsoftware.com/blog/Spine-4.3-released)。

## 10. 皮肤、换装与组合

### 10.1 Skin 的完整结构

| 能力 | 功能说明 |
| --- | --- |
| Skin placeholder | 动画控制一个逻辑占位名，实际图片由当前皮肤提供 |
| Skin attachment | 每个皮肤为占位符提供一个附件或留空 |
| Active skin | 当前编辑的皮肤，一次一个 |
| Pinned skins | 同时显示多个皮肤，用于组合发型、衣服、装备 |
| Skin bones | 只有相关皮肤启用时才激活的骨骼 |
| Skin constraints | 只有相关皮肤启用时才激活的约束 |
| Skin folders | 组织大量皮肤，文件夹路径进入导出的名称 |
| Duplicate skin | 复制皮肤，可结合链接网格减少重复绑定 |
| Add to Skin | 添加骨骼／约束，建立皮肤依赖关系 |
| Export | 排除制作参考或未交付的皮肤 |
| Color | 编辑器中识别皮肤，不给角色自动整体染色 |

示例：动画对 `head`、`weapon` 占位符打帧；红色皮肤、蓝色皮肤分别给它们提供不同图片。这样同一套 run 和 attack 可以用于多套外观。

### 10.2 混搭与依赖

可把身体、眼睛、头发、衣服、帽子、武器分别做成皮肤，再组合使用。启用长发时也启用长发骨骼和物理约束；没有长发时无需让整套长发绑定一直活动。

运行时可构造组合皮肤、添加多个已有皮肤，并恢复正确的附件显示状态；顺序与重名占位符会影响覆盖结果。皮肤切换不是一条天然的“自动随机换装”动作，应用仍需决定用什么组合。来源：[Skins](https://esotericsoftware.com/spine-skins)、[Runtime skins](https://esotericsoftware.com/spine-runtime-skins)、[Skins view](https://esotericsoftware.com/spine-skins-view)。

### 10.3 4.3 皮肤强化

- 拖放合并皮肤。
- 更完善的依赖骨骼／约束自动补全。
- 仅显示 pinned skins 的附件过滤。
- HTML 导出支持组合皮肤。
- 可按皮肤独立生成 atlas，配合按需加载。
- Essential 4.3 开放 skin bones／constraints；高级约束种类仍受版本能力限制。

来源：[4.3 皮肤更新](https://esotericsoftware.com/blog/Spine-4.3-released)、[版本日志](https://esotericsoftware.com/spine-changelog)。

## 11. 全部五类约束

### 11.1 约束的共同能力

约束按指定顺序计算，修改骨骼／姿态。**同一套约束采用不同顺序可能得到不同结果。** 例如先让手沿路径移动，再令手臂 IK 跟随手；如果交换顺序，IK 可能跟随的是手的旧位置。

- 创建、重命名、复制、组织到文件夹。
- 指定受影响骨骼和源骨骼／目标插槽。
- Mix 调节作用强度，并可打帧；不同类型允许的数值范围不同。
- Tree 注释图标显示被约束和被作为目标的关系。
- 约束顺序可调整，Reset 帮助重新计算合理顺序。
- 约束可归属于皮肤，使相关装配只在需要时生效。
- 约束参数可复制粘贴到同类约束。
- 直接操作一个被完全约束的属性可能没有可见效果；应编辑控制骨或 Mix。

来源：[Constraints](https://esotericsoftware.com/spine-constraints)。

### 11.2 IK：反向运动学

**控制末端位置，再由 Spine 算出骨骼旋转。** 常用于手撑墙、脚踩地、抓武器和瞄准。

| 参数／能力 | 作用 |
| --- | --- |
| One-bone IK | 一根骨指向目标 |
| Two-bone IK | 两段肢体链把末端对到目标 |
| Parent / Child | 指定受约束的两根骨骼 |
| Target | 指定目标控制骨 |
| Positive / Bend direction | 选择肘／膝弯折方向，可打帧 |
| Mix | 在 FK 姿态和 IK 结果之间过渡 |
| Compress | 对适用的一骨 IK，目标接近时压缩 |
| Stretch | 目标超出长度时伸长 |
| Softness | 两骨接近伸直极限时平缓变化，减少末端突跳 |
| ScaleY mode【4.3】 | none、uniform、volume，定义 X 伸缩时 Y 怎么响应 |

ScaleY 的 `none` 不改变 Y；`uniform` 让 Y 跟随 X 倍率保持形状比例；`volume` 让 Y 反向补偿以近似保持二维面积，产生挤压拉伸。4.3 在严重压缩时采用平缓化公式，避免简单倒数造成宽度急增；IK 与 Physics 的具体公式不同，详见后文绑定参考补编。

Spine IK 是一骨或两骨求解器，不是任意长度骨链的通用求解器。复杂链可以拆成多段 IK 或用路径控制。目标不能成为导致循环依赖的后代；两骨的关系、非均匀缩放、Softness 和 Stretch 组合有约束条件，应检查极端姿态。旧手册的 Uniform 控件已不足以描述 4.3 ScaleY 模式。

**Essential 4.3 可使用 IK。** 来源：[IK 原理与属性](https://esotericsoftware.com/spine-ik-constraints)、[ScaleYMode](https://esotericsoftware.com/spine-api-reference#ScaleYMode)、[4.3.73-beta](https://esotericsoftware.com/spine-changelog#v4-3-73-beta)、[Essential 变化](https://esotericsoftware.com/spine-changelog#v4-3-78-beta)。

### 11.3 Transform：变换约束，4.3 重点改造

让源骨骼的变换驱动一个或多个其他骨骼。原有功能包括跟随、相对叠加、偏移和混合；**4.3 将它扩展为跨属性映射系统。**

| 能力 | 能解决的需求 |
| --- | --- |
| 一个源驱动多个骨骼 | 控制器移动后，多件机械／身体部位一起动作 |
| 选择输入属性 | 读取源的位移、旋转、缩放或错切 |
| 选择输出属性【4.3】 | 平移可驱动旋转，旋转可驱动缩放，突破同类型复制 |
| 映射输入／输出范围【4.3】 | 定义控制柄运动多少，对应部件变换多少 |
| Clamp【4.3】 | 限制映射输出范围，然后再叠加与混合；不是对最终骨骼值的绝对锁定 |
| 独立空间设置【4.3】 | 源骨与受约束骨分别选择 local／world，处理不同父级 |
| Offset / Mix | 保留装配偏移，调整跟随力度，参与动画过渡 |

**例子（应用解释）：** 控制骨水平移动，卷轴两端随之旋转并缩放；抬手时同步调整肌肉形变骨；方向控制骨越过某范围时让图像镜像。约束减少需要手工给多个部位打帧的数量。

旧在线 Transform 手册仍以单 Local 开关、Target 和 Relative 为主，不应据此推断 4.3 只有“复制目标旋转”。来源：[4.3 Transform Constraints](https://esotericsoftware.com/blog/Spine-4.3-released)、[4.3.09-beta 的映射变化](https://esotericsoftware.com/spine-changelog#v4-3-09-beta)、[Transform 专题](https://esotericsoftware.com/spine-transform-constraints)。

### 11.4 Path：沿路径控制骨骼

让一组骨骼沿当前可见的 Path attachment 排列。目标实际是 Slot，该槽显示不同路径时，可以改变整个骨链的运动轨道。

| 参数 | 选项及用途 |
| --- | --- |
| Bones | 参与骨骼及排列顺序 |
| Target | 提供 Path attachment 的插槽 |
| Position | 沿路径的位置；固定距离或百分比 |
| Spacing | Length、Fixed、Percent、Proportional；按骨长、固定间隔或整段比例排列 |
| Tangent | 骨骼指向该点路径切线 |
| Chain | 连接成链，适合履带等较硬结构 |
| Chain Scale | 旋转并缩放到下一点，适合绳索等柔软结构 |
| Rotate offset | 修改骨骼相对路径朝向 |
| Rotate / Translate mix | 调节旋转与位移影响，可打帧 |
| 位置控制柄 | 在视口沿路径直接调整位置 |

结合路径权重／变形与 Position 动画，可以做触手伸缩、绳索摆动和循环路径。Chain Scale 的父子结构会影响计算，必须按指南配置。来源：[Path constraints](https://esotericsoftware.com/spine-path-constraints)。

### 11.5 Physics：物理与次级运动

根据骨骼运动与力产生滞后、弹性和摆动。适合头发、衣摆、饰品、软肢体和弹性部件。**Physics 是既有功能，4.3 强化求解和响应，并非 4.3 才首次加入。**

| 参数／控制 | 说明 |
| --- | --- |
| Bone | 受影响骨骼，其骨长和尺度会影响行为 |
| Translate X / Y | 位移受物理作用的程度 |
| Rotation | 旋转滞后／摆动的程度 |
| Shear X | 错切受物理影响的程度 |
| Scale X | 受惯性挤压拉伸的程度 |
| ScaleY mode【4.3】 | X 变化时 Y 的响应，含 volume 近似面积补偿及强压缩保护 |
| Limit | 限制影响物理的平移速度 |
| FPS | 模拟更新频率；4.3 新建默认改为 20，既有工程保留自己的值；不是时间轴帧率 |
| Inertia | 原动作传递到物理偏移的程度 |
| Strength | 回到未受物理影响姿态的恢复力 |
| Damping | 减少运动和振荡 |
| Mass | 对力造成加速度的抵抗程度 |
| Wind / Gravity | 风与重力输入 |
| Global | 将可共用属性纳入统一控制 |
| Mix | 混合原始姿态与物理结果 |
| Simulate | 持续模拟或按时间位置计算的工作方式 |
| Deterministic | 编辑器从起点计算一致姿态，便于来回拖时间轴；成本较高 |
| Reset / Reset All | 重置单个／全部物理状态，支持重置关键帧 |
| Warm Up | 导出前预运行，使次级运动进入期望状态 |
| Reference scale | 处理骨架整体尺度与距离相关参数 |

4.3 改善低更新率下的响应与平滑，运行时提供 skeleton 级风／重力控制，并改进 reset timeline 行为。物理会受到角色在游戏世界中的移动影响，不仅受单条动画影响。

**边界：** 不等于完整的刚体碰撞引擎。它不会自动让裙子碰撞身体、让头发避开墙壁。Deterministic 是编辑器预览工具，不是跨所有平台的自动联网确定性保证。

**Key Constrained【4.3】** 可把当前约束结果设为关键帧。它允许将物理等产生的某些运动转成键，但不是一次操作就完整烘焙全部时间轴；逐帧采样、关闭重复约束和结果复验仍是制作步骤。旧 Physics 手册的 No baking 小节与 4.3 新能力不能混用。

来源：[Physics constraints](https://esotericsoftware.com/spine-physics-constraints)、[4.3 改进](https://esotericsoftware.com/blog/Spine-4.3-released)、[Physics 4.3.02](https://esotericsoftware.com/spine-changelog#v4-3-02)。

### 11.6 Slider：用一条动画控制另一套属性【4.3 新增】

这里的 Slider 是约束类型。它把某条动画的指定时刻应用到骨架上，复用该动画里的多个时间轴。

| 参数／能力 | 说明 |
| --- | --- |
| Animation | 要复用的动画 |
| Frame | 手动选取该动画的帧；可在其他动画里给 Frame 打帧 |
| Bone | 可选控制骨，自动决定 Frame |
| Property | rotate、x、y、scaleX、scaleY、shearY 六类输入；没有独立 shearX 映射 |
| Local | 选择骨骼输入的局部／世界空间 |
| Range mapping | 把输入属性的范围映射成动画帧范围 |
| Loop | 超出范围时循环；不循环时使用首／尾姿态 |
| Mix | Slider 对姿态的影响比例，可打帧 |
| Additive | 配合已有姿态叠加；具体时间轴的行为遵循运行时实现 |

**一个完整用例：** 制作 `face-turn` 动画，在不同时间定义左看、正面、右看的网格、眼睛、嘴型和层级。随后让一个控制骨 X 位移映射到 `face-turn` 的帧位置。主动作只需移动控制骨，不必再次给所有脸部属性逐个打帧。

其他用途：手臂角度驱动肌肉阴影，机械控制器驱动一整套零件姿态，嘴型控制器取用表情，混合多个姿态形成二维 blend shape 风格效果。Slider 可控制该动画能够打帧的属性，不局限于骨骼旋转。

注意约束排序和多个驱动对同一属性的竞争；用于复用的动画仍保存在工程中。Essential 不支持 Slider。来源：[Sliders](https://esotericsoftware.com/spine-sliders)、[4.3 交互示例](https://esotericsoftware.com/blog/Spine-4.3-released)。

## 12. 动画、关键帧与可动画属性

### 12.1 动画管理

- 创建、重命名、复制、删除动画；管理 Export 状态。
- 使用文件夹组织 locomotion、combat、face 等动作；文件夹路径进入运行时名称。
- 一个骨架存储多条动画，一个工程的多个骨架可分别启用动画。
- 动画长度由最高时间的关键帧决定，隐藏的 deform key 也可能延长长度。
- 时间轴使用帧方便制作，但运行时本质上按时间插值。
- 支持小数帧；解除帧吸附可查看帧间姿态。
- Repeat 设置编辑器循环；数据导出并不会让应用自动采用同一个 loop 选择。

来源：[Keys](https://esotericsoftware.com/spine-keys)、[Animations view](https://esotericsoftware.com/spine-animations-view)。

### 12.2 完整属性类别

| 时间轴类别 | 能控制什么 |
| --- | --- |
| Bone rotate | 骨骼角度，含连续旋转 |
| Bone translate | X/Y 位移，可分开 |
| Bone scale | X/Y 缩放，可分开，负值可镜像 |
| Bone shear | X/Y 错切，可分开 |
| Bone inherit | 变换继承模式切换 |
| Slot attachment | 切图片／网格／皮肤占位符／无附件 |
| Slot color | RGB、Alpha 与相应双颜色染色通道 |
| Draw order | 骨架整体层级 |
| Draw order folder【4.3】 | 文件夹内层级独立控制 |
| Deform | Mesh、Path、Bounding box、Clipping 的顶点变形 |
| Sequence | 图片序列帧、播放模式和时序 |
| IK | Mix、弯折方向、柔化及适用的伸缩设置 |
| Transform | 约束可动画影响参数 |
| Path | 位置、间距与影响强度 |
| Physics | 惯性、恢复力、阻尼、质量、风、重力、Mix 和 Reset 等 |
| Slider【4.3】 | Frame 与 Mix；也可动画控制源骨骼 |
| Event | 指定时刻触发，附带数值／文字／音频数据 |

不是所有 setup 参数都可打帧。例如骨骼名称、网格拓扑、Slot 所属骨骼以及 Region 自身的 setup transform，不应当作普通动画属性。具体可打帧控件以编辑器关键帧按钮和 4.3 数据模型为准。来源：[可打帧属性](https://esotericsoftware.com/spine-keys#Keyable-properties)、[4.3 API](https://esotericsoftware.com/spine-api-reference)。

### 12.3 打帧与编辑工具

| 工具／机制 | 用途 |
| --- | --- |
| 手动 Key | 将当前属性记录到当前时间 |
| Auto Key | 修改属性时自动建键；注意误操作带来的无关键 |
| Key Edited | 将尚未记录的改动集中打帧 |
| Key Shown | 对 Graph 显示的曲线／Dopesheet 显示的行打帧 |
| 关键帧状态颜色 | 显示当前无键、有未记录修改或已有键 |
| Copy / Paste / Cut | 复用动作片段和姿态，避免重复制作 |
| Shift | 移动后续时间，改动作节奏 |
| Offset | 为循环的局部动作调整相位，制作错峰摆动 |
| 框选缩放／反向 | 按比例改变时间长度，反转关键帧顺序 |
| Clean Up | 删除冗余键或不必要的时间轴；需复验混合和约束结果 |
| Key Constrained【4.3】 | 记录应用约束后的值 |

未打帧的姿态调整，改变时间位置后可能丢失。Auto Key 记录的是编辑动作，不会自动理解应保留哪几个“艺术关键姿势”。来源：[Setting keys](https://esotericsoftware.com/spine-keys#Setting-keys)、[Dopesheet](https://esotericsoftware.com/spine-dopesheet)、[Graph](https://esotericsoftware.com/spine-graph)。

### 12.4 动画制作方法

官方介绍 Straight ahead（从头顺做）、Pose to pose（先关键姿态）、Layered（按部位分遍制作）与组合方法。它们是工作方式，不是几种不同文件格式。

实际可先以 Stepped 规划关键姿态和节奏，再恢复插值、用 Graph 调加速减速，最后用 Offset、Physics 或独立局部动画补次级运动。来源：[Animating](https://esotericsoftware.com/spine-animating)。

## 13. Dopesheet 与 Graph

### 13.1 Dopesheet：主要编辑时间

适合同时管理很多部位的关键帧，快速调整节奏。

| 功能 | 说明 |
| --- | --- |
| Overview row | 汇总当前动画，批量移动整个姿态的键 |
| Bone / Property rows | 展开到骨骼和属性，查看不同时间轴 |
| Draw order / Event 等其他行 | 管理不直接属于某个骨骼的时间轴 |
| Locked / Unlocked | 固定当前显示内容，或跟随视口选择变化 |
| Refresh / Select | 更新固定对象集，或反向选中对应对象 |
| Filters | 只看需要的属性类型或当前工具类型 |
| 行排序和可见性 | 保持有用内容靠近，减少滚动 |
| 时间平移／缩放 | 浏览长动作、适配全部关键帧 |
| 范围循环 | 重复检查局部动作区间 |
| 多选／框选 | 批量移动、复制、缩放关键帧 |
| Shift / Offset / Adjust | 调节动作时间、循环相位及适用的键调整 |
| Key Shown | 给当前显示行集中打帧 |
| Sync【4.3 强化】 | 支持以 Dopesheet 选择驱动 Graph 显示对应曲线 |

Dopesheet 很适合回答“这个动作什么时候发生”，数值速度细节则交给 Graph。来源：[Dopesheet view](https://esotericsoftware.com/spine-dopesheet)。

### 13.2 Graph：同时编辑时间与数值

横轴是时间，纵轴是数值。调曲线可以改变动作速度、停顿、回弹和超调。

| 功能 | 说明 |
| --- | --- |
| Stepped | 保持前一个值，到下个键直接切换 |
| Linear | 匀速插值 |
| Bezier | 用控制柄调节变化速率 |
| Separate properties | 分开 X/Y、颜色／Alpha，给各通道不同节奏 |
| Automatic handles | 根据相邻键自动调整控制柄 |
| Separate handles | 两侧柄独立，产生折点或不同进出速度 |
| Flat | 设置平坦柄，形成停顿或平缓转折 |
| Bounce | 更方便制作突然换方向和回弹 |
| Ease in / Ease out | 调整过渡开始／结束的速度 |
| Frame / Auto zoom | 适配全部／选中曲线，减少手工缩放 |
| Axes | 限定只改时间或只改数值 |
| Snapping | 对齐帧、原值或其他键值 |
| Value / Shape handle modes | 移动关键帧时尽量保留值域或曲线形状 |
| Favor | 中间姿态向前一个／后一个关键姿态靠近 |
| Favor 的 Blend / Shift / Linear 模式 | 不同方式调整所选值与相邻键关系 |
| Favor 的 Average 模式 | 按同曲线、同帧或全部所选曲线的参考均值调整 |
| Favor 的 Default / Setup / Store 模式 | 趋向默认值、setup 值或存储曲线，可用于恢复与夸张 |
| Store / Swap | 保存整条动画的键与控制柄作为背景参考，并与当前曲线交换对比 |
| 同步、过滤、锁定、行排序 | 与 Dopesheet 类似的内容管理 |

**4.3 改善：** 改动关键帧值时更好地维护贝塞尔控制柄；新增 Last chosen 默认曲线类型；Dopesheet-centric 同步使选中的时间轴更快进入 Graph。Retiming 与 Revaluing 分别控制改时间和改数值时怎样维护控制柄，具体选项见 A9。4.3.24 起，**仅选中一个末键**时可查看和修改它之前区段的曲线，不能推广成任意多键选择都修改前段。来源：[Graph](https://esotericsoftware.com/spine-graph)、[4.3 曲线更新](https://esotericsoftware.com/blog/Spine-4.3-released)、[4.3 日志](https://esotericsoftware.com/spine-changelog)。

曲线适用于连续数值；附件切换、事件和层级切换本质是离散变化，不能因为用了贝塞尔就自动把两张图片变成形状渐变。Deform 的顶点位移插值也不等于骨骼旋转弧线。

Store 不是创建正式动作副本：它保存当前全部键和控制柄用于曲线比较。再次 Store 清除参考，Swap 交换参考与当前结果；Favor 的 Store 模式可让修改趋向存储曲线。Favor 越过滑条边界也可产生超调。来源：[Graph Store](https://esotericsoftware.com/spine-graph#Store)。

## 14. 事件、音频与图片序列

### 14.1 Events

在动画时间点触发应用行为，例如脚步、出刀、枪口特效和对话提示。

- 创建命名事件并在时间轴打键。
- 每个事件有 Integer、Float、String 默认数据，各事件键可以覆盖。
- 事件可按文件夹组织。
- 运行时监听事件，决定是否播放音效、生成粒子或触发游戏逻辑。
- 事件不是自动实现业务动作的脚本。

### 14.2 Audio events 与 Audio view

事件指定 Audio path 后，编辑器可播放声音，并提供 Volume、Balance；可查看波形对齐口型、踩地和节拍。

| 能力 | 说明 |
| --- | --- |
| Audio folder | 骨架自己的声音路径，可用相对路径 |
| 支持文件 | WAV、MP3、OGG；WAV 应符合手册规定的 PCM、声道和采样位深 |
| 文件监控 | 外部声音变更后载入更新 |
| 波形时间轴 | 每个事件独立颜色，选择事件定位时间 |
| 音量／静音 | 编辑器全局试听控制 |
| Audio device | 选择试听输出设备 |
| 视频声音 | 适用的视频导出可以包含音频事件 |

**runtime 不统一负责音频播放。** 应用用事件的数据调用自己的声音系统。Audio view 的静音和音频设备选择是编辑器状态。Essential 不提供 Events，因而不能把这套事件音频功能归到基础版本能力。来源：[Events](https://esotericsoftware.com/spine-events)、[Audio view](https://esotericsoftware.com/spine-audio-view)。

### 14.3 Sequence：图片序列

将一组编号图片关联为一个 Region／Mesh attachment，适合烟火、流体、难以用骨骼表示的特效、传统逐帧表情。

- 根据图片路径和编号范围定位序列。
- 设置当前 frame／index 和 sequence 时间轴。
- 支持保持、一次播放、循环、往返及其反向播放模式；实际模式由 sequence key 决定。
- 可把序列动画叠加在骨骼运动上。
- 4.3 重构每帧 region、UV、offset 预计算与链接网格的序列继承，改善渲染正确性。

图片序列仍需要为每一帧准备图片和图集空间，不具备骨骼动画的图片复用优势。来源：[Region sequence](https://esotericsoftware.com/spine-regions#Sequence)、[SequenceMode API](https://esotericsoftware.com/spine-api-reference#SequenceMode)、[4.3 runtime changelog](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/CHANGELOG.md)。

## 15. 所有工作面板

下面覆盖官方 User Guide 的 Views，并补入旧目录遗漏的 Curves 与 4.3 新增的 Problems；旧指南的 Slot Color 在 4.3 已改名为 Color。逐项参数和操作见后文参考补编。

| 面板 | 用途 | 主要控制 |
| --- | --- | --- |
| Animations | 查找、选择当前动作 | 动作列表、筛选、文件夹；4.3 Setup 中也可选动画用于 Slider |
| Audio | 查看声音波形及事件 | 波形、事件选择、音量、静音、输出设备 |
| Dopesheet | 批量调整动作时序 | 行、筛选、锁定、多选、Shift、Offset、同步 |
| Ghosting | 显示过去／未来姿态，判断轨迹 | 帧数、关键帧、前后颜色、透明度、motion vectors、轮廓模式 |
| Graph | 曲线与关键帧数值 | 三种插值、贝塞尔柄、Favor、Store、坐标限制、吸附 |
| Curves | 独立编辑插值曲线与应用预设 | 4.1 已恢复的既有面板；曲线、预设及分离控制，补编结合当前界面解释 |
| Mesh Tools | 软选择顶点 | Size、Feather、Hull vertices |
| Metrics | 查看骨架复杂度与渲染成本指标 | 骨骼、约束、顶点、三角形、面积、裁剪、时间轴 |
| Outline | 在主视口放大／编辑局部时观察整个骨架 | 整体预览、缩放和 ghosting 设置 |
| Playback | 控制制作播放 | Timeline FPS、Speed、Stepped、Interpolated |
| Preview | 检查多个动作实际叠加和过渡 | Tracks、Mix、Repeat、Alpha、Additive、速度 |
| Skins | 选择编辑皮肤和可见组合 | Active、Pins、pinned 顺序 |
| Color（旧 Slot Color） | 调整所选对象颜色；Setup 扩展为任意对象色 | 插槽 tint／Alpha 与编辑标识色应区分 |
| Timeline | 独立、简洁的时间轴导航 | 当前时间、关键帧标记和时间缩放 |
| Tree | 项目对象的主导航与属性 | 层级、搜索、过滤、拖放、重命名、可见性、关键帧与警告 |
| Weights | 骨骼绑定与权重 | Bind、Auto、笔刷、Smooth、Prune、Weld、Lock |
| Problems【4.3】 | 集中查找工程问题 | 警告列表、对象定位、适用问题的自动修复 |

来源：[Views 总目录](https://esotericsoftware.com/spine-user-guide#Views)、[Ghosting](https://esotericsoftware.com/spine-ghosting)、[Outline](https://esotericsoftware.com/spine-outline)、[Playback](https://esotericsoftware.com/spine-playback)、[Preview](https://esotericsoftware.com/spine-preview)、[Slot Color](https://esotericsoftware.com/spine-slot-color)、[Timeline](https://esotericsoftware.com/spine-timeline)、[Tree](https://esotericsoftware.com/spine-tree)。

### 15.1 Ghosting 细节

可以显示固定间隔帧或关键帧的过去／未来姿态，配置图片、纯色、轮廓／Xray 方式；Motion vectors 展示运动方向；Anchor、Offset、Loop、Selection 控制对齐、循环和范围。4.3 也改进骨骼 ghosting。

用途：检查摆动弧线、步幅、拖尾、运动均匀程度和首尾循环接缝。它是编辑辅助，不会作为默认游戏渲染效果导出。来源：[Ghosting](https://esotericsoftware.com/spine-ghosting)。

### 15.2 Preview 细节

可以在多条 Track 上分别播放走路、射击和眨眼，调节每条的速度、循环、Alpha、Mix 和 Additive，模拟运行时组合。制作时尽量让局部动作只给需要的属性打键，减少覆盖全身动作。

旧 Preview 文档仍出现 Hold previous；该按钮已在 4.3.53-beta 移除，runtime 混合系统也已改造。Preview 不包含游戏状态机、输入控制和碰撞逻辑。来源：[Preview](https://esotericsoftware.com/spine-preview)、[编辑器日志](https://esotericsoftware.com/spine-changelog#v4-3-53-beta)、[runtime 4.3 变更](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/CHANGELOG.md)。

## 16. 导入与 PSD 管线

### 16.1 四种主要入口

| 导入内容 | 用途及条件 |
| --- | --- |
| `.spine` 中的骨架 | 合并工程、复用完整角色 |
| `.spine` 中的动画 | 给相同／兼容命名结构的骨架复用动作；缺对象时相应键无法导入 |
| JSON／binary | 导入外部生成的数据、重新构建工程、调整尺度 |
| PSD | 从分层素材提取图片，并生成骨骼／插槽／皮肤和相对位置 |

Import data 的 Scale 会缩放位置、长度、附件和相关动画数据；不同于只把 root scale 调小。可选择新工程、新骨架或导入既有骨架，并针对已有附件 Ignore／Replace。

导入运行数据不一定还原原工程所有编辑信息：Nonessential 关闭时，手工网格边、编辑颜色等信息可能不在导出里。既有骨架的素材更新与动画导入是不同流程，不应认为普通 data merge 会无条件合并所有动作。来源：[Import](https://esotericsoftware.com/spine-import)。

### 16.2 PSD 的导入设置

- PSD File：来源文件。
- Scale：导出图层图片的分辨率。
- Padding：每块部件增加透明边，减少边缘采样问题。
- Trim whitespace：裁掉图层周围透明区。
- Ignore hidden layers：控制隐藏图层是否输出。
- Output folder：PNG 输出位置，支持覆盖确认。
- Images path：关联骨架资源目录。
- Origin：用参考线或 origin 标记设定 Spine 原点。
- Import Data：把图层结构转换为 Spine 对象，而不是仅导出 PNG。

### 16.3 PSD 标签类别

| 标签／结构 | 意义 |
| --- | --- |
| `[bone]` | 创建／归属骨骼；分组可组织层级 |
| `[slot]` | 归入指定插槽 |
| `[skin]` | 归入皮肤和对应占位结构 |
| `[folder]` | 图片输出子目录 |
| `[scale:number]` | 调整图片尺度，同时补偿附件显示比例 |
| `[rotate:degrees]`【4.3 强化】 | 旋转输出图片并补偿附件角度，帮助打包 |
| `[pad:number]` | 针对局部图层改变透明边 |
| `[trim]` | 控制透明裁边／画布／遮罩范围 |
| `[overlay]` | 把图层作为下方内容的覆盖／遮罩处理 |
| `[origin]` | 用图层中心设置 Spine 原点，不输出该图层图片 |
| `[mesh]` / `[mesh:name]` | 创建网格，或按名称匹配源网格生成 linked mesh |
| `[path:name]` | 图片文件名与附件名分离 |
| `[merge]` | 将组内图层合为一张输出图片 |
| `[ignore]` | 跳过该层／组及子内容 |
| `[bones]` / `[slots]` | 为组的直接子项批量应用骨骼／插槽标签 |
| `[name:pattern]` / `[path:pattern]` | 用星号模式为子项名称／路径加前后缀 |
| `[bone:pattern]` / `[slot:pattern]` | 批量转换子项骨骼／插槽归属名称 |
| `[!bones]` / `[!slots]` | 取消从组继承的批量标签 |
| `[!name]` / `[!path]` / `[!bone]` / `[!slot]` | 取消对应组级模式对当前项的影响 |

PSD 混合、遮罩和效果存在格式限制，应检查导出的 PNG 与原设计一致。各绘图软件写出的 PSD 可能包含不同支持程度的特性，不能仅因扩展名是 PSD 就保证完全还原。来源：[Import PSD 全部标签与限制](https://esotericsoftware.com/spine-import-psd)。

名称中的 `/` 用于子目录；以 `/` 开头可阻止相应父组路径继续影响当前值。`[skin]` 和 `[folder]` 的组合有优先级，皮肤占位名与实际附件路径不能随意当成同一个名称。PSD 的 Normal、Multiply、Screen、Linear Dodge 对应 Spine 的 Normal、Multiply、Screen、Additive。图层样式、渐变遮罩、调整／填充层等若没有可导入像素数据，需先生成像素／智能对象或使用官方兼容脚本。来源：[PSD 标签与限制](https://esotericsoftware.com/spine-import-psd#Limitations)。

### 16.4 4.3 的 PSD／资源强化

在 Images 节点管理 PSD，显示附件来源；改进智能对象复用、源网格匹配、缺失 PSD 警告、跨插槽链接网格、scale／merge 标签、重复导入以及覆盖／删除旧输出的处理。

也可使用官方 `spine-scripts` 支持的图像软件导出图片和 JSON。脚本与原生 PSD 导入是两种入口，功能和标签并不保证完全一致。来源：[4.3 发布说明](https://esotericsoftware.com/blog/Spine-4.3-released)、[官方导出脚本](https://github.com/EsotericSoftware/spine-scripts)。

## 17. 全部导出类型与设置

### 17.1 导出格式地图

| 类型 | 主要用途 | 重要区别 |
| --- | --- | --- |
| JSON | runtime 载入、检查／处理数据、自动化制作 | 可读、便于工具处理 |
| Binary，通常 `.skel` | runtime 正式资源 | 更紧凑、解析快，不便人工编辑 |
| PNG | 静帧、透明图片、序列帧 | 无损透明；可把序列打为图集 |
| JPEG | 不需要透明的静帧／序列 | 有损，不保留透明 |
| GIF | 简单循环演示和分享 | 调色板有限，不支持完整半透明 |
| APNG | 透明高质量动画图片 | 无损，通常比 GIF 更大 |
| WEBP | 静帧或序列图片 | 支持透明及有损／无损选择，参数依格式 |
| AWEBP | 动画 WebP | 动画图片，可保留透明并配置压缩与循环 |
| PSD | 分层姿态或帧，交回美术处理 | 可按导出方式组织层；不能恢复原 PSD 全部效果 |
| AVI | 视频成片，部分编码支持透明和音频 | 播放器／编码支持需要确认 |
| MOV | 视频成片，部分编码支持透明和音频 | 容器与编码不同于常见默认 MP4 |
| WEBM | 网页视频 | 编码、透明和音频须同时检查目标播放端 |
| HTML【4.3】 | 直接展示动画的网页 | 可选 Player／Web Components、组合皮肤 |
| Atlas | runtime 合图或序列帧合图 | 描述文件与图片页一起使用，见下一章 |

PNG、JPEG、WEBP 和 PSD 可用于当前姿态或动画帧，具体输出依所选格式。WEBP、AWEBP、WEBM 是早已有的格式，并非 4.3 新增；本机 4.3.26 导出列表已核查。PNG 序列、骨架数据和图集是三类不同交付。来源：[Export](https://esotericsoftware.com/spine-export)、[HTML 4.3](https://esotericsoftware.com/spine-changelog#v4-3-61-beta)。

### 17.2 数据导出

| 设置 | 作用 |
| --- | --- |
| Output / Extension | 指定位置与扩展名；骨架名常决定数据文件名 |
| JSON format | 标准 JSON、JavaScript 风格或 Minimal；后两者不能直接交给严格 JSON parser |
| Pretty print | 便于人工审阅 |
| Nonessential | 保留编辑辅助信息，适合后续再导入和工具往返 |
| Animation clean up | 清理导出数据，不同于保存修改后的源工程 |
| Export all / 对象 Export | 控制是否包含被排除对象；可用选项依格式 |
| Warnings | 提示版本限制和输出问题 |
| Pack | 数据导出的同时生成图集 |
| Relative export paths【4.3】 | 搬工程或换机器时保持目标路径关系 |

旧版本 JSON 导出用于有限恢复，不是可靠的“4.3 新功能降成 4.2”工具。应保留 `.spine` 源文件，以便重新导出。来源：[数据导出](https://esotericsoftware.com/spine-export#Data)、[Versioning](https://esotericsoftware.com/spine-versioning)。

### 17.3 图片／视频公共设置

| 设置类别 | 覆盖能力 |
| --- | --- |
| 导出内容 | 当前姿态或动画；选定／全部骨架、动画和皮肤 |
| 输出组织 | 单文件、逐帧、逐动画、分层或独立姿态，依格式而定 |
| 尺寸 | Scale、Fit、Enlarge、Pad；固定画布避免每帧尺寸跳动 |
| 裁切 | Crop 指定坐标与尺寸；预览中拖动边界 |
| 帧范围 | Range 限定片段，FPS 控制采样密度 |
| 循环 | Repeat、Pause after、是否包含最后一帧 |
| 物理 | Warm Up 预运行，准备次级运动初始状态 |
| 画面内容 | 是否绘制 Bones、Images、其他辅助附件 |
| 画面质量 | Smoothing、MSAA 与适用的各向异性过滤 |
| 背景 | 背景颜色、透明选项和格式支持 |
| 预览 | 逐帧查看画面、尺寸和预计大小 |

首尾姿态相同的循环若同时导出首帧和重复末帧，会多停一帧。导出帧率、时间轴帧率和物理更新率是三个不同参数。来源：[Export common settings](https://esotericsoftware.com/spine-export#Common-settings)、[Playback](https://esotericsoftware.com/spine-playback)、[Physics](https://esotericsoftware.com/spine-physics-constraints)。

### 17.4 各格式特有能力

- **GIF：** 调色数量、颜色／透明抖动、Alpha threshold、Matte、Quality、Forever；半透明边缘会转换，需检查轮廓。
- **PNG／APNG：** 压缩、透明背景和采样；PNG 序列可 Pack，APNG 可控制循环。
- **JPEG：** Quality 和背景颜色，适合不需要 Alpha 的画面。
- **PSD：** RAW／RLE／ZLIB 编码，对比速度与文件大小。
- **AVI／MOV：** 编码、质量／压缩、FPS；透明及音频取决于格式选项，RAW／PNG 等编码可保留透明，播放端未必支持。
- **HTML：** 选择 Player 或 Web Components，配置动画和皮肤组合；交付时确认数据、图集、图片与 JS 路径或内嵌方式齐全。

Additive 附件的透明背景输出，应检查实际像素和目标软件播放。4.3 有相关修复，素材 alpha、背景和显示软件仍影响观感。来源：[各导出格式](https://esotericsoftware.com/spine-export)、[4.3.02](https://esotericsoftware.com/spine-changelog#v4-3-02)。

### 17.5 导出设置复用

导出配置可保存为 JSON，在编辑器或 CLI 再次加载。可为运行数据、透明预览和分享分别保存配置。4.3 支持提取工程中上次导出设置，并增强多线程处理。重复导出快捷键用于重跑上一配置。来源：[Saving and loading export settings](https://esotericsoftware.com/spine-export#Saving-and-loading-export-settings)、[CLI](https://esotericsoftware.com/spine-command-line-interface)。

## 18. 图集打包与解包

### 18.1 Atlas 的内容与工作方式

把多张小图片合成较少的大纹理页，配套 `.atlas` 保存区域、旋转、裁边、偏移、尺寸和纹理设置，运行时据此还原部件位置。

可在数据导出时打包，也可独立运行 Texture Packer／CLI；可只打包附件使用的图片，或扫描 Images folder。多骨架可共用图集或分开；**4.3 支持按皮肤独立 atlas**。

### 18.2 设置类别

| 类别 | 主要选项与意义 |
| --- | --- |
| Region 裁边 | Strip whitespace X/Y、Alpha threshold，减少空白并保留原偏移 |
| Region 复用 | Alias 识别相同像素内容，减少重复区域 |
| Region 旋转 | Rotation 改善排布，runtime 必须处理旋转 |
| Padding | Padding X/Y、Edge padding、Duplicate padding，减少邻图采样串色 |
| 页尺寸 | 最小／最大宽高；超出时多页输出 |
| 页限制 | Power of two、Divisible by 4、Square，匹配目标纹理要求 |
| 排布方式 | Grid、Rectangles、Polygons；多边形使用项目网格轮廓 |
| Alpha | Premultiply alpha 或 straight alpha；Bleed 处理透明像素 RGB |
| 输出格式 | PNG／JPG 页及质量／压缩 |
| 多分辨率 | Scale 列表、Suffix、Resample，输出多套资源 |
| 纹理提示 | Filter min/mag、Wrap X/Y、内存 Format，由 renderer 使用 |
| 路径命名 | Atlas extension、目录合并、区域路径和图片编号 |
| 速度／效率 | Fast、Auto Scale；4.3 brute force 相关设置 |
| 调试与兼容 | Debug 区域边界、Legacy output 旧 atlas 格式 |

多边形打包需工程网格上下文；只有 PNG 不能表达所有 Mesh hull。PMA 与材质必须配套，单独改一边可能出现发黑、亮边或错误混合。来源：[Texture packing](https://esotericsoftware.com/spine-texture-packer)。

### 18.3 目录与配置

- 文件夹可放 `pack.json`，为子目录指定打包规则。
- 配置可保存／加载，在 CLI 重用。
- 九宫格与图片编号有命名规则，atlas 可保存相应数据。
- 项目上下文保护网格所需像素，避免透明裁边误删。
- Per-skin atlas 适合按需下载皮肤；共享素材、缺失 region 和 attachment loader 设置要配套。

### 18.4 Texture Unpacker

根据 atlas 与页图片恢复部件，处理区域旋转和裁边偏移；多边形图集最好提供源工程，否则形状外可能保留邻图像素。只能恢复已有像素，不能恢复原 PSD 效果或未导出的工程属性。来源：[Texture Unpacker](https://esotericsoftware.com/spine-texture-packer#Texture-Unpacker)、[CLI Unpack](https://esotericsoftware.com/spine-command-line-interface#Unpack)。

## 19. 命令行与自动化

### 19.1 官方 CLI 任务范围

| 操作 | 功能 |
| --- | --- |
| Editor | 启动、指定版本、打开工程、帮助与诊断 |
| Export | 配置 JSON 导出，或默认 json／binary，可附带 pack |
| Import | 从项目、JSON、binary 或目录导入骨架，输出 `.spine` |
| Clean up | 动画清理并保存，会修改工程文件 |
| Pack | 独立图集打包，可提供多个工程作为网格上下文 |
| Unpack | 从 atlas 与页图片还原部件 |
| Info | 输出工程／数据版本、动画数等信息 |
| Advanced | 内存、日志、音频、显示、色彩管理与诊断选项 |

4.3 增强输入筛选、导入／合并、参数设置及导出管线。适合固定版本、固定配置的批量制作交付。它不会从视频自动设计骨架或完成美术拆件。来源：[Command line interface](https://esotericsoftware.com/spine-command-line-interface)。

### 19.2 常用参数与 4.3 增量

| 参数／机制 | 作用 |
| --- | --- |
| `-h` / `--help` | 当前安装的 CLI 帮助 |
| `-v` / `--version` | 版本信息，注意 launcher 与编辑器区别 |
| `-u` | 指定编辑器版本；新 launcher 支持 `-u project` 跟随工程 |
| `-i` / `--input` | 输入工程、数据或目录 |
| `-o` / `--output` | 输出位置 |
| `-e` / `--export` | 导出配置或默认类型 |
| `-r` / `--import` | 导入骨架；4.3 用 `--from`／`--to` 指定来源及目标，不沿用旧 `-r NAME` 命名方式 |
| `-s` / `--scale` | 导入尺度 |
| `-m` / `--clean` | 清理；独立操作会保存工程 |
| `-p` / `--pack` | 图集名或打包配置 |
| `-c` / `--unpack` | 待解包 atlas |
| `-j` / `--project` | 网格上下文工程 |
| `--last-export-settings`【4.3】 | 取出工程上次导出配置 |
| `--set`【4.3 增强】 | 覆盖受支持设置，字段按当前 help |
| 通配符／正则【4.3】 | 灵活选择批量输入 |
| `SPINE_ARGS`【4.3】 | 提供额外调用参数的环境机制 |
| `--trace` | 更多日志与诊断 |
| `--ignore-unknown` | 放在参数列表最前，允许 launcher 继续传递未知参数；官方长名为 `--ignore-unknown-parameters`，部分日志打印为 `--ignore-unknown-args`；不保证 editor 执行未支持的选项 |

以当前 launcher／editor help 为准。旧 launcher 拒绝某参数，不代表新编辑器没有此功能。`-i`、`-o` 等输入参数放在 action（如 `-e`、`--last-export-settings`）之前；一个 action 结束本次参数组，后面的参数属于下一组。失败返回非零退出码，自动化应检查。来源：[CLI 参数](https://esotericsoftware.com/spine-command-line-interface)、[4.3 参数变化](https://esotericsoftware.com/spine-changelog)、[Nate 对参数别名、顺序与 Launcher 4.3.06 修复的说明](https://esotericsoftware.com/forum/d/30374-exports-export-setting-in-cli)。

### 19.3 平台入口与限制

Windows 批处理通常调用 `Spine.com`，可等待结束并取得日志；macOS 调用 `.app/Contents/MacOS/Spine`；Linux 使用安装的 `Spine.sh`。

本手册没有执行批量命令或修改现有动画工程。CLI 是导入导出接口，runtime API 是游戏端接口，不能把它们当成覆盖编辑器每个按钮的完整脚本 SDK。来源：[CLI 平台说明](https://esotericsoftware.com/spine-command-line-interface#Running-Spine-with-CLI-parameters)。

## 20. 设置、快捷键与数值表达式

### 20.1 设置覆盖范围

| 分类 | 可调内容 |
| --- | --- |
| Launcher | 编辑器版本、启动行为、代理与相关连接配置 |
| Files | 设置／日志与备份目录、文件打开及路径 |
| General | 语言、提示、欢迎页和默认设置 |
| User interface | 字体、字号、比例、行高、工具栏位置／文字、Tree 缩进 |
| Viewport | 背景、骨骼尺度、背面剔除、color bleed、缺图提示、未选骨架暗化 |
| 图像显示 | Smoothing、MSAA、Pixel grid、各向异性过滤、边缘显示 |
| Behavior | 自动备份、删除确认、双击、中键平移、平移惯性、平滑滚动、提示、缩放中心 |
| Dopesheet | 点击跳帧／跳键、框选保持 |
| Graph | 空白拖动编辑、点击跳帧／跳键 |
| 快捷键 | 个性化键位与键盘布局 |

Pixel grid 是编辑器的像素风格预览；MSAA 主要改善网格／裁剪切过实心像素的边缘。改变编辑器设置不保证目标引擎的纹理和抗锯齿完全一致。来源：[Settings](https://esotericsoftware.com/spine-settings)。

### 20.2 工具与操作

| 操作 | 功能 |
| --- | --- |
| Pose | 快速摆动骨骼链 |
| Create | 创建骨骼和相关结构 |
| Weights | 编辑绑定权重 |
| Rotate / Translate / Scale / Shear | 四类变换、不同空间轴和数值输入 |
| Bone length | 调整骨长 |
| Compensate | 改骨骼时补偿对应后代／附件，便于调整装配 |
| Selection groups / History | 保存集合、导航近期选择 |
| Rulers / Pixels / 吸附 | 测量、对齐和像素位置调整 |
| Copy / Paste | 对象、变换、约束和关键帧的上下文复制 |

常见默认键：Pose `B`、Create `N`、Weights `G`、Rotate `C`、Translate `V`、Scale `X`、Shear `Z`、Key Edited `K`。Mac 主要使用 Cmd；实际以设置和键盘布局为准。来源：[Tools](https://esotericsoftware.com/spine-tools)。

4.3 新快捷操作涵盖：选择／保持／翻转／导航关键帧、视口翻转与缩放、绘制顺序移顶／移底、图片路径、Sequence、Region–Mesh 切换、网格编辑。动作名比固定默认键位可靠，用户可以重新绑定。来源：[4.3 快捷键记录](https://esotericsoftware.com/spine-changelog)。

### 20.3 数值表达式【4.3】

数值框支持直接运算，减少外部计算和逐对象输入。

- `value`／`v` 引用当前值。
- `+=`、`-=`、`*=`、`/=` 按当前值计算。
- 多选可以对每个对象自己的值分别运算。
- 骨骼变量包括 `boneLength`、`boneRotation`。
- 提供 `random`、`clamp` 等函数，函数参数按编辑器支持语法使用。

例如 `v * 0.8` 把当前值缩为 80%，`+=10` 增加 10。表达式仍受字段单位与范围限制。来源：[4.3 Math in numeric fields](https://esotericsoftware.com/blog/Spine-4.3-released)、[更新日志](https://esotericsoftware.com/spine-changelog)。

## 21. 运行时、引擎与 Web

### 21.1 官方运行时共同能力

| 类别 | 能力 |
| --- | --- |
| 载入 | JSON／binary、atlas、纹理，共享 SkeletonData 与独立 Skeleton 实例 |
| 播放 | 时间推进、循环、速度和片段 |
| 排队 | 顺序播放、延迟、结束后切换 |
| 混合 | Crossfade 与 Mix duration |
| 多 Track | 全身运动与局部动作，分别控制速度／Alpha |
| Additive | 按属性支持叠加运动 |
| Empty animation | 淡出当前层影响，交回其他控制或基础姿态 |
| 皮肤／附件 | 改皮肤、组合、换部件，按集成支持重打包 |
| 程序控制 | 骨骼、IK、约束、颜色响应输入和游戏环境 |
| 几何查询 | 世界顶点、包围多边形、Point 世界位置与方向 |
| 事件监听 | 生命周期及用户 event，应用实现声音／业务行为 |
| 渲染 | Region／Mesh、atlas、混合与裁剪，细节取决于 renderer |

多 Track 不自动按身体部位建立遮罩：高层会影响它实际打键的属性。全身打键的射击动作可能覆盖走路，应在制作或程序中限定范围。来源：[Runtimes Guide](https://esotericsoftware.com/spine-runtimes-guide)、[4.3 API](https://esotericsoftware.com/spine-api-reference)、[Runtime skins](https://esotericsoftware.com/spine-runtime-skins)。

### 21.2 4.3 核心变化

- Slider、Transform 映射、draw order folder timelines。
- Setup、未约束 pose、约束后／applied pose 分离，属性访问发生迁移。
- 改善 crossfade hold；旧 `holdPrevious`／`interruptAlpha` 不应照搬。
- `TrackEntry.mixInterpolation` 支持非线性混合。
- Physics 更新、reset、骨架级风／重力改进。
- Convex／inverse clipping、跨 Slot linked mesh。
- Sequence 每帧 region／UV／offset 预处理。
- Per-skin atlas 缺失 region 处理与 attachment loader 增强。

这不是所有语言共用同一属性路径的代码示例。当前 Timeline／Animation 的 apply 文档使用 `MixFrom` 等参数，与发布博客早期 `fromSetup` 摘要不同；代码按对应 4.3 runtime API 编写。来源：[runtime changelog](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/CHANGELOG.md)、[Timeline.apply](https://esotericsoftware.com/spine-api-reference#Timeline-apply)。

### 21.3 引擎／语言地图

| 方向 | 官方集成／基础库 | 核查重点 |
| --- | --- | --- |
| Unity | spine-unity、spine-csharp | 组件、材质、UI、线程 |
| Unreal | spine-ue、spine-cpp | 引擎版本、组件、材质 |
| Godot | spine-godot、spine-cpp | 插件／构建、渲染缺项 |
| 浏览器 | spine-ts 后端、Player、Web Components | alpha、加载与性能 |
| Web 框架 | Pixi、Phaser、Three.js | 框架主版本与包匹配 |
| Java | spine-libgdx | 数据版与更新／渲染循环 |
| C++ | spine-cpp、SDL／SFML／GLFW | 新接口、构建、纹理载入 |
| C | spine-c | 4.3 新包装 API，旧 `sp*` 不兼容 |
| Apple | SpineC／SpineSwift／SpineiOS | Swift 类型、Metal、平台缺项 |
| Flutter | spine-flutter | Dart API 与平台缺项 |
| Haxe | spine-haxe | 框架集成与数据版本 |
| 第三方／内置支持 | 由相应供应方维护 | 确认真实数据版本和特性 |

这是方向索引，不承诺任何引擎版本、任意第三方实现均支持 4.3。来源：[官方 runtime 目录](https://esotericsoftware.com/spine-runtimes)、[4.3 源码目录](https://github.com/EsotericSoftware/spine-runtimes/tree/4.3)。

### 21.4 Unity 的 4.3 改进

**组件拆分：** SkeletonRenderer／SkeletonGraphic 负责渲染，SkeletonAnimation／SkeletonMecanim 负责动画；可用 Mecanim 驱动 UI 的 SkeletonGraphic。旧继承关系、取组件和实例化调用需复核。

**多线程：** 动画计算与 mesh 生成可在工作线程执行，事件回调仍在主线程。Mecanim 查询 Unity 控制器仍有主线程部分，不能认为所有工作完全并行。可先逐场景转换，验证后再全项目转换。

**UI／材质：** UI Toolkit 强化 PMA、混合模式和背面渲染；改善材质检测、alpha 不一致提示、工作流切换与皮肤重打包混合模式。

来源：[Main components](https://esotericsoftware.com/spine-unity-main-components)、[4.3 split component guide](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-unity/Assets/Spine/Documentation/4.3-split-component-upgrade-guide.md)、[4.2→4.3 guide](https://esotericsoftware.com/forum/d/29234-spine-unity-42-to-43-upgrade-guide)。

### 21.5 Player 与 Web Components

| 方案 | 特点 | 适合 |
| --- | --- | --- |
| Spine Player | 现成播放控件和展示／调试界面 | 审核、作品展示 |
| Web Components | HTML 属性配置 `<spine-skeleton>`，共享渲染 overlay | 网页多处角色、互动文档 |
| 框架／原始 runtime | 程序管理骨架、输入和渲染 | 游戏、复杂交互 |

Web Components 的配置覆盖：资源 URL／内嵌数据、动作队列、组合皮肤、尺寸适配、位置、边界、mix、加载 spinner、离屏策略、拖动／指针输入、回调、调试、图集页选择和 Slot 跟随。它不提供 Player 的内建播放控制界面。

共享 WebGL overlay 避免每个元素各开 context 的限制，但成本仍取决于骨架、裁剪、面积和实例数。旧示例可能含 `4.2.*` 地址，4.3 资源须用匹配包。来源：[Web Components](https://esotericsoftware.com/spine-webcomponents)、[官方 4.3 示例](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-webcomponents/example/tutorial.html)。

### 21.6 已确认的平台差异

- iOS 4.3 README：不支持 two-color tinting。
- Flutter 4.3 README：不支持 two-color tinting 和 Screen blend mode。
- spine-godot 的 2D renderer 不支持 Screen；two-color tinting 需要 **Godot 引擎 4.3 或更新**，module 和 GDExtension 均支持。这里的 Godot 引擎版与 Spine 数据版是两个不同版本号。
- spine-cocos2dx 在 4.3 移除，旧项目需保留 4.2 集成或迁移。

“更新到 4.3”表示数据模型支持，不表示所有 renderer 拥有完全相同的视觉功能。来源：[iOS](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ios/README.md)、[Flutter](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-flutter/README.md)、[Godot](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-godot/README.md)、[native 4.3](https://esotericsoftware.com/blog/Spine-4.3-released)。

## 22. 性能指标与优化功能

### 22.1 Metrics 指标

| 指标 | 对应意义 |
| --- | --- |
| Bones | 骨骼变换工作 |
| Constraints | IK、路径、物理求解工作 |
| Slots | 组织规模与最多可见附件数 |
| Total attachments | 全部附件，不等于当前绘制数 |
| Visible attachments | 当前启用附件，辅助几何未必渲染 |
| Vertices | 可见几何顶点 |
| Vertex transforms | 与每顶点影响骨骼数相关的变换次数 |
| Triangles | 渲染三角形规模 |
| Area | 含透明及重复覆盖的绘制面积 |
| Clipping polygons | 凹形分解的凸裁剪多边形数 |
| Clipped triangles | 裁剪处理工作量 |
| Timelines | 动画每帧应用的属性时间轴 |
| Selection | 所选对象与整体对照，定位重成本部位 |

Metrics 不替代手机实际 frame time；官方早年特定设备的骨骼数量示例也不是统一预算。来源：[Metrics](https://esotericsoftware.com/spine-metrics)。

### 22.2 可用优化工具

- Mesh hull 去掉透明区，减少 overdraw。
- Prune 减少微小权重，降低 vertex transforms。
- 适量顶点与手工边，把密度放在需要弯曲的部位。
- Skin bones／constraints 减少未启用装备的活动计算。
- 完全隐藏时用无附件，避免只将 Alpha 设 0。
- 限定 clipping 范围，减少被裁三角形，评估 convex 选项。
- 图集、渲染排序帮助批处理；纹理、材质、混合切换可能拆 batch。
- 清理无用时间轴与冗余键，同时复验动画混合。
- Unity 多线程／Web overlay 的收益要按场景测量。

顺序：先测实际瓶颈，再用 Metrics 定位，改一项后比较画面与性能。不为降低数字牺牲必要变形质量。来源：[Performance](https://esotericsoftware.com/spine-metrics#Performance)、[Meshes](https://esotericsoftware.com/spine-meshes)、[Weights](https://esotericsoftware.com/spine-weights)。

## 23. 4.3 新增及强化功能总表

此表只表示 4.3 的变化，详细说明在对应章节。未发布补丁不计入当前能力。

| 类别 | 4.3 变化 | 本手册位置 |
| --- | --- | --- |
| 新约束 | Slider 复用动画姿态／时间轴 | 11.6 |
| 绑定控制 | Transform 跨属性映射、Clamp、独立空间 | 11.3 |
| 动作叠加 | Draw order 文件夹独立关键帧 | 5.2、12.2 |
| 裁剪 | Inverse、Convex、求解优化 | 7.4 |
| 网格复用 | Linked mesh 跨 Slot，deform／sequence 继承改进 | 8.3 |
| IK／Physics | ScaleY 的 volume 等模式 | 11.2、11.5 |
| 网格生成 | 多网格 Trace、Uniform、描边质量 | 8.1 |
| 权重 | 多网格刷权重、Bind、Auto、Smooth、Prune、Weld | 9 |
| 诊断 | Problems 集中警告、部分自动修复 | 3.3、15 |
| 网页交付 | HTML、Player／Web Components、组合皮肤 | 17、21.5 |
| 曲线同步 | Dopesheet-centric Graph 同步 | 13 |
| 曲线维护 | 改值维护控制柄、Last chosen 默认曲线 | 13.2 |
| 结果设键 | Key Constrained | 11.5、12.3 |
| 数值输入 | 算式、当前值变量、多选逐项运算、函数 | 20.3 |
| 皮肤 | 拖放合并、依赖补全、Only pinned skins | 10 |
| 交接 | Package Project ZIP | 3.1 |
| 导出路径 | 相对路径 | 17.2 |
| PSD | Images 管理、来源标识、标签、智能对象、同步改进 | 16 |
| 图集 | 每皮肤 atlas、brute force 相关设置 | 18 |
| 导出速度 | 图片、视频、数据与 CLI 多线程处理 | 17、19 |
| 视口／Tree | 图标尺寸／方向、面包屑、文件夹复制、选择与性能改善 | 3、4、20 |
| 交互 | Reset、图片路径选择、Sequence／Region–Mesh、新快捷键 | 8、20 |
| 物理 | 平滑、低帧率响应、reset、风／重力改善 | 11.5、21.2 |
| runtime 姿态 | pose 模型和属性访问重构 | 21.2、24 |
| runtime 混合 | hold、Additive、mixInterpolation | 21.2 |
| native 底层 | C++ 共用实现、C 自动包装、Swift／Dart 重写 | 21.3、24 |
| Unity | 组件拆分、线程、Mecanim UI、UI Toolkit／材质 | 21.4 |
| Web 后端 | PMA／straight alpha、共享 renderer、物理继承 | 21 |
| 验证基础 | 核心 runtime 与 libgdx 快照比对，覆盖按官方进度扩展 | 官方 runtime 日志 |
| Essential | IK、skin bones／constraints 开放 | 1.3 |

来源：[Editor Changelog](https://esotericsoftware.com/spine-changelog)、[Runtime Changelog](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/CHANGELOG.md)。普通网格、皮肤、IK、Physics、PSD、CLI 和 Graph 本身并非全部在 4.3 首次加入。

## 24. 升级与容易误解的边界

### 24.1 从 4.2 升到 4.3

1. 保留旧 `.spine`、图片、PSD、图集与导出配置副本。
2. 确认 renderer、引擎集成及第三方支持 4.3。
3. 用 4.3 打开副本，检查约束、网格、皮肤、物理和关键帧。
4. 用 4.3 重新导出，不能靠改 JSON 版本字符串升级。
5. 更新 4.3 runtime，迁移 pose、组件及 native API 调用。
6. 检查动作切换／混合、事件、alpha／材质、皮肤加载和裁剪。
7. 目标设备验证后，再同步团队版本。

新编辑器可打开旧工程，新版保存后一般不能再用旧编辑器打开。Editor 与 runtime 补丁号不必逐位相同，major.minor 要匹配；beta 另查具体兼容版本。来源：[Versioning](https://esotericsoftware.com/spine-versioning)、[Unity guide](https://esotericsoftware.com/forum/d/29234-spine-unity-42-to-43-upgrade-guide)。

### 24.2 Native 的破坏性变化

`spine-c` 不再是旧的手写 C runtime，而是 C++ 的生成包装；旧 `sp*` 调用需迁移为新句柄／函数接口。Swift、Dart 的类层级、可空类型、方法／属性也重整。直接使用底层 API 的项目不能只替换二进制包，需要更新调用并重新编译。

C++ 的头文件位置和公共 API 同样变化；下游引擎应使用匹配的 4.3 集成，不混装旧 core。来源：[spine-c 4.3 README](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-c/README.md)、[runtime changelog](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/CHANGELOG.md)。

### 24.3 常见误解

| 误解 | 正确理解 |
| --- | --- |
| 4.3 还是 Beta | 已有正式版；4.3.27 未发布是另一回事 |
| Essential 没有 IK | 4.3 已开放，旧网页有滞后 |
| Slider 是普通数值条 | 是应用另一条动画的约束 |
| Transform 只能复制同类属性 | 4.3 支持跨属性映射 |
| Volume 就是 3D 体积模拟 | 是二维缩放的面积保持 |
| Physics 自动做布料碰撞 | 不是完整碰撞求解器 |
| Key Constrained 自动烘焙整条动画 | 是结果设键，整段采样和复验仍需制作 |
| 多皮肤让一槽显示多张图 | 一槽最多一个附件，叠层需多个槽 |
| Bounding box 自动造成伤害 | 提供几何，业务与引擎接入由应用负责 |
| 30 FPS 时间轴锁死 runtime | runtime 时间插值与渲染频率分离 |
| 所有 4.3 renderer 功能完全一样 | 染色、混合、线程支持有差异 |
| `.skel` 导入恢复全部制作信息 | 未导出信息与原美术内容无法恢复 |

证据分别见前面的版本、附件、约束、Playback、Import 与 runtime 专题。

### 24.4 验证边界

已核对官方目录、专题、editor/runtime 4.3 日志和 Essential 变化，并只读观察本机 4.3.26 的动画面板、设置、导出与打包对话框。没有逐项执行编辑、导出或运行时功能，没有编译全部 runtime，没有升级或改动现有 `hog-rider` 工程的动画数据。界面观察后恢复原有布局与选中状态。

旧在线资料的明显滞后包括：Essential 的 IK／skin bones 限制；Transform 的旧界面；Physics 的 No baking 结论；部分旧 API、Web 示例版本。使用说明以本手册的版本标记区分；代码以匹配的当前 4.3 实现为准。

## 25. 按需求查功能

| 想做的效果／任务 | 功能组合 | 重点检查 |
| --- | --- | --- |
| 待机／走路 | FK、骨骼关键帧、Graph | 重心与循环 |
| 固定手脚 | IK、Mix、目标控制骨 | 伸直与弯折 |
| 柔软肢体 | Mesh、Weights、手工边 | 关节收缩与轮廓 |
| 头发／衣摆拖动 | Physics、Weights、Warm Up | 大位移、循环、运行移动 |
| 转头／伪 3D | Mesh deform、Slider、Slot 切换 | 轮廓与覆盖 |
| 同动作多套外观 | Skin placeholders、Skins、Linked mesh | 命名与结构 |
| 模块化换装 | 组合皮肤、skin bones／constraints | 覆盖与依赖 |
| 肌肉联动 | Slider、Transform mapping、颜色／deform | 输入范围 |
| 绳索／触手／履带 | Path、Path constraint、Weights | 骨链、间距、模式 |
| 卷轴／机械 | Transform mapping、Clamp | 范围与空间 |
| 武器层级独立 | Draw order folders、多 Track | 分组与竞争 |
| 揭示／挖空 | Clipping、Inverse、End slot | 自交、范围、CPU |
| 枪口／粒子锚点 | Point、runtime 查询 | 世界位置／方向 |
| 攻击框／点击 | Bounding box、Event、应用逻辑 | 时段与接入 |
| 火焰／烟雾 | Sequence、骨骼、Additive | 图集与透明边 |
| 口型／节拍 | Audio、Event、Slot／Slider | 声音运行处理 |
| 走路同时射击 | Preview／AnimationState 多 Track | 属性控制范围 |
| 网页展示 | HTML、Player | 路径与完整资源 |
| 多个网页交互角色 | Web Components、Slider／IK | 离屏与实例数 |
| 批量交付 | CLI、配置、Package Project | 版本、退出码、素材 |
| 移动端降成本 | Metrics、Prune、Mesh hull、skin bones | 真机性能与质量 |

这些是功能应用建议，不表示存在一键完成每种需求的自动化命令。

## 26. 官方功能覆盖索引与资料

### 26.1 User Guide 全模块映射

| 官方条目 | 本手册 |
| --- | --- |
| Getting started | 1、2、3 |
| User interface | 3、20 |
| Skeletons | 2、4 |
| Bones | 4 |
| Slots | 5 |
| Images | 6、16 |
| Tools | 3、8、9、20 |
| Keys | 12、13 |
| Animating | 12.4 |
| Attachments | 6、7 |
| Region / Mesh / Bounding box / Clipping / Path / Point | 7 六节 |
| Skins | 10 |
| Constraints | 11.1 |
| IK / Path / Transform / Physics / Sliders | 11 五类专题 |
| Events | 14 |
| Views 与全部 15 个专题面板 | 15，并结合 8、9、13、14、22 |
| Welcome screen | 3.3 |
| Versioning | 1、24 |
| Export | 17 |
| Texture packing | 18 |
| Import | 16.1 |
| Import PSD | 16.2–16.4 |
| Command line interface | 19 |
| Settings | 20 |
| 4.3 Problems 及新能力 | 3.3、23 |
| runtime／引擎增量 | 21、24 |

覆盖基准：[官方 User Guide](https://esotericsoftware.com/spine-user-guide)。第 27—29 章附有逐小节映射，第 31 章汇总核对结果。本文解释公开具名功能和操作边界；全部补丁修复记录、各引擎的全部 API 重载由原始日志／API 专题保存，不将它们重新复制为功能定义。

### 26.2 官方入口

- [Editor Documentation](https://esotericsoftware.com/spine-editor-documentation)
- [User Guide](https://esotericsoftware.com/spine-user-guide)
- [Spine 4.3 发布说明](https://esotericsoftware.com/blog/Spine-4.3-released)
- [Editor Changelog](https://esotericsoftware.com/spine-changelog)
- [4.3 API Reference](https://esotericsoftware.com/spine-api-reference)
- [Runtimes Guide](https://esotericsoftware.com/spine-runtimes-guide)
- [Runtime 4.3 源码](https://github.com/EsotericSoftware/spine-runtimes/tree/4.3)
- [Runtime Changelog](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/CHANGELOG.md)
- [Example projects](https://esotericsoftware.com/spine-examples)
- [Spine scripts](https://github.com/EsotericSoftware/spine-scripts)
- [JSON 格式](https://esotericsoftware.com/spine-json-format)
- [Binary 格式](https://esotericsoftware.com/spine-binary-format)
- [Atlas 格式](https://esotericsoftware.com/spine-atlas-format)
- [Versioning](https://esotericsoftware.com/spine-versioning)
- [购买与授权版本](https://esotericsoftware.com/spine-purchase)

### 26.3 本工作区相关材料

- `4.3 版本核查笔记`（本机工作区资料，未随仓库发布）：补充 Essential 权限、旧文档冲突与 runtime 迁移证据。
- `自动化制作流程手册`（本机工作区资料，未随仓库发布）：已有制作规程，本次未改动；其中 Essential／IK 的旧判断需结合 1.3 校正后使用。

后续更新先改版本事实，再按受影响模块核对；未来的 Unreleased 条目不能写成现有功能。


<a id="reference-rigging"></a>

## 27. 装配、附件、权重与约束逐项参考

- [G1. 骨架、骨骼与插槽装配](#rigging-g1)

- [G2. 选择、坐标轴、Pose、Create、Compensate 与复制](#rigging-g2)

- [G3. 附件类型的参数与几何操作](#rigging-g3)

- [G4. Mesh 拓扑、UV、链接与软选择](#rigging-g4)

- [G5. Weights：全部模式、操作与诊断](#rigging-g5)

- [G6. Skin：占位符、依赖、复制与组合](#rigging-g6)

- [T1. 全约束共有操作与计算顺序](#rigging-t1)

- [T2. IK：一骨／两骨、伸缩、柔化与体积缩放](#rigging-t2)

- [T3. Path Constraint：排列、间隔、旋转方式与控制柄](#rigging-t3)

- [T4. Transform Constraint：4.3 输入输出映射完整结构](#rigging-t4)

- [T5. Physics：可模拟自由度、力学参数、预览、Reset 与采样](#rigging-t5)

- [T6. Slider：按手动帧／骨属性取整套动画姿态](#rigging-t6)

- [R1. 已核实范围、默认值与公开说明的局限](#rigging-r1)

- [R2. 官方子节逐项覆盖账本](#rigging-r2)

核验：2026-10-09。本章逐项展开第 4—11、20 章涉及的装配功能；保留英文动作名以便搜索。参数中的百分数属于编辑器表示；runtime 常用 0—1。凡未公开说明的编辑器硬范围、工厂初值，不以源码对象的零初始化冒充编辑器默认。示例是原创操作方案，数值是演示值，不能当作产品默认值。

<a id="rigging-g1"></a>

### G1. 骨架、骨骼与插槽装配

#### G1.1 骨架：创建、资源开关、整体层级、尺度

| 项目 | 操作与结果 |
| --- | --- |
| New Skeleton | 在同一工程增加独立骨架；默认 Mac Cmd+N。每个骨架独立存放结构、动作和素材路径 |
| Export | 关闭后整架不进入 JSON、binary、图片或视频；适合装配模板和参考背景 |
| Visibility dot | 暂时卸载该架资源并从各视图隐去；数据交付仍会导出，因此不能拿它代替 Export |
| Skeleton draw order | Tree 中拖动骨架调整前后；列表上方在画面前方。编辑器逐架画完，不能仅凭两架整体顺序实现互相穿插 |
| Reference scale | 距离影响角度／缩放等无长度单位效果的尺度基准；整体缩小到原来 1/2 而希望物理比例一致时，同样缩小此值 |

**原创示例：** 主角在前景栏杆后面、武器却要伸到栏杆前面，可拆为角色主体、栏杆、武器三架，或者将对象纳入同一架并调整插槽顺序。只有“角色架在栏杆架前／后”两个状态不足以表达交叉遮挡。依据：[Skeletons](https://esotericsoftware.com/spine-skeletons)、[Physics Reference Scale](https://esotericsoftware.com/spine-physics-constraints#Reference-Scale)。

#### G1.2 骨骼属性、父级与继承

| 字段／动作 | 具体意义与操作边界 |
| --- | --- |
| Rotation | 围绕骨骼原点转动；不是绕骨长中点。动画 Local 数值可超过一圈 |
| Translation | 骨骼原点相对父级的位置；子骨无需长在父骨末端 |
| Scale | 分轴倍率，1 为原尺寸；单轴负值翻转。非均匀缩放会使后代世界轴夹角改变 |
| Shear | 改变轴向夹角；小幅使用可形成挤压、倾斜和透视感。后代也会继承结果 |
| Length | 不是图片尺寸；除显示，还影响 IK、Path 和自动权重。零长度骨仍能控制附件 |
| Icon / Color | 控制器的视觉区分；不是角色素材染色。4.3 还支持图标尺寸、旋转 |
| Name 复选框 | 强制显示视口名称标签；不是重命名。对象身份用 Rename／F2 修改 |
| Select | 关闭后避免视口误选，Tree 仍能选择 |
| Set Parent | Setup 中 P 后点新父级，或 Tree 拖放；结构变化后应检查所有动作、约束与绑定 |
| Add to Skin | 加入当前活动皮肤；没有活动皮肤时不可用，root 不能加入 |
| Split | 数量指定分段数；Nested 建连续父子链，关闭后为同级；Fibonacci 逐渐缩短，关闭则等长 |
| Separate X/Y | Animate 中对当前动画分别控制位移、缩放或错切两个通道；不改变另一条动画的分离配置 |
| Visibility dot | 临时隐骨，本骨的附件／子骨不会因单独隐骨就全部消失；右击点可整支切换。不能打帧 |
| Bone Order Up / Down | 4.3 可重新排列骨骼控制柄在视口的显示先后；不改变附件插槽遮挡 |

**继承模式对应 runtime：** `normal` 完整继承；`onlyTranslation` 只继承位移；`noRotationOrReflection` 不继承旋转及反射；`noScale` 不继承缩放但仍反射；`noScaleOrReflection` 同时排除缩放和反射。编辑器 Rotation／Scale／Reflection 的组合最终落到这些模式，不是任意三项组合都具有不同 runtime 枚举。模式能打帧，属于瞬时切换，不能期待模式间平滑插值。

**边界：** 关闭旋转继承不保证父级非均匀缩放完全不改变子骨世界朝向；关闭骨骼缩放继承不阻止游戏端 Skeleton.scaleX/Y 对整架的缩放。旧手册“一律禁止 IK 骨关闭继承”的描述需要结合 T2 的一骨／两骨区别。依据：[Bones](https://esotericsoftware.com/spine-bones)、[Inherit](https://esotericsoftware.com/spine-api-reference#Inherit)、[4.3 Bone Order](https://esotericsoftware.com/spine-changelog#v4-3-90-beta)。

#### G1.3 插槽、颜色、遮挡与显示状态

| 项目 | 设置方法及结果 |
| --- | --- |
| Attachment | 点击附件显示图标选择本槽当前附件，再对 Slot 打 attachment key；点击当前附件可置空 |
| Color | 浅色通道 RGB 染色、Alpha 透明度；动画可以打帧。附件自身 setup color 另乘一层且不可动画 |
| Tint Black | 打开深色通道；Light 控制亮部与透明度，Dark 改变暗部。必须由目标 renderer 支持 |
| Blend mode | Normal 普通；Additive 适合发光；Multiply 适合阴影；Screen 适合亮化。切换可能拆分批次 |
| Separate Alpha | 当前动画把 RGB 和 A 拆开，能用不同关键帧与曲线 |
| Draw order | Tree 插槽列表拖动；`+/-` 上下移，Shift 一次移 5，4.3 还有移顶／移底动作 |
| Folder | New→Folder 建分组，拖入插槽；导出名称包括目录路径，例如 `arm/front/hand` |
| Draw order folder keys | 4.3 可给局部分组独立打层级键；分组边界决定哪些动作可独立控制，仍要检查裁剪起止槽 |
| Slot visibility dot | 临时隐藏附件；不打帧，附件保留在数据导出中，但图片／视频导出不绘制该槽 |

一个插槽只有一个可见附件。要同时显示“手掌”和“手套”必须分槽。通过 Slot 的 attachment key 关闭图片，比 Alpha=0 更能消除无用绘制；淡出时先 Alpha 过渡，再置空。

**原创操作：** 将 `hand/front` 放在武器上面、`hand/back` 放在武器下面，两个槽都属于同一手骨；这能表达抓握，骨骼父子顺序不参与图片遮挡。调整 Tree 过滤只显示 Slots 后再移动层级，避免误改骨骼层级。依据：[Slots](https://esotericsoftware.com/spine-slots)、[4.3 局部分组键](https://esotericsoftware.com/blog/Spine-4.3-released)。

<a id="rigging-g2"></a>

### G2. 选择、坐标轴、Pose、Create、Compensate 与复制

#### G2.1 选择与常用动作

点击对象选择；Cmd／Ctrl 点选或框选追加；中键框选不受起点是否空白限制。空格／Esc 取消选择，PageDown／PageUp 回顾选择。Cmd／Ctrl+0—9 保存集合，数字取回，Shift+数字把集合追加进当前选择。多选四肢再使用 Pose 时仍可保持整组选择。

默认工具：Rotate C、Translate V、Scale X、Shear Z、Pose B、Create N、Weights G。视口右击快速回到前一个工具。4.3 的选择命令还可选直接子骨、全部后代、同色骨、插槽、当前可见附件及各类约束；这些命令在快捷键文件中存在但可能无默认键。

Viewport options 按 Bones／Images／Others 分三组；每组独立设“可选”“显示”“名称”。整体关闭显示与 Tree 单个 visibility 的行为不能混为一种开关。标尺显示世界坐标鼠标位置；未缩放图片可按像素对应。Pixels 对 Region 吸附，使坐标小数落在 .0 或 .5，适合像素图；不应假定它会把整个骨骼动画全部改成整数运动。依据：[Tools](https://esotericsoftware.com/spine-tools)、本机 4.3.26 `settings/hotkeys-1.txt`（本次只读）。

**Auto Key：** Animate中启用后，编辑可动画属性自动写当前帧；Setup不因此变成动作。未键修改可能在跳帧后丢失；要试权重姿势先关闭AutoKey，确认没有生成临时动作键。已有键与未打键属性的具体策略还受当前动画设置影响，详细状态见主手册关键帧章。

#### G2.2 四种轴与数值输入

| 轴 | Rotate | Translate | Scale |
| --- | --- | --- | --- |
| Local | 相对父轴的局部角，动画可记录多圈 | 从父原点出发，以本骨轴方向描述距离 | 局部缩放 |
| Parent | 与 Local 旋转相同 | 从父原点出发，以父骨轴方向描述距离 | 与 Local 缩放相同 |
| World | 世界朝向，主要表达 0—360°方向 | 到世界原点的位置 | 显示合成世界倍率；编辑输入时按局部倍率处理 |
| Constrained【4.3】 | 约束后的局部角 | 约束后的局部变换值 | 约束后的局部倍率；可与 Key Constrained／复制受约束姿态配合 |

旋转、缩放、错切以局部数值保存；位置通常相对父空间保存。换操作轴只是换显示和操纵方式。要从 0° 转两圈，使用 Local 输入 `+=720` 并打帧；World 的同一朝向无法说明经历了几圈。

Translate／Scale 的红绿柄限制单轴；Rotate 按住 Shift 可按 15°刻度。多骨使用 Local／Parent 平移，能让朝向不同的两条前臂各自沿本轴伸出相同距离。Scale 无论显示何轴都按对象局部轴作用。

数值可 Enter／Tab 确认；滚轮或上下拖动调节，Shift 精细。4.3 支持 `v`、`value`、数学表达式、`+=/-=/*=//=`，每个多选对象以自己的当前值求值。字段单位仍存在：角度表达式不会变成距离。Constrained 的具体工具编辑结果受约束影响，应以本机 tooltip 与约束状态判断，不能理解成动画存储突然改为世界坐标。依据：[Tools axes](https://esotericsoftware.com/spine-tools#Axes)、[4.3 constrained axes](https://esotericsoftware.com/spine-changelog#v4-3-19-beta)、[4.3 paste](https://esotericsoftware.com/spine-changelog#v4-3-70-beta)。

#### G2.3 Pose：直接摆姿势的逐步使用

1. 选骨或选一条骨链，切 Pose。
2. 在骨原点出现移动符号后拖动，仅移动被拖的那根骨。
3. 拖骨身／末端目标柄，会旋转单骨；选择链时临时用 IK 把末端移向鼠标。
4. Animate 中对实际被改变的骨骼属性打帧；Pose 没有自动创建永久 IK constraint。

**原创例子：** 给八节触手摆一个卷曲姿态，先框选八节，再拖末端；第二段姿态仍可手调中间节。需要角色在游戏中持续追随一个目标，则建立 Path 或正式约束，不能仅凭一次 Pose 操作得到游戏端求解器。Pose 空白点击取消选择，空白拖动框选，与普通 Transform 工具空白拖动不同。依据：[Pose](https://esotericsoftware.com/spine-tools#Pose-tool)。

#### G2.4 Create 与骨长

Setup 选父骨→Create→点击生成零长骨，拖动生成有长度骨；新骨自动被选中，按 Shift 创建可保留父骨选择，连续建兄弟。已有骨放错位置时，选骨后 Option／Alt 点击／拖动重建原点和骨长，对后代／附件作补偿。

**带图快速装配：** 先选父骨，按 Cmd／Ctrl 点击需要归属的附件，松开再创建骨；相关槽移到新骨，名称取第一张附件的槽名。先导入图片再走这个流程比逐槽设置父级方便。通过 Tree→New→Bone 则在父骨末端建立新骨。

Rotate／Translate／Scale 的 Setup 模式下，拖所选骨末端调整骨长；Option／Alt 同时移动子骨。改 Length 不能替代把子骨原点移动到新的关节处。依据：[Create](https://esotericsoftware.com/spine-tools#Create-tool)、[Creating bones](https://esotericsoftware.com/spine-bones#Creating-bones)。

#### G2.5 Compensation：保持附件或后代位置

| 开关 | 实际做法 | 动画边界 |
| --- | --- | --- |
| Bones | 改父骨时反向调整子骨，使该时刻视觉位置保留 | 子骨也需要关键帧；端点保留不保证两键之间轨迹不变 |
| Attachments／旧名 Images | 反向调整附件／顶点，重定位控制骨但保持素材装配 | Region setup transform 无动画键；Mesh 修改需要 deform keys。非均匀缩放可能无法精确补偿 |

**原创例子：** 上臂原点偏离肩关节，Setup 开附件与骨骼补偿，再将上臂原点挪回肩处，检查手和袖子是否仍重合，最后关闭补偿。开关一直开着会让后续“正常移动肩部”看起来像骨架失效。橙色提示意味着正在补偿，不是骨骼被锁定。依据：[Compensation](https://esotericsoftware.com/spine-tools#Compensation)。

#### G2.6 复制粘贴的上下文

| 内容 | 条件与结果 |
| --- | --- |
| 骨骼变换 | 同时存 local/world；按选择轴粘贴整套 rotate/translate/scale/shear。匹配骨架层级用于把一个肢体姿势应用到另一个 |
| 附件变换 | 粘贴 setup 的位置、角、倍率；Tree 把附件放到已有附件上也能取其变换 |
| 顶点位置 | Mesh／Path／Bounding／Clipping 均支持；源和目标选中顶点数相同，选择顺序决定对应 |
| 权重 | Weights 上下文复制分配；不是复制几何形状，具体见 G5 |
| 同类约束 | 选约束或让属性区取得焦点再复制，粘贴到同一类型，可一次多个 |

4.3 允许 Parent 粘贴（行为与 Local 一致）；Constrained 粘贴使用约束后值。复杂情形优先明确焦点再操作，避免将“复制对象”误当“复制变换”。依据：[Copy/paste](https://esotericsoftware.com/spine-tools#Copy-paste)、[约束复制](https://esotericsoftware.com/spine-constraints#Copy-paste)、[4.3 paste](https://esotericsoftware.com/spine-changelog#v4-3-70-beta)。

<a id="rigging-g3"></a>

### G3. 附件类型的参数与几何操作

#### G3.1 所有附件的公共属性

Select 只管理视口可选；Name 只强制显示标签；Color 对 Region／Mesh 为 setup 染色，对其他几何只为编辑器辨识；Set Parent（P 或拖放）改所属槽／骨。Export 关闭会同时排除数据、成片与引用该附件的键；源网格不导出，其 linked meshes 也失去输出依据。

显示附件是 Slot 的属性：点击本槽某个附件让它成为当前附件，动画应对 Slot 打键。重命名、Name 显示标签、图片路径是三个独立概念。依据：[Attachments](https://esotericsoftware.com/spine-attachments)。

#### G3.2 Region：创建、图片查找、Mesh 转换、序列、像素中心

Images 节点拖入、PSD 导入或脚本数据导入可创建 Region。它以图片中心为 position；奇数宽高图严格对齐像素边界时可能需要 0.5 偏移。Image path 空白用附件身份名查图，设置后用路径查图；不带图片编号的序列路径前缀配合 Sequence 编号范围和 Frame 取图。

Mesh 复选框转为四角网格；关闭 Mesh 转回 Region。Region 自己的位置／角／倍率不能做 transform timeline，需所属骨运动；Sequence 则能打帧。序列图片应等宽等高，否则装配和 UV 不能按一套尺寸复用。

**原创示例：** 一个电火花附件使用 `fx/spark`，序列实际为 `spark01.png`…`spark08.png`；spark 附件名可另取 `muzzle-flash`。帧范围由实际文件编号决定，不要把附件重命名成 `spark01` 后再重复增加编号。依据：[Regions](https://esotericsoftware.com/spine-regions)。

#### G3.3 Bounding Box 与 Clipping：多边形编辑统一操作

选骨／槽→New→Bounding Box 或 Clipping→自动进入 New 画轮廓→依次点击顶点→点击首顶点闭合／再次 New 结束。Edit 按钮重新进入；Create 在边上插点，拖顶点移动，Delete 删点；Cmd／Ctrl 多选与框选。Esc／空格退出。

外围 Freeze 将显示用旋转归 0、倍率归 1，保留实际顶点。整体变换或选择顶点变换可拖 pivot 到顶点吸附；Setup 改基础形状，Animate 改 deform keys。deform 记录顶点位置，插值为直线；“用旋转工具摆两端”不会自动产生圆弧顶点轨迹。

Bounding Box 是任意多边形，不一定矩形；运行时可将世界顶点用于命中、触发或物理体生成，但碰撞检测规则、伤害与物理响应由应用实现。只在攻击有效期显示该附件并打 Slot key，能让同一动作启停命中区域。依据：[Bounding boxes](https://esotericsoftware.com/spine-bounding-boxes)。

#### G3.4 Clipping 专有设置与限制

| 设置 | 结果 |
| --- | --- |
| End slot | 点击铅笔选择截止槽，截止槽自身也被裁剪；裁剪的开始来自 Clipping 所在槽，范围依 draw order |
| 默认 End slot 指向自身 | 通常表示裁剪到 draw order 尾端，需检查红色高亮受裁范围，不应认为它只裁自己 |
| Inverse【4.3】 | 保留轮廓外部，挖掉内部；适合揭穿／缺口 |
| Convex hull【4.3】 | 用凸包表达轮廓提高处理效率；凹处被填平，改变结果，要检查画面 |
| Slot attachment key | 显示／隐藏 Clipping，启停作用 |
| Draw order key | 改哪些槽位于裁剪区间；不是直接动画 End slot |

自交轮廓不可用；多个传统裁剪区间应避免重叠，不能假定无限嵌套。优先少顶点、少被裁几何、短起止区间；至少三个顶点。裁剪 polygon 的画面面积小不代表 CPU 少，成本主要与几何有关。实心裁边可用 MSAA 改善，目标 renderer 需另外设置。依据：[Clipping](https://esotericsoftware.com/spine-clipping)、[4.3 clipping](https://esotericsoftware.com/blog/Spine-4.3-released)。

#### G3.5 Path：节点、手柄、闭合、匀速、方向

| 字段／动作 | 效果 |
| --- | --- |
| Knot / Handle | Knot 在曲线上；两侧 Handle 决定两段贝塞尔弯曲。绑定时通常同一 knot 和两柄给相同权重 |
| Length | 显示几何总长；通过移动顶点改变，不是直接输入的拉伸尺度 |
| Closed | 首尾接通；关闭时两端有延长方向虚线 |
| Constant speed | 更精确按弧长取点，让 Path constraint 的均匀进度更接近匀速。不开时长短不对称手柄、变形可能造成变速 |
| Reverse | 反转节点遍历方向，约束沿路径的方向同时反转 |
| Edit Path / New | New 重画节点，拖着创建可同时摆手柄；Create 在两 knot 间插新 knot |
| Freeze | 归零便利旋转／缩放显示值，保留实际点 |
| Shift 拖 Handle | 保持角度，只变到 knot 的距离 |
| Option／Alt 拖 Handle | 不带另一侧手柄，形成尖点 cusp |

Path 不渲染成线条；让骨链上的图片排列才成为可见绳子。超出开放路径 0—100% 的 Position 沿端点切线延长，而不是自动循环；Closed 才给闭合路线。Setup 修改基形，Animate 顶点变形要打 deform，绑定／软选择也适用。依据：[Paths](https://esotericsoftware.com/spine-paths)。

#### G3.6 Point：带朝向的可替换锚点

选骨／槽→New→Point，再摆位置和方向；多个 Point 可在同槽切换，也可由不同皮肤提供枪口偏移。自身位置和旋转不可打 transform keys，运动交由骨；运行时计算 world position／rotation 后生成粒子或投射物。

**原创示例：** `rifle` 皮肤给 `muzzle` 占位符配置长枪枪口 Point，`pistol` 给同名占位符配置短枪枪口；游戏查同一个逻辑点，发射位置随装备变化。Point 比为了静态锚点再增骨更轻，但独立动画需要额外骨。依据：[Points](https://esotericsoftware.com/spine-points)。

<a id="rigging-g4"></a>

### G4. Mesh 拓扑、UV、链接与软选择

#### G4.1 编辑模式中的每项动作

Mesh 是 Professional 功能。Region 勾选 Mesh 后生成四角顶点；取消 Mesh 回到 Region。Image path 为空按附件名查图，非空按 path 查图；Sequence 沿用序列附件配置。公共 Select／Export／Name／Color／Set Parent 见 G3.1。

| 项目 | 使用与副作用 |
| --- | --- |
| Edit Mesh | 进入拓扑／UV 编辑；再次按、关对话框、Esc／空格退出 |
| Modify | 移点；双击删点。移 UV 与移实际形状由 Deformed 决定 |
| Create | 点击增点；拖动在顶点间增手工边；Shift 关闭顶点吸附，建边时 Shift 可水平／垂直对齐；双击删点 |
| Delete | 点删点／边；多选能一次删。改顶点数量会影响绑定、deform 和 linked 共享数据 |
| New | 清空顶点，重新画 hull；可拖点／双击删点，点首点闭合或再按 New 结束。先检查要保留的动画与精调权重 |
| Reset（Edit 内） | 重建为图片四角；原精调权重与 deform keys 不能当作保留。4.3 保留绑定骨并在退出编辑后重新 Auto |
| Generate | 不移动已有点，补内部点；可重复增加密度，四角起点形成网格布局。原精调权重与 deform 失效；4.3 保留绑定骨，退出编辑后重新 Auto |
| Trace | 根据透明度重建外轮廓；原精调权重与 deform 不能当作保留。4.3 保留绑定骨并重新 Auto，支持多网格批量 |
| Triangles | 灰虚线显示自动三角化；不是自动给模型增加顶点 |
| Dim / Isolate | 暗图方便看线／只显当前附件；这是工作显示，不是 runtime 状态 |
| Deformed | 勾选时改顶点同时改实际顶点与 UV；不勾选时看到原图，移动只改 UV，实际形状保留 |
| Wireframe | 未选网格也显示线框，适合按顶点规划骨骼位置 |
| Freeze（属性区） | 当前形状保留，将便利显示 rotation=0、scale=1 |
| Reset（属性区） | 实际顶点重新匹配 UV，解除变形，可能移位；涉及重建权重与移除 deform。4.3 保留绑定骨并重新 Auto；不是只重置工具角度 |

右击可在 Modify／Create／Delete 间切换。Deformed 开启时移点受图片边界限制，接近边界有灰线；Outline 可同时观察整架姿态。

4.3.62-beta 改为移除权重数据时保留骨绑定并重新 Auto，4.3.01 把 Edit Mesh 的 Auto 延迟到退出编辑；这不保证保留原手工比例或 deform keys，也不适用于用户主动 Remove 解除骨绑定。依据：[4.3.62-beta](https://esotericsoftware.com/spine-changelog#v4-3-62-beta)、[4.3.01](https://esotericsoftware.com/spine-changelog#v4-3-01)。

Mesh 显示的平移来自 hull 顶点中心。Mesh 整体并没有独立可动画的附件旋转／缩放通道。操作旋转／缩放本质移动顶点，deform 只保存最终位置；两键之间顶点走直线，时间曲线只改沿线速度，不自动改成 pivot 圆弧。Pivot 可以拖到其他顶点作为编辑中心，不改变骨骼原点。依据：[Meshes](https://esotericsoftware.com/spine-meshes)。

#### G4.2 Trace 的参数、Refresh 与 Refine

| 参数／动作 | 调整方向 |
| --- | --- |
| Detail | 提高轮廓取点数量；先满足形状，再限制几何开销 |
| Concavity | 提高凹处取点优先级，例如腋下与衣摆缺口 |
| Refinement | 花更多优化时间改善低点数与凹轮廓拟合；不等于增加点数 |
| Alpha threshold | 忽略低于阈值的透明像素；过高会切掉半透明边缘 |
| Padding | 在内容边界外预留间距；减少切掉柔边的风险 |
| Uniform【4.3】 | 改善顶点均匀度；关节变形时比只有尖角聚点更稳定 |
| Refresh | 按当前参数重算轮廓，可以比较不同拟合 |
| Refine | 细化当前轮廓；可先手工加点，再让它调整局部配置。4.3 Uniform=0 时保留共线点，方便人工加密指定区 |

**原创流程：** 袖子轮廓已经很好，但手肘附近没有足够控制点。先给 Trace 选择适度 Detail，查看是否切掉抗锯齿边；在手肘段手工加点，再 Refine；接受轮廓后布置内部边，最后 Bind。已有绑定时重新 Trace 后应重新检查并精调权重，复验所有 deform 动画；绑定骨仍在不表示动画数据都保留。

旧页没有 Uniform 与 Refine 详解；精确算法与全滑块硬范围未公开。本文不把“Uniform 最大一定生成等距点”作为保证。依据：[Trace 基本参数](https://esotericsoftware.com/spine-meshes#Trace)、[4.3.61-beta](https://esotericsoftware.com/spine-changelog#v4-3-61-beta)、[官方人员对 Refine 的说明](https://esotericsoftware.com/forum/d/29982-%E5%85%B3%E4%BA%8E43%E7%9A%84%E7%89%B9%E6%80%A7)。

#### G4.3 边线、轮廓、洞、顶点分布与变形

Hull 为外边界；内部灰虚线由 Spine 自动生成；橙色手工边限制自动三角化，自动边不会穿过它。重复覆盖已有自动边通常不改变三角化，但可以帮助整条边选择。Shift 选边连续选同线相邻边，Cmd／Ctrl+Shift 追加。

**原创例子：** 面部鼻尖运动牵动脸颊，是因为相同三角形跨越鼻翼与脸颊。先在鼻翼下方增加隔离点，再给鼻根加手工边，使鼻子相关三角形局限于鼻部；单纯提高整个网格点数不保证解决问题。

轮廓可凹但不能拓扑带洞；小孔用贴图透明区，大孔可拆两附件。外轮廓切掉不透明像素会有锯齿，需要 MSAA；尽量保留柔边，避免把透明过渡裁成硬轮廓。顶点越多、每点骨影响越多，CPU 越重；100 点各两骨大约需要 200 次顶点骨变换，而不是 100。优先用骨权重做主要动作，用少量 deform 修形。

修改图片尺寸时，若只加画布留白，选择重调 UV 保留内容尺度；若图案本身放大则可选择 No，不修改归一化 UV 数值，让它随图尺寸变化。4.3 的 Check All 可以批量选此响应。留白不对称还应手工平移 UV。依据：[Vertex placement / Image resize](https://esotericsoftware.com/spine-meshes#Vertex-placement)、[4.3 Check All](https://esotericsoftware.com/spine-changelog#v4-3-71-beta)。

#### G4.4 Linked Mesh：共享清单与替换图像

New→Linked Mesh 创建共享附件；它共享源的顶点、边、UV、权重。重新取 Name／Image path 可以换贴图；Inherit timelines 决定继承源的 deform／适用 sequence 时间轴，关闭则能单独打相应键。共享顶点带环提示，因此改源拓扑会改所有链接者。当前 runtime 也共享三角索引、hull 长度及源图宽高；Name／Path／Color 可独立。依据：[4.3 MeshAttachment 实现](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/attachments/MeshAttachment.ts)。

4.3 可以处于不同 Slot；旧页“必须同槽、移动就一起移动”的限制已过时。跨槽复用要同时检查绑定骨、Slot 色、层级和 Skin 依赖，它们没有因为共享几何就变成相同。源网格不导出会影响链接者，制作“参考源”不能随意关 Export。需要独立拓扑时建立独立普通网格副本，并确认没有源网格链接；程序侧已有 linked 对象的 `copy()` 仍会创建 linked mesh，不能当作深拷贝。

**原创例子：** 火焰身体和亮边使用相同形状和骨权重，但两槽分别 Additive 与 Normal，共享一个源网格；这样动画同步，亮边仍可在不同层级。源图和亮边图需要相容的尺寸／UV。依据：[Linked meshes](https://esotericsoftware.com/spine-meshes#Linked-meshes)、[4.3 cross-slot](https://esotericsoftware.com/blog/Spine-4.3-released)。

#### G4.5 Mesh Tools 软选择

Size 设周边点影响半径；Feather 定义衰减；0 表示范围内全部作用，增大后边缘逐渐弱。明确选中点青色、未选中点灰色、软选择点由青到深蓝表示影响程度。作用于 Rotate／Translate／Scale 和权重修改，兼容 Mesh、Path、Bounding、Clipping。

关闭 Hull vertices 时，Mesh 外轮廓不会被软选择带走；明确选中的边界点仍须按操作结果检查。适合锁住头型，只改变脸内部五官的转面效果。软选择不是增加新顶点，也不是拓扑平滑。4.3 有独立 Soft Selection 开关动作；Size、Feather 的个人工作值没有统一适合所有图片的“正确默认”。依据：[Mesh Tools](https://esotericsoftware.com/spine-mesh-tools)。

#### G4.6 点动画、UV 与 linked mesh 的数据层边界

##### G4.6.1 顶点旋转不产生旋转式的 deform 插值

Deform 只存位置的实际后果是：两组点位置之间的空间路径是直线。曲线可以改变沿这条线前进的速度，但不会自动把它改成绕某 pivot 的圆弧。[Transform tools](https://esotericsoftware.com/spine-meshes#Transform-tools)

**原创算例：** 一个顶点在某中心右侧 10 单位，第一键是 `(10, 0)`，第二键经工具旋转 90°得到 `(0, 10)`。线性时间中点为 `(5, 5)`，到中心距离约 7.07，而不是半径 10 的 `(7.07, 7.07)`。若必须保持刚体弧线，用骨骼旋转带动网格；若是局部软形变，可加中间姿态控制轮廓。给时间曲线加缓入缓出，不会修复这两种空间路径的区别。

##### G4.6.2 当前 runtime 对“权重、deform、UV”分别存储

当前 4.3 `VertexAttachment`：无权重顶点是二维坐标；有权重时每个影响存骨局部二维坐标和权重，并用骨索引结构标识影响。求世界坐标时，未加权 deform 提供顶点位置，加权 deform 加到各影响的局部坐标，再经骨姿态变换并按比例求和。`MeshAttachment` 独立持有 `regionUVs` 和 `triangles`；runtime 使用导出的三角索引，不在每帧重新三角化。[Attachment.ts](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/attachments/Attachment.ts)、[MeshAttachment.ts](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/attachments/MeshAttachment.ts)

这解释了三个原创诊断：外形正确但图案拉错，先查 UV；极端转动才出现断层，先查三角连接和权重；只在某个动画出现折痕，先查 deform 键及骨姿态。不要把三个问题都用一次 Trace 处理，否则会把原本正确的几何和权重一起重建。

##### G4.6.3 Linked 的共享范围比“复制一张图”更精确

当前 `setSourceMesh()` 共享骨索引、顶点数据、regionUVs、三角索引、hull 长度、边线及原图宽高；Name、Path、Color 可独立。序列对象会复制，timeline 继承是另一层关系。三角索引也是共享，因此改手工边会改变所有链接者的变形连接。[当前 MeshAttachment 实现](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/attachments/MeshAttachment.ts)

Runtime `copy()` 在对象已经是 linked mesh 时返回新的 linked mesh；因此“普通 Duplicate 一定解除链接”不能当作 runtime API 保证。编辑器操作应按以下方式理解：“需要独立拓扑时，应建立独立的普通网格副本，并确认没有源网格链接；程序侧不要把 linked 对象的 copy() 当作深拷贝。”本文没有通过编辑器修改工程来核实各复制入口的具体表现，不能据此断言编辑器所有 Duplicate 都保持链接。

**原创验证：** 对一组共用几何的火焰主体／亮边，先检查两者是否都有环状顶点提示，再检查它们的 Slot 显示顺序和 blend。只改变亮边 Path 时，主体颜色不应因此改动；修改源边线时，两者应共用新的三角连接。动画继承和骨绑定也应分别检查，不能从“图案能换”推出“所有状态都独立”。

#### G4.7 布点、裁边与图片尺寸变动的具体选择

##### G4.7.1 Generate 与手工补点的工作选择

布点时先决定哪片需要动，再决定点数：直条、可弯的长物、需要转面的脸，要求的控制区不同。把点放在折线、结构分界与需隔离的图案附近，比仅提高全图密度更可控。手工局部补点与 Generate / Trace 批量重建不能统一当作同一种数据清理操作。[官方布点建议](https://esotericsoftware.com/blog/Mesh-creation-tips-vertex-placement)、[官方网格分类](https://esotericsoftware.com/blog/A-taxonomy-of-meshes)

**原创方案：** 一条披带只沿长度弯曲，先沿长度安排若干控制截面；末端徽章必须保持近似刚性，则在徽章边界留下独立控制区。若需要徽章翻转出假深度，再为该局部增加能够改变宽度的点。全带均匀加密到同样高密度会增加成本，却没有定义徽章与布料怎样分离。

##### G4.7.2 裁掉透明区涉及两个不同成本

Hull 外的像素不画，减少透明区绘制；紧贴 hull 的图集多边形打包还可减少图集占用。减少点与每点骨影响，减少顶点求值；两项优化不能用同一个数字代替。当前 runtime 可直接看到逐影响求世界坐标的循环，因此查看 Metrics 时，应同时看几何变换量和贴图／填充情况。[Attachment.ts 逐影响计算](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/attachments/Attachment.ts)、[Hull size](https://esotericsoftware.com/spine-meshes#Hull-size)

**原创成本算例：** 80 点各受三骨影响，共 240 个加权贡献；若细节允许 50 点保持三骨、30 点改为两骨，则是 210 个贡献。这只比较顶点贡献次数，不保证帧耗时正好降 12.5%；绘制批次、CPU／GPU、骨架数量等仍会影响整体耗时。

##### G4.7.3 图片扩大：留白与内容扩大分开判断

真正扩大内容时选择 No 的语义是：不修改网格 UV 数值，归一化坐标跟随新图尺寸。Yes 用于调整采样坐标以保留内容原尺度；不能写成所有高清替图都要 Yes。[Image resize](https://esotericsoftware.com/spine-meshes#Image-resize)

**原创检查表：** 替图前截取一个纹理标志点的位置；换图后分别看标志点、网格轮廓和边缘透明过渡。只增加左侧空白时，保持原内容尺度后还需确认横向采样是否偏移；所有内容放大两倍时，先明确项目是否希望角色也变大，再决定是否重调 UV。Check All 只批量执行选择，不替用户判断两类图片是否具有相同意图。

#### G4.8 UV 与路径的原创操作比较

**原创场景：** 三套旗面叫 `flag-red`、`flag-blue`、`flag-gold`，但项目里图像文件名字不同。分别填写 Image path，比为了匹配文件名重命名业务附件更便于维护。Linked Mesh 共用几何不会强迫三套图像具有同一资源路径。

**原创比较：** 袖口几何已经摆到手腕，但图上的金边落错了位置：改 UV 可以保留手腕处几何，让同一顶点采样金边；若目标是把袖口几何实际延长，应改几何位置。两种操作可能产生不同的外观，却解决不同的问题。

<a id="rigging-g5"></a>

### G5. Weights：全部模式、操作与诊断

#### G5.1 Bind、骨列表与显示

先选附件→Bind→点骨→Bind／Space／Esc 结束；也可先多选骨→Bind→点附件。首次自动计算权重。4.3 支持多附件批量，公用骨颜色一致，列表显示共同骨；并非源／linked 全选就建立两套独立拓扑。

骨列表选择决定编辑哪根骨；选择一个顶点可读各列比例，多点不同值用 `*` 表示。Remove 解除所选骨绑定；移除全部后回到所属槽骨的单骨作用。Pies 看单点分配，Overlay 看面内渐变，Selected 隐去无关饼图、以黑色显示其他骨影响。右击骨名跳到该骨。

Triangle order 通过列表骨顺序控制同一网格自重叠：三角形按其顶点主要权重骨归组，再参考骨列表排序。它不改变插槽顺序；若各角都有相近双骨权重，移动列表可能只改变部分三角形。Bone influence limit【4.3】用问题提示暴露过多影响，不是自动替你修剪所有点。依据：[Weights](https://esotericsoftware.com/spine-weights)、[4.3.92—94-beta](https://esotericsoftware.com/spine-changelog#v4-3-92-beta)、[官方 4.3 三角顺序说明](https://esotericsoftware.com/forum/d/30328-%E6%9D%83%E9%87%8D%E6%8E%92%E5%BA%8Fbug)。

#### G5.2 Direct 与三种 Brush

| 模式／字段 | 行为 |
| --- | --- |
| Direct | 选顶点、选骨，上下拖改变比例；Weight 可精确输入 |
| Add | 给所选骨增加；一般比先减旧骨更容易掌控归一化 |
| Remove | 减所选骨；余量分给其余骨 |
| Replace | 设为笔刷目标比例，适合固定某一区到指定值 |
| Strength | 本次最大施加比例，不是物理刚度 |
| Size | 笔刷范围 |
| Feather | 边缘从最大力度衰减到 0 的区域比例 |
| Option／Alt | 4.3 Add/Remove 绘制时临时互换增减方向 |

Direct：无点选择时拖框；Ctrl／Cmd 追加点或框选，Ctrl／Cmd+A 全选，Space／Esc／空白处取消。

不选顶点时笔刷能作用全附件；选了顶点则只作用所选点。每点权重总和归 100%；给某骨加，其他可改骨减少。一个点只有单骨 100% 时不能靠减它凭空创建第二个影响，先绑定另一骨。Lock 锁住列后，归一化不能随意挪动其占比，剩余空间由未锁骨分配。

**原创示例：** 手肘点先有上臂 70%、前臂 30%。Direct 给前臂设 45%，上臂变 55%；袖口点则前臂 100%。保持这两类点分别选择，避免强 Replace 把关节过渡刷成整片刚体。依据：[Weights modes](https://esotericsoftware.com/spine-weights#Mode)、[4.3 Alt brush](https://esotericsoftware.com/spine-changelog#v4-3-67-beta)。

#### G5.3 Auto、Smooth、Prune、Weld、Swap、Lock、Update

| 动作 | 条件、效果与复验 |
| --- | --- |
| Auto | 看几何拓扑计算；不是只按骨距离。选点只改局部；选骨可限制参与计算 |
| Smooth | 与 hull 边／手工内边连接的相邻点平均。无选点则整片；重复按增加传播，同时可能增加微小影响 |
| Prune | Bones 限每点最多影响数；Threshold 去小值；余量重分配。实时预览数量及问题状态后接受 |
| Weld All | 将另一附件权重转移到目标全部适用点，使分层部件同样变形；不是将顶点实体合并 |
| Weld Overlapping | 聚焦重叠部分，适合接缝。目标／源须至少共享两根绑定骨；没有共同骨或重叠会提示 |
| Swap | 选择骨列表两根骨，互换它们的权重分配；不等于把骨身份换成任何第三根骨 |
| Lock | 点骨颜色方块上的锁；仅影响本附件该骨列，并非全骨架锁定 |
| Update bindings | 以当前顶点和骨姿态重新存绑定坐标；保留要观察的当前形状，之后刷比例不再因旧绑定位置漂移 |
| Copy / Paste | 匹配数量选点，复制比例；匹配骨身份／映射由实际粘贴提示确认。不是跨任意点数一键映射 |

Weld 可用于 Path／Bounding／Clipping，源可选 Mesh／Clipping；4.3 可批量多附件。公开页只给 All／Overlapping 名称，未给完整距离阈值算法，所以不写“最近点相差多少以内必焊”的虚构范围。改 Prune 数值看起来更轻，但须测试最大弯折，不能只检查 setup。

**Update 的两个原创场景：** 尾巴原图是直条、绑定后 Setup 故意卷起，此时允许刷权重改变 Setup 曲线，可以保留旧绑定；胳膊已经摆成精确肩姿势且要固定画面位置时，先 Update，再刷手肘，不让比例编辑把现有装配拉散。

复制整支骨时，子骨、槽和附件一起复制，绑定会尽量指向对应复制骨；如果还有跨支权重或外部控制骨，则仍有外部依赖。切换其他工具时保留 Weights 的顶点选择，返回可继续编辑。测试权重最好建专用动作包含最大角度和伸缩，在 Preview 循环，同时刷；Auto Key 关着只试姿势不打键，点时间轴可回到键／setup 结果。依据：[Weights operations](https://esotericsoftware.com/spine-weights#Auto)、[4.3 batch weights](https://esotericsoftware.com/blog/Spine-4.3-released)。

#### G5.4 权重作用范围、平滑与软选择排错

##### G5.4.1 把选择状态写进操作步骤

Direct 常用选点：没有点选择时拖框；Ctrl／Cmd 追加点或框选；Ctrl／Cmd+A 全选；Space／Esc／空白处取消。切换变换工具时会保留 Weights 的选择，返回后不用重选。实际批处理前需同时检查“选了哪些附件、哪些点、哪些骨”，三个维度会改变作用范围。[Direct / Testing weights](https://esotericsoftware.com/spine-weights#Direct)

**原创排错：** 同时选择袖子和手臂后，只想改袖口一圈，先确认点选掩码，再看公用骨列表；如果还停在整片点选择，Replace 会作用过大。若 Auto 没有给肩骨权重，先看该骨是否被选骨范围排除，再决定是否再次全量 Auto。

##### G5.4.2 Smooth 依赖明确的相邻边

Smooth 依据 hull 边和手工内边；不能把所有灰虚线三角邻点都当然视为 Smooth 的邻点。权重平滑与增加控制边存在关系：构造需要传播的手工连接后，再 Smooth，结果才应按这个邻域理解。公开材料没有给自动权重算法和 Weld 的距离阈值，不能杜撰按最近点严格对应的保证。[Smooth](https://esotericsoftware.com/spine-weights#Smooth)

**原创工作顺序：** 将披带端点控制为主骨占优，中间区域混合；先测试最大弯折，发现过渡突变才平滑。平滑后再 Prune 并复验边缘，不先规定一个适合所有资产的 Threshold。这样的顺序能区分“需要过渡”与“多出来的极小影响”两个问题。[权重工作流](https://esotericsoftware.com/blog/Mesh-weight-workflows)

##### G5.4.3 软选择颜色与半径不等于完整锁定

软选择色码是“明确选中点青色、未选中点灰色、受软选择影响点由青到深蓝表示程度”。Hull vertices 关闭只说明 hull 不受软选择传播影响，不应扩写成 hull 被全局锁住。Feather=0 时范围内全量作用；完整落差函数和全部范围未公开，因此不写固定的内圈半径公式。[Mesh Tools](https://esotericsoftware.com/spine-mesh-tools)

**原创操作例：** 做脸的局部转面，明确选眼鼻内部点，Size 扩到过渡区，关闭 Hull vertices，先小幅平移。若头型仍变，检查是否直接选了边界点、选中了整个附件，或改到了共用网格数据；它们不属于“软选择传播到 hull”这一种原因。

<a id="rigging-g6"></a>

### G6. Skin：占位符、依赖、复制与组合

#### G6.1 建立与管理

Skins→New→Skin；活动皮肤仅一个且一定可见，pinned 可以多个。选骨／槽／附件→New→Skin Placeholder 创建逻辑部件名；选多附件可以批量建，现有附件移入活动皮肤。占位名如 `weapon`，皮肤名如 `sword`；动画对 weapon 打附件键，而不是绑定具体剑图。

每皮肤每占位最多一个附件或空。拖到 placeholder 会替换旧附件，旧附件删除；拖到已有附件会额外复制旧变换。Export 排除皮肤及其附件；Color 只区分 Tree；Folders 进入导出名。Add to Skin 模式连续选骨／约束，Esc 退出；已有成员可在皮肤下 Remove，避免误删骨架对象。

**批量首次转皮肤：** Tree 只留附件→建并激活皮肤→多选附件→New Placeholder。重复附件对各皮肤可选 Duplicate attachments、Linked meshes、Inherit deform、Duplicate keys、Rename attachments。最后一个按皮肤目录加前缀查图；它不自动生成缺少的图片。依据：[Skins setup](https://esotericsoftware.com/spine-skins#Setup)。

#### G6.2 皮肤骨、约束与警告

| 警告／依赖 | 原因与修复 |
| --- | --- |
| Bone warning | 子骨不在皮肤父骨所在的必要皮肤里；父不活动，后代世界变换无有效依据。把所需后代纳入相容皮肤 |
| Attachment warning | 权重引用了本皮肤未活动骨或已有警告骨；可能出现顶点塌到原点。修骨层级，再补附件依赖骨 |
| Constraint warning | 源／目标骨属于皮肤，但约束未包含相应依赖；或所有受约束骨都为皮肤骨而约束不在相关皮肤 |
| 部分骨不活动 | 约束可只作用当前活动的部分骨；所有骨都不活动时通常没有事情可做 |
| Show inactive skin bones【4.3】 | 编辑诊断用，显示非当前皮肤控制骨；会掩盖本应缺依赖的装配表现，应关闭后再检查交付效果 |

骨／约束能属于多个皮肤，不活动的对象不参与相应 runtime 更新。4.3 会补必要骨和约束并给提示，自动补全后仍要看成员清单；不是关闭警告即可保证游戏换装正确。Essential 4.3 开放 IK 以及 skin bones／constraints，不能沿用旧页排除它们的说法；其他高级约束仍依专业版能力。依据：[Skin warnings](https://esotericsoftware.com/spine-skins#Warnings)、[4.3 Essential](https://esotericsoftware.com/spine-changelog#v4-3-78-beta)。

#### G6.3 复制、相似皮肤、程序皮肤、删除占位与 Mix and Match

Duplicate skin 的 Linked meshes 节省拓扑复制，Inherit deform 控制共享键；Duplicate keys 给普通复制附件自己的键；Rename attachments 按皮肤前缀查图。尺寸、内容位置与命名规则一致时，Find/Replace 能迅速切换素材目录；不一致时仍需装配调整。

**原创组合：** `body/base`、`hair/long`、`shirt/red`、`weapon/bow` 分为四皮肤。长发皮肤同时拥有长发骨与物理约束；关掉长发时这部分骨和约束不更新。组合共享同名 placeholder 时注意覆盖优先，不能随意将两套完整皮肤全 pin 后期待所有槽都有唯一来源。

大量仅图片名不同的皮肤可由程序生成，而编辑器保留一套模板；需要与程序约定命名和 atlas region。Removing placeholders 时 Delete→Keep current attachment 仅把活动皮肤附件保留到普通槽，其他皮肤附件删除，所以不是“把所有皮肤都转普通附件”。4.3 支持拖放合并皮肤，Only pinned skins 过滤只是显示过滤，导出和依赖仍需分别管理。依据：[Skin workflows](https://esotericsoftware.com/spine-skins#Skin-workflows)、[4.3 skin improvements](https://esotericsoftware.com/blog/Spine-4.3-released)。

<a id="rigging-t1"></a>

### T1. 全约束共有操作与计算顺序

#### T1.1 建立、定位、分组、复制和影响强度

| 动作／字段 | 用法 |
| --- | --- |
| New Constraint | Setup 先选受控骨，再选类型和 source／target；Slider 也可从 Animation 建立 |
| Constrained bones | 属性中的骨列表可点击选择、铅笔重新指定；4.3 有单项 Remove，批量骨无需全删重选 |
| Target / Source | 点名称定位对象，铅笔改引用；IK/Transform 读骨，Path 读槽，Slider 读动画及可选骨 |
| Order | Constraints 节点自上而下执行；Setup 拖动调整 |
| Reset Order | Constraints 节点 Reset 推算合理顺序；可能改变既有结果，仍要复验 |
| Mix | 0 无影响，100% 全影响；IK／Path 典型 0—100%，Transform／Slider 能超范围，Physics 非负范围 |
| Link sliders | 工具层面联动编辑多个 mix；不代表多个输出属性永久存同一个值 |
| Folders | 组织约束，导出路径组成名称 |
| Annotations | Bone 右侧关系图标定位其约束；0 mix 图标状态帮助判定是否生效 |
| Hollow bone | 视口表示骨被约束；手调被覆盖时改 source 或临时 mix=0 |
| Copy / Paste | 同类型设置可复制到多个约束；不要把属性值粘贴混同于跨类型自动转换 |
| Add to Skin | 只在所需外观启用，同时补齐受控与源目标骨依赖 |

#### T1.2 顺序诊断与 4.3 pose 模型

顺序看数据依赖：先生产控制值，再消费。路径让“脚目标”移动，然后腿 IK 消费该位置；反过来 IK 追的是旧脚目标。后续约束重新计算一个祖先世界变换，可能覆盖之前约束作用；多个约束写同一属性时，需要根据实际顺序、mix 和空间逐项检查。

4.3 区分 setup pose、动画／程序输入的 unconstrained pose、约束后 applied pose。普通 Local 数值与视口形状不一致，可能只是看了不同层。使用 Constrained axes 查看结果、Key Constrained 采样结果；动画主要仍记录可再计算的输入，而 runtime 渲染使用应用后结果。

**原创排错顺序：** 肘部不动→先把 IK mix 拉 0 检查 FK→恢复后移动 target 检查单 IK→逐个停后面的 Transform／Slider→最后检查皮肤活动性和约束 order。不要一次同时改五个参数而失去问题来源。依据：[Constraints](https://esotericsoftware.com/spine-constraints)、[4.3 pose model](https://esotericsoftware.com/blog/Spine-4.3-released)。

<a id="rigging-t2"></a>

### T2. IK：一骨／两骨、伸缩、柔化与体积缩放

#### T2.1 创建步骤和全部属性

一骨选择它；两骨选择直接父子。New→IK Constraint 后点现有目标骨，或空白创建目标；点受控骨可把新目标放在末端，避免创建即跳动。目标不要放在受控链后代，否则输入依赖输出。

| 参数 | 含义、动画与范围说明 |
| --- | --- |
| Parent | 第一受控骨；可铅笔重选 |
| Child | 第二受控骨，一骨时空；X 清除退为一骨 |
| Target | 控制末端要指向／触及的位置；常是 root 下独立控制骨 |
| Positive | 两骨弯折的正向／负向，runtime 为 +1/-1，可打键；不是增加旋转力度 |
| Compress | 一骨目标较近时缩短 X，可打键；两骨不提供通用 compress 求解 |
| Stretch | 目标较远时拉伸受控父骨 X，可打键；两骨还有非均匀缩放／子骨 Y 限制 |
| ScaleY | none 维持 Y；uniform 同比例；volume 反向响应。setup 选择，不应拿旧 Uniform 单复选框描述三模式 |
| Softness | 两骨接近伸直极限开始减速的距离范围，可打键；距离随载入缩放比例变化 |
| Mix | FK 与 IK 插值；编辑器 0—100%，runtime 0—1，可打键 |

#### T2.2 4.3 的边界必须按当前实现阅读

两骨 Child 必须直接隶属 Parent；child.y 在 Stretch 或 Parent 非均匀缩放时会按求解需要置 0。当前 4.3 TypeScript 参考实现两骨入口遇到非 `normal` 继承直接返回；编辑器可能允许设置并报 warning，允许保存不等于该链能正常求解。一骨实现处理若干非正常继承模式，因此不能把两骨限制一概套给一骨。

旧指南和当前 API 文字仍说 Softness>0 就不 Stretch；本次检查的 4.3 求解代码先柔化目标距离，再在均匀缩放分支对仍超范围的目标拉伸，所以两者不能简单用旧一句话保证。实际 rig 需要检查 Softness+Stretch 组合；不希望比例变化时，明确关闭 Stretch。当前两骨非均匀缩放分支不执行 Stretch。Mix 中间值按短角方向混合，跨越方向边界可能突然换旋转方向。

**原创测试动作：** 上臂 80、前臂 60，把目标依次放在近肩、正常伸手、刚超 140、很远四个区域；先 Softness=0/Stretch=off，再逐项开启。Positive 设相反方向后重复。这样能区分“伸直抖动”“拉伸”“翻肘”三类表现。依据：[IK guide](https://esotericsoftware.com/spine-ik-constraints)、[4.3 求解实现](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/IkConstraint.ts)。

#### T2.3 Volume 的通常规则与极端保护

令 s 为约束施加的 X 缩放因子。none 不改 Y；uniform 乘 s；volume 通常除 s，二维近似保持面积。极端压缩有保护：当前 IK 在 s<0.7 时用 `0.25+0.642857*s` 作为分母，避免 s→0 时 Y 无限大。Physics 用不同近似，见 T5。它们是二维挤压拉伸，不能当作真正三维体积守恒。

**原创例子：** s=2 时 volume Y≈0.5，面积因子≈1；s=0.5 时 IK Y≈1.75，而严格倒数是 2。因此教材把“所有范围永远面积相等”写成定律会误导调参。Essential 4.3 可以用 IK，旧功能矩阵已不适用。依据：[ScaleYMode](https://esotericsoftware.com/spine-api-reference#ScaleYMode)、[IK ScaleY code](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/IkConstraint.ts)、[Essential 4.3](https://esotericsoftware.com/spine-changelog#v4-3-78-beta)。

<a id="rigging-t3"></a>

### T3. Path Constraint：排列、间隔、旋转方式与控制柄

#### T3.1 创建和参数

选受控骨，New→Path Constraint，点目标槽／Path。约束保存的是槽引用，所以同槽显示不同 Path 可以切轨道；槽没有当前 Path 时不沿路径求解。

| 字段／模式 | 行为 |
| --- | --- |
| Bones | 选择先后为排列顺序；列表拖动重排，铅笔重选 |
| Target | 目标槽；当前可见 Path 是输入 |
| Position Fixed | 第一个骨沿路径的距离，长度单位 |
| Position Percent | 第一个骨沿整路径的比例 |
| Spacing Length | 以上一骨长度为基本间隔，再加 spacing |
| Spacing Fixed | 各骨使用同一绝对间距 |
| Spacing Percent | 各骨使用总路径长度的同一百分比间距 |
| Spacing Proportional | 各骨长度决定分配比例；编辑器 spacing=100 时铺满全路径 |
| Tangent | 骨 X 指向当前点切线；骨末端不保证落到下一点 |
| Chain | 指向下一取样点并连接前一骨末端；适合刚性履带。非零 rotation offset 会影响链式平移 |
| Chain Scale | 指向下一点并改 X 尺度，让起点／末端都落到路径；适合柔绳 |
| Rotate Offset | 在路径求得方向上加角度；例如让图片长轴原为 Y 的部件正确对齐 |
| Rotate / Translate Mix | 控制旋转及平移强度，可打键；runtime 的 mixX/Y 可独立，界面操作按实际控件 |
| Link sliders | 联动编辑 mixes |
| Position handle | 路径上的控制柄，直接拖进度；Cmd／Ctrl 避免挡住下层 knot／handle |
| Color | 柄与 Tree 图标取第一受控骨颜色 |

#### T3.2 拓扑和出界行为

Chain Scale 优先让受控骨具有相同父级，避免一段缩放再次缩放后一段；指南也提到首骨为其余父的特殊配置，需要按实际形变复验。开放路径 Position<0 或>100%沿端点切线延伸；闭合路径可用于环形移动。Position、Spacing、mix 可以动画；mode、目标槽不是自动随时间切换的同类数值通道。

**原创例子：** 十二片履带先选同级骨，Closed Path→Fixed spacing→Chain；同样十二骨做绳子则 Chain Scale，并确认所有骨兄弟关系。需要全绳慢慢增长时用 Position/Spacing 配合材质／附件显示，不必手工为每节重复平移键。路径曲线本身还可绑定／deform，但 Constant speed 会影响取点稳定性与成本。依据：[Path constraints](https://esotericsoftware.com/spine-path-constraints)、[PathConstraintData](https://esotericsoftware.com/spine-api-reference#PathConstraintData)。

<a id="rigging-t4"></a>

### T4. Transform Constraint：4.3 输入输出映射完整结构

#### T4.1 创建、空间、叠加、Clamp、Offset、Match

选受控骨→New→Transform Constraint→选 source。支持一个源驱动多个骨。旧页 Target 指的是“读取的源骨”，不要误把它当受控骨；4.3 schema 名为 source。source 不能构成循环后代依赖；4.3 支持 source 同时为某个受控目标的情况，但需要理解读取／写入先后。

| 项目 | 4.3 精确定义 |
| --- | --- |
| Bones | 输出骨列表；多个骨使用同一组映射和 mixes |
| Source | 读取变换的骨 |
| Local Source | 勾选读 source 局部，关闭读世界；独立于 Local Target |
| Local Target | 勾选写输出骨局部，关闭写世界；可 local→world 或 world→local |
| Additive／旧 Relative 概念 | 叠加到原始姿态；关闭则混向绝对映射值。缩放叠加按倍率处理，不是简单把数值相加 |
| Clamp | 限映射结果在输出起止区间；输入超界时停止继续增大 |
| Offset rotation/x/y/scaleX/scaleY/shearY | 读取源时额外偏移，保存六项；世界位移偏移会随源轴转换，不能只看成世界固定 X/Y 加法 |
| Match | 按当前未约束姿态设偏移／映射基准；4.3 等同 mixes=0 状态取值，不必真的先拖 mix=0 |
| Mix rotate/x/y/scaleX/scaleY/shearY | 六个输出影响强度，可打帧；允许负值／超 100%，不应统一硬限制到 0—100 |
| Link sliders | 一起编辑影响强度，减少重复拖动 |

#### T4.2 属性映射：六种输入、六种输出、一对多

可选输入和输出类别均为 `rotate`、`x`、`y`、`scaleX`、`scaleY`、`shearY`。一个输入可驱动多个不同类型输出，例如 X 位移同时驱动 rotation 和 scaleY；并没有单独 `shearX` 映射类型。映射与只复制同类型的旧 Transform 不同。

把源范围写作 a→b、输出范围写作 c→d，则比例 k=(d-c)/(b-a)，映射结果约为 c+(源值-a)k；逆向输出 d<c 就反向变化，Clamp 按实际较小／较大端点限制。a=b 是退化范围，不应作为有效线性映射设计，软件的具体拒绝／归一处理未公开。

**当前数据层字段：** 每个 `FromProperty` 有 offset 和 to 列表；每个 `ToProperty` 有 offset、scale、max。源码先读含 Offset 的 source 值，再减 From.offset，乘 To.scale、加 To.offset；Clamp 在施加 mix／additive 前限制此映射值。故在 Additive 或超范围 mix 下，最终骨姿态仍可能超出映射区间，不能把 Clamp 当全局最终角度锁。

#### T4.3 两个原创配置例与边界

**卷轴：** source localX -50→50，输出 rotate -180→180，scaleY 0.8→1.2；target local，非 Additive，mix=100%，Clamp 开。source 为 0 时旋转 0、Y 尺度 1；source 为 80 时映射停在旋转 180、尺度 1.2。主动作只给 source X 打键。

**跟随帽子：** source 世界位置驱动帽子骨世界位置，Match 保存装配偏移；抛帽阶段把 mix 淡到 0，由帽子 FK 动画接手。使用 Local Source 读的是手在父级下的局部数值，并不是手在画面里的位置，父级一转就可能与预期世界跟随不同。

世界旋转是朝向，主要 0—360，跨边界要检查短角混合；世界 scale 通过轴长取值，镜像符号不一定和 local negative scale 完全等价。若要从输入数值的正负驱动翻图，优先明确本地坐标与本地输出，并检查 scale=0 附近的退化变换。

依据：[4.3 Transform 概览](https://esotericsoftware.com/blog/Spine-4.3-released)、[TransformConstraintData](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/TransformConstraintData.ts)、[计算与 Clamp 顺序](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/TransformConstraint.ts)、[Match 当前行为](https://esotericsoftware.com/spine-changelog#v4-3-19-beta)。旧 [Transform guide](https://esotericsoftware.com/spine-transform-constraints) 的单 Local／Relative 仅可作为旧名称背景，不能充当 4.3 参数全集。

<a id="rigging-t5"></a>

### T5. Physics：可模拟自由度、力学参数、预览、Reset 与采样

#### T5.1 Setup 和动画参数

选骨→New→Physics Constraint。一个约束作用一骨；要为同骨旋转与错切设置不同力学响应，可分两个约束并安排顺序。骨长度为 0 时旋转、scaleX、shearX 作用可能无效并警告。

| 字段 | 结果与调参方向 |
| --- | --- |
| Translate X/Y | 各方向接受运动滞后程度；setup 0 不受对应作用、100% 满作用 |
| Rotation | 对朝向产生拖尾与恢复 |
| Shear X | 让 X 轴方向产生错切式滞后 |
| Scale X | 原点移动时末端倾向保留旧位置，从而伸缩 |
| ScaleY mode | none/uniform/volume 定义 X 伸缩引出的 Y 响应 |
| Limit | 限制骨移动传给模拟的最大速度；不是限定最终画面偏移距离 |
| FPS | 模拟步进率；不等于 TimelineFPS或导出FPS。4.3 默认从60改20，但既有工程可保存自己的值 |
| Inertia | 骨动作转为偏移的比例；0 不因骨动作产生惯性拖尾，100% 强滞后。与 Mass 的受力响应不同 |
| Strength | 拉回未受约束姿态的恢复力；更大通常更快回位 |
| Damping | 减少运动能量与振荡；需结合实际实现／界面值调节，不能凭名字假定所有软件方向一样 |
| Mass | 对加速度的抵抗；runtime 存 massInverse；大质量与相同 Strength 通常更慢响应 |
| Wind / Gravity | 持续外力系数；4.3 runtime 还可设 skeleton 风／重力向量，不限死世界 X/Y |
| Global | 按属性勾选；同架相关约束可由共同全局时间轴控制，其他未勾选属性仍各自独立 |
| Mix | 原姿态与模拟偏移强度，非负，允许超过100%的影响；可打帧 |

Inertia、Strength、Damping、Mass、Wind、Gravity、Mix 能打时间轴；Setup 自由度、骨、Limit、FPS、ScaleY 的结构设置与这些动态参数分开。每个实际项目不需要同时开启所有自由度；全零会有“无作用”警告。

#### T5.2 当前 4.3 Volume、Damping 与单位细节

令 s 为物理施加的 X 因子，Physics Volume 先取 abs(s)，abs(s)>=0.7 时 Y 乘 1/abs(s)，更小时乘 `4-3.67347*abs(s)`。强压缩下软化膨胀，和 IK 的分母近似不是同一公式。

当前参考求解会把 Damping 作为速度保留系数幂运算；示例 0.85 与 0.95 并不对应“95% 比85%阻尼更强”的直觉，UI 表达须结合曲线和实际表现。runtime 的 step 由 1/FPS 得到，以秒参与时间运算；API 某处说明写 milliseconds，但不能因此再乘1000。

**原创调参顺序：** 头发先只开 Rotation；固定 FPS，给头部急停测试；用 Strength 决定回位快慢，再用 Damping压振荡；需要更重感时改Mass，并同步复查Strength。最后加Wind，避免把“受持续外力偏转”误诊为“惯性不回位”。依据：[Physics guide](https://esotericsoftware.com/spine-physics-constraints)、[4.3 计算](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/PhysicsConstraint.ts)、[参数载入与单位](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/SkeletonJson.ts)。

#### T5.3 Simulate、Deterministic、Reset、Warm Up

| 控制 | 具体用途与限制 |
| --- | --- |
| Simulate | 编辑器是否持续模拟。关闭并跳时间时按起点到当前时刻重算，以查看正常播放到该帧的状态 |
| Deterministic | Animate 从0重算到所看帧，使来回拖动可重现；只保证这种编辑器观察流程，不能自动保证联网跨平台确定性 |
| Reset | 清当前约束模拟状态；可用 Reset key 表达指定时间重置 |
| Reset All | 重置工程相关全部物理；键可表达全局重置，4.3 行为有改进 |
| Warm Up | 输出循环前预跑若干次，使次级运动先进入预期循环状态 |
| Reference scale | 整架缩放时同步缩放此基准，维持距离驱动角度／缩放的相似比例 |

Deterministic 的帧数越后重算越多，编辑成本大；循环起点重置会导致尾首跳变，需检查是否适合所做循环。导出帧率与模拟率可以不同；某个导出帧对应其时刻的模拟结果，不是每导出一帧只走一次Physics步。

**Reset原创场景：** 角色传送到远处后立即 Reset，避免把传送距离当一股巨大惯性；连续跑动则保留模拟状态，才能跨动画继续摆。骨长无效、全自由度为0、Skin依赖错误、无效数值都应修，不只是把图标隐藏。Physics没有自动身体／墙壁碰撞，不代替完整游戏物理引擎。依据：[Physics preview/reset/warnings](https://esotericsoftware.com/spine-physics-constraints#Simulate)、[4.3 physics](https://esotericsoftware.com/blog/Spine-4.3-released)。

#### T5.4 Key Constrained：把某时刻结果转为键

这是 4.3 的快捷动作，记录约束应用后的所需值；可处理Physics及其他约束，不是一次生成全动画所有帧。为所需骨／属性／当前帧操作，确认生成 timeline 后检查值。

**原创采样工作流：** 复制动作作为输出版本→固定起始模拟状态与时间采样间隔→逐个时刻记录KeyConstrained→完成后移除／停用重复作用的相应约束→逐帧及帧间对比。若记录后的键仍叠同一物理，通常会再次施加运动；若只是把约束 mix 关掉，却把控制器键留错层，也不保证与原曲线一致。

本手册没有承诺已有批量 Bake All 按钮，也没执行自动烘焙。旧Physics页“No baking”不能用来否定4.3采样能力；但采样后的循环连续性、负缩放、Frame间插值和复杂层级需要按实际rig复验。依据：[4.3 Key constrained values](https://esotericsoftware.com/blog/Spine-4.3-released)。

<a id="rigging-t6"></a>

### T6. Slider：按手动帧／骨属性取整套动画姿态

#### T6.1 创建和每个参数

Animation节点选动画→New→Slider；4.3也支持从骨开始创建。先做“姿态资源动作”，把要复用属性打好键，再用Slider取某个时刻；常规主动画给Frame或控制骨运动打键。

| 参数 | 行为 |
| --- | --- |
| Animation | 作为取样资源；铅笔更换，右击名称选择源动作 |
| Frame | 无控制骨时手动时刻，可在其他动画打键；0duration动作没有正常可拖进度范围 |
| Bone | 指定输入控制骨；不用时Frame独立控制。4.3后续版本还能显示骨驱动当前帧，不宜沿用旧页“永远隐藏Frame” |
| Local | 读骨局部／世界值；相同控制骨受父级运动时，两者结果可完全不同 |
| Property | 六类输入 rotate/x/y/scaleX/scaleY/shearY；输入两端对应源动作帧两端 |
| Loop | 超范围循环；关闭则负帧取首姿态，超末帧取末姿态 |
| Additive | 对支持叠加的timeline按当前pose叠加；其他瞬时timeline不变成可数学相加数据 |
| Mix | 姿态资源的影响；能负值／超100%，可打帧 |

#### T6.2 原创三帧转脸、映射与叠加边界

源动作 `face-turn` 在0、15、30帧设置左／正／右：眼睛位置、鼻网格deform、嘴attachment和相关draw order。控制骨localX -40→40映射0→30；x=0读取15帧，x=20读取22.5帧（可插值属性会插值，attachment/draw order仍按瞬时键）。关Loop时x=60停在30帧；开Loop用于连续循环输入，不适合把表情首尾自动跳接成无限滚动。

**叠加例：** 主动作已有胸部呼吸，Slider资源只控制肩膀小幅抖动及对应阴影，Additive可保留主动作所支持的相加部分；若资源同样为Slot attachment打键，它仍会选一个附件，不能同时加出两张图。多个Slider写同属性要设计order／mix，资源不要反过来给自身Frame建立难以分析的循环控制。

当前Slider代码 `animation.apply` 传events=null、lastTime=time，取样时不按普通播放自动发资源动作Event；不要把隐藏源动作事件当成游戏声音触发器。控制骨不活动时相关Slider不取骨驱动输入，需检查Skin依赖。取姿态动作仍需导出，否则runtime无法复用；不能因它“只给控制器用”就关Export。

依据：[Sliders](https://esotericsoftware.com/spine-sliders)、[当前Slider实现](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/Slider.ts)、[骨驱动显示帧](https://esotericsoftware.com/spine-changelog#v4-3-19-beta)、[4.3交互示例](https://esotericsoftware.com/blog/Spine-4.3-released)。

<a id="rigging-r1"></a>

### R1. 已核实范围、默认值与公开说明的局限

#### R1.1 默认值必须分三层

| 层次 | 示例／处理原则 |
| --- | --- |
| 源码对象新建值 | PhysicsConstraintPose若干字段初始化为0；不代表编辑器新建Constraint给0 |
| JSON省略字段载入值 | 当前SkeletonJson对IK省略mix取1、softness0、bendPositive true、compress/stretch false；对Physics省略FPS取60、inertia.5、strength100、damping.85、mass1、wind/gravity0、mix1、limit5000 |
| 编辑器新建默认 | 更新日志明确4.3PhysicsFPS从60改20，Transform mixes默认100%；其余全字段未逐项原生UI核验，不能从前两层推断 |

源码载入常量只用于理解省略字段；本次没有修改任何动画工程来导出工厂默认，所以不将它列成编辑器参数初值总表。runtime的百分数常为0—1，编辑器显示0—100%；骨角度为度、位置／长度依骨架尺度。公开资料不足的滑块硬上限、Weld距离算法、Trace细节范围、每种图标选项仍按本机控件，不虚构数值。

已对官方20个装配相关专题的所有功能性小节逐项映射；视频小节只作为外链，不声称逐分钟转写。Context7按要求查询了4.3映射，但结果不足，因此以官方当前4.3代码和guide交叉确认。没有逐个按钮实际改动用户rig来验证，示例为参考操作方案。

<a id="rigging-r2"></a>

### R2. 官方子节逐项覆盖账本

下表使用2026-10-09抓取的官方目录，一行对应一个标题；“实质说明”表示正文有操作或行为说明，不等于逐项原生UI实测。“分类说明”容器下面有字段表；“资料入口”不当作功能覆盖。4.3更名／行为差异已在相应正文说明。

| 官方专题 | 子节 | 本文位置 | 覆盖状态 | 原文 |
| --- | --- | --- | --- | --- |
| skeletons | Skeletons | G1.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-skeletons#Skeletons) |
| skeletons | Properties | G1.1 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-skeletons#Properties) |
| skeletons | Export | G1.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-skeletons#Export) |
| skeletons | Reference scale | G1.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-skeletons#Reference-scale) |
| skeletons | Skeleton draw order | G1.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-skeletons#Skeleton-draw-order) |
| skeletons | Hiding skeletons | G1.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-skeletons#Hiding-skeletons) |
| bones | Bones | G1.2 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-bones#Bones) |
| bones | Bone transforms | G1.2 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-bones#Bone-transforms) |
| bones | Creating bones | G2.4 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-bones#Creating-bones) |
| bones | Properties | G1.2 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-bones#Properties) |
| bones | Transform inheritance | G1.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-bones#Transform-inheritance) |
| bones | Length | G1.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-bones#Length) |
| bones | Icon | G1.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-bones#Icon) |
| bones | Name | G1.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-bones#Name) |
| bones | Select | G1.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-bones#Select) |
| bones | Color | G1.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-bones#Color) |
| bones | Parent | G1.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-bones#Parent) |
| bones | Add to Skin | G1.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-bones#Add-to-Skin) |
| bones | Split | G1.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-bones#Split) |
| bones | Separate X and Y | G1.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-bones#Separate-X-and-Y) |
| bones | Hiding bones | G1.2 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-bones#Hiding-bones) |
| bones | Bone draw order | G1.2 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-bones#Bone-draw-order) |
| bones | Attachments | G3.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-bones#Attachments) |
| slots | Slots | G1.3 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-slots#Slots) |
| slots | Draw order | G1.3 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-slots#Draw-order) |
| slots | Changing the draw order | G1.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-slots#Changing-the-draw-order) |
| slots | Tree filter | G1.3；G2.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-slots#Tree-filter) |
| slots | Properties | G1.3 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-slots#Properties) |
| slots | Color | G1.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-slots#Color) |
| slots | Tint black | G1.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-slots#Tint-black) |
| slots | Blending | G1.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-slots#Blending) |
| slots | Separate color and alpha | G1.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-slots#Separate-color-and-alpha) |
| slots | Hiding slots | G1.3 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-slots#Hiding-slots) |
| slots | Folders | G1.3 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-slots#Folders) |
| tools | Tools | G2.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-tools#Tools) |
| tools | Selection | G2.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-tools#Selection) |
| tools | Deselect | G2.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-tools#Deselect) |
| tools | Selection history | G2.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-tools#Selection-history) |
| tools | Selection groups | G2.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-tools#Selection-groups) |
| tools | Transform tools | G2.2 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-tools#Transform-tools) |
| tools | Numeric entry | G2.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-tools#Numeric-entry) |
| tools | Axes | G2.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-tools#Axes) |
| tools | Rotate tool | G2.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-tools#Rotate-tool) |
| tools | Translate tool | G2.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-tools#Translate-tool) |
| tools | Scale tool | G2.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-tools#Scale-tool) |
| tools | Scale examples | G2.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-tools#Scale-examples) |
| tools | Shear tool | G2.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-tools#Shear-tool) |
| tools | Pose tool | G2.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-tools#Pose-tool) |
| tools | Pose examples | G2.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-tools#Pose-examples) |
| tools | Bone length tool | G2.4 | 实质说明 | [官方](https://esotericsoftware.com/spine-tools#Bone-length-tool) |
| tools | Other tools | G2.4；G5 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-tools#Other-tools) |
| tools | Weights tool | G5.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-tools#Weights-tool) |
| tools | Create tool | G2.4 | 实质说明 | [官方](https://esotericsoftware.com/spine-tools#Create-tool) |
| tools | Create workflow | G2.4 | 实质说明 | [官方](https://esotericsoftware.com/spine-tools#Create-workflow) |
| tools | Compensation | G2.5 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-tools#Compensation) |
| tools | Pixels | G2.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-tools#Pixels) |
| tools | Auto key | G2.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-tools#Auto-key) |
| tools | Viewport options | G2.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-tools#Viewport-options) |
| tools | Rulers | G2.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-tools#Rulers) |
| tools | Copy/paste | G2.6 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-tools#Copypaste) |
| tools | Bone transforms | G2.6 | 实质说明 | [官方](https://esotericsoftware.com/spine-tools#Bone-transforms) |
| tools | Attachment transforms | G2.6 | 实质说明 | [官方](https://esotericsoftware.com/spine-tools#Attachment-transforms) |
| tools | Vertex positions | G2.6 | 实质说明 | [官方](https://esotericsoftware.com/spine-tools#Vertex-positions) |
| tools | Video | 该官方专题Video外链 | 资料入口；不声称视频转写 | [官方](https://esotericsoftware.com/spine-tools#Video) |
| attachments | Attachments | G3.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-attachments#Attachments) |
| attachments | Types | G3.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-attachments#Types) |
| attachments | Common properties | G3.1 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-attachments#Common-properties) |
| attachments | Select | G3.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-attachments#Select) |
| attachments | Export | G3.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-attachments#Export) |
| attachments | Name | G3.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-attachments#Name) |
| attachments | Color | G3.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-attachments#Color) |
| attachments | Set Parent | G3.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-attachments#Set-Parent) |
| attachments | Hiding attachments | G3.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-attachments#Hiding-attachments) |
| regions | Region attachments | G3.2 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-regions#Region-attachments) |
| regions | Setup | G3.2 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-regions#Setup) |
| regions | Properties | G3.2 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-regions#Properties) |
| regions | Image path | G3.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-regions#Image-path) |
| regions | Mesh | G3.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-regions#Mesh) |
| regions | Sequence | G3.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-regions#Sequence) |
| regions | Translation | G3.2 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-regions#Translation) |
| regions | Video | 该官方专题Video外链 | 资料入口；不声称视频转写 | [官方](https://esotericsoftware.com/spine-regions#Video) |
| meshes | Mesh attachments | G4.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-meshes#Mesh-attachments) |
| meshes | Setup | G3.2；G4.1 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-meshes#Setup) |
| meshes | Properties | G4.1 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-meshes#Properties) |
| meshes | Image path | G3.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Image-path) |
| meshes | Mesh | G3.2；G4.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Mesh) |
| meshes | Wireframe | G4.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Wireframe) |
| meshes | Sequence | G3.2；主手册14章 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Sequence) |
| meshes | Edit Mesh | G4.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Edit-Mesh) |
| meshes | Freeze | G4.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Freeze) |
| meshes | Reset | G4.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Reset) |
| meshes | Edit mode | G4.1 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-meshes#Edit-mode) |
| meshes | Modify tool | G4.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Modify-tool) |
| meshes | Create tool | G4.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Create-tool) |
| meshes | Delete tool | G4.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Delete-tool) |
| meshes | New vertices mode | G4.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#New-vertices-mode) |
| meshes | Reset vertices | G4.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Reset-vertices) |
| meshes | Generate | G4.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Generate) |
| meshes | Trace | G4.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Trace) |
| meshes | Triangles | G4.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Triangles) |
| meshes | Dim | G4.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Dim) |
| meshes | Isolate | G4.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Isolate) |
| meshes | Deformed | G4.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Deformed) |
| meshes | Edges | G4.3 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-meshes#Edges) |
| meshes | Edge examples | G4.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Edge-examples) |
| meshes | Edge loop selection | G4.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Edge-loop-selection) |
| meshes | Vertex placement | G4.3 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-meshes#Vertex-placement) |
| meshes | Hull size | G4.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Hull-size) |
| meshes | Hull edges | G4.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Hull-edges) |
| meshes | Holes | G4.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Holes) |
| meshes | Vertex count | G4.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Vertex-count) |
| meshes | Deformation | G4.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-meshes#Deformation) |
| meshes | Transform tools | G4.1 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-meshes#Transform-tools) |
| meshes | Image resize | G4.3 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-meshes#Image-resize) |
| meshes | Linked meshes | G4.4 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-meshes#Linked-meshes) |
| meshes | Video | 该官方专题Video外链 | 资料入口；不声称视频转写 | [官方](https://esotericsoftware.com/spine-meshes#Video) |
| bounding-boxes | Bounding box attachments | G3.3 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-bounding-boxes#Bounding-box-attachments) |
| bounding-boxes | Setup | G3.3 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-bounding-boxes#Setup) |
| bounding-boxes | Properties | G3.3 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-bounding-boxes#Properties) |
| bounding-boxes | Edit Bounding Box | G3.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-bounding-boxes#Edit-Bounding-Box) |
| bounding-boxes | Freeze | G3.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-bounding-boxes#Freeze) |
| bounding-boxes | Edit mode | G3.3 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-bounding-boxes#Edit-mode) |
| bounding-boxes | Create tool | G3.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-bounding-boxes#Create-tool) |
| bounding-boxes | Delete tool | G3.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-bounding-boxes#Delete-tool) |
| bounding-boxes | New vertices mode | G3.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-bounding-boxes#New-vertices-mode) |
| bounding-boxes | Transform tools | G3.3 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-bounding-boxes#Transform-tools) |
| bounding-boxes | Video | 该官方专题Video外链 | 资料入口；不声称视频转写 | [官方](https://esotericsoftware.com/spine-bounding-boxes#Video) |
| clipping | Clipping attachments | G3.4 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-clipping#Clipping-attachments) |
| clipping | Setup | G3.3；G3.4 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-clipping#Setup) |
| clipping | Properties | G3.3；G3.4 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-clipping#Properties) |
| clipping | End slot | G3.4 | 实质说明 | [官方](https://esotericsoftware.com/spine-clipping#End-slot) |
| clipping | Edit Clipping | G3.3；G3.4 | 实质说明 | [官方](https://esotericsoftware.com/spine-clipping#Edit-Clipping) |
| clipping | Freeze | G3.3；G3.4 | 实质说明 | [官方](https://esotericsoftware.com/spine-clipping#Freeze) |
| clipping | Edit mode | G3.3；G3.4 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-clipping#Edit-mode) |
| clipping | Create tool | G3.3；G3.4 | 实质说明 | [官方](https://esotericsoftware.com/spine-clipping#Create-tool) |
| clipping | Delete tool | G3.3；G3.4 | 实质说明 | [官方](https://esotericsoftware.com/spine-clipping#Delete-tool) |
| clipping | New vertices mode | G3.3；G3.4 | 实质说明 | [官方](https://esotericsoftware.com/spine-clipping#New-vertices-mode) |
| clipping | Transform tools | G3.3；G3.4 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-clipping#Transform-tools) |
| clipping | Draw order | G3.4 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-clipping#Draw-order) |
| clipping | Self-intersection | G3.4 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-clipping#Self-intersection) |
| clipping | Performance | G3.4 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-clipping#Performance) |
| paths | Paths | G3.5 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-paths#Paths) |
| paths | Setup | G3.5 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-paths#Setup) |
| paths | Properties | G3.5 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-paths#Properties) |
| paths | Length | G3.5 | 实质说明 | [官方](https://esotericsoftware.com/spine-paths#Length) |
| paths | Closed | G3.5 | 实质说明 | [官方](https://esotericsoftware.com/spine-paths#Closed) |
| paths | Constant speed | G3.5 | 实质说明 | [官方](https://esotericsoftware.com/spine-paths#Constant-speed) |
| paths | Edit Path | G3.5 | 实质说明 | [官方](https://esotericsoftware.com/spine-paths#Edit-Path) |
| paths | Freeze | G3.5 | 实质说明 | [官方](https://esotericsoftware.com/spine-paths#Freeze) |
| paths | Reverse | G3.5 | 实质说明 | [官方](https://esotericsoftware.com/spine-paths#Reverse) |
| paths | Edit mode | G3.5 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-paths#Edit-mode) |
| paths | Create tool | G3.5 | 实质说明 | [官方](https://esotericsoftware.com/spine-paths#Create-tool) |
| paths | Delete tool | G3.5 | 实质说明 | [官方](https://esotericsoftware.com/spine-paths#Delete-tool) |
| paths | New vertices mode | G3.5 | 实质说明 | [官方](https://esotericsoftware.com/spine-paths#New-vertices-mode) |
| paths | Transform tools | G3.5 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-paths#Transform-tools) |
| paths | Angle lock | G3.5 | 实质说明 | [官方](https://esotericsoftware.com/spine-paths#Angle-lock) |
| paths | Cusps | G3.5 | 实质说明 | [官方](https://esotericsoftware.com/spine-paths#Cusps) |
| paths | Video | 该官方专题Video外链 | 资料入口；不声称视频转写 | [官方](https://esotericsoftware.com/spine-paths#Video) |
| points | Point attachments | G3.6 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-points#Point-attachments) |
| points | Setup | G3.6 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-points#Setup) |
| points | Properties | G3.6 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-points#Properties) |
| mesh-tools | Mesh Tools view | G4.5 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-mesh-tools#Mesh-Tools-view) |
| mesh-tools | Size | G4.5 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-mesh-tools#Size) |
| mesh-tools | Feather | G4.5 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-mesh-tools#Feather) |
| mesh-tools | Hull vertices | G4.5 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-mesh-tools#Hull-vertices) |
| mesh-tools | Video | 该官方专题Video外链 | 资料入口；不声称视频转写 | [官方](https://esotericsoftware.com/spine-mesh-tools#Video) |
| weights | Weights view | G5.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-weights#Weights-view) |
| weights | Bind | G5.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-weights#Bind) |
| weights | Bones list | G5.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-weights#Bones-list) |
| weights | Triangle order | G5.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-weights#Triangle-order) |
| weights | Pies | G5.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-weights#Pies) |
| weights | Overlay | G5.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-weights#Overlay) |
| weights | Selected | G5.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-weights#Selected) |
| weights | Weights tool | G5.2 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-weights#Weights-tool) |
| weights | Mode | G5.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-weights#Mode) |
| weights | Direct | G5.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-weights#Direct) |
| weights | Brushes | G5.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-weights#Brushes) |
| weights | Adjusting weights | G5.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-weights#Adjusting-weights) |
| weights | Weights copy and paste | G5.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-weights#Weights-copy-and-paste) |
| weights | Testing weights | G5.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-weights#Testing-weights) |
| weights | Auto | G5.3 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-weights#Auto) |
| weights | Smooth | G5.3 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-weights#Smooth) |
| weights | Prune | G5.3 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-weights#Prune) |
| weights | Weld | G5.3 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-weights#Weld) |
| weights | Swap | G5.3 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-weights#Swap) |
| weights | Lock | G5.3 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-weights#Lock) |
| weights | Update bindings | G5.3 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-weights#Update-bindings) |
| weights | Example without update bindings | G5.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-weights#Example-without-update-bindings) |
| weights | Example with update bindings | G5.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-weights#Example-with-update-bindings) |
| weights | Duplicating bones | G5.3 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-weights#Duplicating-bones) |
| weights | Video | 该官方专题Video外链 | 资料入口；不声称视频转写 | [官方](https://esotericsoftware.com/spine-weights#Video) |
| skins | Skins | G6.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-skins#Skins) |
| skins | Setup | G6.1 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-skins#Setup) |
| skins | Skin placeholders | G6.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-skins#Skin-placeholders) |
| skins | Selected attachments | G6.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-skins#Selected-attachments) |
| skins | Properties | G6.1 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-skins#Properties) |
| skins | Export | G6.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-skins#Export) |
| skins | Color | G6.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-skins#Color) |
| skins | Add to Skin | G6.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-skins#Add-to-Skin) |
| skins | Folders | G6.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-skins#Folders) |
| skins | Active skin | G6.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-skins#Active-skin) |
| skins | Skin attachments | G6.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-skins#Skin-attachments) |
| skins | Skin bones | G6.2 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-skins#Skin-bones) |
| skins | Skin constraints | G6.2 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-skins#Skin-constraints) |
| skins | Constrained bones | G6.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-skins#Constrained-bones) |
| skins | Skin constraints example | G6.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-skins#Skin-constraints-example) |
| skins | Warnings | G6.2 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-skins#Warnings) |
| skins | Bone warnings | G6.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-skins#Bone-warnings) |
| skins | Attachment warnings | G6.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-skins#Attachment-warnings) |
| skins | Constraint warnings | G6.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-skins#Constraint-warnings) |
| skins | Duplicating a skin | G6.3 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-skins#Duplicating-a-skin) |
| skins | Skin workflows | G6.3 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-skins#Skin-workflows) |
| skins | The first skin | G6.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-skins#The-first-skin) |
| skins | Similar skins | G6.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-skins#Similar-skins) |
| skins | Programmatic skins | G6.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-skins#Programmatic-skins) |
| skins | Removing skin placeholders | G6.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-skins#Removing-skin-placeholders) |
| skins | Mix and match | G6.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-skins#Mix-and-match) |
| skins | Video | 该官方专题Video外链 | 资料入口；不声称视频转写 | [官方](https://esotericsoftware.com/spine-skins#Video) |
| constraints | Constraints | T1.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-constraints#Constraints) |
| constraints | Order | T1.2 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-constraints#Order) |
| constraints | Order example | T1.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-constraints#Order-example) |
| constraints | Bone transforms | T1.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-constraints#Bone-transforms) |
| constraints | Mix | T1.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-constraints#Mix) |
| constraints | Folders | T1.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-constraints#Folders) |
| constraints | Viewport bones | T1.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-constraints#Viewport-bones) |
| constraints | Tree annotations | T1.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-constraints#Tree-annotations) |
| constraints | Copy/paste | T1.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-constraints#Copypaste) |
| constraints | Video | 该官方专题Video外链 | 资料入口；不声称视频转写 | [官方](https://esotericsoftware.com/spine-constraints#Video) |
| ik-constraints | IK constraints | T2.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-ik-constraints#IK-constraints) |
| ik-constraints | Setup | T2.1 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-ik-constraints#Setup) |
| ik-constraints | Properties | T2.1 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-ik-constraints#Properties) |
| ik-constraints | Parent | T2.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-ik-constraints#Parent) |
| ik-constraints | Child | T2.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-ik-constraints#Child) |
| ik-constraints | Target | T2.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-ik-constraints#Target) |
| ik-constraints | Positive | T2.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-ik-constraints#Positive) |
| ik-constraints | Compress | T2.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-ik-constraints#Compress) |
| ik-constraints | Stretch | T2.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-ik-constraints#Stretch) |
| ik-constraints | Uniform | T2.3（4.3三模式） | 实质说明 | [官方](https://esotericsoftware.com/spine-ik-constraints#Uniform) |
| ik-constraints | Softness | T2.1；T2.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-ik-constraints#Softness) |
| ik-constraints | Mix | T2.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-ik-constraints#Mix) |
| ik-constraints | Limitations | T2.2 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-ik-constraints#Limitations) |
| ik-constraints | Video | 该官方专题Video外链 | 资料入口；不声称视频转写 | [官方](https://esotericsoftware.com/spine-ik-constraints#Video) |
| path-constraints | Path constraints | T3.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-path-constraints#Path-constraints) |
| path-constraints | Setup | T3.1 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-path-constraints#Setup) |
| path-constraints | Properties | T3.1 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-path-constraints#Properties) |
| path-constraints | Bones | T3.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-path-constraints#Bones) |
| path-constraints | Target | T3.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-path-constraints#Target) |
| path-constraints | Spacing | T3.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-path-constraints#Spacing) |
| path-constraints | Position | T3.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-path-constraints#Position) |
| path-constraints | Position handle | T3.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-path-constraints#Position-handle) |
| path-constraints | Rotate Offset | T3.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-path-constraints#Rotate-Offset) |
| path-constraints | Rotate Mix | T3.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-path-constraints#Rotate-Mix) |
| path-constraints | Translate Mix | T3.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-path-constraints#Translate-Mix) |
| path-constraints | Color | T3.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-path-constraints#Color) |
| path-constraints | Video | 该官方专题Video外链 | 资料入口；不声称视频转写 | [官方](https://esotericsoftware.com/spine-path-constraints#Video) |
| transform-constraints | Transform constraints | T4.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-transform-constraints#Transform-constraints) |
| transform-constraints | Setup | T4.1 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-transform-constraints#Setup) |
| transform-constraints | Properties | T4.1；T4.2 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-transform-constraints#Properties) |
| transform-constraints | Bones | T4.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-transform-constraints#Bones) |
| transform-constraints | Local | T4.1（源/目标独立） | 实质说明 | [官方](https://esotericsoftware.com/spine-transform-constraints#Local) |
| transform-constraints | Relative | T4.1（Additive） | 实质说明 | [官方](https://esotericsoftware.com/spine-transform-constraints#Relative) |
| transform-constraints | Target | T4.1（source） | 实质说明 | [官方](https://esotericsoftware.com/spine-transform-constraints#Target) |
| transform-constraints | Offset | T4.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-transform-constraints#Offset) |
| transform-constraints | Match | T4.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-transform-constraints#Match) |
| transform-constraints | Mix | T4.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-transform-constraints#Mix) |
| transform-constraints | Video | 该官方专题Video外链 | 资料入口；不声称视频转写 | [官方](https://esotericsoftware.com/spine-transform-constraints#Video) |
| physics-constraints | Physics constraints | T5.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-physics-constraints#Physics-constraints) |
| physics-constraints | Setup | T5.1 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-physics-constraints#Setup) |
| physics-constraints | Bone | T5.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Bone) |
| physics-constraints | Translate X | T5.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Translate-X) |
| physics-constraints | Translate Y | T5.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Translate-Y) |
| physics-constraints | Rotation | T5.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Rotation) |
| physics-constraints | Shear X | T5.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Shear-X) |
| physics-constraints | Scale X | T5.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Scale-X) |
| physics-constraints | Limit | T5.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Limit) |
| physics-constraints | FPS | T5.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#FPS) |
| physics-constraints | Properties | T5.1 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-physics-constraints#Properties) |
| physics-constraints | Inertia | T5.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Inertia) |
| physics-constraints | Strength | T5.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Strength) |
| physics-constraints | Damping | T5.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Damping) |
| physics-constraints | Mass | T5.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Mass) |
| physics-constraints | Wind | T5.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Wind) |
| physics-constraints | Gravity | T5.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Gravity) |
| physics-constraints | Global | T5.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Global) |
| physics-constraints | Mix | T5.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Mix) |
| physics-constraints | Simulate | T5.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Simulate) |
| physics-constraints | Deterministic | T5.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Deterministic) |
| physics-constraints | Reset | T5.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Reset) |
| physics-constraints | Reset All | T5.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Reset-All) |
| physics-constraints | Warnings | T5.3 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-physics-constraints#Warnings) |
| physics-constraints | Limitations | T5.3 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-physics-constraints#Limitations) |
| physics-constraints | Warm Up | T5.3 | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#Warm-Up) |
| physics-constraints | No baking | T5.4（4.3能力更新） | 实质说明 | [官方](https://esotericsoftware.com/spine-physics-constraints#No-baking) |
| physics-constraints | Reference Scale | T5.3 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-physics-constraints#Reference-Scale) |
| physics-constraints | Video | 该官方专题Video外链 | 资料入口；不声称视频转写 | [官方](https://esotericsoftware.com/spine-physics-constraints#Video) |
| sliders | Sliders | T6.1 | 专题容器；下项展开 | [官方](https://esotericsoftware.com/spine-sliders#Sliders) |
| sliders | Setup | T6.1 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-sliders#Setup) |
| sliders | Properties | T6.1 | 分类说明＋各项展开 | [官方](https://esotericsoftware.com/spine-sliders#Properties) |
| sliders | Animation | T6.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-sliders#Animation) |
| sliders | Loop | T6.1；T6.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-sliders#Loop) |
| sliders | Additive | T6.1；T6.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-sliders#Additive) |
| sliders | Frame | T6.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-sliders#Frame) |
| sliders | Bone | T6.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-sliders#Bone) |
| sliders | Local | T6.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-sliders#Local) |
| sliders | Property | T6.1；T6.2 | 实质说明 | [官方](https://esotericsoftware.com/spine-sliders#Property) |
| sliders | Mix | T6.1 | 实质说明 | [官方](https://esotericsoftware.com/spine-sliders#Mix) |

本分册检查 20 个专题、306 个标题。功能描述之外，编辑器未公开且未实测的数值硬范围／全字段初值由R1明确保留；不是把未写功能藏在“以官方为准”中。视频本身不属于已转写内容。


<a id="reference-animation"></a>

## 28. 动画、关键帧、曲线、声音与序列逐项参考

- [A1. 动画对象、列表与工作方式](#animation-a1)

- [A2. 时间、帧、范围与播放](#animation-a2)

- [A3. 打帧入口与颜色状态](#animation-a3)

- [A4. 可动画属性的记录粒度](#animation-a4)

- [A5. 关键帧剪贴板、姿态剪贴板与 Hold](#animation-a5)

- [A6. Shift、Offset、Adjust、缩放与清理](#animation-a6)

- [A7. Dopesheet 的行、选择、过滤与同步](#animation-a7)

- [A8. Graph 曲线、控制柄与编辑](#animation-a8)

- [A9. 改时间与改数值的控制柄维护](#animation-a9)

- [A10. Favor、Store 与 Curves 面板](#animation-a10)

- [A11. Events、声音文件与波形](#animation-a11)

- [A12. Sequence 的资源、七种模式与时间](#animation-a12)

- [A13. 播放、选择和曲线相关命令索引](#animation-a13)

- [A14. 核对边界与公开资料差异](#animation-a14)

- [A15. 官方小节覆盖账本](#animation-a15)

核对日期：2026-10-09。适用于 Spine 4.3；网页 User Guide 并非全部同步到 4.3，因此以下同时采用官方发布记录、官方团队论坛答复，以及本机 **4.3.26 Professional** 的只读界面观察。本章集中说明动画专题；骨骼、附件、约束与运行时分别参见其他章节。

操作标识使用英文命令名，中文标签依界面翻译可能不同。`Ctrl` 在 Mac 对应 `Cmd`，`Alt` 对应 `Option`。快捷键可以自定义；文中的已绑定值来自本机现有配置，不能保证其他机器同样绑定。参数没有明确公开默认值或范围时，不用猜测补齐。

<a id="animation-a1"></a>



### A1. 动画对象、列表与工作方式

在 Animate 模式激活动画后，关键帧才有明确归属。Tree 的 Animations → New Animation 建立动作；Animations 面板的新增按钮提供同一入口。列表单击切换当前动作，双击重命名，右击只定位 Tree；多骨架工程用骨架选择框决定列出哪个骨架的动画。文件夹参与导出名称，例如 `combat/hit` 与 `hit` 是两个不同的运行时名称。[Animations view](https://esotericsoftware.com/spine-animations-view)、[动画对象](https://esotericsoftware.com/spine-keys#Animations)。

4.3 的 Animations 面板还提供复制、删除，支持文件夹右击展开／折叠；Setup 模式选择的动画可用于 Slider 设置，不应误认为 Setup 已经开始给普通动画打帧。[4.3 变更记录](https://esotericsoftware.com/spine-changelog)。

| 工作方式 | 实际组织方式 | 本手册建议的使用条件 |
| --- | --- | --- |
| Straight ahead | 沿时间前进，连续建立姿态 | 轨迹或形态演化比明确终点更重要时，如烟雾、柔性摆动 |
| Pose to pose | 先完成主要姿态，再安排时间和过渡 | 攻击、起跳、受击等需要先确认动作意图的制作 |
| Layered | 分遍处理少量部位，可隐藏其他部位 | 先髋／躯干，再脚，再手，最后饰品 |
| Combined | 在同一制作过程中组合上述方法 | 先姿态规划，再分部位顺做，最后修曲线 |

这些是制作策略。动画属性中的 `Layered` 清理保护和运行时多轨播放各有独立含义，不能只因为采用 Layered 制作策略就认为保护已启用。曲线决定键之间的过渡节奏；整齐的时间线不保证动作自然。[Animating](https://esotericsoftware.com/spine-animating)。

动画文件夹通过先选动画再 New Folder 建立，拖动动画可以换文件夹。为避免运行时动作名改变后查找失败，移动前核对应用中的名称映射。Straight ahead 容易让动作自然延续，也容易超出预期时长；Pose to pose 方便早期调整，但须补足中间姿态避免生硬；Layered 有利于集中处理部位，同时须在每一遍后检查重心和部位联系。[文件夹操作](https://esotericsoftware.com/spine-keys#Folders)、[工作方式](https://esotericsoftware.com/spine-animating#Workflow)。

<a id="animation-a2"></a>



### A2. 时间、帧、范围与播放

| 控件／概念 | 作用与存储边界 |
| --- | --- |
| Timeline position | 青色竖线是当前取样位置；点击或拖动时间尺改变位置 |
| 橙色菱形／末端竖线 | 标识有键的帧／最高时间的键；最后一个键决定动作长度 |
| Frame snapping | 整数帧对齐；按住 Shift 查看或建立小数帧 |
| Repeat | 播放抵达末端后回到开头；设置按动画保存，JSON／binary 不携带该编辑器循环选择 |
| 多骨架 Repeat | 同时激活动作时，以这些动作中最高键的时间作为共同循环末端 |
| Timeline view | 独立摆放同一时间尺，减少从视口移到旁边 Graph／Dopesheet 的距离 |
| Loop Start / End | 指定局部检查区间；须同时设起点、终点并启用 Repeat；双击按钮或清空数字解除 |
| Current | 播放时横向跟踪当前时间；适合时间尺容不下整个动作的情况 |

时间尺右键拖动平移、滚轮缩放；Dopesheet底部缩放滑条直接调视野，Zoom Keys将所有键纳入时间范围。Current旁数值框可输入整数或小数并按Enter/Tab跳转。播放按钮提供向前／向后播放、前／后帧和前／后键导航；Frame单位只用于编辑定位，运行时按实际时间取样，导出影片帧率另由格式设置。[时间尺操作](https://esotericsoftware.com/spine-keys#Timeline)。

Repeat关闭后可以超过末键继续播放。Repeat开启时，**从末键之前开始拖动时间指针**，越过末端会绕回；若从末键之后开始拖动，则可查看末端之后的姿态。Loop检查区间不按动画保存，不能与按动画保存的Repeat选项混淆。[拖动循环](https://esotericsoftware.com/spine-keys#Timeline-position)、[Loop范围](https://esotericsoftware.com/spine-dopesheet#Loop)。

隐藏皮肤的 deform key、隐藏骨骼的键也可能决定长度。定位不明的末端先检查当前筛选、骨骼可见性和皮肤附件显示，再判断是否存在多余键。[Timeline](https://esotericsoftware.com/spine-keys#Timeline)、[Timeline view](https://esotericsoftware.com/spine-timeline)、[Dopesheet position](https://esotericsoftware.com/spine-dopesheet#Timeline-position)。

| Playback 选项 | 精确区别 | 示例／注意 |
| --- | --- | --- |
| Timeline FPS | 工程所有骨架、动作共享的帧单位换算；改变时，键的帧号不动，秒数改变 | 30 FPS 下第 30 帧是 1 秒；60 FPS 下是 0.5 秒 |
| Speed | 仅编辑器试听速度，统一影响工程播放，不导出且不跨编辑器运行保存 | 用慢放检查快速手脚，但交付时仍需按正常速度审阅 |
| Stepped | 试听时在两个关键帧之间保持前键 | 0、10 帧有键时，中间显示第 0 帧关键姿态 |
| Interpolated 关闭 | 将播放位置舍入到最近的整数帧；整数帧本身仍可处在插值区间 | 0、10 帧有键时，第 4 帧仍能显示由曲线算出的中间值 |

因此 Stepped 和关闭 Interpolated 能产生不同画面。前者用于关键姿态检查，后者用于有帧感的播放。要改变 FPS 同时保留秒数，可备份并导出数据，建立设置了新 FPS 的工程再导入数据，导入时关闭 New project；这属于数据往返，保留原 `.spine` 以免丢失未导出的编辑资料。[Playback](https://esotericsoftware.com/spine-playback)。

<a id="animation-a3"></a>



### A3. 打帧入口与颜色状态

| 命令／入口 | 记录范围 | 适用场景 |
| --- | --- | --- |
| 属性旁 Key 按钮 | 对应属性／同组参数 | 明确只给一个通道或一组约束参数打帧 |
| Key Edited，K | 当前已编辑但未记录的属性 | 调整一组姿态后一次记录 |
| Key Active，L | 当前工具对应的所选骨骼变换 | 用 Rotate 工作时只记录旋转 |
| Key Selected，Cmd+L | 所选对象所有可由该命令记录的属性，包含未修改者 | 要完整记录所选对象，先理解会增加哪些通道 |
| Key Shown，Cmd+Shift+L | Graph 可见曲线／Dopesheet 当前显示且已有时间轴的属性 | 保留现有制作范围的当前姿态 |
| 属性专用 Key 命令 | Rotation、Translate／X／Y、Scale／X／Y、Shear／X／Y、Color、Attachment | 自定义设备按钮快速记录特定属性 |
| Auto Key | 修改时立刻建键 | 先确认动作和当前帧，避免把检查性编辑写进动作 |
| Key Constrained | 记录约束后的当前值 | 需要保留约束结果时；更详细流程见约束专题 |

`Key Selected` 与 `Key Shown` 并不相同：选择没有位移时间轴的骨骼，再用前者可能建立位移键，后者主要覆盖已存在且显示出来的通道。官方人员说明 4.2 已移除“骨骼行有未记录变换时显示橙色快捷按钮”的旧机制，当前 Keys 网页仍保留旧图文；4.3 不应要求寻找那个旧按钮。[官方 Key 命令解释](https://eu.esotericsoftware.com/forum/d/26273-ability-to-key-bones-from-the-tree-removed-in-42-)。

属性键按钮绿色表示当前时间无键，橙色表示改动尚未写键，红色表示此帧已有键。切换时间会重新计算姿态，未记录改动会丢失；某些情况下 Undo 可以在新位置恢复刚才姿态，但不应将其当作保存方式。[Setting keys](https://esotericsoftware.com/spine-keys#Setting-keys)。

本机 4.3.26 的快捷键清单另有 `Key Shown Hold`、`Select Keys`、`Select Previous Keys`。前者用于给显示通道建立保持姿态的键，后者允许定位前面的键以调整进入当前姿态的曲线；官方变更确认这些入口已存在。`Hold` 命令的全部跨属性细节尚未在本机执行验证，不把它泛称为“所有键改成 Stepped”。[保持姿态的官方讨论](https://us.esotericsoftware.com/forum/d/29172-adding-same-value-keyframes-at-end-of-animation-creates-unwanted-interpolation)。

<a id="animation-a4"></a>



### A4. 可动画属性的记录粒度

| 属性 | 键的归属／记录粒度 | 重要边界 |
| --- | --- | --- |
| Rotation、Translation、Scale、Shear | Bone；Translate／Scale／Shear 可按 X/Y 分离 | 骨骼变换以 setup 为基准；改 setup 会影响既有动画 |
| Inherit | Bone 的变换继承选项 | 是离散模式切换，修改时还要看父级与约束效果 |
| Attachment | Slot 当前附件或空附件 | Slot／Bone 的编辑器隐藏状态不等于动画隐藏 |
| Color | Slot；通常 RGB 与 Alpha 一起，Separate 可拆开 | 分离后左色块含 Alpha，右色块是不透明 RGB；不是两套图片 |
| Draw order | 总层级；4.3 另有 folder 层级键 | 改文件夹内层级可独立于其他文件夹 |
| Deform | Mesh、Path、Bounding box、Clipping 的顶点 | 一键记录整个附件的变形，不会只记录当时选中的顶点 |
| Sequence | Region／Mesh attachment 的序列控制 | Frame、模式、帧率定义该键后的序列播放 |
| Event | 命名事件和该次发生的附加数据 | 没有数值曲线的中间“半次事件” |
| IK | 约束 Mix、Softness、Bend 等可动画项 | 某些布尔选项与数值一起记录；4.3 伸缩选项参看约束章节 |
| Transform | 各受控变换 Mix | source／target 映射配置与可动画 Mix 需要区别 |
| Path | Position、Spacing、Mix 分组 | 非闭合路径复位可用相邻小数帧避免长时间倒退插值 |
| Physics／Slider | 4.3 对应可动画参数与重置／Frame／Mix | 由约束专题逐参数说明，不由旧 Keys 的类型列表穷举 |

旋转、缩放、错切键存 local 值，位移键存 parent 轴值。世界轴数字是编辑时的转换结果；欲制作连续整圈旋转，应使用 Local／Parent，而非只记录 0–360 的 World 方向。Deform 会增加顶点变换数据；新增顶点后，既有 deform keys 需要复查新顶点的姿态。Deform插值把顶点沿直线移到下一键，即便制作时用Rotate工具摆了顶点，也不会自动保留旋转圆弧。复杂的分部位动作宜用权重骨骼：deform一键含全附件顶点、每顶点每影响骨骼又增加数据，影响太多时需要结合Prune；权重骨骼还可复用于其他附件。动画中粉色顶点标识发生变形的顶点，Ctrl 双击可选择变形／未变形顶点组。[Keyable properties](https://esotericsoftware.com/spine-keys#Keyable-properties)。

<a id="animation-a5"></a>



### A5. 关键帧剪贴板、姿态剪贴板与 Hold

| 操作 | 复制内容／粘贴目标 | 必查项 |
| --- | --- | --- |
| Copy／Cut 关键帧 | 当前选中的键；Cut 随后删除原键 | 当前时间指针与所选键的时间可以不同，复制的是所选键 |
| Paste 关键帧 | 以当前时间位置粘贴拷贝的键；包含键值及曲线信息 | 源片段的时间关系、目标已有键以及当前选择对象 |
| 跨对象 Paste | 变换、颜色、附件、deform 键可指定对应骨骼／插槽／附件 | 选择的目标必须适合该键类型，不等于通用重定向整个角色 |
| Cmd+Shift 开始拖键 | 复制所选键同时拖动副本 | 与 Shift 单独解除吸附区分 |
| Delete／双击键 | 删除所选键／被双击的键 | 汇总行上的键可能对应多个实际键 |
| Paste Keys - Hold【4.3.22】 | 粘贴相同数值键时，使用 Linear 保持区间，避免延用贝塞尔柄产生中间漂移 | 它是独立命令；默认普通 Paste 仍保留 |

4.3.19起不允许一次复制来自多个骨架的键；同一组骨骼即使选择顺序改变，也不会误按“粘贴到不同骨骼”处理。跨骨架复制先明确选一个骨架的键，跨不同骨骼粘贴先明确目标选择。[4.3.19](https://esotericsoftware.com/spine-changelog#v4-3-19)。

`Paste Keys - Hold` 的名字不代表“粘贴后统一使用 Stepped”。官方发布记录更准确地描述为对同值键间使用 Linear。用独立快捷键同时保留普通 Paste；把 Cmd+V 改绑给 Hold 会影响其他地方的普通粘贴命令，不能一个按键同时完成两者。[Hold 官方解释](https://eu.esotericsoftware.com/forum/d/30481-stop-animation-play-forward)、[4.3.22](https://esotericsoftware.com/spine-changelog#v4-3-22)。

**姿态剪贴板是另一条入口。** 在视口／Tree 选骨骼、附件或顶点复制，保存的是当前姿态的变换／坐标，包含世界与局部信息；粘贴时使用 World 或 Local 决定如何应用。骨骼姿态包含 rotation、translation、scale、shear，并考虑层级；附件姿态包含 rotation、translation、scale；顶点跨附件粘贴要求选中数量一致，选择顺序决定映射。它不以关键帧片段的曲线为内容。[Tools Copy/paste](https://esotericsoftware.com/spine-tools#Copy-paste)。

本手册建议：复制“手在屏幕上的这个位置”时，先选骨骼复制当前姿态，并按世界轴粘贴；复制“一段局部挥手的动画”时，选择时间轴键复制。不要仅切换工具坐标轴就期待已经复制的关键帧改变其存储坐标。[官方坐标区别说明](https://esotericsoftware.com/forum/d/18466-copying-key-in-localparentworld-always-returns-local)。

<a id="animation-a6"></a>



### A6. Shift、Offset、Adjust、缩放与清理

| 操作 | 修改什么 | 前提与边界 |
| --- | --- | --- |
| Shift Keys | 拖动键时，后面的键跟着移动，以保留后段相互间距 | Alt 临时启用；影响后续时间，不等于改数值 |
| Offset Keys | 将循环片段移相，越过边界的键绕回 | 首尾键值须相同；Loop Start／End 可指定绕回区间 |
| Adjust Keys | 用视口 Rotate／Translate／Scale／Pose 操作，相对调整所选键的数值 | 先选键再启用；不改变键的时间 |
| 框选左右边缘拖动 | 按比例伸缩键间时间 | 边缘越过另一边可反转顺序；Shift 可解除帧吸附 |
| Graph 上下缩放 | 缩放数值范围 | 改的是所选曲线的值，须留意单位及setup基准 |
| Clean Up | 删除对该动画姿态无贡献的冗余键 | 支持同时选择多动作；操作后复验分层播放 |

Offset可以按住Cmd+Alt开始拖键临时启用；未设置Loop范围时，用第0帧到动作最高键的范围绕回。它适合把衣摆、发梢、脚步的循环峰值向后挪，从而产生跟随动作，而不延长整个循环。[Offset入口](https://esotericsoftware.com/spine-keys#Offset)。

Offset 会在循环接缝建立键。连续保留同一次键选择继续移相，编辑器记住原始键集，不反复增加第二个接缝键；改选其他键后再 Offset，会重新建立接缝键。反复“选中 → Offset → 取消 → 再选”可能累积多余键。[Offset](https://esotericsoftware.com/spine-keys#Offset)。

Adjust 的数值例子：旋转键为 −10、25、70 度，整体加 8 度后为 −2、33、78 度，相邻角度差不变。它适合将整段摆动向上调整，不能作为将动作后半段延长 5 帧的工具。[Adjust](https://esotericsoftware.com/spine-dopesheet#Adjust)。

Clean Up 需要理解 `Layered`。高轨动作里，一个与 setup 一样的键仍可能用于覆盖下层动作；单看该动作，这个键像冗余，组合播放时却有意义。将用于分层覆盖的动画勾选 Layered，清理时保留此类键。如果应用会直接查找或修改特定键，也不要假设清理不影响这种代码约定。[Clean Up](https://esotericsoftware.com/spine-keys#Clean-Up)。

<a id="animation-a7"></a>



### A7. Dopesheet 的行、选择、过滤与同步

| 行／内容控制 | 语义 |
| --- | --- |
| Overview row | 汇总当前显示行；多骨架各有汇总行；选某帧汇总键会选其下的实际键 |
| Bone row | 折叠时汇总全部子属性；展开时只显示同帧多个属性同时有键的汇总 |
| Property row | 每种属性的实际键；点击变换属性名也会切到相应工具 |
| Interpolation 标记 | 直线 Linear、曲线 Bezier、虚线 Stepped；无连线可能同值或离散键；汇总行不画曲线 |
| Other rows | Event／Draw order 等不归属某骨骼的通道 |
| Unlocked | 跟随视口／Tree 所选骨骼；无选中时列出全部 |
| Locked | 固定行集合；切换对象仍保留这些行 |
| Refresh／Select | 用当前选择更新固定集合／反向选中当前显示行对应的骨骼 |
| Order | 以骨骼选择顺序起始排列，可拖动骨骼行排序 |
| Visibility | Skin bone、Skin attachment 的显示受皮肤及Tree设置影响 |
| Filters | 可多选属性类型，Current tool 随工具，Reset 选回全部，右击按钮临时开关 |
| Rows／Toolbar | 显示／隐藏左侧行区和工具栏，节省面板空间 |

Dopesheet不同键类型使用不同颜色，白色表示同帧汇总了多种键；滚轮滚动行，右键拖动可上下／左右浏览。骨骼名右击或左右的加减图标折叠／展开，单击骨骼／属性行名选相关对象；Cmd追加／切换对象选择。单击Overview中的动画名定位Tree。隐藏整个骨架也会隐藏其Graph／Dopesheet行。[行操作](https://esotericsoftware.com/spine-dopesheet#Rows)。

单击选键，Cmd 切换／追加选择。Cmd+A 先选同一行，再按选全部当前显示键。`Jump to key` 决定选择键是否同步移动时间指针；`Jump to frame` 决定空白点击是否移到相应时间。快速释放框选只选对象，框选后稍停再释放可保留选择框供缩放（本机4.3.26另有Box selection pause行为设置，关闭后无需等待）；Dopesheet 可用 Cmd 追加多个框。[Dopesheet](https://esotericsoftware.com/spine-dopesheet)。

**同步有两层。** Graph 与 Dopesheet 关键帧选择本来会互相对应；Sync 另控制显示内容。旧的 G→D 由 Graph 可见曲线决定 Dopesheet 行；4.3 增加 D→G，由 Dopesheet 选中键迅速显示 Graph 对应通道。上下相邻摆放时，同步时间缩放可使两面板的键横向对齐。旧网页只描述 G→D，其“锁定／筛选按钮被隐藏”不能未经核对推广到全部 4.3 模式。[4.3 Sync](https://esotericsoftware.com/blog/Spine-4.3-released)。

4.3.26 可绑定的筛选命令清单：Current Tool、Rotate、Translate／X／Y、Scale／X／Y、Shear／X／Y、Inherit、RGBA、RGB、Alpha、Attach、IK、Deform、Transform、Sequence、Path、Event、Physics、Draw Order、Slider。Graph 提供对应清单。某类型没有键时，选中该筛选不会替你建立时间轴。`本机快捷键清单`（本机 Spine settings/hotkeys-1.txt）。

按Space、Escape或视口空白双击清空骨骼选择，可在Unlocked模式列出所有骨骼。锁定时单击一个键同时选中该键的骨骼／附件，让视口调整更直接；Cmd在键上开始拖动仍可启动框选。筛选可Cmd／Shift多选，生效时按钮为红色。Dopesheet菜单Rows关闭是隐藏左侧名称区域，并非删掉时间轴。[内容和选择](https://esotericsoftware.com/spine-dopesheet#Contents)。

<a id="animation-a8"></a>



### A8. Graph 曲线、控制柄与编辑

Graph行上的可见点可右击或Cmd单击批量切换；Overview点控制当前所有行，Bone点控制该骨骼的属性行。Unlocked且无所选骨骼时，少量骨骼会自动显示其曲线，骨骼过多时通常只自动显示首个骨骼，避免曲线完全叠在一起。滚轮及右键拖动行区可以上下浏览；Hide Rows省出宽度，也同时失去用可见点选择曲线的入口。[Graph行](https://esotericsoftware.com/spine-graph#Rows)、[Unlocked](https://esotericsoftware.com/spine-graph#Unlocked)。

Graph 横轴是时间、纵轴是属性数值。一项属性可有多条曲线，例如 RGBA 有四条；没有拆分属性时，一个键一起记录各通道，但各连续通道的贝塞尔柄仍可独立调整。左侧 Overview／Bone／Property 的可见点控制实际画出的曲线；视口中选择骨骼与“曲线可见点”是两个层面的选择。[Graph Curves](https://esotericsoftware.com/spine-graph#Curves)。

| 曲线／柄操作 | 用途与边界 |
| --- | --- |
| Stepped | 前键保持直到后键 |
| Linear | 键间数值变化率固定；相同值键间保持该值 |
| Bezier | 通过两个控制柄安排键间节奏；再次点击已选的 Bezier 会重置柄 |
| Automatic | 邻键驱动柄角度，三角柄标记自动；手动拖后成为手动柄 |
| Separate | 左右柄可独立，允许速度在键处突然换向；Alt 拖柄临时控制 |
| Flat | 柄与键处于同一数值高度，键附近变化变缓 |
| Bounce | 柄分离并指向两侧过渡，便于表现碰撞后转向 |
| Ease out／Ease in | 分别放慢当前键附近／下一键附近的变化；名称按曲线区段方向理解 |
| Frame | 无选择时适配全部曲线；有选键／柄时适配这些选择 |
| Auto Frame | 自动调整视野，同时仍可手动浏览 |
| Axes X／Y | 仅改时间／仅改数值，减少拖动混入另一轴 |
| Snapping | 原帧／原值、其他键数值的辅助对齐；白线反馈实际吸附 |
| Hide Bezier Handles | 4.3.26 命令入口存在，用于管理柄显示 |

预设按钮同时将插值设为Bezier；不是仅移动柄却保留Linear／Stepped。曲线类型属于一个键到后续键的区段。4.3.65-beta引入末键查看前段曲线，4.3.24进一步限定为**只选中一个末键**时，曲线按钮可修改其之前区段，Curves 显示前一段；旧网页“最后键没有下一键所以没有曲线”的交互解释需要加上这个新规则。[Graph](https://esotericsoftware.com/spine-graph#Curve-types)、[4.3 变更](https://esotericsoftware.com/spine-changelog)。

| 手势 | 结果 |
| --- | --- |
| 右键拖动／滚轮 | 平移／同时缩放两轴；Alt+右键拖动可分别缩放两轴 |
| 单击键／柄，Cmd追加 | 选择对象；Jump to key 可同时定位时间 |
| 右击键 | 定位其骨骼／对象；Unlocked 时显示范围也可能随选择收窄 |
| Cmd+A两次 | 先同曲线的键／柄，再全部显示的键／柄 |
| 从空白框选 | 选键；要框选柄，先选择一个柄再 Cmd 开始拖框 |
| 框边拖动 | 比例调整时间或数值；可反向；Shift解除帧吸附 |
| Shift拖柄／Cmd+Shift拖柄 | 调柄长度／同时调两侧柄长度 |

关闭Repeat，首键左边显示setup值，末键右边显示末值；开启Repeat后，动作两边显示暗色循环参考。首尾同值且为Bezier时，接缝的另一侧柄也可显示；第0帧下方箭头增加零帧之前的空间。重叠曲线选中／悬停键或柄可置顶查看。曲线显示的小直线段反映运行时近似，若特大曲线可见分段，应增加必要的键而非仅放大图窗。[曲线显示](https://esotericsoftware.com/spine-graph#Curves)。

Snapping上／下拖键时优先保持原帧，左／右拖时优先保持原值；启用Key snapping后可对齐其他键的数值，并优先同曲线的键。先短暂停留在目标键上可偏向该键吸附；Shift+Alt临时切换键吸附，白线表示实际已吸附。开启Drag to edit后，已选键／柄还可从空白区域拖动编辑；关闭则需直接拖所选对象。[Snapping](https://esotericsoftware.com/spine-graph#Snapping)、[拖动](https://esotericsoftware.com/spine-graph#Manipulating-keys)。

新增键落在现有 Stepped段内沿用Stepped；落在现有 Bezier 段内，编辑器会调整邻柄以维持原曲线；插键后依然需要检查结果，尤其已经采用自动柄、分离柄或不同默认曲线设置的工程。4.3 的 Default curve type 提供 Last chosen，故“所有新键默认 Linear”的旧网页描述不能作为一律规则。[New keys](https://esotericsoftware.com/spine-graph#New-keys)、[4.3 曲线改进](https://esotericsoftware.com/blog/Spine-4.3-released)。

<a id="animation-a9"></a>



### A9. 改时间与改数值的控制柄维护

本机 4.3.26 Graph 面板菜单有 `Retiming` 和 `Revaluing`，不能只列旧版的两种 Handle mode。

| 菜单／模式 | 可公开确定的行为 |
| --- | --- |
| Retiming — None | 不使用 Shape／Value 的自动邻柄维护 |
| Retiming — Shape | 改键的时间时维护曲线形状；可能改变实际极值 |
| Retiming — Value | 横向调整柄，保持曲线先前达到的极值范围 |
| Revaluing — None | 关闭该项改值时的自动柄缩放维护 |
| Revaluing — Scale | 4.3 改键的值时缩放相关柄，减少变更后重新修柄的工作 |

本次 UI 只读取菜单并确认枚举，未修改曲线做数值实验。Shape／Value 的区分由 Guide 明确描述；Revaluing 的完整缩放公式和退化区段行为没有公开逐参数说明，不能从“Scale”名字编造固定倍率。实际工程可能已保存用户设置；截图里 Shape／Scale 被选中，不等于每个新工程默认相同。[Handle modes](https://esotericsoftware.com/spine-graph#Handle-modes)、[4.3 曲线说明](https://esotericsoftware.com/blog/Spine-4.3-released)。

本手册建议：先复制动画，再移动一键把区段时长减半，观察超调是否改变；随后只改该键的值，分别审阅接近峰值的速度。运动目标要求“最高只到某位置”时，特别检查 Value 模式与新值之间的关系；要求“保持弯曲节奏轮廓”时，再评估 Shape。这个检查方法不假定软件存在未公开的公式。

<a id="animation-a10"></a>



### A10. Favor、Store 与 Curves 面板

Favor 的调整对象由选择决定：有选键时改这些键；无选键时会给所有可见曲线建键再调整。拖左／右趋向前／后参考，超出滑条边界可产生超调；心形按钮回到中点，等价于清选择再重选建立新的调整参考，不是撤销刚才的键值修改。[Favor](https://esotericsoftware.com/spine-graph#Favor)。

| Favor 模式 | 参考／变化方式 |
| --- | --- |
| Favor | 靠近前／后键；一批键距参考较远者移动较慢 |
| Blend | 与 Favor 同类目标，但批量键以相同速度调整 |
| Shift | 批量一起移值，保留它们彼此关系 |
| Linear | 趋向前后参考键之间的直线 |
| Average (curve) | 同曲线前后参考值的平均 |
| Average (frame) | 同一帧其他所选键的平均 |
| Average (all) | 全部所选曲线前后参考值的平均 |
| Default | 属性默认值，如旋转0、scale1；不能泛称setup |
| Setup | 当前 setup pose 的值 |
| Store | 已存储参考曲线 |

Store 保存整条当前动画的键和柄，用背景曲线比较；再次 Store 清空，Swap 交换参考和当前状态。它不是正式动画副本或项目保存，也不应作为跨工程备份。需要可交付的备选版本，仍复制动画或保存工程。[Store](https://esotericsoftware.com/spine-graph#Store)。

**Curves 是已有功能，不能漏写。** 4.1 已恢复独立 Curves 面板；4.3.26 菜单可见。它把一键到下一键的变化按相对区段显示，用来把不同值域、方向甚至属性类型的插值节奏对齐；Graph 则让人看到整段动画的实际值与时间。Curves 不改变关键帧值。[4.1 发布说明](https://eu.esotericsoftware.com/blog/Spine-4-1-released)。

| Curves 控件／操作 | 意义 |
| --- | --- |
| Stepped／Linear／Bezier | 改选中区段的插值 |
| Match | 改一个显示的曲线柄时，匹配所有所选曲线；也控制预设是否应用到全部所选 |
| Separate | 编辑时分离键两侧的柄切线，可形成折点；不是把Translate X/Y拆成不同时间轴 |
| 彩色曲线／灰色曲线 | 首个选择突出显示，其余作为参考；选择另一曲线可使其成为突出者 |
| Presets | 保存、复用区段的曲线形状；4.3界面右侧列表有新增、替换、删除图标 |

官方建议先在 Graph 理解数值变化，再用 Curves 批量匹配。匹配曲线的目标是“改变进程一样”，不会把 +100 的位移与 +10 度旋转改成相同单位。相邻键同值时，Curves 无法用该差值显示某些曲线，Graph 仍适合查看同值段是否有超调。[Nate 对 Curves 的解释](https://esotericsoftware.com/forum/d/27891-why-are-there-two-curves-in-the-curve-view)、[Match 预设](https://jp.esotericsoftware.com/forum/d/17597-curves-presets-and-selected-parameters-bugrequest-4126b/5)、[2026 曲线匹配工作流](https://ko.esotericsoftware.com/forum/d/30108-spine%E6%9B%B2%E7%BA%BF%E8%B0%83%E8%8A%82)。

<a id="animation-a11"></a>



### A11. Events、声音文件与波形

创建事件：Tree Events → New Event。在 Setup 给 Integer、Float、String 默认值；Animate 中在所需时刻改数据并给事件打键。事件的数值、文本及音频 Volume／Balance 作为该次发生的数据记录。文件夹进入最终事件名，例如 `combat/slash`；改变文件夹等于改变运行时要识别的名称。[Events](https://esotericsoftware.com/spine-events)。

| 参数／入口 | 用法与文件边界 |
| --- | --- |
| Integer／Float／String | 整数／小数／文本，事件键可分别携带不同内容 |
| Audio path | 从骨架 Audio 目录寻找的相对资源名；设置后成为audio event |
| Volume | 此事件键的试听音量；与Audio面板总音量独立 |
| Balance | 双声道调左右声道音量，单声道做左右声像 |
| Audio node path | 相对于项目文件的位置或绝对路径；监控文件变化 |
| New Event（选音频文件） | 建同名事件并设置资源路径；也可把文件拖到已有事件 |
| 文件图标 | 红表示没有事件使用，绿表示至少一个事件使用 |
| Limit scanning | 默认只显示最先发现的2000个音频文件，必要时解除限制 |
| WAV／MP3／OGG | WAV须PCM、1或2声道、每采样16位 |

事件的Integer、Float、String、Volume、Balance由同一事件键一起记录；不是先给五个属性分别建立五条事件通道。若只改数据不按事件键按钮，当前修改不会自动变成一次事件发生。[事件记录粒度](https://esotericsoftware.com/spine-keys#Events)。

查找路径允许子目录与省略扩展名；例如 Audio 为 `./sfx/`，事件 Audio path 为 `weapon/swing`，可找到该目录的对应 wav/mp3/ogg。大小写在部分系统影响查找，交付时检查资源名一致。[Audio file lookup](https://esotericsoftware.com/spine-events#Audio-file-lookup)。

Audio view 给活动动画的每个声音事件键分配颜色并显示波形；点击列表项移到该键，其他波形变暗。清除Tree中事件的可见点会抑制编辑器播放、波形和视口事件名提示。Audio view 的总音量、静音和输出设备用于编辑器试听。普通事件不会因隐藏提示而自动删除键。要统一隐藏视口所有事件名，可让Graph和Dopesheet两者筛选都排除Event；这与逐事件清除可见点不同。[Audio view](https://esotericsoftware.com/spine-audio-view)。

官方明确 runtime 不统一管理音频系统，应用根据事件数据自己播放；声音已开始后是否停止、重叠、跟随角色距离，均由应用决定。事件不在 Essential 提供。编辑器查看到的波形也不是游戏渲染对象；导出视频支持的音轨选项由导出格式决定。[Events runtime 边界](https://esotericsoftware.com/spine-events)。

本手册建议的声音制作检查：准备相对目录 → 选文件建audio event → 录入脚步键 → 用波形定位接触时刻 → 常速试听 → 低速检查 → 在应用验证事件名和声音库映射。多个脚步可复用一个事件，以Integer／String区分左右脚或地面材质；应用应先约定这些数据的解释。

<a id="animation-a12"></a>



### A12. Sequence 的资源、七种模式与时间

Region／Mesh 可启用 Sequence，用同路径的一组编号图片作为单一附件；素材须一致宽高，Image path 填编号前的完整路径，序列字段指定编号范围，Setup模式的Frame决定未被序列键覆盖时显示的图。[Regions Sequence](https://esotericsoftware.com/spine-regions#Sequence)。

| Sequence key 模式 | 时间往后时的帧进程 |
| --- | --- |
| Hold | 保持指定Frame |
| Once | 从指定起始状态向前播放一次 |
| Loop | 向前循环 |
| Pingpong | 到末端后反向，再向前往返 |
| Once reverse | 反向一次 |
| Loop reverse | 反向循环 |
| Pingpong reverse | 先向开头反向，再往末端，持续往返 |
| FPS | 控制序列换图片的速度，与工程Timeline FPS独立 |
| Frame | 指定该序列键的当前帧位置／起始索引 |

例如工程30 FPS、序列10 FPS时，连续播放约每3个工程帧换一张图；骨骼仍可在这3帧内连续运动。Sequence 会产生序列时间轴，Slot Attachment 则负责附件整体出现／消失／切换，两者可一起使用。一次播放结束后是否隐藏，不能靠“Once”名字推断，应另设空附件键或由应用处理。[Sequence keys](https://esotericsoftware.com/spine-keys#Sequence-keys)。

本手册建议：先将所有图片放同一目录并核对尺寸 → 填路径前缀与范围 → 用Frame检查每一张 → 给Sequence key指定模式及FPS → 给Slot Attachment设开始／结束显隐 → 检查循环和皮肤切换。图片缺号处理、序列范围字段输入以及UI Frame与运行时Index转换，在网页中没有完整输入语法；补零的资源规则见下面源码说明。本参考不编造“必须从0或1开始”规则，实际工程以编辑器读到的帧和导出数据核对。

**导出数据与运行时编号规则已有源码可查。** 4.3 Sequence使用`start`作为编号起点，`digits`作为最少位数（不足左补0），`count`作为图片数；内部index从0计，资源后缀为`start + index`。例如前缀`spark_`、start=3、digits=3，index0查`spark_003`，index1查`spark_004`。`setupIndex`决定setup使用第几项；这些是数据/运行时名，本参考不把它们冒称为UI控件标签。无编号后缀的序列另有pathSuffix=false，原路径直接使用。[4.3 Sequence源码](https://raw.githubusercontent.com/EsotericSoftware/spine-runtimes/4.3/spine-ts/spine-core/src/attachments/Sequence.ts)。

Region 的一般setup属性补充：空 Image path 用附件名查图，勾 Mesh 转为网格；Region自身变换不可普通打动画键，应动画其骨骼。Region可从Images资源创建、PSD导入或图片编辑器数据导入；Select、Export、Name、Color、Set Parent属于附件通用属性，由主册附件章节展开。Region坐标是图片中心；奇数尺寸图片需要半像素偏移才能与屏幕像素对齐。[Regions](https://esotericsoftware.com/spine-regions)。

<a id="animation-a13"></a>



### A13. 播放、选择和曲线相关命令索引

以下是本机4.3.26可绑定的命令入口。空绑定只说明用户尚未配置；不能当作功能不可用。未给默认快捷键者请到 Settings 的 Hotkeys 对应文件查实际绑定。

| 命令组 | 全部相关命令入口 |
| --- | --- |
| Animate | Auto Key；Animation Clean Up；Next／Previous Animation；Track Current Frame；Frame Keys；Setup Draw Order；Setup Pose |
| Keying | Key Edited／Active／Selected／Shown／Shown Hold；Rotation；Translate／X／Y；Scale／X／Y；Shear／X／Y；Color；Attachment；Select Keys／Previous Keys；Stepped／Linear／Bezier Curve；Shift／Offset／Adjust Keys；Key Constrained |
| Playback | Play Forward／Backward；各自的Reset版本；Stop；Repeat；Stepped；Interpolated；First／Last／Next／Previous Key；Next／Previous Frame及10帧版；Loop Start／End；Speed Slower／Faster／100%；Timeline Pan Drag／Move；Timeline Frame Drag／Move |
| Graph | Lock／Refresh／Select Bones；Filter及全部属性类型；Toolbar／Rows；Hide Bezier Handles；Frame／Auto Frame；Handle Auto／Separate／Bounce／Flat／Ease Out／Ease In／Ease；Snapping；X／Y限制及Momentary；Retiming None／Shape／Value；Revaluing None／Scale；Store／Swap |
| Favor | 开关；加／减5、10、15；Favor／Blend／Shift／Linear／Average(curve,frame,all)／Default／Setup／Store |
| Dopesheet | Sync；Lock／Refresh／Select Bones；Filter及全部属性类型；Rows／Toolbar；Set Loop Start／End |

功能含义参看上面各节；Momentary是在按键保持期间使用的限制入口，实际按键绑定与焦点影响是否触发。播放正反向不代表游戏同样自动启用反向；4.3 runtime 的 `TrackEntry.reverse`、反向事件设置由运行时参考说明。不要把旧运行时“不支持AnimationState反向”的描述套用4.3。

依据：`本机快捷键清单`（本机 Spine settings/hotkeys-1.txt）、[Spine快捷键设置](https://esotericsoftware.com/spine-settings#Hotkeys)。本节是入口清单，后面的公开资料差异表明确指出没有完整官方语义说明的项。

<a id="animation-a14"></a>



### A14. 核对边界与公开资料差异

| 项目 | 本次结果 | 手册如何处理 |
| --- | --- | --- |
| Key Edited／Active／Selected／Shown | 官方团队清楚区分；本机清单确认入口 | 已分别说明，纠正旧Keys橙色骨骼按钮描述 |
| Paste Keys - Hold | 4.3.22记录明确同值键使用Linear；官方论坛补充快捷键冲突 | 已说明，不写成所有键Stepped |
| Key Shown Hold | 4.3已实现及撤销修复可核查；缺全部特殊通道语义 | 说明用途，不发明事件／Sequence上的保证 |
| Graph Retiming | Guide有Shape／Value语义；UI另确认None | 已逐模式列出 |
| Graph Revaluing | 4.3发布和UI确认None／Scale | 未公布缩放公式、极值边界和退化段行为 |
| D→G Sync | 4.3发布证实；旧Guide仅写G→D | 已区分方向，未笼统说全部模式隐藏锁定按钮 |
| Skin attachment可见性 | Graph／Dopesheet网页称取消Show all；Keys网页称勾选Show all | 按设置意图和4.3改名理解；不照抄矛盾开关方向 |
| Constrained key | 本机命令及4.3发布确认 | 具体受控值与约束移除流程交给约束附录 |
| 曲线默认值 | Last chosen是4.3新选项；用户工程可保存其他模式 | 不保证“所有新键Linear”或在机状态等于产品默认 |
| Sequence编号输入 | 源码确定start+index及digits补零；完整UI范围字段语法未实测 | 已补资源编号例子，保留UI输入语法边界 |
| 数值范围 | Volume／Balance及部分滑条范围未逐个公开 | 写意义，未猜范围或默认值 |
| 实际操作核验 | 原生UI只读看过Curves、Graph菜单及Settings行为选项；没有修改任何键测试 | 资料核对不等于逐功能行为实测 |
| 仅存在于命令清单的细分入口 | Timeline Pan/Frame Drag/Move、Play Reset等缺完整按键行为文字 | A13保留入口，不编造拖动倍率、焦点与重置边界 |

本参考可覆盖公开命名功能与操作边界；仍然保留上述资料未明确规定的行为，不将这些未知项目改写成确定结论。

<a id="animation-a15"></a>



### A15. 官方小节覆盖账本

下表按官方正文标题逐项列出。`已说明` 表示本文件已有实质功能说明；`主册负责` 表示不重复附件／约束参数正文；`导航` 表示视频、练习或章节父标题，仅提供对应入口，不作为独立产品功能计数。各页父标题也保留以便审计，不能把父标题数量当作功能数量。


| 官方页 | 官方小节 | 实质说明位置 | 状态 | 版本／分工边界 |
| --- | --- | --- | --- | --- |
| keys | [Keys](https://esotericsoftware.com/spine-keys#Keys) | [A1 动画对象、列表与工作方式](#animation-a1) | 已说明 | — |
| keys | [Animations](https://esotericsoftware.com/spine-keys#Animations) | [A1 动画对象、列表与工作方式](#animation-a1) | 已说明 | — |
| keys | [Folders](https://esotericsoftware.com/spine-keys#Folders) | [A1 动画对象、列表与工作方式](#animation-a1) | 已说明 | — |
| keys | [Timeline](https://esotericsoftware.com/spine-keys#Timeline) | [A2 时间、帧、范围与播放](#animation-a2) | 已说明 | — |
| keys | [Timeline position](https://esotericsoftware.com/spine-keys#Timeline-position) | [A2 时间、帧、范围与播放](#animation-a2) | 已说明 | — |
| keys | [Frames](https://esotericsoftware.com/spine-keys#Frames) | [A2 时间、帧、范围与播放](#animation-a2) | 已说明 | — |
| keys | [Frame snapping](https://esotericsoftware.com/spine-keys#Frame-snapping) | [A2 时间、帧、范围与播放](#animation-a2) | 已说明 | — |
| keys | [Repeat](https://esotericsoftware.com/spine-keys#Repeat) | [A2 时间、帧、范围与播放](#animation-a2) | 已说明 | — |
| keys | [Setting keys](https://esotericsoftware.com/spine-keys#Setting-keys) | [A3 打帧入口与颜色状态](#animation-a3) | 已说明 | A3纠正旧版橙色骨骼按钮 |
| keys | [Auto key](https://esotericsoftware.com/spine-keys#Auto-key) | [A3 打帧入口与颜色状态](#animation-a3) | 已说明 | — |
| keys | [Key shown](https://esotericsoftware.com/spine-keys#Key-shown) | [A3 打帧入口与颜色状态](#animation-a3) | 已说明 | — |
| keys | [Keyable properties](https://esotericsoftware.com/spine-keys#Keyable-properties) | [A4 可动画属性的记录粒度](#animation-a4) | 已说明 | — |
| keys | [Bone transforms](https://esotericsoftware.com/spine-keys#Bone-transforms) | [A4 可动画属性的记录粒度](#animation-a4) | 已说明 | — |
| keys | [Setup pose](https://esotericsoftware.com/spine-keys#Setup-pose) | [A4 可动画属性的记录粒度](#animation-a4) | 已说明 | — |
| keys | [Separate X and Y](https://esotericsoftware.com/spine-keys#Separate-X-and-Y) | [A4 可动画属性的记录粒度](#animation-a4) | 已说明 | — |
| keys | [Transform inheritance](https://esotericsoftware.com/spine-keys#Transform-inheritance) | [A4 可动画属性的记录粒度](#animation-a4) | 已说明 | — |
| keys | [Slot attachment](https://esotericsoftware.com/spine-keys#Slot-attachment) | [A4 可动画属性的记录粒度](#animation-a4) | 已说明 | — |
| keys | [Slot color](https://esotericsoftware.com/spine-keys#Slot-color) | [A4 可动画属性的记录粒度](#animation-a4) | 已说明 | — |
| keys | [Separate color and alpha](https://esotericsoftware.com/spine-keys#Separate-color-and-alpha) | [A4 可动画属性的记录粒度](#animation-a4) | 已说明 | — |
| keys | [Draw order](https://esotericsoftware.com/spine-keys#Draw-order) | [A4 可动画属性的记录粒度](#animation-a4) | 已说明 | — |
| keys | [Events](https://esotericsoftware.com/spine-keys#Events) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| keys | [Sequence keys](https://esotericsoftware.com/spine-keys#Sequence-keys) | [A12 Sequence 的资源、七种模式与时间](#animation-a12) | 已说明 | — |
| keys | [Deform keys](https://esotericsoftware.com/spine-keys#Deform-keys) | [A4 可动画属性的记录粒度](#animation-a4) | 已说明 | — |
| keys | [Deform highlight](https://esotericsoftware.com/spine-keys#Deform-highlight) | [A4 可动画属性的记录粒度](#animation-a4) | 已说明 | — |
| keys | [IK constraints](https://esotericsoftware.com/spine-keys#IK-constraints) | [A4 可动画属性的记录粒度](#animation-a4) | 主册负责 | A4列记录组；完整4.3参数见约束附录 |
| keys | [Transform constraints](https://esotericsoftware.com/spine-keys#Transform-constraints) | [A4 可动画属性的记录粒度](#animation-a4) | 主册负责 | A4列记录组；完整4.3参数见约束附录 |
| keys | [Path constraints](https://esotericsoftware.com/spine-keys#Path-constraints) | [A4 可动画属性的记录粒度](#animation-a4) | 主册负责 | A4列记录组；完整4.3参数见约束附录 |
| keys | [Manipulating keys](https://esotericsoftware.com/spine-keys#Manipulating-keys) | [A5 关键帧剪贴板、姿态剪贴板与 Hold](#animation-a5) | 已说明 | — |
| keys | [Clipboard buttons](https://esotericsoftware.com/spine-keys#Clipboard-buttons) | [A5 关键帧剪贴板、姿态剪贴板与 Hold](#animation-a5) | 已说明 | — |
| keys | [Shift](https://esotericsoftware.com/spine-keys#Shift) | [A6 Shift、Offset、Adjust、缩放与清理](#animation-a6) | 已说明 | — |
| keys | [Offset](https://esotericsoftware.com/spine-keys#Offset) | [A6 Shift、Offset、Adjust、缩放与清理](#animation-a6) | 已说明 | — |
| keys | [Clean Up](https://esotericsoftware.com/spine-keys#Clean-Up) | [A6 Shift、Offset、Adjust、缩放与清理](#animation-a6) | 已说明 | — |
| keys | [Hands on](https://esotericsoftware.com/spine-keys#Hands-on) | [A4 可动画属性的记录粒度](#animation-a4) | 导航 | 官方学习入口，非另一个操作选项 |
| keys | [Video](https://esotericsoftware.com/spine-keys#Video) | [A4 可动画属性的记录粒度](#animation-a4) | 导航 | 官方学习入口，非另一个操作选项 |
| animating | [Animating](https://esotericsoftware.com/spine-animating#Animating) | [A1 动画对象、列表与工作方式](#animation-a1) | 已说明 | — |
| animating | [Workflow](https://esotericsoftware.com/spine-animating#Workflow) | [A1 动画对象、列表与工作方式](#animation-a1) | 已说明 | — |
| animating | [Straight ahead](https://esotericsoftware.com/spine-animating#Straight-ahead) | [A1 动画对象、列表与工作方式](#animation-a1) | 已说明 | — |
| animating | [Pose to pose](https://esotericsoftware.com/spine-animating#Pose-to-pose) | [A1 动画对象、列表与工作方式](#animation-a1) | 已说明 | — |
| animating | [Layered](https://esotericsoftware.com/spine-animating#Layered) | [A1 动画对象、列表与工作方式](#animation-a1) | 已说明 | — |
| animating | [Combined](https://esotericsoftware.com/spine-animating#Combined) | [A1 动画对象、列表与工作方式](#animation-a1) | 已说明 | — |
| animating | [Curves](https://esotericsoftware.com/spine-animating#Curves) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| animating | [Videos](https://esotericsoftware.com/spine-animating#Videos) | [A1 动画对象、列表与工作方式](#animation-a1) | 导航 | 官方学习入口，非另一个操作选项 |
| regions | [Region attachments](https://esotericsoftware.com/spine-regions#Region-attachments) | [A12 Sequence 的资源、七种模式与时间](#animation-a12) | 已说明 | — |
| regions | [Setup](https://esotericsoftware.com/spine-regions#Setup) | [A12 Sequence 的资源、七种模式与时间](#animation-a12) | 已说明 | — |
| regions | [Properties](https://esotericsoftware.com/spine-regions#Properties) | [A12 Sequence 的资源、七种模式与时间](#animation-a12) | 主册负责 | 通用附件属性见主册；A12列资源/序列特性 |
| regions | [Image path](https://esotericsoftware.com/spine-regions#Image-path) | [A12 Sequence 的资源、七种模式与时间](#animation-a12) | 已说明 | — |
| regions | [Mesh](https://esotericsoftware.com/spine-regions#Mesh) | [A12 Sequence 的资源、七种模式与时间](#animation-a12) | 已说明 | — |
| regions | [Sequence](https://esotericsoftware.com/spine-regions#Sequence) | [A12 Sequence 的资源、七种模式与时间](#animation-a12) | 已说明 | — |
| regions | [Translation](https://esotericsoftware.com/spine-regions#Translation) | [A12 Sequence 的资源、七种模式与时间](#animation-a12) | 已说明 | — |
| regions | [Video](https://esotericsoftware.com/spine-regions#Video) | [A12 Sequence 的资源、七种模式与时间](#animation-a12) | 导航 | 官方学习入口，非另一个操作选项 |
| events | [Events](https://esotericsoftware.com/spine-events#Events) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| events | [Setup](https://esotericsoftware.com/spine-events#Setup) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| events | [Properties](https://esotericsoftware.com/spine-events#Properties) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| events | [Integer](https://esotericsoftware.com/spine-events#Integer) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| events | [Float](https://esotericsoftware.com/spine-events#Float) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| events | [String](https://esotericsoftware.com/spine-events#String) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| events | [Audio path](https://esotericsoftware.com/spine-events#Audio-path) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| events | [Volume](https://esotericsoftware.com/spine-events#Volume) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| events | [Balance](https://esotericsoftware.com/spine-events#Balance) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| events | [Audio node](https://esotericsoftware.com/spine-events#Audio-node) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| events | [Audio events](https://esotericsoftware.com/spine-events#Audio-events) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| events | [Audio file lookup](https://esotericsoftware.com/spine-events#Audio-file-lookup) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| events | [Audio formats](https://esotericsoftware.com/spine-events#Audio-formats) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| events | [Viewport events](https://esotericsoftware.com/spine-events#Viewport-events) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| events | [Folders](https://esotericsoftware.com/spine-events#Folders) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| events | [Video](https://esotericsoftware.com/spine-events#Video) | [A11 Events、声音文件与波形](#animation-a11) | 导航 | 官方学习入口，非另一个操作选项 |
| animations-view | [Animations view](https://esotericsoftware.com/spine-animations-view#Animations-view) | [A1 动画对象、列表与工作方式](#animation-a1) | 已说明 | — |
| animations-view | [Animation list](https://esotericsoftware.com/spine-animations-view#Animation-list) | [A1 动画对象、列表与工作方式](#animation-a1) | 已说明 | — |
| animations-view | [Video](https://esotericsoftware.com/spine-animations-view#Video) | [A1 动画对象、列表与工作方式](#animation-a1) | 导航 | 官方学习入口，非另一个操作选项 |
| audio-view | [Audio view](https://esotericsoftware.com/spine-audio-view#Audio-view) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| audio-view | [Audio events](https://esotericsoftware.com/spine-audio-view#Audio-events) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| audio-view | [Volume](https://esotericsoftware.com/spine-audio-view#Volume) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| audio-view | [Audio device](https://esotericsoftware.com/spine-audio-view#Audio-device) | [A11 Events、声音文件与波形](#animation-a11) | 已说明 | — |
| dopesheet | [Dopesheet view](https://esotericsoftware.com/spine-dopesheet#Dopesheet-view) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| dopesheet | [Rows](https://esotericsoftware.com/spine-dopesheet#Rows) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| dopesheet | [Overview row](https://esotericsoftware.com/spine-dopesheet#Overview-row) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| dopesheet | [Bone rows](https://esotericsoftware.com/spine-dopesheet#Bone-rows) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| dopesheet | [Property rows](https://esotericsoftware.com/spine-dopesheet#Property-rows) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| dopesheet | [Interpolation](https://esotericsoftware.com/spine-dopesheet#Interpolation) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| dopesheet | [Other rows](https://esotericsoftware.com/spine-dopesheet#Other-rows) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| dopesheet | [Contents](https://esotericsoftware.com/spine-dopesheet#Contents) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| dopesheet | [Unlocked](https://esotericsoftware.com/spine-dopesheet#Unlocked) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| dopesheet | [Locked](https://esotericsoftware.com/spine-dopesheet#Locked) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| dopesheet | [Refresh](https://esotericsoftware.com/spine-dopesheet#Refresh) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| dopesheet | [Select](https://esotericsoftware.com/spine-dopesheet#Select) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| dopesheet | [Order](https://esotericsoftware.com/spine-dopesheet#Order) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| dopesheet | [Visibility](https://esotericsoftware.com/spine-dopesheet#Visibility) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | 旧网页开关方向矛盾见A14 |
| dopesheet | [Filters](https://esotericsoftware.com/spine-dopesheet#Filters) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| dopesheet | [Timeline position](https://esotericsoftware.com/spine-dopesheet#Timeline-position) | [A2 时间、帧、范围与播放](#animation-a2) | 已说明 | — |
| dopesheet | [Loop](https://esotericsoftware.com/spine-dopesheet#Loop) | [A2 时间、帧、范围与播放](#animation-a2) | 已说明 | — |
| dopesheet | [Selection](https://esotericsoftware.com/spine-dopesheet#Selection) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| dopesheet | [Box selection](https://esotericsoftware.com/spine-dopesheet#Box-selection) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| dopesheet | [Manipulating keys](https://esotericsoftware.com/spine-dopesheet#Manipulating-keys) | [A5 关键帧剪贴板、姿态剪贴板与 Hold](#animation-a5) | 已说明 | — |
| dopesheet | [Clipboard buttons](https://esotericsoftware.com/spine-dopesheet#Clipboard-buttons) | [A5 关键帧剪贴板、姿态剪贴板与 Hold](#animation-a5) | 已说明 | — |
| dopesheet | [Shift](https://esotericsoftware.com/spine-dopesheet#Shift) | [A6 Shift、Offset、Adjust、缩放与清理](#animation-a6) | 已说明 | — |
| dopesheet | [Offset](https://esotericsoftware.com/spine-dopesheet#Offset) | [A6 Shift、Offset、Adjust、缩放与清理](#animation-a6) | 已说明 | — |
| dopesheet | [Adjust](https://esotericsoftware.com/spine-dopesheet#Adjust) | [A6 Shift、Offset、Adjust、缩放与清理](#animation-a6) | 已说明 | — |
| dopesheet | [Key shown](https://esotericsoftware.com/spine-dopesheet#Key-shown) | [A3 打帧入口与颜色状态](#animation-a3) | 已说明 | — |
| dopesheet | [Sync](https://esotericsoftware.com/spine-dopesheet#Sync) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | A7区分旧G→D及4.3 D→G |
| dopesheet | [View settings](https://esotericsoftware.com/spine-dopesheet#View-settings) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| dopesheet | [Toolbar](https://esotericsoftware.com/spine-dopesheet#Toolbar) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| dopesheet | [Rows](https://esotericsoftware.com/spine-dopesheet#Rows) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| dopesheet | [Video](https://esotericsoftware.com/spine-dopesheet#Video) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 导航 | 官方学习入口，非另一个操作选项 |
| graph | [Graph](https://esotericsoftware.com/spine-graph#Graph) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Rows](https://esotericsoftware.com/spine-graph#Rows) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Overview row](https://esotericsoftware.com/spine-graph#Overview-row) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Bone rows](https://esotericsoftware.com/spine-graph#Bone-rows) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Property rows](https://esotericsoftware.com/spine-graph#Property-rows) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Other rows](https://esotericsoftware.com/spine-graph#Other-rows) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Contents](https://esotericsoftware.com/spine-graph#Contents) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| graph | [Unlocked](https://esotericsoftware.com/spine-graph#Unlocked) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| graph | [Locked](https://esotericsoftware.com/spine-graph#Locked) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| graph | [Refresh](https://esotericsoftware.com/spine-graph#Refresh) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| graph | [Select](https://esotericsoftware.com/spine-graph#Select) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| graph | [Order](https://esotericsoftware.com/spine-graph#Order) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| graph | [Visibility](https://esotericsoftware.com/spine-graph#Visibility) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | 旧网页开关方向矛盾见A14 |
| graph | [Filters](https://esotericsoftware.com/spine-graph#Filters) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | — |
| graph | [Curves](https://esotericsoftware.com/spine-graph#Curves) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Repeat](https://esotericsoftware.com/spine-graph#Repeat) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Separate properties](https://esotericsoftware.com/spine-graph#Separate-properties) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | A4分离X/Y、RGB/A；A8区分独立柄 |
| graph | [Curve types](https://esotericsoftware.com/spine-graph#Curve-types) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Stepped](https://esotericsoftware.com/spine-graph#Stepped) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Linear](https://esotericsoftware.com/spine-graph#Linear) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Bezier](https://esotericsoftware.com/spine-graph#Bezier) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Presets](https://esotericsoftware.com/spine-graph#Presets) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Automatic](https://esotericsoftware.com/spine-graph#Automatic) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Separate](https://esotericsoftware.com/spine-graph#Separate) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Flat](https://esotericsoftware.com/spine-graph#Flat) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Bounce](https://esotericsoftware.com/spine-graph#Bounce) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Ease out](https://esotericsoftware.com/spine-graph#Ease-out) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Ease in](https://esotericsoftware.com/spine-graph#Ease-in) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Navigation](https://esotericsoftware.com/spine-graph#Navigation) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Frame](https://esotericsoftware.com/spine-graph#Frame) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Auto](https://esotericsoftware.com/spine-graph#Auto) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Selection](https://esotericsoftware.com/spine-graph#Selection) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Sync](https://esotericsoftware.com/spine-graph#Sync) | [A7 Dopesheet 的行、选择、过滤与同步](#animation-a7) | 已说明 | A7区分旧G→D及4.3 D→G |
| graph | [Box selection](https://esotericsoftware.com/spine-graph#Box-selection) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | A8手势及A7/A14的4.3行为设置限定 |
| graph | [Manipulating keys](https://esotericsoftware.com/spine-graph#Manipulating-keys) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [New keys](https://esotericsoftware.com/spine-graph#New-keys) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Axes](https://esotericsoftware.com/spine-graph#Axes) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Snapping](https://esotericsoftware.com/spine-graph#Snapping) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Clipboard buttons](https://esotericsoftware.com/spine-graph#Clipboard-buttons) | [A5 关键帧剪贴板、姿态剪贴板与 Hold](#animation-a5) | 已说明 | — |
| graph | [Shift](https://esotericsoftware.com/spine-graph#Shift) | [A6 Shift、Offset、Adjust、缩放与清理](#animation-a6) | 已说明 | — |
| graph | [Offset](https://esotericsoftware.com/spine-graph#Offset) | [A6 Shift、Offset、Adjust、缩放与清理](#animation-a6) | 已说明 | — |
| graph | [Key shown](https://esotericsoftware.com/spine-graph#Key-shown) | [A3 打帧入口与颜色状态](#animation-a3) | 已说明 | — |
| graph | [Handle modes](https://esotericsoftware.com/spine-graph#Handle-modes) | [A9 改时间与改数值的控制柄维护](#animation-a9) | 已说明 | 旧标题；A9另补4.3 Retiming/Revaluing及枚举 |
| graph | [Value](https://esotericsoftware.com/spine-graph#Value) | [A9 改时间与改数值的控制柄维护](#animation-a9) | 已说明 | — |
| graph | [Shape](https://esotericsoftware.com/spine-graph#Shape) | [A9 改时间与改数值的控制柄维护](#animation-a9) | 已说明 | — |
| graph | [Favor](https://esotericsoftware.com/spine-graph#Favor) | [A10 Favor、Store 与 Curves 面板](#animation-a10) | 已说明 | — |
| graph | [Store](https://esotericsoftware.com/spine-graph#Store) | [A10 Favor、Store 与 Curves 面板](#animation-a10) | 已说明 | — |
| graph | [View settings](https://esotericsoftware.com/spine-graph#View-settings) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Hide toolbar](https://esotericsoftware.com/spine-graph#Hide-toolbar) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Hide rows](https://esotericsoftware.com/spine-graph#Hide-rows) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 已说明 | — |
| graph | [Video](https://esotericsoftware.com/spine-graph#Video) | [A8 Graph 曲线、控制柄与编辑](#animation-a8) | 导航 | 官方学习入口，非另一个操作选项 |
| playback | [Playback view](https://esotericsoftware.com/spine-playback#Playback-view) | [A2 时间、帧、范围与播放](#animation-a2) | 已说明 | — |
| playback | [Timeline FPS](https://esotericsoftware.com/spine-playback#Timeline-FPS) | [A2 时间、帧、范围与播放](#animation-a2) | 已说明 | — |
| playback | [Changing the timeline FPS](https://esotericsoftware.com/spine-playback#Changing-the-timeline-FPS) | [A2 时间、帧、范围与播放](#animation-a2) | 已说明 | — |
| playback | [Speed](https://esotericsoftware.com/spine-playback#Speed) | [A2 时间、帧、范围与播放](#animation-a2) | 已说明 | — |
| playback | [Stepped](https://esotericsoftware.com/spine-playback#Stepped) | [A2 时间、帧、范围与播放](#animation-a2) | 已说明 | — |
| playback | [Interpolated](https://esotericsoftware.com/spine-playback#Interpolated) | [A2 时间、帧、范围与播放](#animation-a2) | 已说明 | — |
| timeline | [Timeline view](https://esotericsoftware.com/spine-timeline#Timeline-view) | [A2 时间、帧、范围与播放](#animation-a2) | 已说明 | — |

账本共 **161 个官方标题**：已说明 149，主册负责 4，导航 8。同时另补4.3发布／变更记录中的Key Constrained、Hold、新同步方向、Retiming／Revaluing、末键前段曲线规则及当前可绑定命令。标题计数只用于逐页核对，不能等同于独立功能数量。


<a id="reference-pipeline"></a>

## 29. 工作区、资源、导入导出、CLI 与设置逐项参考

- [P1. 工作区、工程入口与文件选择](#pipeline-p1)

- [P2. Images 资源与查找规则](#pipeline-p2)

- [P3. Tree 的全部操作类别](#pipeline-p3)

- [P4. 辅助工作面板](#pipeline-p4)

- [P5. Project / Data 导入的决策表](#pipeline-p5)

- [P6. PSD 输入、标签与命名](#pipeline-p6)

- [P7. 导出格式与字段参考](#pipeline-p7)

- [P8. Texture Packer / Unpacker 参数与配置](#pipeline-p8)

- [P9. CLI完整任务表、参数与可复用调用](#pipeline-p9)

- [P10. Settings逐项参考](#pipeline-p10)

- [P11. Versioning与工程归档](#pipeline-p11)

- [P12. 官方子目录覆盖台账](#pipeline-p12)

核查日期：2026-10-09。供主手册第 3、6、15—20、22、24 章合并使用。本文件以官方用户指南的子目录为检查单位，并补入更新记录能确定的 4.3 差异。术语后的英文是定位控件的索引；推荐工作步骤是本手册编排的操作方案，不冒充厂商原文。

三个容易混淆的已有功能：**WEBP、AWEBP、WEBM 是已有导出格式；Slot Color 在 4.3 改名为 Color；Show inactive skin bones 是旧 Hide viewport skin bones 的反向表达。** HTML 才是 4.3 新增的导出入口。后文区分可确认的功能与文档尚未公开的参数默认值。

<a id="pipeline-p1"></a>

### P1. 工作区、工程入口与文件选择

#### P1.1 主菜单与标题栏

主菜单入口为左上 Spine 标志；默认 `Alt+F`，Mac 为 `Option+F`。标题栏提供打开、保存、撤销、重做、Welcome。打开按钮拖动显示近期工程；按 Shift 点击保存进入另存为。工程可从菜单、快捷键、最近记录、系统双击、拖入窗口或 Welcome 示例打开。默认 `Ctrl+O`，Mac 用 `Cmd+O`；加 Shift 选择系统文件对话框。邮件图标的未读状态提示新消息或更新记录。[官方 UI](https://esotericsoftware.com/spine-ui#Main-menu)

编排建议：准备进入新版本的工程先另存独立副本，然后记录所用编辑器完整版本与 runtime 分支。标题栏出现未保存状态时，先明确是要保留当前修改，还是恢复磁盘版本；不要把导出成功等同于工程已保存。

#### P1.2 Spine 文件对话框

常用文件或文件夹可以加星固定；右键删除最近记录。输入文字过滤，Enter 选择首项；空格或 Browse 打开文件选择器。条目旁图标可定位到对应目录。可从系统复制文件，再在对话框粘贴路径。这里移除的是历史条目，不能把它当作文件删除操作。[官方 File dialogs](https://esotericsoftware.com/spine-ui#File-dialogs)

#### P1.3 视口导航、模式与撤销

右键拖动平移；无右键设备可用默认 `J` 加左键拖动。滚轮围绕鼠标缩放；默认 `U` 加左键拖动也可缩放。左下倍率控件、100% 和框选适配按钮用于恢复观察尺度。`Ctrl+F`／Mac `Cmd+F` 为100%，加 Shift 适配骨架。Setup 建结构和初始姿态；Animate 编辑时间轴。撤销默认 `Ctrl+Z`，重做 `Ctrl+Shift+Z` 或 `Ctrl+Y`，Mac 替换为 Cmd。各面板焦点可能改变同一快捷键的作用。[官方 Viewport 与 Undo](https://esotericsoftware.com/spine-ui#Viewport)

导航示例：先在 Outline 点击目标区域，再回到视口放大细节；检查图像是否模糊时先恢复100%，避免把任意缩放的采样结果误判为源素材质量。

#### P1.4 Views 布局管理

Views 菜单随 Setup／Animate 模式显示不同面板；已打开项也可点击以聚焦。拖标签重新停靠，橙框是落点；拖边缘调整大小。标签下划线表示焦点。菜单可 Minimize 或 Close；右键菜单图标可最小化，双击可关闭。最小化图标保留恢复入口；默认 F9 隐藏／恢复全部面板。面板目前不能独立拖到其他显示器，跨屏可扩大主窗口；对话框位置可重新拖动。4.3 增加关闭单个标签的操作。[官方 Views](https://esotericsoftware.com/spine-views)、[4.3 更新记录](https://esotericsoftware.com/spine-changelog#v4-3-84-beta)

#### P1.5 Welcome 的五个分区

| 分区 | 查什么 | 操作 |
| --- | --- | --- |
| Projects | 新建、打开、近期、示例 | 输入过滤近期项；Enter 打开首项；示例入口打开官方工程 |
| News | 官方博客 | 点击标题阅读对应发布或功能文章 |
| Tips | 使用技巧 | 启动时轮换；点击进入对应技巧 |
| Learn | 文档、论坛、学习材料 | 进入相应官方入口 |
| Changelog | 最近修复和变化 | 确认工程所用补丁具体修复 |

Escape／右上关闭退出 Welcome 并回到最近工程；标题栏邮件按钮可再次打开。Settings 可关闭启动显示。[官方 Welcome](https://esotericsoftware.com/spine-welcome-screen)

<a id="pipeline-p2"></a>

### P2. Images 资源与查找规则

#### P2.1 Images path、扫描与热更新

Images 节点指定素材根目录，可用工程相对路径或绝对路径。支持 PNG、JPG、JPEG；设置路径后节点列出文件，磁盘图像变化会重新载入。`Limit scanning` 默认只列出最先找到的 2,000 文件，防止误选大目录；素材确实超过数量时再解除。相对路径依赖工程保存位置。[官方 Images](https://esotericsoftware.com/spine-images#Images-path)

建议工程布局：源工程、拆件图片和导出配置分别放固定目录；保留原始 PSD。移动工程后先检查 Images path，再进行打包。单独挪 `.spine` 不会自动把外部图片搬走。

#### P2.2 新建 Region 的入口

图片拖入视口会在 root 下创建 slot 和 region；可多选一起拖。拖到骨骼或 slot 时以骨骼为中心创建；拖到现有 attachment 时继承其变换。图片属性中的 Set Parent 或默认 P 允许选目标骨骼／slot。Images 中橙图标表示未被附件使用，绿图标表示已有引用。[官方 Creating region attachments](https://esotericsoftware.com/spine-images#Creating-region-attachments)

装配步骤：设置图片根目录 → 将图像拖到目标骨骼 → 检查 slot 和附件名称 → 使用 Rotate／Translate／Scale 对齐 → 再绑定网格或调整骨骼。批量图片的准确原始位置可通过 PSD 或官方绘图软件脚本带入。

#### P2.3 Name 与 Path 的查图优先级

attachment 没有 Path 时以 Name 找图；有 Path 时使用 Path。扩展名可省略，子目录用 `/`；大小写规则受操作系统影响。同一 slot 中附件名不能重复，但不同附件可指向同一图片路径。已有 atlas 要先 Unpack 成独立图片；编辑器的 Images 不是加载任意图集页后自动识别拆件的入口。[官方 Image file lookup](https://esotericsoftware.com/spine-images#Image-file-lookup)

排错顺序：根目录 → Path／Name → 大小写 → 子目录 → 扩展名 → 输出图片是否存在 → 扫描限制。检查路径时区分附件逻辑名、skin placeholder 名、磁盘相对路径和 atlas region 名，这四者可以相互对应但不必完全相同。

#### P2.4 4.3 的 PSD 来源追踪

4.3 在 Images 节点组织关联 PSD、显示附件来源，并改进重复导入与源网格匹配。工程素材更新时应先确认来源 PSD 与目标 images 目录，再决定覆盖和清理旧输出；覆盖后的 PNG 不依靠编辑器 Undo 恢复。`Package Project` 将工程和图片打 ZIP，适合提交可重现工程，但仍应检查工程所依赖的其他外部文件是否包含。[4.3 更新记录](https://esotericsoftware.com/spine-changelog#v4-3-56-beta)

#### P2.5 官方绘图软件脚本入口

官方spine-scripts包含Photoshop、Illustrator、Inkscape、GIMP、After Effects相关导出；Affinity Designer自身有Spine导出入口。脚本常生成图片和JSON，使层位置带入工程；并不是把任意绘图软件的全部特效直接转换为Spine附件。安装方式、标签和功能应看对应脚本自身说明。[官方 Scripts索引](https://esotericsoftware.com/spine-images#Scripts)、[spine-scripts仓库](https://github.com/EsotericSoftware/spine-scripts)

<a id="pipeline-p3"></a>

### P3. Tree 的全部操作类别

#### P3.1 节点、选择、定位和属性

| 操作 | 如何使用 |
| --- | --- |
| Expand / Collapse | 点击图标左侧展开；右键节点递归展开／折叠；顶部按钮操作选中项及其后代，无选择或双击时操作全部 |
| Selection | 单击节点文字选中；Ctrl／Cmd 多选，Shift 连续范围；Page Up／Down 浏览选择历史 |
| Auto scroll | 视口选中时自动展开并定位；关闭可保持当前浏览位置，右键按钮单次定位 |
| Visibility | 左侧点显示／隐藏；右键可连带后代切换 |
| Keying | Animate 中左侧键图标为该对象属性打键 |
| Annotations | 右侧关系图标指向皮肤、约束等对象；右键跳转可避免自动滚动 |
| Properties | 下方属性区；多选仅显示共同可编辑项；常用 Duplicate、Rename、Delete |
| Drag and drop | 拖到新父项相当于 Set Parent；拖动中滚轮滚动、右键展开，靠边自动滚动 |
| Image preview | 悬停 slot／region／mesh 查看图片；F1 可即时显示；Tooltips 关闭后仍可按 F1 |
| Shortcuts | F2 重命名；双击可重命名；双击 Animations／Events／Skins 创建相应对象；Selection groups 保存选择 |

不要把某对象在 Tree 暂时隐藏理解为已从导出数据移除；导出还受 Export 属性控制。上表按[官方 Tree nodes](https://esotericsoftware.com/spine-tree#Tree-nodes)组织。

#### P3.2 过滤与搜索的两套机制

类型过滤按钮可只显示 Bones、Slots、Attachments 等类型，Ctrl／Cmd 或 Shift 组合，Reset 恢复；右键总过滤按钮切换开关。Text search filters 决定非匹配节点是隐藏还是仅标记。隐藏 attachments 时，在视口选图片可能转为选择它的 slot；隐藏 bones 时可集中观察 draw order。[官方 Filters](https://esotericsoftware.com/spine-tree#Filters)

Text search 支持 `*` 任意长度、`?` 单字符、反斜杠转义；`/表达式/` 形式按正则处理。Enter／F3 找下一个，Shift 反向，Escape 清空。[官方 Text search](https://esotericsoftware.com/spine-tree#Text-search)

建议：搜索 `weapon*` 找一组资源 → 类型限制 Mesh → 用 Select 形成选择 → 再批量处理。执行后清空过滤，以免下一步误以为缺少节点。

#### P3.3 Find and replace 字段表

| 字段／按钮 | 含义 |
| --- | --- |
| Find / Replace | 查找文本与替换文本 |
| Match case | 区分大小写 |
| First occurrence | 每个名称／路径仅替换首个匹配 |
| Regular expression | 正则匹配；捕获组可在替换中引用 |
| Scope | Entire project、Tree selection、Current skin、Selected skeletons |
| Field | Name 或 Path |
| Types | 限制对象类型，可多选 |
| Unused | 筛选没有动画键的项；不能等同于没有任何运行时用途 |
| Missing images | 查找查图失败项 |
| Show folders | 结果包含文件夹 |
| All / None | 选择／取消全部结果 |
| Select / Replace | 在 Tree 形成选择／真正改名或改路径 |

右侧先预览，绿箭头为有效替换，红箭头提示空名、重名等冲突；可排除单个结果。[官方 Find and replace](https://esotericsoftware.com/spine-tree#Find-and-replace)

原创例：若只想把当前选择的附件路径 `old/` 改为 `new/`，先选择这些附件，Scope 设 Tree selection，Field 设 Path，再预览；不要同时改 Name，除非外部代码也要变更附件逻辑名称。

#### P3.4 Tree 显示选项与 4.3 对照

| 选项 | 看见的变化 | 实际用途／注意 |
| --- | --- | --- |
| Hide skeleton names | 骨架名前缀显示为省略号 | 缩短树文字，不改真实路径 |
| Show slot folders under bones | slot folder 在相应父骨骼下显示 | 同一 folder 可能在多个骨骼下出现 |
| Show slot paths | slot 名旁显示 folder 路径 | 便于区分同名末级节点 |
| Show all skin attachments | 展示所有皮肤中的占位附件 | 增加 Tree、Graph、Dopesheet 的可见候选项 |
| Only pinned skins【4.3】 | 限定上项展示的皮肤 | 减少大型换装工程节点量 |
| Hide skin names | 隐去 active skin 的同名前缀 | 只影响展示 |
| Hide skin bones and constraints | 隐藏当前未激活的皮肤骨骼／约束 | 查看当前组合的有效结构 |
| Show inactive skin bones【4.3】 | 显示未激活的皮肤骨骼 | 旧文档称 Hide viewport skin bones；不要反向套用开关意义 |

建议验证最终皮肤时关闭 Show inactive skin bones；否则编辑辅助显示可能掩盖 runtime 中骨骼未激活造成的问题。来源：[Tree View settings](https://esotericsoftware.com/spine-tree#View-settings)、[4.3 重命名记录](https://esotericsoftware.com/spine-changelog#v4-3-65-beta)、[Only pinned skins](https://esotericsoftware.com/spine-changelog#v4-3-62-beta)

<a id="pipeline-p4"></a>

### P4. 辅助工作面板

#### P4.1 Ghosting：采样、显示与对齐

| 分区 | 选项 | 用法 |
| --- | --- | --- |
| Frames | Before / After / Current；Before frames / After frames；Frame step | 范围内等间距取姿态；步长越小，叠图越多 |
| Key Frames | 对应前后范围与 Key frame step | 只显示有键的位置；关键帧采样总是 anchored |
| Motion Vectors | 前／后范围 | 显示 region 和 mesh 顶点位移线，检查方向和间距 |
| Display | 前后颜色；None / Image / Solid；None / Silhouette / Xray | 图片、纯色、总轮廓或各附件轮廓 |
| Anchor | 帧0固定采样 | 关闭则以当前时间作相对采样；影响 Frames |
| On top | ghost 放在骨架上层 | 提高可见性，重叠严重时与 Offset 配合 |
| Loop | 越过首尾显示 | 需要时间轴 Repeat 开启 |
| Offset | X offset / Y offset | 移开叠图，或模拟每帧角色移动距离 |
| Selection | 限定选中骨骼；Lock；Refresh | 固定观察对象，避免每次换选择改变 ghost |
| Bones【4.3】 | ghost 绘制骨骼 | 检查骨骼弧线，骨骼颜色随 ghost alpha 淡化 |

[官方 Ghosting](https://esotericsoftware.com/spine-ghosting)、[Bones 新增](https://esotericsoftware.com/spine-changelog#v4-3-17)

原创检查例：角色移动速度为 `S` 像素／秒、工程时间轴为 `F` 帧／秒，参考偏移取 `S/F` 像素／帧，再观察支撑脚是否重合。旧指南写死30FPS，本手册用实际项目 F；导出FPS或编辑器刷新率不能代替时间轴FPS。偏移只改变观察，不自动修正脚滑，也不写入 root 位移键。

#### P4.2 Outline

Outline 提供另一份当前姿态观察；右键平移、滚轮缩放。在其画面点击可将主视口定位到该处，橙框短暂表示主视口范围。View settings 的 Ghosting 决定是否显示 onion skin。Outline 看的是编辑姿态，Preview 则有独立播放逻辑。[官方 Outline](https://esotericsoftware.com/spine-outline)

#### P4.3 Preview：组合动作与版本边界

Animations 列表属于当前骨架；点击给 active track 播放，再点可清空；右键跳到 Tree。Tracks 模拟多层动作；Speed 控制速度，Reset 恢复100%；Mix 是切换淡入淡出时长，0为即时；Repeat 决定循环；Alpha 调整上轨对下轨姿态的影响；Additive 加入姿态增量。旧指南还列 Hold previous，并注明部分按钮影响下一次播放、track0隐藏某些控件、最多15轨。[官方 Preview](https://esotericsoftware.com/spine-preview)

**4.3 注意：** 上述为功能定位，不应直接复制旧指南的 `holdPrevious`／`mixBlend` API。4.3.53-beta已删除Hold previous按钮；它不是当前4.3面板能力。4.3重构混合模型，旧字段不能当作新API的完整列表。当前指南的 View settings 包括 Hide controls、Play current animation、Show bones，4.3 也将预览控制移出旧 View menu。[移除Hold previous](https://esotericsoftware.com/spine-changelog#v4-3-53-beta)、[4.3 记录](https://esotericsoftware.com/spine-changelog#v4-3-73-beta)、[runtime 4.3 Changelog](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/CHANGELOG.md)

原创组合检查：基础轨播跑步，上轨只播头部动作；先看 Alpha=1 的覆盖，再降低 Alpha，最后切换两条头部动画检查过渡。若上轨还给腿打了键，先检查它是否覆盖了基础轨，而不是把异常全部归因于 Mix。

#### P4.4 Skins view

当前骨架的 Skin list 点击切换 active skin，双击改名，右键在 Tree 选择但不激活。Pins 将皮肤固定到 Pinned skins；拖动可重排。列表从下往上应用，上方同 placeholder 覆盖下方；未固定的 active skin 最后应用。视口可编辑的是 active skin 对应附件，不是每个可见固定皮肤的附件。4.3 支持 folder pin 与拖动复制合并皮肤。[官方 Skins view](https://esotericsoftware.com/spine-skins-view)、[4.3 更新记录](https://esotericsoftware.com/spine-changelog#v4-3-56-beta)

原创排错：把 body、clothes、weapon 分别固定 → 让需要编辑的 skin 成为 active → 检查同 placeholder 的覆盖顺序 → 再检查 skin bones/constraints 的激活。换装缺图既可能是 region 缺失，也可能是占位覆盖或骨骼未激活。

#### P4.5 Color（原 Slot Color）

旧 Slot Color 面板为当前 slot 提供常驻颜色选择器；选附件时可直接改其 slot 色，展开面板显示 HSB、RGB、Alpha，小面板隐藏部分输入。4.3 名称为 Color，Setup 可设置其他对象颜色。对象编辑标识色与运行渲染 tint 应按所选对象区分；给骨骼改显示颜色不会自动给图片换色。[旧 Slot Color](https://esotericsoftware.com/spine-slot-color)、[4.3 重命名](https://esotericsoftware.com/spine-changelog#v4-3-73-beta)

#### P4.6 Metrics：指标逐项解释

| 指标 | 计量内容 | 用来判断 |
| --- | --- | --- |
| Bones | 可见骨架的骨骼总数 | 世界变换计算规模 |
| Constraints | 约束数量 | 求解规模，成本还取决于类型和对象数 |
| Slots | slot 总数 | 潜在可见附件上限，非直接draw call数 |
| Attachments / Total | 全附件数量 | 资源规模 |
| Attachments / Visible | 当前可见附件 | 当前姿态规模，非全部都产生绘制 |
| Vertices | 可见附件顶点 | 几何规模 |
| Vertex transforms | 顶点变换次数 | 顶点数和权重影响骨骼数共同决定 |
| Triangles | 可见region/mesh三角形 | 提交几何规模 |
| Area | 全尺寸绘制像素含透明与重叠 | fill rate／overdraw线索 |
| Clipping polygons | 分解后的凸裁剪多边形 | 凹形可能拆成多个 |
| Clipped triangles | 被裁剪附件三角形 | 裁剪计算规模 |
| Selection | 选择项／全部两组数字 | 比较局部与整体 |
| Animation / Timelines | 当前动画属性时间轴 | 每帧动画采样规模 |

指标为所有可见骨架合计，动画指标在 Animate 显示。CPU、Fill rate、Draw calls 是不同瓶颈；减少影响骨骼、清理无效时间轴、减少像素覆盖、合图／控制材质切换分别解决不同问题。Metrics 不是目标手机上的毫秒数或保证FPS的评分器。[官方 Metrics](https://esotericsoftware.com/spine-metrics)

<a id="pipeline-p5"></a>

### P5. Project / Data 导入的决策表

#### P5.1 Import Project

| 导入内容 | 选择内容 | 结果与边界 |
| --- | --- | --- |
| Skeleton | 来源工程、骨架、目标名称 | 带入骨架，再按需要在骨架间拖移可转移对象 |
| Animation | 来源骨架、动画、接收骨架 | 依据名称匹配对象；缺对象的键舍弃并警告 |
| 多人动画合并 | 同一结构命名约定 | 保持骨骼、slot、约束和附件名称一致才可可靠映射 |

[官方 Import Project](https://esotericsoftware.com/spine-import#Project)

原创安全流程：复制目标工程 → 导入一条动作 → 查看 warnings → 检查关键姿态、attachment切换、deform及约束 → 再合入剩余动作。名称一致仍不保证结构、初始变换、皮肤依赖和权重一致。

#### P5.2 Import Data

| 字段 | 说明 |
| --- | --- |
| Input | JSON／binary文件或含数据的目录 |
| Name | 导入骨架名 |
| Scale | 按尺寸缩放骨架、附件、动画相关坐标；不等同把root的scale改小 |
| New project | 打开新工程接收数据 |
| Create a new skeleton | 当前工程增加新骨架 |
| Import into an existing skeleton | 合入结构、slot、skin、attachments；此入口不负责已有骨架的动画合并 |
| Existing attachments / Ignore | 保留已存在项及人工改动 |
| Existing attachments / Replace | 使用输入数据替换对应项 |
| Nonessential data | 导出往返时保留颜色、手工mesh边等编辑信息 |

重新导入不带Nonessential的数据时，网格已有三角形可保留，但手工边丢失，之后改网格会重新三角化。缩放骨架后应配套缩放图片或正确关联资源。[官方 Import Data](https://esotericsoftware.com/spine-import#Data)

原创尺度例：将骨架和源图像一起减为50%，可用导出Nonessential JSON → Scale=0.5导入独立工程 → 图片也缩为50% → 核查骨长、附件位置、路径和物理参考尺度。不要在同一次流程再把root缩为0.5，否则可能重复缩小。

<a id="pipeline-p6"></a>

### P6. PSD 输入、标签与命名

#### P6.1 导入面板三区

| 区域 | 控件 | 用途 |
| --- | --- | --- |
| Input PSD File | PSD File | 指定来源 |
| 同上 | Scale | 输出拆件的分辨率 |
| 同上 | Padding | 增加透明边 |
| 同上 | Trim whitespace | 裁层透明外缘；关时按画布大小 |
| 同上 | Ignore hidden layers | 不输出隐藏层／组 |
| Output PNG Files | Folder | 图片目标目录 |
| 同上 | Overwrite confirmation | 覆盖已有PNG前提示 |
| 同上 | Images path | 使用所选骨架图片目录，或PSD路径下images |
| Import Data | 与P5.2相同的接收项 | 生成或更新Spine结构 |

Origin 来自首条横向／纵向参考线，无该轴参考线时用相应画布中心；也可用 `[origin]` 层。后续参考线不改变原点。奇数画布和像素中线会影响像素对齐。[官方 Import PSD](https://esotericsoftware.com/spine-import-psd#Usage)

#### P6.2 标签速查：按适用对象分类

以下是语法事实索引，未给值的可选命名标签使用当前层／组名称。标签可出现在名称中任意位置。

| 标签 | 可用处 | 语义索引 |
| --- | --- | --- |
| `[bone]`、`[bone:name]` | 组／层 | 指定骨骼；新骨骼定位首个可见层中心；已有骨骼位置保留 |
| `[slot]`、`[slot:name]` | 组／层 | 指定slot |
| `[skin]`、`[skin:name]` | 组／层 | 生成skin placeholder；图片按皮肤目录组织 |
| `[folder]`、`[folder:name]` | 组／层 | 图片子目录，可嵌套 |
| `[scale:number]` | 组／层 | 缩放输出像素；反向补偿attachment尺度 |
| `[rotate:degrees]` | 组／层 | 旋转输出像素；反向补偿attachment角度 |
| `[pad:number]` | 组／层 | 覆盖通用padding |
| `[overlay]` | 组／层 | 用作下方内容的裁剪遮罩 |
| `[trim]`、`[trim:true]`、`[trim:ws]` | 组／层 | 强制裁透明边 |
| `[trim:false]`、`[trim:canvas]` | 组／层 | 强制画布尺寸 |
| `[trim:mask]` | 组／层 | 按mask范围；没有可用mask时忽略 |
| `[ignore]` | 组／层 | 跳过该项及子树 |
| `[mesh]` | 层／merge组 | 创建mesh |
| `[mesh:name]` | 层／merge组 | 创建linked mesh，引用已有源mesh |
| `[path:name]` | 层／merge组 | 指定磁盘图片名，与attachment名分离 |
| `[origin]` | 层／merge组 | 以中心为原点，不输出其图片 |
| `[merge]` | 组 | 输出组内合成的一张图片 |
| `[bones]`、`[slots]` | 组 | 给直接子项补bone／slot标签 |
| `[name:prefix*suffix]` | 组 | 批量名称模式，先于其他模式处理 |
| `[path:prefix*suffix]` | 组 | 批量路径；子层没有path标签时也可应用 |
| `[bone:prefix*suffix]`、`[slot:prefix*suffix]` | 组 | 批量归属名称模式 |
| `[!bones]`、`[!slots]` | 组／层 | 停止相应批量标签影响 |
| `[!name]`、`[!path]`、`[!bone]`、`[!slot]` | 组／层 | 停止相应祖先模式影响 |

`[mesh:name]` 按 `/` 分隔段从右往左找最具体源名；`[mesh:front/head]` 比 `[mesh:head]` 更能区分来源。linked mesh使用源mesh尺寸，不是任意尺寸重新自动绑权重。[官方 Tags](https://esotericsoftware.com/spine-import-psd#Tags)

#### P6.3 五类名称的处理关系

| Spine字段 | 形成依据 |
| --- | --- |
| Slot | 最近的slot标签；没有则取层名 |
| Skin | 最近的skin标签决定skin名；更外层skin目录影响输出组织 |
| Placeholder | folder层级加层名 |
| Attachment | folder和skin层级加层名 |
| Attachment image path | folder层级加层名，path标签可替代层名 |

同一层／组同时有skin和folder时skin优先。名称／slot、skin、folder、path标签值以 `/` 开头可阻止父组影响；中间的 `/` 表示子目录。不要把领先 `/` 当作本机文件系统绝对路径。[官方 Attachment properties](https://esotericsoftware.com/spine-import-psd#Attachment-properties)

原创命名样例：给两套头部图片设置不同skin，均使用同一slot及placeholder；磁盘图片由各自skin目录区分。更换skin时切换实际图片，动画仍对共同slot/placeholder操作。重导入前先用一个skin验证生成结果，确认命名规则后再批量应用。

#### P6.4 混合、绘制顺序与限制

Normal、Multiply、Screen、Linear Dodge 分别对应Spine的Normal、Multiply、Screen、Additive。PSD层顺序通常用于draw order；slot folders不能同时处在另一folder的两侧，冲突排列需要重新组织层级。导入读取可用像素；Layer styles、渐变mask、非像素形式的调整／填充层、百分比Fill等可能需先生成可用像素或采用官方脚本。[官方 Blending / Draw order / Limitations](https://esotericsoftware.com/spine-import-psd#Limitations)

原创更新检查：保存PSD → 输出到独立临时图片目录 → 比较透明边、mask、blend和尺寸 → 合入复制工程并选Ignore／Replace → 看warnings → 通过后再更新正式素材目录。原生PSD和Photoshop脚本并非完全相同的标签实现。

<a id="pipeline-p7"></a>

### P7. 导出格式与字段参考

#### P7.1 完整格式地图与旧指南漏项

| 格式 | 交付性质 | 与其他格式的区别 |
| --- | --- | --- |
| JSON | 骨架数据 | 可读；外部工具处理方便 |
| Binary | 骨架数据 | 更紧凑；需匹配runtime读取 |
| HTML【4.3新增】 | 网页展示 | Spine Player或Web Components配置 |
| GIF | 动画图片 | 调色板、二值透明，适合简短分享 |
| PNG | 静图／序列 | 无损RGBA |
| APNG | 动画图片 | PNG动画、完整Alpha |
| WEBP | 静图／序列 | 可选损耗和压缩策略 |
| AWEBP | 动画图片 | WebP动画及帧／循环控制 |
| PSD | 分层图像文件 | 姿态或帧交回图像工具 |
| JPEG | 静图／序列 | 不含Alpha、有损 |
| AVI | 视频容器 | 具体能力由编码决定 |
| MOV | 视频容器 | 含透明的编码可用于合成 |
| WEBM | 视频容器 | 可配置视频、音频编码及码率 |

上述13项已由当前本机 **Spine 4.3.26 导出窗口**只读观察确认，未执行导出。官方 Export 页面未列WEBP/AWEBP/WEBM，不能用其短列表当全部。它们在4.1已引入；4.3针对WEBP的PMA适用条件有调整。[官方格式历史](https://esotericsoftware.com/spine-changelog#v4-1-21-beta)、[WEBP历史](https://esotericsoftware.com/spine-changelog#v4-1-20-beta)、[4.3 WEBP PMA](https://esotericsoftware.com/spine-changelog#v4-3-40-beta)

#### P7.2 JSON / Binary 字段

| 字段 | 用途与边界 |
| --- | --- |
| Output folder | 每骨架生成数据文件，文件名通常由骨架名决定 |
| Extension | 文件扩展名；改扩展名不改变数据格式 |
| JSON Format | JSON、JavaScript、Minimal；后两项不保证严格JSON解析器接受 |
| Pretty print | 可读排版，体积增大 |
| Version（JSON） | 旧版本回退恢复；新功能不能保证保留 |
| Nonessential data | 保留往返所需编辑信息 |
| Animation clean up | 清理输出，源工程不因此改写 |
| Export all | 含Export关闭项；具体格式以可见选项为准 |
| Warnings | 导出后显示警告 |
| Pack | 同时生成atlas；另见P8 |

运行数据不是逐帧图；runtime仍需数据、atlas和页图片。保存 `.spine` 是跨版本维护的基础。[官方 Data](https://esotericsoftware.com/spine-export#Data)

#### P7.3 各格式独有字段索引

| 格式 | 官方可核对字段 | 选择依据 |
| --- | --- | --- |
| GIF | Colors、Color dither、Alpha threshold、Alpha dither、Background、Transparent、Matte、Quality、FPS、Forever、Include last frame | 渐变用抖动；半透明边用matte或透明策略；透明阈值会影响边缘 |
| PNG | Background、Compression、FPS、Include last frame、Pack | 压缩影响耗时和体积；Pack用于图像序列合图 |
| APNG | Background、Transparent、Compression、FPS、Forever、Include last frame | 完整透明和动画循环 |
| PSD | Background、Transparent、Encoding、FPS、Include last frame | RAW快且大，RLE折中，ZLIB较紧凑 |
| JPEG | Background、Quality、FPS、Include last frame | 提高quality改善图像同时增加体积 |
| AVI | Encoding、Background、Compression、Quality、FPS、Forever、Include last frame | RAW／PNG编码可保留透明；播放端支持需验证 |
| MOV | 上项并含Transparent | 容器支持不等于任意播放器能读任意codec |

各格式控件会受当前Export type／Output type变化，静态姿态不需要动画帧字段。[官方 Images / Video](https://esotericsoftware.com/spine-export#Images)

#### P7.4 WEBP / AWEBP 的当前可见参数

本节是本机4.3.26窗口观察的字段记录；中文显示可能随语言包调整。参数全范围未测，界面显示的“默认”标签与本次当前值分开记录。

| 字段 | 窗口观察 | 操作意义 |
| --- | --- | --- |
| Background／透明 | 可设背景与透明 | 先按交付是否需Alpha选择 |
| Preprocessing／预处理 | 模式当前“正常”，数字6标中（默认） | 编码前处理；模式与强度分别检查 |
| Alpha compression／压缩Alpha | 数字4标中（默认） | 透明通道的压缩控制 |
| Strength／力度 | 强／自动，数值45标中（默认） | 调整编码策略力度 |
| Sharpness／锐度 | 0标无（默认） | 控制编码锐化相关策略 |
| Noise shaping／噪音整形 | 下拉4段，75标高（默认） | 影响编码后的视觉与压缩取舍 |
| Quality／品质 | 下拉有损，75标高（默认） | 选择有损／无损路线与质量 |
| Alpha quality／Alpha质量 | 最大勾选；100标无损（默认） | Alpha的质量单独控制 |
| Partitions／分区 | 当前0 | 当前选项索引，不当作全部可用范围 |
| Multithreading／多线程 | checkbox | 编码计算方式 |
| Target file size（AWEBP） | 当前0标“无” | 目标体积策略；不保证绝对达到输入值 |
| FPS（AWEBP） | 本次当前20、0.05秒／帧 | 当前工程观察值，不写成出厂默认 |
| Forever／Include last frame（AWEBP） | 两项均有 | 循环及重复末帧控制 |

4.3官方明确：WEBP只有compression=0且lossless quality时可开启PMA。对runtime atlas，要在格式选择、PMA、材质三者之间保持一致；对普通透明图片交付，先以目标工具实际读取结果判断。[4.3.40-beta](https://esotericsoftware.com/spine-changelog#v4-3-40-beta)

原创编码比较方法：同一帧含细线、透明边和渐变，输出一个无损基准和一个有损候选；对比边缘、Alpha及大小，再选择适用设置。AWEBP额外比较首尾接缝和播放器兼容性。此方法是验证方案，本次未执行。

#### P7.5 共通字段及相互作用

| 字段 | 定义 | 常见组合 |
| --- | --- | --- |
| Defaults | 重置配置 | 重置后重新检查输出位置和格式 |
| Preview | 打开导出预览 | 看边界、帧、预计体积 |
| Export type | 当前姿态／动画 | 静态图用姿态；序列用动画 |
| Skeletons | 同时、分别或指定骨架 | 多角色合成与独立交付区别 |
| Animations | 当前、全部或指定动画 | 不必把整个工程所有动作输出成预览 |
| Skins | 当前可见、全部、全部并含无skin | 组合皮肤与逐皮肤单独导出不同 |
| Output type | 单文件／逐帧／逐动画／分层等 | 可用项取决于格式 |
| Output file / folder / prefix | 单文件或批量命名路径 | 新格式标签可能不同，先看预览输出组织 |
| Maximum bounds | 各输出帧统一尺寸 | 防止序列画布跳动 |
| Animation repeat | 动画播放次数 | 展示持续时长 |
| Pause after | 每动作后暂停秒数 | 演示观察停顿 |
| Bones / Images / Others | 骨骼／图片／其他附件渲染 | 调试图和正式成片的内容不同 |
| Selection / Title | 4.3窗口可见辅助绘制项 | 交付前确认是否需编辑选区及标题 |
| Smoothing | 0最近邻；1—10线性；11 bicubic | 像素风优先0；缩放审阅实际结果 |
| Multisample AA | MSAA采样 | 主要改善几何切边，GPU需支持 |
| Crop | 世界坐标X/Y加宽／高 | 预览拖角改变固定区域 |
| Size / Scale | 相对缩放百分比 | 与Fit二选一式尺寸策略 |
| Fit | 限制像素框内 | 保持比例适配 |
| Enlarge | 较小时允许放大 | 不勾时避免不必要上采样 |
| Pad | 补空白到目标尺寸 | 固定交付画布 |
| Range | 限定帧区间 | 输出片段 |
| Warm Up | 正式输出前预播次数 | 让物理进入运动状态 |

[官方 Common settings](https://esotericsoftware.com/spine-export#Common-settings)

原创尺寸例：做512×512透明表情图，先把目标姿态放到Crop区域，选择Fit 512×512并Pad，只有需要补放大时才开Enlarge；逐帧动画则同时检查Maximum bounds。Crop决定区域、Fit决定尺寸、Pad决定空白，这三个步骤解决不同问题。

#### P7.6 Preview、透明Additive与重复导出

预览底部帧号取决于导出FPS，不必与编辑器时间轴帧号一一对应。白框是输出边界，Crop启用后橙角和框可拖动；显示像素尺寸与估计大小。透明底下的Additive区域与在有色背景上合成的结果可能不同，尤其不透明黑色素材；应以实际目标合成检查。Save／Load保存配置JSON；默认 `Ctrl+E`／Mac `Cmd+E`打开，加Shift重复上次配置。骨架、动画、附件和皮肤的Export属性可排除参考对象。[官方 Preview / Additive / Settings / Disabling export](https://esotericsoftware.com/spine-export#Preview-panel)

#### P7.7 WEBM 当前界面补充核验

本机Spine4.3.26只读观察到Background/透明、Quality（Q-score／最大比特率）、FPS、Include last frame、Audio（None／Vorbis）。本次Q-score=70和FPS=20仅为当前值。动画通用Repeat、Pause after、Warm Up也存在。**没有Forever复选框**；连续循环由播放端控制。没有为其捏造视频编码全集，也没有实际导出验证透明和音频兼容性。[WEBM官方引入记录](https://esotericsoftware.com/spine-changelog#v4-1-21-beta)

<a id="pipeline-p8"></a>

### P8. Texture Packer / Unpacker 参数与配置

#### P8.1 四组容易混淆的入口选择

| 选择 | 含义 |
| --- | --- |
| 数据导出 + Pack | 骨架数据和atlas一起交付 |
| 独立Texture Packer | 对指定图片目录单独打包，可服务其他类型资源 |
| Attachments | 只打包附件引用图；不使用目录pack.json |
| Image folders | 按Images根目录扫描；目录pack.json参与 |
| Atlas per skeleton | 每骨架一份atlas |
| Single atlas | 多骨架共享atlas |
| Atlas per skin【4.3】 | 按皮肤资源拆包，需协调共享region与运行时加载 |

atlas由描述文件和一页／多页图片组成；减小纹理切换可能改善批处理，但“单atlas”不保证“单texture page”。[官方 Packing](https://esotericsoftware.com/spine-texture-packer#Packing)、[per-skin新增](https://esotericsoftware.com/spine-changelog#v4-3-40-beta)

#### P8.2 所有指南字段：界面名与配置名

这些是参数索引与简短用途，**示例JSON中的数值不是普遍出厂默认保证**。

| 类别 | 界面名／JSON名 | 用途 |
| --- | --- | --- |
| Regions | Strip whitespace X/Y / `stripWhitespaceX/Y` | 裁透明外边 |
| Regions | Rotation / `rotation` | 90°旋转排布 |
| Regions | Alias / `alias` | 完全相同像素共用区域 |
| Regions | Ignore blank images / `ignoreBlankImages` | 跳过全透明图 |
| Regions | Alpha threshold / `alphaThreshold` | 裁边用Alpha阈值 |
| Padding | Padding X/Y / `paddingX/Y` | region间距 |
| Padding | Edge padding / `edgePadding` | 页边留距 |
| Padding | Duplicate padding / `duplicatePadding` | 复制近邻边缘像素 |
| Pages | Min width/height / `minWidth/Height` | 最小页尺寸 |
| Pages | Max width/height / `maxWidth/Height` | 最大页尺寸 |
| Pages | Power of two / `pot` | 2的幂尺寸 |
| Pages | Divisible by 4 / `multipleOfFour` | 四倍数尺寸 |
| Pages | Square / `square` | 正方形页 |
| Runtime | Filter min/mag / `filterMin/Mag` | 纹理采样提示 |
| Runtime | Wrap X/Y / `wrapX/Y` | 纹理寻址提示 |
| Runtime | Format / `format` | 内存格式提示 |
| Output | Format / `outputFormat` | 页图文件格式；指南列PNG/JPG，WEBP已支持 |
| Output | JPG quality / `jpegQuality` | JPEG质量 |
| Output | Packing / `packing` | Grid / Rectangles / Polygons |
| Output | Premultiply alpha / `premultiplyAlpha` | RGB乘Alpha |
| Output | Bleed / `bleed` | 透明像素RGB补色 |
| Output | Scale / `scale` | 多倍率atlas |
| Output | Suffix / `scaleSuffix` | 各倍率命名后缀 |
| Output | Resample / `scaleResampling` | 缩放算法 |
| Options | Atlas extension / `atlasExtension` | 描述文件扩展名 |
| Options | Combine subdirectories / `combineSubdirectories` | 合并子目录页 |
| Options | Flatten paths / `flattenPaths` | 去除region子目录名 |
| Options | Indexes / `useIndexes` | 解析末尾数字索引 |
| Options | Legacy output / `legacyOutput` | 旧atlas格式，不把4.3骨架数据降版 |
| Options | Debug / `debug` | 画区域边界 |
| Options | Auto Scale / `autoScale` | 缩小至单页 |
| Options | Fast / `fast` | 更快但排布可能较松 |
| Options | Limit memory / `limitMemory` | 控制同时加载图片量 |
| Options | Pretty print / `prettyPrint` | atlas可读排版 |
| Options | Current project / `currentProject` | mesh上下文保护UV所需像素 |
| Other | `ignore` | 忽略目录和后代 |
| Other | `bleedIterations` | 官方明确默认2 |
| Other | `separator` | 文件数字分隔，官方明确默认`_` |
| Other | `silent` | 官方JSON例存在；页面没有独立语义说明，按当前生成配置／help核对 |
| 4.3 | PNG brute force / `bruteForce` | 提高PNG压缩搜索；4.3记录默认关闭 |
| 4.3 | Open after packing | 完成后打开输出 |

[官方参数表](https://esotericsoftware.com/spine-texture-packer#Settings)、[4.3 brute force](https://esotericsoftware.com/spine-changelog#v4-3-82-beta)

#### P8.3 参数组合与约束

- **Grid**适合统一网格spritesheet；**Rectangles**按矩形排；**Polygons**利用源mesh hull，需要工程上下文。只给PNG无法重建完整Spine网格关系。
- **Duplicate padding**填的是相邻region边缘，**Bleed**填的是透明像素RGB；两者解决不同采样区域。PMA需配对应材质；关闭PMA时更要检查透明RGB边缘。
- `Flatten paths`前必须检查文件基名是否冲突。`Alias`可共用像素，不代表两个attachment的变换或动画合为一项。
- 多`scale`产生多套atlas；不提供suffix时输出到各倍率目录。小倍率需要实际检查细线和Alpha，不能只看原图。
- `Max width/height`控制一页上限，不是整个atlas总容量；纹理压缩需要的Power of two／Divisible by4／Square取决于目标引擎。

这是针对上表的使用推论，依据[官方参数说明](https://esotericsoftware.com/spine-texture-packer#Settings)。

#### P8.4 Folder structure / pack.json

目录递归扫描；默认相关目录各有页组。每层 `pack.json`继承父配置，子层覆盖。含独立pack.json的子目录不按普通Combine subdirectories路径简单合并。生成配置应从窗口Save取得，减少字段拼错。配置可只写要覆盖的字段。

原创配置例（只示范字段，不声称是4.3默认）：

```json
{
  "maxWidth": 1024,
  "maxHeight": 1024,
  "paddingX": 2,
  "paddingY": 2,
  "flattenPaths": false,
  "combineSubdirectories": true
}
```

放在像素风子目录的配置可仅覆盖`filterMin`与`filterMag`为`Nearest`。这些runtime提示仍需目标loader遵守。[官方 JSON Configuration](https://esotericsoftware.com/spine-texture-packer#JSON-Configuration)

#### P8.5 Atlas名字、Ninepatch、Image indexes与解包

atlas名称作为描述文件和页图前缀。`.9.png`式图片用1px边框记录stretch和content padding区域，打包时移除边框并保留描述；它通常服务UI素材。`animation_23.png`式名字在Indexes启用时把数字作为index，region名去掉数字后缀，便于排序取帧。Unpacker输入Atlas和Output folder，恢复旋转和裁边；PMA图集需要Unpremultiply alpha，atlas中的`pma:true`优先。多边形图集使用源工程可清理hull外邻图像素。[官方末节](https://esotericsoftware.com/spine-texture-packer#Texture-Unpacker)

原创验证：打包后确认data引用的每个region都存在，再在目标runtime载入；解包仅用于已有atlas恢复图片，无法恢复未打入的PSD图层、效果或编辑结构。

#### P8.6 当前图集窗口补充核验

本机Spine4.3.26只读检查确认：Output Format下拉为 **PNG / WEBP / JPEG**；Packing为 **Grid / Rectangles / Polygons**。有Brute force、五组Scale/Suffix/Resample，以及Legacy output、Pretty print、Current project等选项。界面当前padding=2、alpha threshold=3、min=16、max=2048是本机保存状态，不能据此宣布出厂默认。WEBP存在的官方依据：[4.1.20-beta](https://esotericsoftware.com/spine-changelog#v4-1-20-beta)。

#### P8.7 atlas文本字段与省略值

文件协议的省略值与Texture Packer窗口出厂默认是两件事。以下来自格式规范：

| 位置 | 字段 | 省略值／协议意义 |
| --- | --- | --- |
| Page | name | 第一行页图片名，通常相对atlas目录 |
| Page | size | `0,0` |
| Page | format | `RGBA8888`；可为Alpha、Intensity、LuminanceAlpha、RGB565、RGBA4444、RGB888、RGBA8888 |
| Page | filter | `Nearest`；也有Linear及MipMap系列 |
| Page | repeat | `none`；也有x、y、xy |
| Page | pma | `false` |
| Region | name | 区域名；不同index可同名 |
| Region | index | `-1` |
| Region | bounds | `0,0,0,0`，位置与打包宽高 |
| Region | offsets | 左／下裁边0，原宽高取打包宽高 |
| Region | rotate | `0`；true表示逆时针90°，也可角度值 |
| Region | split / pad | `null`，九宫格与内容内边距 |
| Region | 其他name/value | 额外数据，如序列导出原点与骨骼位置 |

纹理filter／format／repeat是loader可忽略的提示；UV应补偿rotate，绘制位置应补偿offsets。页面间用空行分隔。[官方 Atlas format](https://esotericsoftware.com/spine-atlas-format)

<a id="pipeline-p9"></a>

### P9. CLI完整任务表、参数与可复用调用

#### P9.1 基本语法与平台入口

Windows推荐`Spine.com`等待结束并接收控制台输出；`Spine.exe`面向GUI。Mac直接调用`/Applications/Spine.app/Contents/MacOS/Spine`，Linux使用安装目录`Spine.sh`。多数数据任务可headless，图像／视频导出需要窗口系统和OpenGL。失败返回非零状态；输出目录可自动创建。[官方 CLI](https://esotericsoftware.com/spine-command-line-interface#Running-Spine-with-CLI-parameters)

本次仅执行了本机`--help`与`--advanced`，二者显示 **Launcher 4.2.03**。当前运行编辑器可以是4.3.26，但launcher帮助并不因此自动成为4.3帮助。本手册没有执行import/export/pack/clean，也没有更换本机启动版本。

#### P9.2 Editor / Launcher参数

| 参数 | 作用 |
| --- | --- |
| `-h` / `--help` | 打印基本帮助 |
| `--advanced` | 打印高级帮助 |
| `-v` / `--version` | 打印版本信息 |
| `-u` / `--update VERSION` | 指定要加载的编辑器版本 |
| `-f` / `--force` | 强制重下载更新 |
| `-x` / `--proxy HOST:PORT` | 下载检查的代理 |
| `-t` / `--notimeout` | 禁用更新请求超时 |
| `-l` / `--logout` | 移除本机激活码；不是无副作用诊断命令 |
| `project.spine` | 打开工程 |

`4.3.xx`选择4.3最新补丁；stable/lateststable/latest与beta/latestbeta跟随当前通道。稳定构建建议锁定完整版本而不是滚动别名。新launcher的`-u project`按工程保存版本打开，旧launcher未必认识。[官方 Editor](https://esotericsoftware.com/spine-command-line-interface#Editor)、[launcher 4.3.04](https://esotericsoftware.com/spine-changelog#v4-3-06)

#### P9.3 数据与资源任务参数

| 任务 | 语法结构 | 参数说明 |
| --- | --- | --- |
| Export | `Spine [-i INPUT] [-m] [-o OUTPUT] -e CONFIG` | INPUT/OUTPUT覆盖配置JSON；CONFIG也可为json、binary、json+pack、binary+pack |
| Import skeleton【4.3】 | `Spine -i INPUT [-s SCALE] -o PROJECT [--from SOURCE] [--to TARGET] [--replace] -r` | 输出工程不存在时创建；source/target明确选择源与接收骨架 |
| Merge skeleton【4.3】 | 上式增加`--merge` | 合入目标骨架而不是增加独立骨架 |
| Import animation【4.3】 | 上式使用`-a NAME`可重复，或`-a`导入全部 | 需对象匹配；`--replace`决定替换重名动画 |
| Clean up | `Spine -i PROJECT -m` | 清理全部动画并保存源工程，会改变文件 |
| Pack | `Spine -i IMAGES [-j PROJECT] -o DIR -p NAME` | 按atlas名和默认／目录配置打包 |
| Pack配置 | `Spine -i IMAGES [-j PROJECT] -o DIR [-n NAME] -p CONFIG` | CONFIG来自Texture Packer Save |
| Unpack | `Spine -i PAGE_DIR -o DIR -c ATLAS` | 多边形上下文可加`-j PROJECT` |
| Info | `Spine -i INPUT` | 输出工程／数据信息 |
| 上次配置【4.3】 | `--last-export-settings` | 从工程提取最近导出配置，默认邻近工程或按-o指定 |

`-i/--input`、`-o/--output`、`-e/--export`、`-r/--import`、`-s/--scale`、`-m/--clean`、`-p/--pack`、`-j/--project`、`-n/--name`、`-c/--unpack`可用对应长名。`-j`可重复以提供多个源工程。[官方 Export / Import / Pack / Unpack](https://esotericsoftware.com/spine-command-line-interface#Export)

#### P9.4 Advanced完整官方列表与本机差异

| 参数 | 说明 |
| --- | --- |
| `-Xmx2048m` / `-Xmx8192m` | 显式最大内存示例；旧文档默认2048MB不代表当前版本；新launcher已改动态内存上限 |
| `--trace` | 增加日志与检查 |
| `--auto-start` / `--no-auto-start` | launcher是否自动开始 |
| `--ping` | 测试服务器延迟 |
| `--server x` | 指定偏好服务器 |
| `--disable-audio` | 关闭音频 |
| `--pretty-settings` | 配置排版 |
| `--keys` | 热键提示 |
| `--hide-license` | launcher不显示姓名邮箱 |
| `--ui-scale x` | UI比例 |
| `--icc-profile x` | ICC文件 |
| `--intro` | 启动logo |
| `--clean-all` | 对所有导出清理输出数据 |
| `--mesh-debug` | 网格debug绘制 |
| `--export-selection` | 导出编辑选择标记 |
| `--ignore-unknown` | 放在其他参数前，允许launcher转交未识别参数；官方也称长写法`--ignore-unknown-parameters` |

官方Nate的4.3参数表也列`--animate-mode`、`--no-save-prompt`、`--skeleton-viewer`和`--reuse-instance`／`--no-reuse-instance`。no-save-prompt可能放弃未保存修改，不应当作通用无人值守模板。4.3.14更新记录说明launcher4.3.05改用动态最大内存；以实际launcher为准。[官方 Advanced](https://esotericsoftware.com/spine-command-line-interface#Advanced)

#### P9.5 4.3新增CLI机制的准确边界

- `--set name=value`覆盖export或pack JSON字段；4.3.08支持数组和对象。字段名应从当前编辑器生成配置取，不把UI中文标签作为JSON键。
- `-i`、`-j`支持wildcards和regex；shell可能先展开，所以表达式应正确引用。
- `SPINE_ARGS`统一附加参数，复现问题时检查进程环境，避免隐藏的全局参数影响结果。
- 4.3增加动画／骨架import与merge，`-a/--animation`省略名称可导入全部动画。完整语法已由官方Nate提供，见P9.8；在线User Guide仍旧，不能继续使用其`-r [NAME]`作为新版本命名模板。
- 未显式传scale时，4.3 data import可使用atlas scale；批处理要明确是否需要该行为。
- 未识别参数先确认launcher版本；需要兼容时把`--ignore-unknown`放在最前。它用于launcher放行未知参数，不能保证editor实现了实际功能选项；不认识的选项不会因此获得实现。

[4.3.73-beta](https://esotericsoftware.com/spine-changelog#v4-3-73-beta)、[4.3.08](https://esotericsoftware.com/spine-changelog#v4-3-08)、[4.3.07](https://esotericsoftware.com/spine-changelog#v4-3-07)

#### P9.6 原创macOS命令模板

以下假定一个示例工程位于`/work/hero/hero.spine`，配置已在编辑器保存；是可读模板，不是本工作区执行记录。输出统一放新目录，import也创建新工程。

```sh
/Applications/Spine.app/Contents/MacOS/Spine --help
/Applications/Spine.app/Contents/MacOS/Spine --advanced
/Applications/Spine.app/Contents/MacOS/Spine -i /work/hero/hero.spine
/Applications/Spine.app/Contents/MacOS/Spine -u 4.3.26 -i /work/hero/hero.spine -o /work/hero/build -e /work/hero/export-runtime.json
/Applications/Spine.app/Contents/MacOS/Spine -u 4.3.26 -i /work/hero/hero.spine -o /work/hero/build-default -e binary+pack
/Applications/Spine.app/Contents/MacOS/Spine -u 4.3.26 -i /work/hero/images -j /work/hero/hero.spine -o /work/hero/atlas -n hero -p /work/hero/pack.json
/Applications/Spine.app/Contents/MacOS/Spine -u 4.3.26 -i /work/hero/atlas -j /work/hero/hero.spine -o /work/hero/unpacked -c /work/hero/atlas/hero.atlas
/Applications/Spine.app/Contents/MacOS/Spine -u 4.3.26 -i /work/hero/build/hero.json -o /work/hero/reimport.spine -s 1 --to hero -r
```

命令退出后在脚本检查状态码，再检查数据文件、atlas和页图是否齐全。import的数据版本需匹配；处理旧data时先用旧版导入成工程，再由新版本打开工程并导出。配置含空格的路径按shell规则加引号。不要用独立`-m`检查效果，因为它会保存修改。[官方 CLI Examples](https://esotericsoftware.com/spine-command-line-interface#Examples)

`--last-export-settings`也是action参数：先给本次`-i`／`-o`，再写action；action之后的`-o`会属于下一次操作。官方已确认launcher4.3.05的相关解析bug在4.3.06修复；遇到这种错误不能只加ignore绕过，也不能把论坛Spinebot的“复播导出”解释代替User Guide的“写出设置JSON”。[Nate对action顺序与4.3.06修复的说明](https://esotericsoftware.com/forum/d/30374-exports-export-setting-in-cli)

#### P9.7 --set与配置字段的实际对应

本机4.3保存设置中可核对Texture Packer的`legacyOutput`、`autoScale`、`prettyPrint`、`bruteForce`、`separator`等键名。因此P8.2可供CLI字段定位；值仍须按该格式实际接受的类型。以下为只改变一次打包参数的原创例，不改源工程：

```sh
/Applications/Spine.app/Contents/MacOS/Spine -u 4.3.26 -i /work/hero/images -j /work/hero/hero.spine -o /work/hero/atlas-small -n hero -p /work/hero/pack.json --set maxWidth=1024 --set maxHeight=1024
```

脚本应检查未知参数警告与输出结果。官方仅声明override export/pack设置，不能推导`--set`可调用任意编辑器操作、增加骨骼或修改动画曲线。[官方新增记录](https://esotericsoftware.com/spine-changelog#v4-3-73-beta)

#### P9.8 4.3骨架／动画合并及路径模式

官方Nate给出的参数关系：`--from`选择源骨架，输入仅一个骨架时可省；`--to`选择目标名称，存在时合入，否则创建。`--merge`合并骨架；`--replace`分别控制同名骨架、合并的附件或动画替换。`-a`可多次选择动画，4.3.07后可不带名选全部。`-s`接受数值或atlas文件。

原创示例假定copy.spine是独立目标副本：

```sh
/Applications/Spine.app/Contents/MacOS/Spine -u 4.3.26 -i /work/hero/source.spine -o /work/hero/copy.spine --from sourceHero --to hero --merge -r
/Applications/Spine.app/Contents/MacOS/Spine -u 4.3.26 -i /work/hero/source.spine -o /work/hero/copy.spine --from sourceHero --to hero -a walk -a run --replace -r
/Applications/Spine.app/Contents/MacOS/Spine -u 4.3.26 -i /work/hero/source.spine -o /work/hero/copy.spine --from sourceHero --to hero -a -r
```

`-i`／`-j`模式应引用；`**`递归，逗号分隔根目录与模式，`!`排除，`~`正则。例如输入`'/work/hero,**/*.spine,!**/wip/**'`；正则可用`'/work/hero,~.*[0-9]\.spine'`。先用Info确认选中文件，再增加变更操作。未知参数统一推荐最前的`--ignore-unknown`。Nate后续明确它是launcher选项`--ignore-unknown-parameters`的简称；4.3.05日志把该简称打印成`--ignore-unknown-args`，早先Nate也使用该args拼法。不能把args与unknown解释成互斥的launcher／editor专属参数。当前证据能确认简称作用于launcher，不能保证所有历史版本都接受每种长拼法。[Nate的4.3 CLI完整说明](https://esotericsoftware.com/forum/d/30191-将a-skeletons的一切内容合并到b-skeletons的问题)、[Nate后续对ignore拼法与位置的说明](https://esotericsoftware.com/forum/d/30374-exports-export-setting-in-cli)

<a id="pipeline-p10"></a>

### P10. Settings逐项参考

#### P10.1 Application

| 分组 | 字段 | 影响 |
| --- | --- | --- |
| Launcher | Version | 下次加载编辑器版本；Other输入完整版本 |
| Launcher | Start automatically | 是否停在launcher选择版本 |
| Files | Backups | 保存前副本与自动备份目录 |
| Files | Hotkeys | 打开可编辑热键文件 |
| Files | Log | 本次启动spine.log位置 |
| General | Color management | 按显示器配置渲染；gamma／linear混合；Linux不可用 |
| General | Editor frame rate | 编辑器渲染上限；不改动画时间轴FPS |
| General | Reuse instance | 系统打开工程时复用进程；Mac无此选项 |
| General | Show FPS | 播放时标题栏显示刷新率 |
| General | Welcome screen | 启动显示Welcome，关时打开最近工程 |

传统备份／热键／日志基础目录：Windows用户目录`Spine`，Mac `~/Library/Application Support/Spine`，Linux `~/.spine`。本机4.3热键实际在`settings/hotkeys-1.txt`，因此应通过Settings的Files入口定位，不把旧指南文件布局当成所有版本固定路径。[官方 Application](https://esotericsoftware.com/spine-settings#Application)

#### P10.2 User interface

| 字段 | 说明 |
| --- | --- |
| Language | 界面语言，默认尽可能跟系统 |
| Font | Bitmap适合有限字符集；Unicode及中日韩对应字体 |
| Font size | 字号；Bitmap仅中／大 |
| Default timeline FPS | 新工程默认时间轴FPS |
| Interface scale | UI比例；100%／200%通常更锐利 |
| Row height | Graph／Dopesheet行高 |
| Toolbar position | 主要工具栏左／中／右；Setup／Animate可分别设置 |
| Toolbar text labels | 总显示／隐藏／Automatic空间不足隐藏 |
| Tree indentation | Tree节点缩进像素 |
| Tree colors【本机4.3.26】 | 根据每项编辑器颜色为Tree线条着色 |
| Fewer timeline ticks【本机4.3.26】 | 刻度使用1—2—5系列，替代按Timeline FPS的除数取刻度 |

[官方 User interface](https://esotericsoftware.com/spine-settings#User-interface)

#### P10.3 Viewport

| 字段 | 作用与数值边界 |
| --- | --- |
| Background | 背景颜色与图案；部分颜色Alpha=0可隐藏 |
| Backface culling | 不画背向屏幕的三角形 |
| Bone scale | 匹配不同尺寸骨架的骨骼显示和缩放体验 |
| Color bleed | 图片载入时为透明RGB补色；更大值增加加载处理 |
| Dim unselected skeletons | 多骨架中暗化未选项 |
| Highlight attachments | 悬停／选择时边缘高亮 |
| Highlight smoothing | 高亮平滑或像素化 |
| Missing images | 红色缺图标记，关时黑色 |
| Multisample anti-aliasing | GPU支持的MSAA |
| Pixel grid | 像素栅格预览；单骨架渲染限2048×2048 |
| Smoothing | 0最近邻；1—10线性强度；11 bicubic |
| Anisotropic filtering | 改善缩小采样；图片GPU内存增加；影响相应导出 |
| Keep edges | 半透明像素不应用相同平滑 |
| Breadcrumbs【本机4.3.26】 | 为所选项目显示多少层父项；本机当前20不能当出厂默认 |

这些设置不会自动修改目标引擎的材质或texture importer。对齐运行结果时需要两边分别核查。[官方 Viewport](https://esotericsoftware.com/spine-settings#Viewport)

#### P10.4 Behavior / Dopesheet / Graph

| 分组 | 字段 | 行为 |
| --- | --- | --- |
| Behavior | Automatic backup | 有修改时周期备份 |
| Behavior | Delete confirmation | 删除对象前确认 |
| Behavior | Double click shortcuts | 双击重命名／取消选择等快捷行为 |
| Behavior | Interface animations | 菜单／对话框过渡动画 |
| Behavior | Middle mouse pans | 中键改为平移；默认中键新选择 |
| Behavior | Pan momentum | 平移短暂惯性 |
| Behavior | Smooth scrolling | 滚动插值 |
| Behavior | Tooltips | 悬停提示；关时F1仍可显示 |
| Behavior | Zoom to mouse | 鼠标位置或视口中心缩放 |
| Dopesheet | Box select pause | 选择框短暂保持 |
| Dopesheet | Jump to frame | 点击空白跳到对应帧 |
| Dopesheet | Jump to key | 点击键跳到键所在帧 |
| Graph | Box select pause【本机4.3.26】 | 短暂暂停以防选择框消失 |
| Graph | Drag to edit | 空白拖动编辑已有选择；关时需抓键／手柄 |
| Graph | Jump to frame / key | 点击跳帧／键 |

上述标注本机4.3.26的四项由原生中文Settings界面及提示只读核实，2026-10-09；没有更改用户值。macOS的Files分类只见Backups／Log／Hotkeys，未见Windows network paths；不能由配置文件存在该键推断所有平台均有同名设置。

默认F12打开Settings。热键文件可自定义，4.3允许`#`或`//`注释；数值表达式与完整工具热键属于主手册动画／工具参考，不在这里重复。[官方 Behavior](https://esotericsoftware.com/spine-settings#Behavior)、[4.3 热键注释](https://esotericsoftware.com/spine-changelog#v4-3-07)

<a id="pipeline-p11"></a>

### P11. Versioning与工程归档

#### P11.1 版本选择

Editor是major.minor.patch，runtime通常对应major.minor分支。Stable不含-beta；Beta用于新功能试用，具体runtime可能尚未支持。Launcher或Settings可选Latest stable、Latest beta或Specific version；正式项目锁定完整编辑器版本和runtime提交更利于复现。Start automatically开启时，启动窗口出现后点击可阻止立即进入。[官方 Versioning](https://esotericsoftware.com/spine-versioning#Choosing-a-Spine-editor-version)

#### P11.2 同步与回退

| 情况 | 处理 |
| --- | --- |
| 编辑器与runtime都是4.3 | 同一major.minor的数据路径 |
| 更新编辑器补丁 | 同分支兼容策略；仍看相关修复与工程回归 |
| 4.2 runtime升级4.3 | 升级调用／材质并重新导出全部工程 |
| 新版打开旧.spine | 可以；新版本保存会改变后续旧版可打开性 |
| 新版误保存 | 先找旧备份；JSON Version回退仅部分恢复 |
| 只有旧JSON／binary | 由匹配旧版本导入成.spine，再在新版本打开 |
| 长期归档 | 保存.spine、源图片／PSD、配置、版本记录；不只保存运行时导出 |

[官方 Synchronizing versions / Project files](https://esotericsoftware.com/spine-versioning#Synchronizing-versions)

原创升级验收顺序：复制工程和导出 → 在4.3打开并输出到新目录 → 用4.3runtime核对关键姿态、组合皮肤、约束、事件、图集与blend → 再替换正式构建资源。版本一致是能读取的前提，动画效果、材质和自定义脚本仍需工程级验证。


<a id="pipeline-p12"></a>

### P12. 官方子目录覆盖台账

下表逐项列出本次负责的18个官方页面的全部具名标题。表中的章节号指本补编P节；“已覆盖”表示已给出功能／字段说明和适用边界，并非已在所有平台执行测试。Video标题只指外部教程，并非缺少一类编辑器功能；这里保留官方视频入口，不转录视频内容。

#### P12.ui 标题映射

| 官方标题 | 本补编位置 | 状态 |
| --- | --- | --- |
| [User interface](https://esotericsoftware.com/spine-ui#User-interface) | P1.1 | 已覆盖 |
| [Main menu](https://esotericsoftware.com/spine-ui#Main-menu) | P1.1 | 已覆盖 |
| [Titlebar buttons](https://esotericsoftware.com/spine-ui#Titlebar-buttons) | P1.1 | 已覆盖 |
| [Opening projects](https://esotericsoftware.com/spine-ui#Opening-projects) | P1.1 | 已覆盖 |
| [File dialogs](https://esotericsoftware.com/spine-ui#File-dialogs) | P1.2 | 已覆盖 |
| [Views](https://esotericsoftware.com/spine-ui#Views) | P1.4 | 已覆盖 |
| [Tree](https://esotericsoftware.com/spine-ui#Tree) | P3.1 | 已覆盖 |
| [Viewport](https://esotericsoftware.com/spine-ui#Viewport) | P1.3 | 已覆盖 |
| [Panning](https://esotericsoftware.com/spine-ui#Panning) | P1.3 | 已覆盖 |
| [Zooming](https://esotericsoftware.com/spine-ui#Zooming) | P1.3 | 已覆盖 |
| [Setup/animate mode](https://esotericsoftware.com/spine-ui#Setupanimate-mode) | P1.3 | 已覆盖 |
| [Undo/redo](https://esotericsoftware.com/spine-ui#Undoredo) | P1.3 | 已覆盖 |
| [Video](https://esotericsoftware.com/spine-ui#Video) | 官方Video入口 | 外部教程链接 |

#### P12.images 标题映射

| 官方标题 | 本补编位置 | 状态 |
| --- | --- | --- |
| [Images](https://esotericsoftware.com/spine-images#Images) | P2.1 | 已覆盖 |
| [Images path](https://esotericsoftware.com/spine-images#Images-path) | P2.1 | 已覆盖 |
| [Creating region attachments](https://esotericsoftware.com/spine-images#Creating-region-attachments) | P2.2 | 已覆盖 |
| [Image file lookup](https://esotericsoftware.com/spine-images#Image-file-lookup) | P2.3 | 已覆盖 |
| [Import PSD](https://esotericsoftware.com/spine-images#Import-PSD) | P6 | 已覆盖 |
| [Scripts](https://esotericsoftware.com/spine-images#Scripts) | P2.5 | 已覆盖 |
| [Video](https://esotericsoftware.com/spine-images#Video) | 官方Video入口 | 外部教程链接 |

#### P12.views 标题映射

| 官方标题 | 本补编位置 | 状态 |
| --- | --- | --- |
| [Views](https://esotericsoftware.com/spine-views#Views) | P1.4 | 已覆盖 |
| [Open](https://esotericsoftware.com/spine-views#Open) | P1.4 | 已覆盖 |
| [Tabs](https://esotericsoftware.com/spine-views#Tabs) | P1.4 | 已覆盖 |
| [Focus](https://esotericsoftware.com/spine-views#Focus) | P1.4 | 已覆盖 |
| [Resize](https://esotericsoftware.com/spine-views#Resize) | P1.4 | 已覆盖 |
| [Minimize](https://esotericsoftware.com/spine-views#Minimize) | P1.4 | 已覆盖 |
| [Close](https://esotericsoftware.com/spine-views#Close) | P1.4 | 已覆盖 |
| [Multiple monitors](https://esotericsoftware.com/spine-views#Multiple-monitors) | P1.4 | 已覆盖 |
| [Video](https://esotericsoftware.com/spine-views#Video) | 官方Video入口 | 外部教程链接 |

#### P12.ghosting 标题映射

| 官方标题 | 本补编位置 | 状态 |
| --- | --- | --- |
| [Ghosting view](https://esotericsoftware.com/spine-ghosting#Ghosting-view) | P4.1 | 已覆盖 |
| [Frames](https://esotericsoftware.com/spine-ghosting#Frames) | P4.1 | 已覆盖 |
| [Key Frames](https://esotericsoftware.com/spine-ghosting#Key-Frames) | P4.1 | 已覆盖 |
| [Motion Vectors](https://esotericsoftware.com/spine-ghosting#Motion-Vectors) | P4.1 | 已覆盖 |
| [Display](https://esotericsoftware.com/spine-ghosting#Display) | P4.1 | 已覆盖 |
| [Options](https://esotericsoftware.com/spine-ghosting#Options) | P4.1 | 已覆盖 |
| [Anchor](https://esotericsoftware.com/spine-ghosting#Anchor) | P4.1 | 已覆盖 |
| [On top](https://esotericsoftware.com/spine-ghosting#On-top) | P4.1 | 已覆盖 |
| [Loop](https://esotericsoftware.com/spine-ghosting#Loop) | P4.1 | 已覆盖 |
| [Offset](https://esotericsoftware.com/spine-ghosting#Offset) | P4.1 | 已覆盖 |
| [Selection](https://esotericsoftware.com/spine-ghosting#Selection) | P4.1 | 已覆盖 |
| [Video](https://esotericsoftware.com/spine-ghosting#Video) | 官方Video入口 | 外部教程链接 |

#### P12.metrics 标题映射

| 官方标题 | 本补编位置 | 状态 |
| --- | --- | --- |
| [Metrics view](https://esotericsoftware.com/spine-metrics#Metrics-view) | P4.6 | 已覆盖 |
| [Skeletons](https://esotericsoftware.com/spine-metrics#Skeletons) | P4.6 | 已覆盖 |
| [Bones](https://esotericsoftware.com/spine-metrics#Bones) | P4.6 | 已覆盖 |
| [Constraints](https://esotericsoftware.com/spine-metrics#Constraints) | P4.6 | 已覆盖 |
| [Slots](https://esotericsoftware.com/spine-metrics#Slots) | P4.6 | 已覆盖 |
| [Attachments](https://esotericsoftware.com/spine-metrics#Attachments) | P4.6 | 已覆盖 |
| [Total](https://esotericsoftware.com/spine-metrics#Total) | P4.6 | 已覆盖 |
| [Visible](https://esotericsoftware.com/spine-metrics#Visible) | P4.6 | 已覆盖 |
| [Vertices](https://esotericsoftware.com/spine-metrics#Vertices) | P4.6 | 已覆盖 |
| [Vertex transforms](https://esotericsoftware.com/spine-metrics#Vertex-transforms) | P4.6 | 已覆盖 |
| [Triangles](https://esotericsoftware.com/spine-metrics#Triangles) | P4.6 | 已覆盖 |
| [Area](https://esotericsoftware.com/spine-metrics#Area) | P4.6 | 已覆盖 |
| [Clipping polygons](https://esotericsoftware.com/spine-metrics#Clipping-polygons) | P4.6 | 已覆盖 |
| [Clipped triangles](https://esotericsoftware.com/spine-metrics#Clipped-triangles) | P4.6 | 已覆盖 |
| [Selection](https://esotericsoftware.com/spine-metrics#Selection) | P4.6 | 已覆盖 |
| [Animation](https://esotericsoftware.com/spine-metrics#Animation) | P4.6 | 已覆盖 |
| [Timelines](https://esotericsoftware.com/spine-metrics#Timelines) | P4.6 | 已覆盖 |
| [Performance](https://esotericsoftware.com/spine-metrics#Performance) | P4.6 | 已覆盖 |
| [CPU usage](https://esotericsoftware.com/spine-metrics#CPU-usage) | P4.6 | 已覆盖 |
| [Fill rate](https://esotericsoftware.com/spine-metrics#Fill-rate) | P4.6 | 已覆盖 |
| [Draw calls](https://esotericsoftware.com/spine-metrics#Draw-calls) | P4.6 | 已覆盖 |

#### P12.outline 标题映射

| 官方标题 | 本补编位置 | 状态 |
| --- | --- | --- |
| [Outline view](https://esotericsoftware.com/spine-outline#Outline-view) | P4.2 | 已覆盖 |
| [View settings](https://esotericsoftware.com/spine-outline#View-settings) | P4.2 | 已覆盖 |
| [Ghosting](https://esotericsoftware.com/spine-outline#Ghosting) | P4.2 | 已覆盖 |

#### P12.preview 标题映射

| 官方标题 | 本补编位置 | 状态 |
| --- | --- | --- |
| [Preview view](https://esotericsoftware.com/spine-preview#Preview-view) | P4.3 | 已覆盖 |
| [Animations](https://esotericsoftware.com/spine-preview#Animations) | P4.3 | 已覆盖 |
| [Tracks](https://esotericsoftware.com/spine-preview#Tracks) | P4.3 | 已覆盖 |
| [Speed](https://esotericsoftware.com/spine-preview#Speed) | P4.3 | 已覆盖 |
| [Mix](https://esotericsoftware.com/spine-preview#Mix) | P4.3 | 已覆盖 |
| [Repeat](https://esotericsoftware.com/spine-preview#Repeat) | P4.3 | 已覆盖 |
| [Alpha](https://esotericsoftware.com/spine-preview#Alpha) | P4.3 | 已覆盖 |
| [Hold previous](https://esotericsoftware.com/spine-preview#Hold-previous) | P4.3 | 旧指南名称／行为，另标4.3差异 |
| [Additive](https://esotericsoftware.com/spine-preview#Additive) | P4.3 | 已覆盖 |
| [View settings](https://esotericsoftware.com/spine-preview#View-settings) | P4.3 | 已覆盖 |
| [Hide controls](https://esotericsoftware.com/spine-preview#Hide-controls) | P4.3 | 已覆盖 |
| [Play current animation](https://esotericsoftware.com/spine-preview#Play-current-animation) | P4.3 | 已覆盖 |
| [Show bones](https://esotericsoftware.com/spine-preview#Show-bones) | P4.3 | 已覆盖 |
| [Video](https://esotericsoftware.com/spine-preview#Video) | 官方Video入口 | 外部教程链接 |

#### P12.skins-view 标题映射

| 官方标题 | 本补编位置 | 状态 |
| --- | --- | --- |
| [Skins view](https://esotericsoftware.com/spine-skins-view#Skins-view) | P4.4 | 已覆盖 |
| [Skin list](https://esotericsoftware.com/spine-skins-view#Skin-list) | P4.4 | 已覆盖 |
| [Pins](https://esotericsoftware.com/spine-skins-view#Pins) | P4.4 | 已覆盖 |
| [Pinned skins](https://esotericsoftware.com/spine-skins-view#Pinned-skins) | P4.4 | 已覆盖 |

#### P12.slot-color 标题映射

| 官方标题 | 本补编位置 | 状态 |
| --- | --- | --- |
| [Slot Color view](https://esotericsoftware.com/spine-slot-color#Slot-Color-view) | P4.5 | 旧指南名称／行为，另标4.3差异 |

#### P12.tree 标题映射

| 官方标题 | 本补编位置 | 状态 |
| --- | --- | --- |
| [Tree view](https://esotericsoftware.com/spine-tree#Tree-view) | P3.1 | 已覆盖 |
| [Tree nodes](https://esotericsoftware.com/spine-tree#Tree-nodes) | P3.1 | 已覆盖 |
| [Expand and collapse](https://esotericsoftware.com/spine-tree#Expand-and-collapse) | P3.1 | 已覆盖 |
| [Selection](https://esotericsoftware.com/spine-tree#Selection) | P3.1 | 已覆盖 |
| [Auto scroll](https://esotericsoftware.com/spine-tree#Auto-scroll) | P3.1 | 已覆盖 |
| [Visibility](https://esotericsoftware.com/spine-tree#Visibility) | P3.1 | 已覆盖 |
| [Keying](https://esotericsoftware.com/spine-tree#Keying) | P3.1 | 已覆盖 |
| [Annotations](https://esotericsoftware.com/spine-tree#Annotations) | P3.1 | 已覆盖 |
| [Properties](https://esotericsoftware.com/spine-tree#Properties) | P3.1 | 已覆盖 |
| [Drag and drop](https://esotericsoftware.com/spine-tree#Drag-and-drop) | P3.1 | 已覆盖 |
| [Image preview](https://esotericsoftware.com/spine-tree#Image-preview) | P3.1 | 已覆盖 |
| [Shortcuts](https://esotericsoftware.com/spine-tree#Shortcuts) | P3.1 | 已覆盖 |
| [Filters](https://esotericsoftware.com/spine-tree#Filters) | P3.2 | 已覆盖 |
| [Filter buttons](https://esotericsoftware.com/spine-tree#Filter-buttons) | P3.2 | 已覆盖 |
| [Using filters](https://esotericsoftware.com/spine-tree#Using-filters) | P3.2 | 已覆盖 |
| [Find and replace](https://esotericsoftware.com/spine-tree#Find-and-replace) | P3.3 | 已覆盖 |
| [Text search](https://esotericsoftware.com/spine-tree#Text-search) | P3.2 | 已覆盖 |
| [View settings](https://esotericsoftware.com/spine-tree#View-settings) | P3.4 | 已覆盖 |
| [Hide skeleton names](https://esotericsoftware.com/spine-tree#Hide-skeleton-names) | P3.4 | 已覆盖 |
| [Show slot folders under bones](https://esotericsoftware.com/spine-tree#Show-slot-folders-under-bones) | P3.4 | 已覆盖 |
| [Show slot paths](https://esotericsoftware.com/spine-tree#Show-slot-paths) | P3.4 | 已覆盖 |
| [Show all skin attachments](https://esotericsoftware.com/spine-tree#Show-all-skin-attachments) | P3.4 | 已覆盖 |
| [Hide skin names](https://esotericsoftware.com/spine-tree#Hide-skin-names) | P3.4 | 已覆盖 |
| [Hide skin bones and constraints](https://esotericsoftware.com/spine-tree#Hide-skin-bones-and-constraints) | P3.4 | 已覆盖 |
| [Hide viewport skin bones](https://esotericsoftware.com/spine-tree#Hide-viewport-skin-bones) | P3.4 | 旧指南名称／行为，另标4.3差异 |
| [Video](https://esotericsoftware.com/spine-tree#Video) | 官方Video入口 | 外部教程链接 |

#### P12.welcome-screen 标题映射

| 官方标题 | 本补编位置 | 状态 |
| --- | --- | --- |
| [Welcome screen](https://esotericsoftware.com/spine-welcome-screen#Welcome-screen) | P1.5 | 已覆盖 |
| [Projects](https://esotericsoftware.com/spine-welcome-screen#Projects) | P1.5 | 已覆盖 |
| [News](https://esotericsoftware.com/spine-welcome-screen#News) | P1.5 | 已覆盖 |
| [Tips](https://esotericsoftware.com/spine-welcome-screen#Tips) | P1.5 | 已覆盖 |
| [Learn](https://esotericsoftware.com/spine-welcome-screen#Learn) | P1.5 | 已覆盖 |
| [Changelog](https://esotericsoftware.com/spine-welcome-screen#Changelog) | P1.5 | 已覆盖 |

#### P12.versioning 标题映射

| 官方标题 | 本补编位置 | 状态 |
| --- | --- | --- |
| [Versioning](https://esotericsoftware.com/spine-versioning#Versioning) | P11.1 | 已覆盖 |
| [Spine editor version numbers](https://esotericsoftware.com/spine-versioning#Spine-editor-version-numbers) | P11.1 | 已覆盖 |
| [Spine Runtimes version numbers](https://esotericsoftware.com/spine-versioning#Spine-Runtimes-version-numbers) | P11.1 | 已覆盖 |
| [Stable releases](https://esotericsoftware.com/spine-versioning#Stable-releases) | P11.1 | 已覆盖 |
| [Beta releases](https://esotericsoftware.com/spine-versioning#Beta-releases) | P11.1 | 已覆盖 |
| [Choosing a Spine editor version](https://esotericsoftware.com/spine-versioning#Choosing-a-Spine-editor-version) | P11.1 | 已覆盖 |
| [Latest stable](https://esotericsoftware.com/spine-versioning#Latest-stable) | P11.1 | 已覆盖 |
| [Latest beta](https://esotericsoftware.com/spine-versioning#Latest-beta) | P11.1 | 已覆盖 |
| [Specific version](https://esotericsoftware.com/spine-versioning#Specific-version) | P11.1 | 已覆盖 |
| [Synchronizing versions](https://esotericsoftware.com/spine-versioning#Synchronizing-versions) | P11.2 | 已覆盖 |
| [Updating patch versions](https://esotericsoftware.com/spine-versioning#Updating-patch-versions) | P11.2 | 已覆盖 |
| [Updating major or minor versions](https://esotericsoftware.com/spine-versioning#Updating-major-or-minor-versions) | P11.2 | 已覆盖 |
| [Project files](https://esotericsoftware.com/spine-versioning#Project-files) | P11.2 | 已覆盖 |
| [Recovering work from a newer version](https://esotericsoftware.com/spine-versioning#Recovering-work-from-a-newer-version) | P11.2 | 已覆盖 |
| [File storage](https://esotericsoftware.com/spine-versioning#File-storage) | P11.2 | 已覆盖 |

#### P12.export 标题映射

| 官方标题 | 本补编位置 | 状态 |
| --- | --- | --- |
| [Export](https://esotericsoftware.com/spine-export#Export) | P7.1 | 已覆盖 |
| [Data](https://esotericsoftware.com/spine-export#Data) | P7.2 | 已覆盖 |
| [JSON](https://esotericsoftware.com/spine-export#JSON) | P7.2 | 已覆盖 |
| [Binary](https://esotericsoftware.com/spine-export#Binary) | P7.2 | 已覆盖 |
| [Images](https://esotericsoftware.com/spine-export#Images) | P7.3 | 已覆盖 |
| [GIF](https://esotericsoftware.com/spine-export#GIF) | P7.3 | 已覆盖 |
| [PNG](https://esotericsoftware.com/spine-export#PNG) | P7.3 | 已覆盖 |
| [APNG](https://esotericsoftware.com/spine-export#APNG) | P7.3 | 已覆盖 |
| [PSD](https://esotericsoftware.com/spine-export#PSD) | P7.3 | 已覆盖 |
| [JPEG](https://esotericsoftware.com/spine-export#JPEG) | P7.3 | 已覆盖 |
| [Video](https://esotericsoftware.com/spine-export#Video) | P7.3 | 已覆盖 |
| [AVI](https://esotericsoftware.com/spine-export#AVI) | P7.3 | 已覆盖 |
| [MOV](https://esotericsoftware.com/spine-export#MOV) | P7.3 | 已覆盖 |
| [Common settings](https://esotericsoftware.com/spine-export#Common-settings) | P7.5 | 已覆盖 |
| [Preview panel](https://esotericsoftware.com/spine-export#Preview-panel) | P7.6 | 已覆盖 |
| [Additive blending](https://esotericsoftware.com/spine-export#Additive-blending) | P7.6 | 已覆盖 |
| [Saving and loading export settings](https://esotericsoftware.com/spine-export#Saving-and-loading-export-settings) | P7.6 | 已覆盖 |
| [Disabling export](https://esotericsoftware.com/spine-export#Disabling-export) | P7.6 | 已覆盖 |

#### P12.texture-packer 标题映射

| 官方标题 | 本补编位置 | 状态 |
| --- | --- | --- |
| [Texture packing](https://esotericsoftware.com/spine-texture-packer#Texture-packing) | P8.1 | 已覆盖 |
| [Texture atlas files](https://esotericsoftware.com/spine-texture-packer#Texture-atlas-files) | P8.1 | 已覆盖 |
| [Packing](https://esotericsoftware.com/spine-texture-packer#Packing) | P8.1 | 已覆盖 |
| [Packing during data export](https://esotericsoftware.com/spine-texture-packer#Packing-during-data-export) | P8.1 | 已覆盖 |
| [Running the texture packer separately](https://esotericsoftware.com/spine-texture-packer#Running-the-texture-packer-separately) | P8.1 | 已覆盖 |
| [Settings](https://esotericsoftware.com/spine-texture-packer#Settings) | P8.2 | 已覆盖 |
| [Regions](https://esotericsoftware.com/spine-texture-packer#Regions) | P8.2 | 已覆盖 |
| [Region padding](https://esotericsoftware.com/spine-texture-packer#Region-padding) | P8.2 | 已覆盖 |
| [Pages](https://esotericsoftware.com/spine-texture-packer#Pages) | P8.2 | 已覆盖 |
| [Runtime](https://esotericsoftware.com/spine-texture-packer#Runtime) | P8.2 | 已覆盖 |
| [Output](https://esotericsoftware.com/spine-texture-packer#Output) | P8.2 / P8.6 | 已覆盖 |
| [Options](https://esotericsoftware.com/spine-texture-packer#Options) | P8.2 | 已覆盖 |
| [Other](https://esotericsoftware.com/spine-texture-packer#Other) | P8.2 | 已覆盖 |
| [Folder structure](https://esotericsoftware.com/spine-texture-packer#Folder-structure) | P8.4 | 已覆盖 |
| [JSON Configuration](https://esotericsoftware.com/spine-texture-packer#JSON-Configuration) | P8.4 | 已覆盖 |
| [Texture atlas name](https://esotericsoftware.com/spine-texture-packer#Texture-atlas-name) | P8.5 | 已覆盖 |
| [Ninepatches](https://esotericsoftware.com/spine-texture-packer#Ninepatches) | P8.5 | 已覆盖 |
| [Image indexes](https://esotericsoftware.com/spine-texture-packer#Image-indexes) | P8.5 | 已覆盖 |
| [Texture Unpacker](https://esotericsoftware.com/spine-texture-packer#Texture-Unpacker) | P8.5 | 已覆盖 |

#### P12.import 标题映射

| 官方标题 | 本补编位置 | 状态 |
| --- | --- | --- |
| [Import](https://esotericsoftware.com/spine-import#Import) | P5.2 | 已覆盖 |
| [Project](https://esotericsoftware.com/spine-import#Project) | P5.1 | 已覆盖 |
| [Skeleton](https://esotericsoftware.com/spine-import#Skeleton) | P5.1 | 已覆盖 |
| [Animation](https://esotericsoftware.com/spine-import#Animation) | P5.1 | 已覆盖 |
| [Data](https://esotericsoftware.com/spine-import#Data) | P5.2 | 已覆盖 |
| [Scale](https://esotericsoftware.com/spine-import#Scale) | P5.2 | 已覆盖 |
| [New project](https://esotericsoftware.com/spine-import#New-project) | P5.2 | 已覆盖 |
| [Create a new skeleton](https://esotericsoftware.com/spine-import#Create-a-new-skeleton) | P5.2 | 已覆盖 |
| [Import into an existing skeleton](https://esotericsoftware.com/spine-import#Import-into-an-existing-skeleton) | P5.2 | 已覆盖 |
| [Existing attachments](https://esotericsoftware.com/spine-import#Existing-attachments) | P5.2 | 已覆盖 |
| [Nonessential data](https://esotericsoftware.com/spine-import#Nonessential-data) | P5.2 | 已覆盖 |

#### P12.import-psd 标题映射

| 官方标题 | 本补编位置 | 状态 |
| --- | --- | --- |
| [Import PSD](https://esotericsoftware.com/spine-import-psd#Import-PSD) | P6.1 | 已覆盖 |
| [Usage](https://esotericsoftware.com/spine-import-psd#Usage) | P6.1 | 已覆盖 |
| [Import PSD File](https://esotericsoftware.com/spine-import-psd#Import-PSD-File) | P6.1 | 已覆盖 |
| [Output PNG Files](https://esotericsoftware.com/spine-import-psd#Output-PNG-Files) | P6.1 | 已覆盖 |
| [Import Data](https://esotericsoftware.com/spine-import-psd#Import-Data) | P6.1 | 已覆盖 |
| [PSD Setup](https://esotericsoftware.com/spine-import-psd#PSD-Setup) | P6.1 | 已覆盖 |
| [Origin](https://esotericsoftware.com/spine-import-psd#Origin) | P6.1 | 已覆盖 |
| [Tags](https://esotericsoftware.com/spine-import-psd#Tags) | P6.2 | 已覆盖 |
| [Group and layer tags](https://esotericsoftware.com/spine-import-psd#Group-and-layer-tags) | P6.2 | 已覆盖 |
| [Layer tags](https://esotericsoftware.com/spine-import-psd#Layer-tags) | P6.2 | 已覆盖 |
| [Group tags](https://esotericsoftware.com/spine-import-psd#Group-tags) | P6.2 | 已覆盖 |
| [Attachment properties](https://esotericsoftware.com/spine-import-psd#Attachment-properties) | P6.3 | 已覆盖 |
| [Slashes](https://esotericsoftware.com/spine-import-psd#Slashes) | P6.3 | 已覆盖 |
| [Blending Modes](https://esotericsoftware.com/spine-import-psd#Blending-Modes) | P6.4 | 已覆盖 |
| [Draw order](https://esotericsoftware.com/spine-import-psd#Draw-order) | P6.4 | 已覆盖 |
| [Limitations](https://esotericsoftware.com/spine-import-psd#Limitations) | P6.4 | 已覆盖 |

#### P12.command-line-interface 标题映射

| 官方标题 | 本补编位置 | 状态 |
| --- | --- | --- |
| [Command line interface](https://esotericsoftware.com/spine-command-line-interface#Command-line-interface) | P9.1 | 已覆盖 |
| [Usage](https://esotericsoftware.com/spine-command-line-interface#Usage) | P9.1 | 已覆盖 |
| [Editor](https://esotericsoftware.com/spine-command-line-interface#Editor) | P9.2 | 已覆盖 |
| [Export](https://esotericsoftware.com/spine-command-line-interface#Export) | P9.3 / P9.7 | 已覆盖 |
| [Import](https://esotericsoftware.com/spine-command-line-interface#Import) | P9.3 / P9.8 | 已覆盖 |
| [Clean up](https://esotericsoftware.com/spine-command-line-interface#Clean-up) | P9.3 | 已覆盖 |
| [Pack](https://esotericsoftware.com/spine-command-line-interface#Pack) | P9.3 / P9.7 | 已覆盖 |
| [Unpack](https://esotericsoftware.com/spine-command-line-interface#Unpack) | P9.3 | 已覆盖 |
| [Info](https://esotericsoftware.com/spine-command-line-interface#Info) | P9.3 | 已覆盖 |
| [Advanced](https://esotericsoftware.com/spine-command-line-interface#Advanced) | P9.4 | 已覆盖 |
| [Examples](https://esotericsoftware.com/spine-command-line-interface#Examples) | P9.6 / P9.8 | 已覆盖 |
| [Unknown parameters](https://esotericsoftware.com/spine-command-line-interface#Unknown-parameters) | P9.5 / P9.8 | 已覆盖 |
| [Running Spine with CLI parameters](https://esotericsoftware.com/spine-command-line-interface#Running-Spine-with-CLI-parameters) | P9.1 | 已覆盖 |
| [Windows](https://esotericsoftware.com/spine-command-line-interface#Windows) | P9.1 | 已覆盖 |
| [Mac](https://esotericsoftware.com/spine-command-line-interface#Mac) | P9.1 | 已覆盖 |
| [Linux](https://esotericsoftware.com/spine-command-line-interface#Linux) | P9.1 | 已覆盖 |

#### P12.settings 标题映射

| 官方标题 | 本补编位置 | 状态 |
| --- | --- | --- |
| [Settings](https://esotericsoftware.com/spine-settings#Settings) | P10.1 | 已覆盖 |
| [Application](https://esotericsoftware.com/spine-settings#Application) | P10.1 | 已覆盖 |
| [Launcher](https://esotericsoftware.com/spine-settings#Launcher) | P10.1 | 已覆盖 |
| [Files](https://esotericsoftware.com/spine-settings#Files) | P10.1 | 已覆盖 |
| [General](https://esotericsoftware.com/spine-settings#General) | P10.1 | 已覆盖 |
| [User interface](https://esotericsoftware.com/spine-settings#User-interface) | P10.2 | 已覆盖 |
| [Viewport](https://esotericsoftware.com/spine-settings#Viewport) | P10.3 | 已覆盖 |
| [Behavior](https://esotericsoftware.com/spine-settings#Behavior) | P10.4 | 已覆盖 |
| [Dopesheet](https://esotericsoftware.com/spine-settings#Dopesheet) | P10.4 | 已覆盖 |
| [Graph](https://esotericsoftware.com/spine-settings#Graph) | P10.4 | 已覆盖 |

核对结果：18个页面，221个官方标题均有映射；标题层级本身不能证明所有隐藏参数和未公开默认值都已穷尽。


<a id="reference-runtime"></a>

## 30. 运行时、Player 与 Web Components 参考

- [R1. 数据、实例和资源的职责](#runtime-r1)

- [R2. 加载链和三种缩放](#runtime-r2)

- [R3. 每帧执行顺序与 4.3 Pose](#runtime-r3)

- [R4. 轨道控制、排队和淡出](#runtime-r4)

- [R5. 时间、混合与瞬时属性](#runtime-r5)

- [R6. 生命周期和业务事件](#runtime-r6)

- [R7. 换皮肤、换附件和绘制结果](#runtime-r7)

- [R8. 几何查询与交互](#runtime-r8)

- [R9. Player 的配置参考](#runtime-r9)

- [R10. Web Components 的全部配置类别](#runtime-r10)

- [R11. 排错流程与验证范围](#runtime-r11)

核对日期：2026-10-09。本篇补充主手册第 21、24 章，说明从编辑器数据到应用行为的完整工作链。示例采用当前官方 `4.3` 分支的 TypeScript 核心接口，其他语言需使用各自对应名称。本篇的应用设计例子由本手册提出，没有将旧版伪代码直接改一个版本号。

<a id="runtime-r1"></a>

### R1. 数据、实例和资源的职责

| 对象层 | 保存什么 | 应怎样使用 |
| --- | --- | --- |
| SkeletonData | 装配姿态、骨骼与插槽定义、皮肤、附件、动画定义 | 同一角色的大量实例共享；不要把其中的值当某一个角色的当前状态 |
| Skeleton | 当前皮肤、骨骼／插槽／约束的实例、骨架位置和颜色 | 每个独立角色各建一个实例 |
| AnimationStateData | 默认及动画对之间的混合时长 | 同一套转场配置可共享 |
| AnimationState | 当前轨道、队列、播放时间、混合状态 | 有独立动作状态的角色各建一个 |
| TextureAtlas／纹理 | 素材页、区域位置与 GPU 资源 | 按资源生命周期管理，避免角色销毁时误释放其他实例仍使用的纹理 |

修改共享定义会影响使用它的所有实例。例如“某一个敌人胳膊变长”应先判断需要改当前姿态，还是复制其专属数据；直接修改共享 BoneData 会让其他敌人一起变化。[Runtime Architecture](https://esotericsoftware.com/spine-runtime-architecture)

<a id="runtime-r2"></a>

### R2. 加载链和三种缩放

加载链是：atlas 文本解析 → 纹理页加载 → 创建 AttachmentLoader → JSON／Binary 解析 → SkeletonData → Skeleton 与 AnimationState。应用提供的纹理加载器负责实际的图像创建／释放，骨架解析器不负责猜测引擎的纹理类型。自定义 AttachmentLoader 可提供不同附件对象，适用于延迟加载或只计算几何的应用。[Loading Skeleton Data](https://esotericsoftware.com/spine-loading-skeleton-data)

应区分三种缩放：

| 缩放位置 | 用途 | 实际需要检查 |
| --- | --- | --- |
| Loader 的 scale | 在载入时换算坐标单位，例如像素转换为游戏单位 | 程序传给 IK／碰撞的坐标是否也采用相同单位 |
| Skeleton.scaleX／scaleY | 调整整个实例显示大小，包括不继承父缩放的骨骼 | 镜像后坐标方向、裁剪与目标点 |
| 某个 bone.pose 的缩放 | 局部角色动作，例如手臂拉伸 | 继承模式、约束、权重变形和体积补偿 |

纹理分辨率也可以变化，但它是素材质量选择；不要把“换半分辨率 atlas”误当“角色坐标单位自动变一半”。当前 Skeleton 的整体缩放行为见 [Skeleton.ts](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/Skeleton.ts)。

交付时保存四个版本信息：编辑器具体版本、导出数据版本、运行时分支／提交、引擎集成版本。`4.3` 只说明主要数据版本，不能代替可复现的补丁版本记录。

<a id="runtime-r3"></a>

### R3. 每帧执行顺序与 4.3 Pose

4.3 将装配值、未约束姿态与应用后的姿态区分开。用于动画／程序输入的 `pose` 与供渲染和结果查询的 `appliedPose` 不应混用。例如输入鼠标目标后，要读取本帧约束求解完成的枪口，而不是上一帧的世界坐标。[BonePose.ts](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/BonePose.ts)、[SlotPose.ts](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/SlotPose.ts)

当前 TypeScript 核心的最小执行顺序如下；`state`、`skeleton`、`aimTarget` 和 `spine.Physics` 须已由应用初始化：

```javascript
state.update(deltaSeconds);
state.apply(skeleton);
aimTarget.pose.x = localAimX;
aimTarget.pose.y = localAimY;
skeleton.update(deltaSeconds);
skeleton.updateWorldTransform(spine.Physics.update);
// 此后从 appliedPose 查询世界坐标，再由对应 renderer 绘制。
```

这里刻意将程序输入放在动画应用之后，否则同帧的动画关键帧可能覆盖输入。目标若是屏幕坐标，应先经过相机／场景变换，再换算为目标骨骼父坐标空间。该代码只展示更新顺序，不包含某个引擎的相机转换实现。[Runtime Skeletons](https://esotericsoftware.com/spine-runtime-skeletons)

| Physics 模式 | 是否推进模拟 | 是否应用物理结果 | 场景 |
| --- | --- | --- | --- |
| none | 否 | 否 | 有意计算无物理姿态 |
| reset | 重置 | 按重置路径处理 | 瞬移、重新开始后建立新状态 |
| update | 是 | 是 | 通常每帧主更新 |
| pose | 否 | 是 | 需要复用已有物理状态再次计算姿态 |

第二次计算世界变换时先决定是否需要保留物理结果；`none` 并不等于“保留结果但暂停时间”。动画状态时间和骨架物理时间是两条需要协调的输入。不能依赖暂停动画自动暂停一切程序或物理行为。[Physics.ts](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/Physics.ts)

<a id="runtime-r4"></a>

### R4. 轨道控制、排队和淡出

| 操作 | 行为 | 应用中的选择 |
| --- | --- | --- |
| setAnimation | 替换该轨道当前动作及后续队列 | 玩家收到立即生效的移动／攻击指令 |
| addAnimation | 放到当前或末尾队列之后 | 攻击后回到待机 |
| clearTrack／clearTracks | 移除动作控制 | 还需自己决定保留姿态还是恢复 |
| setEmptyAnimation／addEmptyAnimation | 用无时间线的动作将该层影响淡出 | 上身射击层逐渐交回走路层 |
| getTrack（当前 TypeScript 4.3） | 获取当前播放实例；其他语言／旧版本名称可能不同 | 读取当前动作或临时修改参数 |

轨道从低到高应用；高轨道影响它实际打过键的属性。一个没有腿部键的“射击”动作可以覆盖手臂而保留低轨道步行。若制作者为所有骨骼补了键，高轨道就会覆盖这些骨骼，程序不能仅凭“这个动画叫射击”知道它是上身动作。[Applying Animations](https://esotericsoftware.com/spine-applying-animations)

队列的 delay 不是一律表示“上一个动作播完后再等待多少秒”。传入正值与非正值的计算含义不同，后者会结合前一个动作结束时间及混合时长；改变排队后的 mix 或速度，也应复核切换点。具体计算以当前 [AnimationState.ts](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/AnimationState.ts) 为准。

本手册建议的动作组织例子：轨道 0 放待机／走／跑，轨道 1 放局部表情，轨道 2 放受击反应；这个编号是制作约定，Spine 没有固定要求这三个用途。为每层写出“允许控制哪些属性”，比在程序里猜动作名称更容易排查覆盖问题。

<a id="runtime-r5"></a>

### R5. 时间、混合与瞬时属性

| 控制项 | 应回答的问题 |
| --- | --- |
| loop | 动作片段是否循环？ |
| trackTime | 当前播放实例推进到了哪里？ |
| animationStart／animationEnd | 播放整个动作，还是其中一个片段？ |
| trackEnd | 何时退出轨道控制？它与动画自身结束不是一回事 |
| AnimationState.timeScale | 整个动画状态加速还是减速？ |
| TrackEntry.timeScale | 单独这一个动作实例加速还是减速？ |
| alpha | 高层动作覆盖低层多大程度？ |
| mixDuration／mixTime | 转场需要多久／目前进行到哪里？ |
| mixInterpolation | 转场权重按哪条插值规律变化？ |
| additive | 支持叠加的属性是否改为加法组合？ |
| shortestRotation | 混合时是否持续选择最短旋转方向？ |
| reverse | 是否反向应用动作？ |

上述是播放参数，不是所有字段都可以任意在任何时点修改；例如 additive 应在新播放实例首次 apply 前设置。局部 timeScale 也不自动按同一比例改变 mixTime。低层与高层的工作方式见 [4.3 TrackEntry](https://esotericsoftware.com/spine-api-reference#TrackEntry)，实现细节见 [AnimationState.ts](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/AnimationState.ts)。

颜色、位移通常能连续插值；附件切换、事件、绘制顺序等具有瞬时行为。做淡出时应同时检查事件和附件／draw order 的阈值，而不只看数值权重。4.3 的阈值名称包括 `eventThreshold`、`mixAttachmentThreshold`、`alphaAttachmentThreshold`、`mixDrawOrderThreshold`；旧 `attachmentThreshold`／`drawOrderThreshold` 名称不宜直接套用。

**旧指南冲突：** 在线 Applying Animations 曾写 AnimationState 不支持反向播放；当前 4.3 实现已有 `reverse` 及反向事件收集。反播应使用这个接口，而不是给不支持负值的 TrackEntry.timeScale 传负数。涉及事件方向的应用仍需测试自己的通知处理。

<a id="runtime-r6"></a>

### R6. 生命周期和业务事件

| 通知 | 本手册的业务解释 |
| --- | --- |
| start | 播放实例开始成为当前动作 |
| interrupt | 被另一个动作打断；可能仍参与混合 |
| complete | 非循环动作达到末尾，或循环动作完成一轮 |
| end | 此实例停止继续应用 |
| dispose | 此播放实例不再可用，应释放程序持有的引用 |
| event | 制作者在动画中设置的事件被触发 |

`complete` 与 `end` 不能作为同一个条件。攻击动作到末尾后还可能保持最后姿态，循环动作则可多次 complete。用于“扣一次伤害”的逻辑需要自己的对象／攻击编号，不能仅靠通知名称判断次数。可在整个 AnimationState 或一个 TrackEntry 上注册监听。[4.3 AnimationStateListener](https://esotericsoftware.com/spine-api-reference#AnimationStateListener)

需要捕获 start 时，先注册全局监听，再调用 setAnimation；给返回的 TrackEntry 后补监听可能已错过开始通知。不要持有 dispose 后的 TrackEntry。回调里切动作会影响后续姿态应用时机；为回调设置防重入逻辑，并明确新动作从当前帧还是下一帧开始。官方状态管理行为见 [AnimationState.ts](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/AnimationState.ts)。

音频事件的应用侧职责是选择声音资源、调用音频系统、处理音量／平衡及停止策略；伤害事件的应用侧职责是找到目标、核验命中和更新游戏状态。这些业务处理不由附件类型或编辑器预览自动完成。

<a id="runtime-r7"></a>

### R7. 换皮肤、换附件和绘制结果

皮肤通过“插槽＋占位名”查找附件；普通附件可从 default skin 回退查找。组合外套、头发、武器时构建一个合成 Skin，再应用到实例；同一个键的覆盖顺序必须由应用约定。[Runtime Skins](https://esotericsoftware.com/spine-runtime-skins)

切换过程建议逐层恢复：先 setSkin，再恢复插槽装配状态，再 apply 当前动画，最后应用有意由程序控制的附件。当前 TypeScript 4.3 恢复方法名为 `setupPoseSlots()`；在线旧文中的 `setSlotsToSetupPose()` 不是本例的新接口。需要只换一个槽时使用 setAttachment；传 null 清空槽。此后的动画附件键仍可以覆盖它。[Skeleton.ts](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/Skeleton.ts)

应用的检查例子：换皮肤后身体已变而武器未变，先检查武器是否属于 default skin、名称是否为占位名、是否被当前动画设成另一个附件，而不是立即重导素材。

**实际绘制链：** 读取 `drawOrder.appliedPose` → 读取各槽 `appliedPose.attachment` → 为 Region／Mesh 计算世界顶点 → 采用 atlas UV、纹理、颜色和混合方式 → 按裁剪作用域处理三角形 → 提交对应 renderer。其他附件类型提供辅助几何／控制数据，不能全部当有纹理的图片绘制。平台功能差异仍以第 21.6 节为准。

<a id="runtime-r8"></a>

### R8. 几何查询与交互

| 需要的结果 | 采用的数据 | 不应混淆 |
| --- | --- | --- |
| 角色可见图片范围 | Region／Mesh 的世界顶点范围 | 不等于作者设计的伤害框 |
| 精确点击／攻击框 | BoundingBoxAttachment 的世界多边形 | 不自动注册为引擎 Collider |
| 枪口／挂件锚点 | PointAttachment 与骨骼应用后变换 | 不能直接用局部 x／y 当屏幕坐标 |
| 粗筛命中 | AABB | 粗筛通过后仍需多边形或业务规则确认 |
| 排序／显示 | 当前 draw order 与槽附件 | 骨骼父子树不是最终绘制顺序 |

SkeletonBounds 可更新当前显示的 bounding boxes、多边形和 AABB，并提供点／线段命中查询；没有显示的框不会因“文件中定义过”自动成为本帧的有效框。若禁用 updateAabb，不应把 AABB 方法当真实边界查询。[SkeletonBounds.ts](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-core/src/SkeletonBounds.ts)

本手册推荐的命中处理步骤：在动画和约束更新完成后获取几何；将输入点换成同一坐标空间；粗筛后查多边形；最后应用阵营、无敌、攻击窗口等游戏规则。对需要逐像素透明区域点击的界面，应另行设计透明度采样规则，包围多边形不等于像素透明度。

<a id="runtime-r9"></a>

### R9. Player 的配置参考

Player 适合带播放器控制的网页展示；配置对象与 Web Components 的 HTML 属性并不通用。资源准备、控件和视口行为见 [Spine Player](https://esotericsoftware.com/spine-player)；本表以当前 [Player.ts](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-player/src/Player.ts) 的公开配置为准。

| 配置组 | 字段 | 制作时要确定的事情 |
| --- | --- | --- |
| 资源 | skeleton、atlas、jsonField、rawDataURIs、scale | 数据来源、JSON子字段、内嵌映射、坐标单位 |
| 动作／皮肤 | animation、animations、skin、skins、defaultMix | 初始动作、可选列表、组合皮肤、转场 |
| 控件 | showControls、showLoading、interactive、controlBones | 是否展示控制、加载提示、是否接管指针、可拖动控制骨 |
| 显示 | alpha、backgroundColor、fullScreenBackgroundColor、backgroundImage | 透明网页背景和骨架世界坐标中的背景图 |
| 图像／资源共享 | mipmaps、preserveDrawingBuffer、downloader | 缩小时采样质量、截图需要、共享下载 |
| 调试 | debug | bones／regions／meshes／bounds／paths／clipping／points／hulls |
| 视口 | viewport | x／y／width／height、四侧padding、clip、debugRender、transitionTime、动画单独视口 |
| 回调 | loading、success、error、frame、updateWorldTransform、update、draw | 加载、更新和绘制的接入点 |

当前源码确认：showControls／showLoading／interactive 默认开启；defaultMix 为 0.25 秒；viewport.clip 默认关闭；视口 padding 为 10%；源码 transitionTime 默认 0.25 秒，在线指南中 0.2 秒属于冲突描述。`fullScreenBackgroundColor` 的大小写须与源码一致。

4.3 配置没有公开 `premultipliedAlpha` 字段，不能照在线旧示例假定该设置仍有效；颜色问题要从 atlas 的 alpha 标记与当前纹理／renderer 管线排查。未设置 animation 且没有可选列表替代时，当前代码先采用空动画，不是旧指南说的自动播放骨架第一条动画；交付配置应直接写 animation。

通过 Player.setAnimation 切动作会配合调整视口；直接操作 animationState 时应自行决定镜头范围。自定义 updateWorldTransform 回调替代默认计算，必须主动完成需要的更新。success 之后才操作骨架和动作状态。

<a id="runtime-r10"></a>

### R10. Web Components 的全部配置类别

Web Components 使用共享 overlay，适合多个网页元素中的角色。字段名以当前 [SpineWebComponentSkeleton.ts](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-webcomponents/src/SpineWebComponentSkeleton.ts) 为准；使用方式和效果见 [Web Components](https://esotericsoftware.com/spine-webcomponents)。

| HTML 属性／程序能力 | 含义与使用选择 |
| --- | --- |
| skeleton、atlas、raw-data、json-skeleton-key | URL／内嵌资源及多骨架 JSON 中的骨架键 |
| style／width／height | HTML 元素布局大小；高度为零时没有正常展示区域 |
| animation、animations、default-mix | 单动作、分轨队列和默认转场；队列支持特殊空动作 |
| skin | 可指定多个皮肤组合；槽占位覆盖顺序仍需检查 |
| fit | contain／cover／fill／width／height／none／scaleDown／origin |
| scale | Loader的坐标缩放，与 fit 的显示适配区分 |
| x-axis、y-axis | 元素内的轴线定位 |
| offset-x、offset-y | 骨架偏移 |
| pad-left／right／top／bottom | 留白范围 |
| bounds-x、bounds-y、bounds-width、bounds-height | 指定骨架世界坐标中的适配边界；当前属性映射用复数 bounds，部分旧注释用单数 bound |
| auto-calculate-bounds、animation-bounds | 自动重算及参与边界采样的动作范围 |
| clip、identifier、overlay-id | 元素裁切、实例查找、指定共享 overlay |
| spinner、offscreen | 加载指示与离屏行为；pause／update／pose |
| drag、interactive | 拖动与指针交互 |
| pages | 选择 atlas 页；空列表可先不加载纹理 |
| debug | 原点、边界与拖动区域辅助显示 |
| whenReady、update、beforeUpdateWorldTransforms、afterUpdateWorldTransforms | 就绪等待及更新扩展 |
| pointerEventCallback、addPointerSlotEventCallback | 元素或槽附件的 down／up／enter／leave／move／drag |
| followSlot | 让 HTML 内容跟随槽，选择透明度／缩放／旋转／显示，并可隐藏原附件 |
| createSkeleton、appendTo、start、dispose | 程序创建、挂载、开始加载和释放 |
| manual-start、start-when-visible、onScreenFunction | 手工开始加载／首次进入屏幕才开始／进入屏幕通知 |

fit选择的应用例子：作品完整展示用 contain，封面填满用 cover，保持原始比例且不缩放用 none；fill 会允许比例变化。动态换动作后，边界策略决定角色会保持镜头还是改变适配尺寸，不能把截图时刚好合适的初始边界当所有动作都合适。

资源准备完成前等待 whenReady；移除 DOM 元素不等于自动释放实例。手工 overlay 适用于滚动容器或固定容器中的特殊定位需求。共享渲染减少 WebGL context 数量，但并不免除所有骨架的动画／顶点／裁剪成本。

**延迟加载：** 当前源码 `manualStart=false` 为立即加载，设为 true 后调用 start() 才加载。`startWhenVisible=true` 会启用手工开始状态，并由默认 onScreenFunction 在首次进入可视范围时开始加载。在线指南中的 `manualStart: "false"` 不应当作延迟加载的正确布尔配置。

offscreen 默认 pause，不推进状态也不应用姿态；update 推进状态但不应用；pose 同时推进并应用。选择 pose 可以让离屏后的程序读取持续更新的姿态，成本也较高。默认 fit=contain、defaultMix=0、scale=1、spinner=false、clip=false。

手工 `<spine-overlay>` 还公开 `overlay-id`、`no-auto-parent-transform`、`overflow-top`／bottom／left／right：它们控制实例关联、是否由库给父容器补变换，以及扩大画布以缓解滚动时边缘错位。增加画布范围也增加像素成本。调试时可用 SHOW_FPS，资源共享和最终释放由 overlay 生命周期管理。[SpineWebComponentOverlay.ts](https://github.com/EsotericSoftware/spine-runtimes/blob/4.3/spine-ts/spine-webcomponents/src/SpineWebComponentOverlay.ts)

<a id="runtime-r11"></a>

### R11. 排错流程与验证范围

| 症状 | 优先核对 |
| --- | --- |
| 数据加载失败 | editor／runtime 数据版本、路径、atlas区域名、二进制是否误当JSON |
| 角色局部空白 | 占位名与皮肤、附件键、选择的atlas页、缺失region |
| 黑边／白边 | alpha管线、过滤、透明像素RGB、padding、纹理压缩 |
| 鼠标目标偏移 | 屏幕→场景→骨骼父空间的转换和整体缩放 |
| 走路被上身动作盖掉 | 高轨道是否给下身属性也设键 |
| 淡出后仍保持姿态 | clear与empty的选择、后续控制的基础姿态 |
| 物理跳动 | 时间是否重复推进、是否瞬移、reset／pose／update的选择 |
| 回调重复或递归 | complete循环、混合中event、回调内再次apply |
| 帧率下降 | 骨骼实例数、受权重影响数、裁剪三角形、材质／页切换与屏幕面积 |

这些是本手册提出的排查顺序，不表示每种症状只有一个原因。验证时使用包含换皮肤、附件切换、混合、裁剪、序列和物理的代表动作；单一待机成功不能证明所有功能都成功接入。

本篇核查了当前4.3接口与源码，未构建或执行全部引擎集成。它补齐运行时功能的使用链与配置分类；每个语言的全部重载、数据格式编码和第三方平台接口仍由对应官方 API／源码提供，主手册第21.3和26.2可查入口。


<a id="reference-coverage"></a>

## 31. 官方小节覆盖核对与验证范围

- [C1. 专题覆盖汇总](#coverage-c1)

- [C2. 入门页逐项映射](#coverage-c2)

- [C3. 新旧功能的核对方法](#coverage-c3)

- [C4. 实际核验与保留事项](#coverage-c4)

核对日期：2026-10-09。基准是[官方 User Guide](https://esotericsoftware.com/spine-user-guide)链接的 **48 个专题、693 个正文标题**。标题包含章节容器、示例、练习和视频入口，不等于独立功能数量。抓取目录与逐项映射用于发现遗漏，不能代替行为验证。

<a id="coverage-c1"></a>

### C1. 专题覆盖汇总

主手册 27 为装配参考，28 为动画参考，29 为管线参考；各章末尾另有逐小节映射。

| 官方专题 | 标题数 | 主手册参考位置 |
| --- | ---: | --- |
| [getting-started](https://esotericsoftware.com/spine-getting-started) | 13 | 1-3 / C2 |
| [ui](https://esotericsoftware.com/spine-ui) | 13 | 29 |
| [skeletons](https://esotericsoftware.com/spine-skeletons) | 6 | 27 |
| [bones](https://esotericsoftware.com/spine-bones) | 17 | 27 |
| [slots](https://esotericsoftware.com/spine-slots) | 11 | 27 |
| [images](https://esotericsoftware.com/spine-images) | 7 | 29 |
| [tools](https://esotericsoftware.com/spine-tools) | 30 | 27 |
| [keys](https://esotericsoftware.com/spine-keys) | 34 | 28 |
| [animating](https://esotericsoftware.com/spine-animating) | 8 | 28 |
| [attachments](https://esotericsoftware.com/spine-attachments) | 9 | 27 |
| [regions](https://esotericsoftware.com/spine-regions) | 8 | 27 / 28 |
| [meshes](https://esotericsoftware.com/spine-meshes) | 35 | 27 |
| [bounding-boxes](https://esotericsoftware.com/spine-bounding-boxes) | 11 | 27 |
| [clipping](https://esotericsoftware.com/spine-clipping) | 14 | 27 |
| [paths](https://esotericsoftware.com/spine-paths) | 17 | 27 |
| [points](https://esotericsoftware.com/spine-points) | 3 | 27 |
| [skins](https://esotericsoftware.com/spine-skins) | 27 | 27 |
| [constraints](https://esotericsoftware.com/spine-constraints) | 10 | 27 |
| [ik-constraints](https://esotericsoftware.com/spine-ik-constraints) | 14 | 27 |
| [path-constraints](https://esotericsoftware.com/spine-path-constraints) | 13 | 27 |
| [transform-constraints](https://esotericsoftware.com/spine-transform-constraints) | 11 | 27 |
| [physics-constraints](https://esotericsoftware.com/spine-physics-constraints) | 29 | 27 |
| [sliders](https://esotericsoftware.com/spine-sliders) | 11 | 27 |
| [events](https://esotericsoftware.com/spine-events) | 16 | 28 |
| [views](https://esotericsoftware.com/spine-views) | 9 | 29 |
| [animations-view](https://esotericsoftware.com/spine-animations-view) | 3 | 28 |
| [audio-view](https://esotericsoftware.com/spine-audio-view) | 4 | 28 |
| [dopesheet](https://esotericsoftware.com/spine-dopesheet) | 30 | 28 |
| [ghosting](https://esotericsoftware.com/spine-ghosting) | 12 | 29 |
| [graph](https://esotericsoftware.com/spine-graph) | 51 | 28 |
| [mesh-tools](https://esotericsoftware.com/spine-mesh-tools) | 5 | 27 |
| [metrics](https://esotericsoftware.com/spine-metrics) | 21 | 29 |
| [outline](https://esotericsoftware.com/spine-outline) | 3 | 29 |
| [playback](https://esotericsoftware.com/spine-playback) | 6 | 28 |
| [preview](https://esotericsoftware.com/spine-preview) | 14 | 29 |
| [skins-view](https://esotericsoftware.com/spine-skins-view) | 4 | 29 |
| [slot-color](https://esotericsoftware.com/spine-slot-color) | 1 | 29 |
| [timeline](https://esotericsoftware.com/spine-timeline) | 1 | 28 |
| [tree](https://esotericsoftware.com/spine-tree) | 26 | 29 |
| [weights](https://esotericsoftware.com/spine-weights) | 25 | 27 |
| [welcome-screen](https://esotericsoftware.com/spine-welcome-screen) | 6 | 29 |
| [versioning](https://esotericsoftware.com/spine-versioning) | 15 | 29 |
| [export](https://esotericsoftware.com/spine-export) | 18 | 29 |
| [texture-packer](https://esotericsoftware.com/spine-texture-packer) | 19 | 29 |
| [import](https://esotericsoftware.com/spine-import) | 11 | 29 |
| [import-psd](https://esotericsoftware.com/spine-import-psd) | 16 | 29 |
| [command-line-interface](https://esotericsoftware.com/spine-command-line-interface) | 16 | 29 |
| [settings](https://esotericsoftware.com/spine-settings) | 10 | 29 |

<a id="coverage-c2"></a>

### C2. 入门页逐项映射

| 官方标题 | 主手册位置 |
| --- | --- |
| [Getting started](https://esotericsoftware.com/spine-getting-started#Getting-started) | 1.2 / 2 |
| [Trying and purchasing Spine](https://esotericsoftware.com/spine-getting-started#Trying-and-purchasing-Spine) | 1.3 |
| [Activation](https://esotericsoftware.com/spine-getting-started#Activation) | 1.4 |
| [Running Spine](https://esotericsoftware.com/spine-getting-started#Running-Spine) | 1.4 |
| [Spine editor versions](https://esotericsoftware.com/spine-getting-started#Spine-editor-versions) | 1.1 / 1.4 / P11 |
| [Latest stable](https://esotericsoftware.com/spine-getting-started#Latest-stable) | 1.4 / P11.1 |
| [Specific version](https://esotericsoftware.com/spine-getting-started#Specific-version) | 1.4 / P11.1 |
| [Welcome to Spine](https://esotericsoftware.com/spine-getting-started#Welcome-to-Spine) | 3.3 / P1.5 |
| [Getting to know the Spine editor](https://esotericsoftware.com/spine-getting-started#Getting-to-know-the-Spine-editor) | 2 / 3 / 27-29 |
| [Getting to know the Spine Runtimes](https://esotericsoftware.com/spine-getting-started#Getting-to-know-the-Spine-Runtimes) | 21 / 30 |
| [Finding help](https://esotericsoftware.com/spine-getting-started#Finding-help) | 1.4 / 26.2 |
| [Keeping up-to-date](https://esotericsoftware.com/spine-getting-started#Keeping-up-to-date) | 1.1 / 1.4 / 24 / P11 |
| [Next steps](https://esotericsoftware.com/spine-getting-started#Next-steps) | 25 / 26.2 |

<a id="coverage-c3"></a>

### C3. 新旧功能的核对方法

装配参考的 R2、动画参考的 A15、管线参考的 P12 保留每个官方标题及其正文位置；共同涉及 Region 与 Sequence 的标题只计一次。入门页另由 C2 映射。**合并后 48 页的 693 个标题均有对应位置，没有缺少未映射标题。** 对视频、教程入口只保留来源，并未声称逐帧观看或转录。

4.3 新能力另外对照主手册第 23 章、正式发布说明与 4.3 更新日志，补上旧网页目录尚无的 Slider、Key Constrained、Volume、Transform 跨属性映射、Graph 改进、Problems、HTML 导出与命令行合并等。WEBP／AWEBP／WEBM、Curves、基础网格、皮肤、IK、Physics 与 CLI 本身是已有能力，正文明确区分。

<a id="coverage-c4"></a>

### C4. 实际核验与保留事项

| 项目 | 本次做了什么／未覆盖什么 |
| --- | --- |
| 功能与字段 | 官方具名功能逐项说明用途、操作或参数；必要时给出应用例子、关联与限制 |
| 版本冲突 | 对照 4.3 源码、日志及官方答复，纠正 Essential、Transform、网格重建、Physics 与旧 Web/API 示例 |
| 本机界面 | 只读观察 4.3.26 Professional 的部分面板、设置、导出与打包界面，并检查快捷命令；没有修改用户动画数据 |
| Runtime／Web | 核对 4.3 的数据加载、更新生命周期、动画混合、事件、皮肤、Player 和 Web Components；不逐一复制各语言全部方法重载 |
| 默认值与硬范围 | 区分编辑器新建值、JSON 省略值和源码对象初值；未公开或未实测的值保留说明 |
| 算法与组合测试 | 没有证明 Trace／Auto／Weld 等未公开算法，也未执行全部约束组合、批量导出和所有引擎渲染 |

使用时应按具体动作、平台及素材复验。完整的公开功能说明，不等于每项功能、全部数值与所有组合都已经实测。
