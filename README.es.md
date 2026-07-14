<h1 align="center">Guía de FastFlags de Roblox para Sober</h1>
<p align="center">FastFlags verificadas, seguras y permitidas (allowlisted) para el cliente Sober en Linux.</p>

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
> Esta guía ha sido traducida utilizando IA. Las revisiones o contribuciones humanas son bienvenidas para mejorar la calidad. ¡No dudes en abrir un Pull Request si encuentras algún error!

---

## Índice

- [¿Qué es Sober?](#¿qué-es-sober)
- [¿Qué son las FastFlags?](#¿qué-son-las-fastflags)
- [FFlags activas confirmadas](#fflags-activas-confirmadas)
  - [Renderizado y Rendimiento](#renderizado-y-rendimiento)
  - [Estabilidad y VRAM](#estabilidad-y-vram)
  - [Interfaz de usuario y Entorno](#interfaz-de-usuario-y-entorno)
- [Preajustes de configuración](#preajustes-de-configuración)
  - [Preset 1: Gama baja / Solución para crash por VRAM](#preset-1-gama-baja--solución-para-crash-por-vram)
  - [Preset 2: Equilibrado / Gama media](#preset-2-equilibrado--gama-media)
  - [Preset 3: Máxima fidelidad gráfica](#preset-3-máxima-fidelidad-gráfica)
- [Temas detallados](#temas-detallados)
- [Desbloqueo de FPS (Tasa de fotogramas)](#desbloqueo-de-fps-tasa-de-fotogramas)
- [Riesgos de seguridad y Anti-Cheat](#riesgos-de-seguridad-y-anti-cheat)
- [FFlags obsoletas](#fflags-obsoletas)
- [Descargo de responsabilidad y Fuentes](#descargo-de-responsabilidad-y-fuentes)

---

## ¿Qué es Sober?

[Sober](https://sober.vinegarhq.org/) es una capa de compatibilidad que ejecuta la aplicación Android de Roblox (APK) de forma nativa en escritorios Linux. Se distribuye como Flatpak (`org.vinegarhq.Sober`) y utiliza Vulkan como backend de renderizado principal, con OpenGL como alternativa (fallback). La configuración se administra a través de `~/.var/app/org.vinegarhq.Sober/config/sober/config.json`.

## ¿Qué son las FastFlags?

Las FastFlags (FFlags) son variables internas del motor de Roblox que controlan el renderizado, la interfaz de usuario, la estabilidad y más. Desde el **29 de septiembre de 2025**, Roblox impone una lista de permitidos (allowlist) estricta: solo un pequeño subconjunto de flags se puede anular localmente mediante archivos de configuración. Cualquier flag que no esté en la lista es ignorada silenciosamente por el cliente.

> [!IMPORTANT]
> Esta guía solo cubre las flags que se ha confirmado que están en la lista de permitidos actual. Las flags de guías comunitarias más antiguas podrían ya no funcionar.

---

## FFlags activas confirmadas

### Renderizado y Rendimiento

Estas flags controlan el detalle de la geometría, el suavizado de líneas (anti-aliasing), la iluminación y la distancia de renderizado de la hierba.

| Nombre de la Flag | Tipo | Rango de valores | Qué hace |
| :--- | :--- | :--- | :--- |
| `DFIntCSGLevelOfDetailSwitchingDistance` | int | `0` - `1000` | Distancia principal de culling LOD para modelos CSG. Menor valor = mejores FPS. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL12` | int | `0` - `1000` | Distancia LOD para calidad gráfica 1–2. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL23` | int | `0` - `1000` | Distancia LOD para calidad gráfica 2–3. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL34` | int | `0` - `1000` | Distancia LOD para calidad gráfica 3–4. |
| `FIntDebugForceMSAASamples` | int | `1`, `2`, `4` | Fuerza el suavizado MSAA (bordes más suaves, costo en GPU). |
| `DFFlagDebugPauseVoxelizer` | bool | `true` / `false` | Pausa la iluminación por vóxeles, las sombras y la oclusión ambiental. Gran aumento de FPS. |
| `FFlagDebugSkyGray` | bool | `true` / `false` | Reemplaza el cielo por un gris plano. Elimina la sobrecarga del sombreador (shader) del cielo. |
| `DFIntDebugFRMQualityLevelOverride` | int | `0` - `21` | Anula el control deslizante del nivel de gráficos (va más allá del valor predeterminado 1–10). |
| `FIntFRMMaxGrassDistance` | int | `0` - `1000` | Distancia de renderizado máxima para la hierba del terreno. Establézcalo en `0` para desactivar la hierba. |
| `FIntFRMMinGrassDistance` | int | `0` - `1000` | Distancia mínima a la que comienza a renderizarse la hierba. |

### Estabilidad y VRAM

Estas flags ayudan a prevenir caídas por falta de memoria (out-of-memory), especialmente en GPUs con VRAM limitada.

| Nombre de la Flag | Tipo | Rango de valores | Qué hace |
| :--- | :--- | :--- | :--- |
| `DFFlagTextureQualityOverrideEnabled` | bool | `true` / `false` | Permite el control manual sobre la resolución de texturas. |
| `DFIntTextureQualityOverride` | int | `0` - `3` | Establece la calidad de las texturas (0 = la más baja, 3 = la máxima). |

> [!WARNING]
> Establecer `DFIntTextureQualityOverride` en `3` en GPUs con 4 GB de VRAM o menos probablemente causará un crash instantáneo `RBXCRASH: OutOfMemory`. Use `2` o `1` para mayor estabilidad.

### Interfaz de usuario y Entorno

Flags menores que afectan a la comodidad visual y al comportamiento de la interfaz.

| Nombre de la Flag | Tipo | Rango de valores | Qué hace |
| :--- | :--- | :--- | :--- |
| `FIntGrassMovementReducedMotionFactor` | int | `0` - `1000` | Controla la intensidad de la animación del movimiento de la hierba. `0` = completamente estática. |

---

## Preajustes de configuración

Copie uno de los siguientes preajustes y péguelo en su archivo de configuración de Sober:

```
~/.var/app/org.vinegarhq.Sober/config/sober/config.json
```

### Preset 1: Gama baja / Solución para crash por VRAM

Para GPUs con menos de 4 GB de VRAM, gráficos integrados o sistemas que experimentan crashes por falta de memoria (`OutOfMemory`).

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

### Preset 2: Equilibrado / Gama media

Para GPUs de gama media (clase GTX 1650, RX 580) con 4–6 GB de VRAM. Buen equilibrio entre calidad visual y rendimiento.

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

### Preset 3: Máxima fidelidad gráfica

Para sistemas de gama alta con más de 8 GB de VRAM. Fuerza el máximo detalle, anti-aliasing y calidad de texturas.

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
> Puede mezclar y combinar flags entre preajustes. Por ejemplo, use las distancias LOD del preajuste Equilibrado con la calidad máxima de texturas si su GPU tiene suficiente VRAM pero tiene problemas con la geometría.

---

## Temas detallados

<details>
<summary><strong>Nivel de detalle (LOD) y Escala de geometría</strong></summary>

Los mapas de Roblox a menudo usan uniones complejas de Geometría Sólida Constructiva (CSG). Renderizar estas a grandes distancias supone una carga pesada tanto para la CPU como para la GPU.

Al reducir `DFIntCSGLevelOfDetailSwitchingDistance` (por ejemplo, a `150`), obliga al juego a cambiar modelos complejos por versiones de pocos polígonos (low-polygon) más cerca de la cámara. Esto aumenta significativamente la tasa de fotogramas (FPS) sin cambiar las cajas de colisión físicas (hitboxes): los objetos se comportan igual, pero se ven más simples desde lejos.

Las variantes escalonadas (`L12`, `L23`, `L34`) le permiten ajustar esto por nivel de calidad gráfica, de modo que los ajustes de calidad más bajos descartan (cull) geometría de forma más agresiva.

</details>

<details>
<summary><strong>Asignación de VRAM y Caídas por falta de memoria (OOM)</strong></summary>

Sober ejecuta el binario de Android de Roblox dentro de un entorno Linux Flatpak. El binario de Android asume un modelo de memoria móvil compartida (shared mobile memory), que no coincide con la forma en que los controladores de GPU de Linux de escritorio gestionan la VRAM.

**El problema:** Cuando el motor solicita texturas de calidad máxima, puede agotar rápidamente la VRAM dedicada de la GPU. En Windows de escritorio, el controlador derivaría el exceso de datos a la RAM del sistema. En Linux (especialmente con controladores propietarios de NVIDIA), esta alternativa no funciona de manera confiable, lo que provoca una caída instantánea `RBXCRASH: OutOfMemory`.

**La solución:** Establezca `DFFlagTextureQualityOverrideEnabled` en `true` y `DFIntTextureQualityOverride` en `2` (medio) o `1` (bajo). Esto obliga al motor a solicitar texturas más pequeñas del servidor, manteniendo el uso de VRAM dentro de límites seguros.

</details>

<details>
<summary><strong>APIs Gráficas: Vulkan frente a OpenGL</strong></summary>

La selección de la API gráfica se gestiona mediante la configuración del contenedor (wrapper) de Sober, **no** mediante FFlags internas.

- Por defecto, Sober utiliza **Vulkan** para un rendimiento óptimo.
- Si ya experimenta artefactos gráficos, pantallas negras o fallos de inicio (comunes en GPUs antiguas o configuraciones de portátiles híbridos), fuerce OpenGL estableciendo `"use_opengl": true` en el nivel raíz de su `config.json`.

No use FFlags del motor como `FFlagDebugGraphicsPreferVulkan` o `FFlagDebugGraphicsPreferOpenGL` para esto; pueden causar caídas por discordancia de contexto (context mismatch) al entrar en conflicto con la selección de API a nivel de contenedor de Sober.

</details>

---

## Desbloqueo de FPS (Tasa de fotogramas)

La FastFlag `DFIntTaskSchedulerTargetFps` ya no funciona; fue eliminada de la lista de permitidos. Para cambiar el límite de fotogramas, debe editar el archivo de configuración XML directamente.

### Pasos:

1. Inicie cualquier experiencia de Roblox en Sober para generar los archivos de configuración.
2. Cierre el cliente por completo.
3. Navegue a `~/.var/app/org.vinegarhq.Sober/data/sober/appData/`.
4. Abra `GlobalBasicSettings_13.xml` en un editor de texto.
5. Busque la línea: `<int name="FramerateCap">60</int>`
6. Cambie `60` por la tasa de fotogramas deseada (por ejemplo, `144`, `240`) o `0` para desbloquearla por completo (uncap).
7. Guarde, cierre y vuelva a iniciar Roblox.

> [!NOTE]
> Debe cerrar Roblox antes de editar este archivo. El cliente sobrescribe este archivo al salir, por lo que cualquier cambio realizado mientras el juego se ejecuta se perderá.

---

## Riesgos de seguridad y Anti-Cheat

Roblox utiliza el sistema anti-cheat Hyperion (Byfron). Configurar las FFlags permitidas en `config.json` es completamente seguro. Intentar eludir la lista de permitidos no lo es.

> [!CAUTION]
> Las siguientes acciones conllevan un alto riesgo de expulsión (ban) permanente de la cuenta o del hardware. No las intente.

- **Manipulación de archivos caché (`IxpSettings.json`):** Inyectar flags no autorizadas en los archivos caché y bloquearlos como de solo lectura se detecta como manipulación por parte de Hyperion.
- **Edición de memoria:** El uso de herramientas para forzar la carga de flags no permitidas (por ejemplo, manipulación del paso de tiempo de físicas a través de `DFIntTimestepArbiterThresholdCFLThou`, o eludir texturas para wallhacks) activa expulsiones automáticas.
- **Programas de elusión de la lista de permitidos:** Cualquier programa o argumento de lanzamiento diseñado para esquivar el filtro de FFlags se clasifica como exploit.

---

## FFlags obsoletas

Las siguientes flags se encuentran habitualmente en guías antiguas pero ya no están en la lista de permitidos. Añadirlas a `config.json` tiene un efecto nulo: son ignoradas en silencio.

| Flag | Por qué está obsoleta |
| :--- | :--- |
| `DFIntTaskSchedulerTargetFps` | Reemplazada editando `GlobalBasicSettings_13.xml`. |
| `FFlagTaskSchedulerLimitTargetFpsTo2402` | Eliminada de la lista de permitidos. |
| `DFIntConnectionMTUSize` | Las flags de ajuste de red están bloqueadas. |
| `FFlagDebugDisableTelemetryEphemeralCounter` | La supresión de telemetría está bloqueada. |
| `FFlagAdServiceEnabled` | La desactivación del servicio de publicidad está bloqueada. |
| `FFlagMovePrerender` | Las flags de manipulación de hilos están bloqueadas. |
| `DFIntDebugDynamicRenderKiloPixels` | La escala de resolución de renderizado fue vetada por el equipo de ingeniería de Roblox. |

---

## Descargo de responsabilidad y Fuentes

> [!NOTE]
> Roblox Corporation mantiene la lista de permitidos de FFlags y esta puede cambiar en cualquier momento con futuras actualizaciones del cliente. Esta guía es precisa a fecha de **julio de 2026**. Verifique siempre con la fuente oficial antes de implementar configuraciones.

**Fuente oficial:** [Allowlist for local client configuration via Fast Flags — Roblox DevForum](https://devforum.roblox.com/t/allowlist-for-local-client-configuration-via-fast-flags/3966569)
