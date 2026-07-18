<h1 align="center">Sober 客户端 Roblox FastFlags 指南</h1>
<p align="center">适用于 Linux 上 Sober 客户端的经验证、安全且在白名单（allowlist）内的 FastFlags。</p>

<p align="center">
  <b>Traducciones / Translations:</b>
  <a href="README.md">English</a> |
  <a href="README.es.md">Español</a> |
  <a href="README.zh.md">简体中文</a> |
  <a href="README.ru.md">Русский</a> |
  <a href="README.id.md">Bahasa Indonesia</a> |
  <a href="README.pt.md">Português</a>
</p>

---

> [!NOTE]
> 本指南是由人工智能（AI）翻译的。欢迎人工校对和贡献以提高翻译质量。如果您发现任何错误，欢迎提交 Pull Request！

---

## 目录

- [什么是 Sober？](#什么是-sober)
- [什么是 FastFlags？](#什么是-fastflags)
- [确认有效的 FFlags](#确认有效的-fflags)
  - [渲染与性能](#渲染与性能)
  - [稳定性与显存 (VRAM)](#稳定性与显存-vram)
  - [界面与环境](#界面与环境)
- [配置预设](#配置预设)
  - [预设 1：低端配置 / 显存崩溃修复](#预设-1低端配置--显存崩溃修复)
  - [预设 2：平衡配置 / 中端配置](#预设-2平衡配置--中端配置)
  - [预设 3：极致画质](#预设-3极致画质)
- [深度探讨](#深度探讨)
- [解锁帧率（FPS）上限](#解锁帧率fps上限)
- [安全与反作弊风险](#安全与反作弊风险)
- [已弃用的 FFlags](#已弃用的-fflags)
- [免责声明与来源](#免责声明与来源)

---

## 什么是 Sober？

[Sober](https://sober.vinegarhq.org/) 是一个在 Linux 桌面原生运行 Roblox Android 应用 (APK) 的兼容层。它以 Flatpak (`org.vinegarhq.Sober`) 格式分发，使用 Vulkan 作为主渲染后端，并以 OpenGL 作为备用（fallback）。配置文件路径为 `~/.var/app/org.vinegarhq.Sober/config/sober/config.json`。您也可以在终端执行 `flatpak run org.vinegarhq.Sober config` 或在应用菜单中右键点击 Sober 并选择 **Settings** 来打开图形化设置菜单。

## 什么是 FastFlags？

FastFlags (FFlags) 是 Roblox 引擎的内部变量，用于控制渲染、界面、稳定性和其他功能。自 **2025 年 9 月 29 日**起，Roblox 开始执行严格的白名单政策——只有少数白名单内的 FFlags 可以通过本地配置文件进行覆盖。任何不在白名单内的 flag 都会被客户端静默忽略。

> [!IMPORTANT]
> 本指南仅包含**确认位于当前白名单内**的 flag。旧版社区指南中的许多 flag 可能已失效。

---

## 确认有效的 FFlags

### 渲染与性能

这些 flag 控制几何细节、抗锯齿、光照以及草地渲染距离。

| Flag 名称 | 类型 | 取值范围 | 功能描述 |
| :--- | :--- | :--- | :--- |
| `DFIntCSGLevelOfDetailSwitchingDistance` | int | `0` - `1000` | CSG 模型的主 LOD 剔除距离。数值越低 = 帧率越高。 |
| `DFIntCSGLevelOfDetailSwitchingDistanceL12` | int | `0` - `1000` | 图形质量 1–2 时的 LOD 距离。 |
| `DFIntCSGLevelOfDetailSwitchingDistanceL23` | int | `0` - `1000` | 图形质量 2–3 时的 LOD 距离。 |
| `DFIntCSGLevelOfDetailSwitchingDistanceL34` | int | `0` - `1000` | 图形质量 3–4 时的 LOD 距离。 |
| `FIntDebugForceMSAASamples` | int | `1`, `2`, `4` | 强制开启 MSAA 抗锯齿（边缘更平滑，消耗 GPU）。 |
| `DFIntDebugFRMQualityLevelOverride` | int | `0` - `21` | 覆盖图形质量滑块（突破默认的 1–10 限制）。 |
| `FIntFRMMaxGrassDistance` | int | `0` - `1000` | 地形草的最大渲染距离。设置为 `0` 可禁用草。 |
| `FIntFRMMinGrassDistance` | int | `0` - `1000` | 草开始渲染的最小距离。 |

### 稳定性与显存 (VRAM)

这些 flag 有助于防止显存不足导致的崩溃，特别是对于显存（VRAM）有限的显卡。

| Flag 名称 | 类型 | 取值范围 | 功能描述 |
| :--- | :--- | :--- | :--- |
| `DFFlagTextureQualityOverrideEnabled` | bool | `true` / `false` | 允许手动控制纹理分辨率。 |
| `DFIntTextureQualityOverride` | int | `0` - `3` | 设置贴图纹理质量（0 = 最低，3 = 最高）。 |

> [!WARNING]
> 在显存为 4GB 或更低的显卡上将 `DFIntTextureQualityOverride` 设置为 `3` 极易导致瞬间发生 `RBXCRASH: OutOfMemory`（显存不足）崩溃。建议使用 `2` 或 `1` 以确保稳定。

### 界面与环境

影响视觉舒适度和界面表现的辅助 flag。

| Flag 名称 | 类型 | 取值范围 | 功能描述 |
| :--- | :--- | :--- | :--- |
| `FIntGrassMovementReducedMotionFactor` | int | `0` - `1000` | 控制草地摆动动画的强度。设置为 `0` 表示草地静止。 |

---

## 配置预设

复制下方其中一个预设，然后将其粘贴到您的 Sober 配置文件中：

```
~/.var/app/org.vinegarhq.Sober/config/sober/config.json
```

### 预设 1：低端配置 / 显存崩溃修复

适用于显存不足 4GB 的显卡、核显，或经常遇到 `OutOfMemory` 崩溃的系统。

```json
{
  "enable_hidpi": false,
  "fflags": {
    "DFFlagTextureQualityOverrideEnabled": true,
    "DFIntTextureQualityOverride": 1,
    "DFIntCSGLevelOfDetailSwitchingDistance": 100,
    "DFIntCSGLevelOfDetailSwitchingDistanceL12": 75,
    "DFIntCSGLevelOfDetailSwitchingDistanceL23": 100,
    "DFIntCSGLevelOfDetailSwitchingDistanceL34": 150,
    "FIntFRMMaxGrassDistance": 0,
    "FIntGrassMovementReducedMotionFactor": 0
  }
}
```

### 预设 2：平衡配置 / 中端配置

适用于具有 4–6GB 显存的中端显卡（如 GTX 1650、RX 580 级别）。在画质与性能之间取得良好平衡。

```json
{
  "enable_hidpi": false,
  "fflags": {
    "DFFlagTextureQualityOverrideEnabled": true,
    "DFIntTextureQualityOverride": 2,
    "DFIntCSGLevelOfDetailSwitchingDistance": 400,
    "DFIntCSGLevelOfDetailSwitchingDistanceL12": 200,
    "DFIntCSGLevelOfDetailSwitchingDistanceL23": 350,
    "DFIntCSGLevelOfDetailSwitchingDistanceL34": 500,
    "FIntDebugForceMSAASamples": 2,
    "FIntFRMMaxGrassDistance": 200,
    "FIntGrassMovementReducedMotionFactor": 50
  }
}
```

### 预设 3：极致画质

适用于拥有 8GB 以上显存的高端系统。强制启用最高画质细节、抗锯齿和贴图质量。

```json
{
  "enable_hidpi": true,
  "fflags": {
    "DFFlagTextureQualityOverrideEnabled": true,
    "DFIntTextureQualityOverride": 3,
    "DFIntCSGLevelOfDetailSwitchingDistance": 1000,
    "DFIntCSGLevelOfDetailSwitchingDistanceL34": 1000,
    "FIntDebugForceMSAASamples": 4,
    "DFIntDebugFRMQualityLevelOverride": 21
  }
}
```

> [!TIP]
> 您可以自由组合不同预设中的 flag。例如，如果您的显卡显存充足但几何计算性能不足，可以使用“平衡配置”的 LOD 距离以及“极致画质”的贴图质量。

---

## 深度探讨

<details>
<summary><strong>细节水平（LOD）与几何细节缩放</strong></summary>

Roblox 地图经常使用复杂的构造实体几何（CSG）联合体。在远距离渲染这些物体会给 CPU 和 GPU 带来沉重负担。

通过降低 `DFIntCSGLevelOfDetailSwitchingDistance`（例如设为 150），可以强制游戏在离相机较近的距离就将复杂的模型替换为低多边形（low-poly）版本。这可以在不改变物理碰撞箱（hitbox）的前提下显著提升帧率——物体的碰撞行为完全一致，只是从远处看显得较为简易。

分级变体（`L12`、`L23`、`L34`）允许您针对不同的画质等级进行微调，从而使低画质设置下的模型剔除（cull）表现得更加激进。

</details>

<details>
<summary><strong>显存（VRAM）分配与内存不足（OOM）崩溃</strong></summary>

Sober 在 Linux Flatpak 环境中运行 Roblox Android 二进制文件。Android 端程序默认采用的是移动端的共享内存模型，这与桌面端 Linux 显卡驱动处理显存（VRAM）的方式不同。

**问题所在：**当引擎请求最高画质的纹理时，可能会迅速耗尽显卡的独立显存。在 Windows 桌面端，显卡驱动会将超出限制的数据转移到系统内存中。而在 Linux 上（特别是使用 NVIDIA 闭源驱动时），这种后备机制并不可靠，从而导致瞬间发生 `RBXCRASH: OutOfMemory` 崩溃。

**解决办法：**将 `DFFlagTextureQualityOverrideEnabled` 设置为 `true`，并将 `DFIntTextureQualityOverride` 设置为 `2`（中等质量）或 `1`（低质量）。这将强制引擎从服务器请求分辨率较低的贴图，从而将显存占用限制在安全范围内。

</details>

<details>
<summary><strong>图形 API：Vulkan 与 OpenGL</strong></summary>

图形 API 的选择由 Sober 的外壳配置管理，**而非**通过内部 FFlags 管理。

- 默认情况下，Sober 使用 **Vulkan** 以获得最佳性能。
- 如果遇到图形异常、黑屏或启动崩溃（常见于较旧的 GPU 或混合显卡笔记本），可以在 `config.json` 根节点设置 `"use_opengl": true` 强制使用 OpenGL。

请勿使用像 `FFlagDebugGraphicsPreferVulkan` 或 `FFlagDebugGraphicsPreferOpenGL` 这样的引擎 FFlags——它们可能会与 Sober 外壳层 API 选择冲突导致上下文不匹配崩溃。

</details>

<details>
<summary><strong>资源覆盖 (自定义纹理与光标)</strong></summary>

Sober 允许通过 `asset_overlay` 目录替换游戏资源，路径为：
`~/.var/app/org.vinegarhq.Sober/data/sober/asset_overlay`

放置在此处的文件在重启应用后优先级高于标准 Roblox 资源。目录结构与 `packages/*/com.roblox.client/base.apk/assets` 镜像对应。

自定义鼠标光标示例：
```
~/.var/app/org.vinegarhq.Sober/data/sober/asset_overlay
└── content
    └── textures
        └── Cursors
            └── KeyboardMouse
                ├── ArrowCursor.png
                ├── ArrowFarCursor.png
                └── IBeamCursor.png
```
要还原更改，只需清空 `asset_overlay` 目录中的文件。

</details>

<details>
<summary><strong>全屏 (F11) 与退出控制</strong></summary>

Roblox 游戏内置的全屏切换按键在移动端 Android 构建中不起作用。在 Sober 中，按下 `F11` 键即可进入或退出全屏模式。Sober 会在下次启动时记住全屏状态。

若想在离开体验时自动关闭应用，可在 `config.json` 中添加 `"close_on_leave": true`。

</details>

---

## 解锁帧率（FPS）上限

FastFlag `DFIntTaskSchedulerTargetFps` 已不再起作用——它已被移出白名单。要更改帧率限制，您必须直接编辑 XML 配置文件。

### 操作步骤：

1. 在 Sober 上启动任意 Roblox 游戏以生成配置文件。
2. 完全关闭 Roblox 客户端。
3. 导航至目录 `~/.var/app/org.vinegarhq.Sober/data/sober/appData/`。
4. 在文本编辑器中打开 `GlobalBasicSettings_13.xml`。
5. 找到此行：`<int name="FramerateCap">60</int>`
6. 将 `60` 修改为您期望的帧率（例如 `144`、`240`），或改为 `0` 以解除帧率限制。
7. 保存文件，关闭编辑器，然后重新启动 Roblox。

> [!NOTE]
> 修改此文件前必须完全关闭 Roblox。客户端在退出时会重写此文件，因此在游戏运行时做出的任何修改都会丢失。

---

## 安全与反作弊风险

Roblox 使用 Hyperion (Byfron) 反作弊系统。在 `config.json` 中配置白名单内的 FFlags 是完全安全的，但试图绕过白名单则不然。

> [!CAUTION]
> 以下行为会带来被永久封号或封锁硬件的高风险。请千万不要尝试：

- **修改缓存文件（`IxpSettings.json`）：**向缓存文件中注入未经授权的 flag 并将其锁定为只读，会被 Hyperion 检测为恶意篡改文件。
- **内存编辑：**使用工具强制加载未允许的 flag（例如通过 `DFIntTimestepArbiterThresholdCFLThou` 修改物理时间步长，或通过透视修改贴图）会触发自动封号。
- **绕过白名单程序：**任何旨在绕过 FFlag 过滤器的程序或启动参数都会被归类为外挂/辅助工具。

---

## 已弃用的 FFlags

以下 flag 常见于较旧的指南中，但它们已不再白名单内。将它们添加到 `config.json` 不会产生任何效果——它们会被客户端静默忽略。

| Flag | 弃用原因 |
| :--- | :--- |
| `DFIntTaskSchedulerTargetFps` | 已由编辑 `GlobalBasicSettings_13.xml` 取代。 |
| `FFlagTaskSchedulerLimitTargetFpsTo2402` | 已从白名单中移除。 |
| `DFFlagDebugPauseVoxelizer` | 体素照明抑制在当前白名单中已被拦截。 |
| `FFlagDebugSkyGray` | 纯灰天空盒覆盖在当前白名单中已被拦截。 |
| `DFIntConnectionMTUSize` | 网络调优 Flag 已被拦截。 |
| `FFlagDebugDisableTelemetryEphemeralCounter` | 遥测抑制已被拦截。 |
| `FFlagAdServiceEnabled` | 广告服务开关已被拦截。 |
| `FFlagMovePrerender` | 线程操作 Flag 已被拦截。 |
| `DFIntDebugDynamicRenderKiloPixels` | 渲染分辨率缩放已被 Roblox 工程团队否决。 |

---

## 免责声明与来源

> [!NOTE]
> FFlag 白名单由 Roblox 官方维护，可能随客户端的后续更新而发生变化。本指南最后更新于 **2026 年 7 月**。在部署配置前，请务必参阅官方来源核实。

**官方来源：**
- [Allowlist for local client configuration via Fast Flags — Roblox DevForum](https://devforum.roblox.com/t/allowlist-for-local-client-configuration-via-fast-flags/3966569)
- [Sober Configuration Tips & Tricks — Vinegar 官方文档](https://vinegarhq.org/Sober/Configuration/TipsAndTricks.html)
