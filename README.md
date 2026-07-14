<h1 align="center">Roblox FastFlags Guide for Sober</h1>
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

---

## Table of Contents

- [What is Sober?](#what-is-sober)
- [What are FastFlags?](#what-are-fastflags)
- [Confirmed Active FFlags](#confirmed-active-fflags)
  - [Rendering & Performance](#rendering--performance)
  - [Stability & VRAM](#stability--vram)
  - [UI & Environment](#ui--environment)
- [Configuration Presets](#configuration-presets)
  - [Preset 1: Low-End / VRAM Crash Fix](#preset-1-low-end--vram-crash-fix)
  - [Preset 2: Balanced / Mid-Range](#preset-2-balanced--mid-range)
  - [Preset 3: Maximum Graphics Fidelity](#preset-3-maximum-graphics-fidelity)
- [In-Depth Topics](#in-depth-topics)
- [Framerate Uncapping](#framerate-uncapping)
- [Security & Anti-Cheat Risks](#security--anti-cheat-risks)
- [Deprecated FFlags](#deprecated-fflags)
- [Disclaimer & Sources](#disclaimer--sources)

---

## What is Sober?

[Sober](https://sober.vinegarhq.org/) is a compatibility layer that runs the Roblox Android application (APK) natively on Linux desktops. It is distributed as a Flatpak (`org.vinegarhq.Sober`) and uses Vulkan as its primary rendering backend, with OpenGL as a fallback. Configuration is managed through `~/.var/app/org.vinegarhq.Sober/config/sober/config.json`.

## What are FastFlags?

FastFlags (FFlags) are internal Roblox engine variables that control rendering, UI, stability, and more. Since **September 29, 2025**, Roblox enforces a strict allowlist — only a small subset of flags can be overridden locally via configuration files. Any flag not on the allowlist is silently ignored by the client.

> [!IMPORTANT]
> This guide only covers flags that are **confirmed to be on the current allowlist**. Flags from older community guides may no longer work.

---

## Confirmed Active FFlags

### Rendering & Performance

These flags control geometry detail, anti-aliasing, lighting, and grass rendering distance.

| Flag Name | Type | Value Range | What It Does |
| :--- | :--- | :--- | :--- |
| `DFIntCSGLevelOfDetailSwitchingDistance` | int | `0` - `1000` | Master LOD culling distance for CSG models. Lower = better FPS. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL12` | int | `0` - `1000` | LOD distance for Graphics Quality 1–2. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL23` | int | `0` - `1000` | LOD distance for Graphics Quality 2–3. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL34` | int | `0` - `1000` | LOD distance for Graphics Quality 3–4. |
| `FIntDebugForceMSAASamples` | int | `1`, `2`, `4` | Forces MSAA anti-aliasing (smoother edges, costs GPU). |
| `DFFlagDebugPauseVoxelizer` | bool | `true` / `false` | Pauses voxel lighting, shadows, and ambient occlusion. Big FPS boost. |
| `FFlagDebugSkyGray` | bool | `true` / `false` | Replaces skybox with flat gray. Removes sky shader overhead. |
| `DFIntDebugFRMQualityLevelOverride` | int | `0` - `21` | Overrides the graphics level slider (goes beyond the default 1–10). |
| `FIntFRMMaxGrassDistance` | int | `0` - `1000` | Max render distance for terrain grass. Set to `0` to disable grass. |
| `FIntFRMMinGrassDistance` | int | `0` - `1000` | Min distance where grass starts rendering. |

### Stability & VRAM

These flags help prevent out-of-memory crashes, especially on GPUs with limited VRAM.

| Flag Name | Type | Value Range | What It Does |
| :--- | :--- | :--- | :--- |
| `DFFlagTextureQualityOverrideEnabled` | bool | `true` / `false` | Enables manual control over texture resolution. |
| `DFIntTextureQualityOverride` | int | `0` - `3` | Sets texture quality (0 = lowest, 3 = max). |

> [!WARNING]
> Setting `DFIntTextureQualityOverride` to `3` on GPUs with 4GB VRAM or less will very likely cause an instant `RBXCRASH: OutOfMemory` crash. Use `2` or `1` for stability.

### UI & Environment

Minor flags that affect visual comfort and interface behavior.

| Flag Name | Type | Value Range | What It Does |
| :--- | :--- | :--- | :--- |
| `FIntGrassMovementReducedMotionFactor` | int | `0` - `1000` | Controls grass waving animation intensity. `0` = completely frozen. |

---

## Configuration Presets

Copy one of the presets below and paste it into your Sober configuration file:

```
~/.var/app/org.vinegarhq.Sober/config/sober/config.json
```

### Preset 1: Low-End / VRAM Crash Fix

For GPUs with less than 4GB VRAM, integrated graphics, or systems experiencing `OutOfMemory` crashes.

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
    "DFFlagDebugPauseVoxelizer": true,
    "FIntFRMMaxGrassDistance": 0,
    "FIntGrassMovementReducedMotionFactor": 0,
    "FFlagDebugSkyGray": true
  }
}
```

### Preset 2: Balanced / Mid-Range

For mid-tier GPUs (GTX 1650, RX 580 class) with 4–6GB VRAM. Good balance between visuals and performance.

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

### Preset 3: Maximum Graphics Fidelity

For high-end systems with 8GB+ VRAM. Forces maximum detail, anti-aliasing, and texture quality.

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

---

## In-Depth Topics

<details>
<summary><strong>Level of Detail (LOD) & Geometry Scaling</strong></summary>

Roblox maps often use complex Constructive Solid Geometry (CSG) unions. Rendering these at far distances puts heavy load on both CPU and GPU.

By lowering `DFIntCSGLevelOfDetailSwitchingDistance` (e.g., to `150`), you force the game to swap complex models for low-polygon versions closer to the camera. This significantly boosts framerate without changing the physical collision hitboxes — objects still behave the same, they just look simpler from far away.

The tiered variants (`L12`, `L23`, `L34`) let you fine-tune this per graphics quality level, so lower quality settings cull more aggressively.

</details>

<details>
<summary><strong>VRAM Allocation & Out-of-Memory (OOM) Crashes</strong></summary>

Sober runs the Roblox Android binary inside a Linux Flatpak environment. The Android binary assumes a shared mobile memory model, which does not match how desktop Linux GPU drivers handle VRAM.

**The problem:** When the engine requests maximum quality textures, it can quickly exhaust the GPU's dedicated VRAM. On desktop Windows, the driver would spill excess data into system RAM. On Linux (especially with proprietary NVIDIA drivers), this fallback does not work reliably — causing an instant `RBXCRASH: OutOfMemory` crash.

**The fix:** Set `DFFlagTextureQualityOverrideEnabled` to `true` and `DFIntTextureQualityOverride` to `2` (medium) or `1` (low). This forces the engine to request smaller textures from the server, keeping VRAM usage within safe limits.

</details>

<details>
<summary><strong>Graphics APIs: Vulkan vs. OpenGL</strong></summary>

Graphics API selection is managed by the Sober wrapper config, **not** by internal FFlags.

- By default, Sober uses **Vulkan** for optimal performance.
- If you experience graphic artifacts, black screens, or startup crashes (common on older GPUs or hybrid laptop setups), force OpenGL by setting `"use_opengl": true` at the root level of your `config.json`.

Do not use engine FFlags like `FFlagDebugGraphicsPreferVulkan` or `FFlagDebugGraphicsPreferOpenGL` for this — they can cause context mismatch crashes when they conflict with Sober's wrapper-level API selection.

</details>

---

## Framerate Uncapping

The `DFIntTaskSchedulerTargetFps` FastFlag no longer works — it was removed from the allowlist. To change your framerate cap, you must edit the XML settings file directly.

### Steps:

1. Launch any Roblox experience on Sober to generate the config files.
2. Close the client completely.
3. Navigate to `~/.var/app/org.vinegarhq.Sober/data/sober/appData/`.
4. Open `GlobalBasicSettings_13.xml` in a text editor.
5. Find the line: `<int name="FramerateCap">60</int>`
6. Change `60` to your target framerate (e.g., `144`, `240`) or `0` to uncap.
7. Save, close, and relaunch Roblox.

> [!NOTE]
> You must close Roblox before editing this file. The client overwrites this file on exit, so any changes made while the game is running will be lost.

---

## Security & Anti-Cheat Risks

Roblox uses the Hyperion (Byfron) anti-cheat system. Configuring allowlisted FFlags in `config.json` is completely safe. Attempting to bypass the allowlist is not.

> [!CAUTION]
> The following actions carry a high risk of **permanent account or hardware bans**. Do not attempt them.

- **Cache file manipulation (`IxpSettings.json`):** Injecting unauthorized flags into cache files and locking them to read-only is detected as tampering by Hyperion.
- **Memory editing:** Using tools to force-load disallowed flags (e.g., physics timestep manipulation via `DFIntTimestepArbiterThresholdCFLThou`, or texture bypasses for wallhacks) triggers automated bans.
- **Allowlist bypass programs:** Any program or launch argument designed to circumvent the FFlag filter is classified as an exploit.

---

## Deprecated FFlags

The following flags are commonly found in older guides but are **no longer on the allowlist**. Adding them to `config.json` has zero effect — they are silently ignored.

| Flag | Why It's Deprecated |
| :--- | :--- |
| `DFIntTaskSchedulerTargetFps` | Replaced by editing `GlobalBasicSettings_13.xml`. |
| `FFlagTaskSchedulerLimitTargetFpsTo2402` | Removed from the allowlist. |
| `DFIntConnectionMTUSize` | Network tuning flags are blocked. |
| `FFlagDebugDisableTelemetryEphemeralCounter` | Telemetry suppression is blocked. |
| `FFlagAdServiceEnabled` | Ad service toggling is blocked. |
| `FFlagMovePrerender` | Thread manipulation flags are blocked. |
| `DFIntDebugDynamicRenderKiloPixels` | Render resolution scaling was vetoed by Roblox engineering. |

---

## Disclaimer & Sources

> [!NOTE]
> The FFlag allowlist is maintained by Roblox Corporation and may change at any time with future client updates. This guide is accurate as of **July 2026**. Always verify against the official source before deploying configurations.

**Official Source:** [Allowlist for local client configuration via Fast Flags — Roblox DevForum](https://devforum.roblox.com/t/allowlist-for-local-client-configuration-via-fast-flags/3966569)
