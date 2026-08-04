<h1 align="center">Roblox FastFlags guide for Sober</h1>
<p align="center">Verified, safe, and allowlisted FastFlags for the Sober client on Linux.</p>

<p align="center">
  <b>Translations:</b>
  <a href="README.md">English</a> |
  <a href="README.es.md">Español</a> |
  <a href="README.zh.md">简体中文</a> |
  <a href="README.ru.md">Русский</a> |
  <a href="README.id.md">Bahasa Indonesia</a> |
  <a href="README.pt.md">Português</a>
</p>

## Table of contents

- [What is Sober?](#what-is-sober)
- [What are FastFlags?](#what-are-fastflags)
- [Confirmed active FFlags](#confirmed-active-fflags)
  - [Rendering and performance](#rendering-and-performance)
  - [Stability and VRAM](#stability-and-vram)
  - [UI and environment](#ui-and-environment)
- [Configuration presets](#configuration-presets)
  - [Preset 1: Low-end / VRAM crash fix](#preset-1-low-end--vram-crash-fix)
  - [Preset 2: Balanced / mid-range](#preset-2-balanced--mid-range)
  - [Preset 3: Maximum graphics fidelity](#preset-3-maximum-graphics-fidelity)
- [Detailed technical topics](#detailed-technical-topics)
- [Uncapping framerates](#uncapping-framerates)
- [Security and anti-cheat risks](#security-and-anti-cheat-risks)
- [Deprecated FFlags](#deprecated-fflags)
- [Disclaimer and sources](#disclaimer-and-sources)

## What is Sober?

[Sober](https://sober.vinegarhq.org/) is a compatibility layer that runs the Roblox Android application (APK) natively on Linux desktops. It is distributed as a Flatpak (`org.vinegarhq.Sober`) and uses Vulkan as its primary rendering backend, with OpenGL as a fallback. Configuration is managed through `~/.var/app/org.vinegarhq.Sober/config/sober/config.json`. You can also open the settings menu graphically using the command `flatpak run org.vinegarhq.Sober config` or by right-clicking Sober in your application menu and selecting **Settings**.

## What are FastFlags?

FastFlags (FFlags) are internal Roblox engine variables that control rendering, UI, stability, and other settings. Since September 29, 2025, Roblox enforces a strict allowlist: only a small subset of flags can be overridden locally via configuration files. Any flag not on the allowlist is ignored by the client.

> [!IMPORTANT]
> This guide only covers flags confirmed to be on the current allowlist. Flags from older community guides may no longer work.

## Confirmed active FFlags

### Rendering and performance

These flags control geometry detail, anti-aliasing, lighting, and grass rendering distance.

| Flag Name | Type | Value Range | What It Does |
| :--- | :--- | :--- | :--- |
| `DFIntCSGLevelOfDetailSwitchingDistance` | int | `0` - `1000` | Master LOD culling distance for CSG models. Lower = better FPS. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL12` | int | `0` - `1000` | LOD distance for Graphics Quality 1 to 2. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL23` | int | `0` - `1000` | LOD distance for Graphics Quality 2 to 3. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL34` | int | `0` - `1000` | LOD distance for Graphics Quality 3 to 4. |
| `FIntDebugForceMSAASamples` | int | `1`, `2`, `4` | Forces MSAA anti-aliasing (smoother edges, costs GPU). |
| `DFIntDebugFRMQualityLevelOverride` | int | `0` - `21` | Overrides the graphics level slider (goes beyond default 1 to 10). |
| `FIntFRMMaxGrassDistance` | int | `0` - `1000` | Max render distance for terrain grass. Set to `0` to disable grass. |
| `FIntFRMMinGrassDistance` | int | `0` - `1000` | Min distance where grass starts rendering. |
| `DFFlagDebugPauseVoxelizer` | bool | `true` / `false` | Disables voxel lighting. |
| `FFlagDebugSkyGray` | bool | `true` / `false` | Overrides skybox color to gray and removes atmospheric stars. |
| `FFlagDebugGraphicsPreferVulkan` | bool | `true` / `false` | Prefers Vulkan for rendering. |
| `FFlagDebugGraphicsPreferOpenGL` | bool | `true` / `false` | Prefers OpenGL for rendering. |

### Stability and VRAM

These flags help prevent out-of-memory crashes, especially on GPUs with limited VRAM.

| Flag Name | Type | Value Range | What It Does |
| :--- | :--- | :--- | :--- |
| `DFFlagTextureQualityOverrideEnabled` | bool | `true` / `false` | Enables manual control over texture resolution. |
| `DFIntTextureQualityOverride` | int | `0` - `3` | Sets texture quality (0 = lowest, 3 = max). |

> [!WARNING]
> Setting `DFIntTextureQualityOverride` to `3` on GPUs with 4 GB VRAM or less often causes an instant `RBXCRASH: OutOfMemory` crash. Use `2` or `1` for stability.

### UI and environment

Minor flags that affect visual comfort and interface behavior.

| Flag Name | Type | Value Range | What It Does |
| :--- | :--- | :--- | :--- |
| `FIntGrassMovementReducedMotionFactor` | bool | `true` / `false` | Reduces motion for grass animations. *(Note: Uses `FInt` name prefix but expects boolean `true`/`false`).* |

## Configuration presets

Copy one of the presets below and paste it into your Sober configuration file:

```
~/.var/app/org.vinegarhq.Sober/config/sober/config.json
```

### Preset 1: Low-end / VRAM crash fix

For GPUs with less than 4 GB VRAM, integrated graphics, or systems experiencing `OutOfMemory` crashes.

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
    "FIntGrassMovementReducedMotionFactor": true
  }
}
```

### Preset 2: Balanced / mid-range

For mid-tier GPUs (GTX 1650, RX 580 class) with 4 to 6 GB VRAM. Good balance between visuals and performance.

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
    "FIntGrassMovementReducedMotionFactor": true
  }
}
```

### Preset 3: Maximum graphics fidelity

For high-end systems with 8 GB+ VRAM. Forces maximum detail, anti-aliasing, and texture quality.

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
> You can mix and match flags between presets. For example, use the Balanced LOD distances with Maximum texture quality if your GPU has enough VRAM but struggles with geometry.

## Detailed technical topics

<details>
<summary><strong>Level of detail (LOD) and geometry scaling</strong></summary>

Roblox maps often use complex Constructive Solid Geometry (CSG) unions. Rendering these at far distances puts heavy load on both CPU and GPU.

By lowering `DFIntCSGLevelOfDetailSwitchingDistance` (e.g., to `150`), you force the game to swap complex models for low-polygon versions closer to the camera. This increases framerates without changing physical collision hitboxes: objects still behave the same, but look simpler from far away.

The tiered variants (`L12`, `L23`, `L34`) let you fine-tune this per graphics quality level, so lower quality settings cull more aggressively.

</details>

<details>
<summary><strong>VRAM allocation and out-of-memory (OOM) crashes</strong></summary>

Sober runs the Roblox Android binary inside a Linux Flatpak environment. The Android binary assumes a shared mobile memory model, which differs from how desktop Linux GPU drivers handle VRAM.

When the engine requests maximum quality textures, it can quickly exhaust dedicated GPU VRAM. On Windows desktop, drivers spill excess data into system RAM. On Linux (especially with proprietary NVIDIA drivers), this fallback does not work reliably, causing an instant `RBXCRASH: OutOfMemory` crash.

To fix this, set `DFFlagTextureQualityOverrideEnabled` to `true` and `DFIntTextureQualityOverride` to `2` (medium) or `1` (low). This forces the engine to request smaller textures from the server, keeping VRAM usage within safe limits.

</details>

<details>
<summary><strong>Graphics APIs: Vulkan vs. OpenGL</strong></summary>

Graphics API selection can be configured via `config.json` or FFlags (`FFlagDebugGraphicsPreferVulkan` / `FFlagDebugGraphicsPreferOpenGL`).

- By default, Sober uses Vulkan for optimal performance.
- If you experience graphic artifacts, black screens, or startup crashes (common on older GPUs or hybrid laptop setups), Vinegar documentation recommends running `flatpak run org.vinegarhq.Sober config` in your terminal and selecting **Force Legacy Rendering** (or setting `"use_opengl": true` in `config.json`).

</details>

<details>
<summary><strong>Asset overlay (custom textures and cursors)</strong></summary>

Sober allows replacing game assets via the `asset_overlay` directory located at:
`~/.var/app/org.vinegarhq.Sober/data/sober/asset_overlay`

Files placed here take priority over standard Roblox assets upon restarting the app. The directory structure mirrors `packages/*/com.roblox.client/base.apk/assets`.

Example for custom mouse cursors:
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
To revert changes, clear the files from `asset_overlay`.

</details>

<details>
<summary><strong>Fullscreen (F11) and exit controls</strong></summary>

The in-game fullscreen toggle in Roblox does not function on mobile Android builds. On Sober, press `F11` to enter or exit fullscreen mode. Sober remembers the fullscreen state across launches.

To close the app automatically upon leaving an experience, add `"close_on_leave": true` to your `config.json`.

</details>

## Uncapping framerates

The `DFIntTaskSchedulerTargetFps` FastFlag no longer works because Roblox removed it from the allowlist. To change your framerate cap, edit the XML settings file directly.

### Steps:

1. Launch any Roblox experience on Sober to generate the config files.
2. Close the client completely.
3. Navigate to `~/.var/app/org.vinegarhq.Sober/data/sober/appData/`.
4. Open `GlobalBasicSettings_13.xml` in a text editor.
5. Find the line: `<int name="FramerateCap">60</int>`
6. Change `60` to your target framerate (e.g., `144`, `240`) or `0` to uncap.
7. Save, close, and relaunch Roblox.

> [!NOTE]
> Close Roblox before editing this file. The client overwrites this file on exit, so any changes made while the game is running will be lost.

## Security and anti-cheat risks

Roblox uses the Hyperion (Byfron) anti-cheat system. Configuring allowlisted FFlags in `config.json` is safe. Attempting to bypass the allowlist is not.

> [!CAUTION]
> The following actions carry a high risk of permanent account or hardware bans. Do not attempt them.

- **Cache file manipulation (`IxpSettings.json`):** Injecting unauthorized flags into cache files and setting them to read-only is detected as tampering by Hyperion.
- **Memory editing:** Using tools to force-load disallowed flags (such as physics timestep manipulation via `DFIntTimestepArbiterThresholdCFLThou` or texture bypasses for wallhacks) triggers automated bans.
- **Allowlist bypass programs:** Programs or launch arguments designed to circumvent the FFlag filter are classified as exploits.

## Deprecated FFlags

These flags were popular in older guides, but Roblox removed them from the allowlist. Adding them to `config.json` has no effect because the client ignores them.

| Flag | Why It's Deprecated |
| :--- | :--- |
| `DFIntTaskSchedulerTargetFps` | Replaced by editing `GlobalBasicSettings_13.xml`. |
| `FFlagTaskSchedulerLimitTargetFpsTo2402` | Removed from the allowlist. |
| `DFIntConnectionMTUSize` | Network tuning flags are blocked. |
| `FFlagDebugDisableTelemetryEphemeralCounter` | Telemetry suppression is blocked. |
| `FFlagAdServiceEnabled` | Ad service toggling is blocked. |
| `FFlagMovePrerender` | Thread manipulation flags are blocked. |
| `DFIntDebugDynamicRenderKiloPixels` | Render resolution scaling was vetoed by Roblox engineering. |

## Disclaimer and sources

> [!NOTE]
> Roblox Corporation maintains the FFlag allowlist, which may change with future client updates. This guide is accurate as of **July 2026**. Always verify against official sources before deploying configurations.

**Official sources:**
- [Allowlist for local client configuration via Fast Flags on the Roblox DevForum](https://devforum.roblox.com/t/allowlist-for-local-client-configuration-via-fast-flags/3966569)
- [Sober Configuration Tips & Tricks from Vinegar Documentation](https://vinegarhq.org/Sober/Configuration/TipsAndTricks.html)
