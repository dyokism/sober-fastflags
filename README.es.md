<h1 align="center">Guía de FastFlags de Roblox para Sober</h1>
<p align="center">FastFlags verificadas, seguras y en la lista de permitidos (allowlist) para el cliente Sober en Linux.</p>

<p align="center">
  <b>Traducciones / Translations:</b>
  <a href="README.md">English</a> |
  <a href="README.es.md">Español</a> |
  <a href="README.zh.md">简体中文</a> |
  <a href="README.ru.md">Русский</a> |
  <a href="README.id.md">Bahasa Indonesia</a> |
  <a href="README.pt.md">Português</a>
</p>

## Índice

- [¿Qué es Sober?](#¿qué-es-sober)
- [¿Qué son las FastFlags?](#¿qué-son-las-fastflags)
- [FFlags activas confirmadas](#fflags-activas-confirmadas)
  - [Renderizado y rendimiento](#renderizado-y-rendimiento)
  - [Estabilidad y VRAM](#estabilidad-y-vram)
  - [Interfaz de usuario y entorno](#interfaz-de-usuario-y-entorno)
- [Preajustes de configuración](#preajustes-de-configuración)
  - [Preset 1: Gama baja / Solución para crash por VRAM](#preset-1-gama-baja--solución-para-crash-por-vram)
  - [Preset 2: Equilibrado / Gama media](#preset-2-equilibrado--gama-media)
  - [Preset 3: Máxima fidelidad gráfica](#preset-3-máxima-fidelidad-gráfica)
- [Temas detallados](#temas-detallados)
- [Desbloqueo de FPS](#desbloqueo-de-fps)
- [Riesgos de seguridad y anti-cheat](#riesgos-de-seguridad-y-anti-cheat)
- [FFlags obsoletas](#fflags-obsoletas)
- [Descargo de responsabilidad y fuentes](#descargo-de-responsabilidad-y-fuentes)

## ¿Qué es Sober?

[Sober](https://sober.vinegarhq.org/) es una capa de compatibilidad que ejecuta la aplicación de Roblox para Android (APK) de forma nativa en escritorios Linux. Se distribuye como un Flatpak (`org.vinegarhq.Sober`) y utiliza Vulkan como motor de renderizado principal, con OpenGL como alternativa. La configuración se gestiona en `~/.var/app/org.vinegarhq.Sober/config/sober/config.json`. También puedes abrir el menú de configuración de forma gráfica usando el comando `flatpak run org.vinegarhq.Sober config` o haciendo clic derecho en Sober en el menú de aplicaciones y seleccionando **Settings**.

## ¿Qué son las FastFlags?

Las FastFlags (FFlags) son variables internas del motor de Roblox que controlan el renderizado, la interfaz de usuario, la estabilidad y más. Desde el 29 de septiembre de 2025, Roblox impone una lista de permitidos (allowlist) estricta: solo un pequeño subconjunto de flags se puede anular localmente mediante archivos de configuración. Cualquier flag fuera de la lista es ignorada por el cliente.

> [!IMPORTANT]
> Esta guía solo cubre las flags confirmadas en la lista de permitidos actual. Las flags de guías comunitarias más antiguas pueden no funcionar.

## FFlags activas confirmadas

### Renderizado y rendimiento

Estas flags controlan el detalle de la geometría, el suavizado de líneas (anti-aliasing), la iluminación y la distancia de renderizado del césped.

| Nombre de la Flag | Tipo | Rango de valores | Qué hace |
| :--- | :--- | :--- | :--- |
| `DFIntCSGLevelOfDetailSwitchingDistance` | int | `0` - `1000` | Distancia principal de culling LOD para modelos CSG. Menor = mejor FPS. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL12` | int | `0` - `1000` | Distancia LOD para calidad gráfica 1 a 2. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL23` | int | `0` - `1000` | Distancia LOD para calidad gráfica 2 a 3. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL34` | int | `0` - `1000` | Distancia LOD para calidad gráfica 3 a 4. |
| `FIntDebugForceMSAASamples` | int | `1`, `2`, `4` | Fuerza el suavizado de bordes MSAA (bordes más suaves, mayor costo de GPU). |
| `DFIntDebugFRMQualityLevelOverride` | int | `0` - `21` | Invalida el deslizador de calidad gráfica (supera el valor por defecto de 1 a 10). |
| `FIntFRMMaxGrassDistance` | int | `0` - `1000` | Distancia máxima de renderizado del césped. Usa `0` para desactivarlo. |
| `FIntFRMMinGrassDistance` | int | `0` - `1000` | Distancia mínima donde comienza a renderizarse el césped. |
| `DFFlagDebugPauseVoxelizer` | bool | `true` / `false` | Desactiva la iluminación por vóxeles. |
| `FFlagDebugSkyGray` | bool | `true` / `false` | Cambia el color del skybox a gris y elimina las estrellas atmosféricas. |
| `FFlagDebugGraphicsPreferVulkan` | bool | `true` / `false` | Prefiere Vulkan para el renderizado. |
| `FFlagDebugGraphicsPreferOpenGL` | bool | `true` / `false` | Prefiere OpenGL para el renderizado. |

### Estabilidad y VRAM

Estas flags ayudan a prevenir caídas por falta de memoria (out-of-memory), especialmente en GPUs con VRAM limitada.

| Nombre de la Flag | Tipo | Rango de valores | Qué hace |
| :--- | :--- | :--- | :--- |
| `DFFlagTextureQualityOverrideEnabled` | bool | `true` / `false` | Permite el control manual sobre la resolución de texturas. |
| `DFIntTextureQualityOverride` | int | `0` - `3` | Establece la calidad de las texturas (0 = la más baja, 3 = la máxima). |

> [!WARNING]
> Establecer `DFIntTextureQualityOverride` en `3` en GPUs con 4 GB de VRAM o menos causará un crash instantáneo `RBXCRASH: OutOfMemory`. Usa `2` o `1` para mayor estabilidad.

### Interfaz de usuario y entorno

Flags menores que afectan la comodidad visual y el comportamiento de la interfaz.

| Nombre de la Flag | Tipo | Rango de valores | Qué hace |
| :--- | :--- | :--- | :--- |
| `FIntGrassMovementReducedMotionFactor` | bool | `true` / `false` | Reduce la animación del movimiento del césped. *(Nota: Utiliza el prefijo `FInt` pero requiere un valor booleano `true`/`false`).* |

## Preajustes de configuración

Copia uno de los siguientes preajustes en tu archivo de configuración de Sober:

```
~/.var/app/org.vinegarhq.Sober/config/sober/config.json
```

### Preset 1: Gama baja / Solución para crash por VRAM

Para GPUs con menos de 4 GB de VRAM, gráficos integrados o sistemas que experimentan crashes `OutOfMemory`.

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

### Preset 2: Equilibrado / Gama media

Para GPUs de gama media (clase GTX 1650, RX 580) con 4 a 6 GB de VRAM. Buen equilibrio entre calidad visual y rendimiento.

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
> Puedes combinar flags de diferentes preajustes. Por ejemplo, usa las distancias LOD del Preset 2 con la calidad de texturas del Preset 3 si tu GPU tiene suficiente VRAM pero sufre con la geometría.

## Temas detallados

<details>
<summary><strong>Nivel de detalle (LOD) y escala de geometría</strong></summary>

Los mapas de Roblox a menudo usan uniones complejas de Geometría Sólida Constructiva (CSG). Renderizar estas a grandes distancias supone una carga pesada tanto para la CPU como para la GPU.

Al reducir `DFIntCSGLevelOfDetailSwitchingDistance` (por ejemplo, a `150`), obligas al juego a cambiar modelos complejos por versiones de pocos polígonos (low-poly) más cerca de la cámara. Esto aumenta la tasa de fotogramas (FPS) sin cambiar las cajas de colisión físicas (hitboxes): los objetos se comportan igual, pero se ven más simples desde lejos.

Las variantes escalonadas (`L12`, `L23`, `L34`) permiten ajustar esto por nivel de calidad gráfica, de modo que los ajustes de calidad más bajos descartan geometría de forma más agresiva.

</details>

<details>
<summary><strong>Asignación de VRAM y caídas por falta de memoria (OOM)</strong></summary>

Sober ejecuta el binario de Android de Roblox dentro de un entorno Linux Flatpak. El binario de Android asume un modelo de memoria móvil compartida, que no coincide con la forma en que los controladores de GPU de Linux de escritorio gestionan la VRAM.

Cuando el motor solicita texturas de calidad máxima, puede agotar rápidamente la VRAM dedicada de la GPU. En Windows de escritorio, el controlador derivaría el exceso de datos a la RAM del sistema. En Linux (especialmente con controladores propietarios de NVIDIA), esta alternativa no funciona de manera confiable, lo que provoca una caída instantánea `RBXCRASH: OutOfMemory`.

Para solucionarlo, establece `DFFlagTextureQualityOverrideEnabled` en `true` y `DFIntTextureQualityOverride` en `2` (medio) o `1` (bajo). Esto obliga al motor a solicitar texturas más pequeñas del servidor, manteniendo el uso de VRAM dentro de límites seguros.

</details>

<details>
<summary><strong>APIs gráficas: Vulkan vs. OpenGL</strong></summary>

La selección de la API gráfica se configura en `config.json` o mediante FFlags (`FFlagDebugGraphicsPreferVulkan` / `FFlagDebugGraphicsPreferOpenGL`).

- Por defecto, Sober utiliza Vulkan para un rendimiento óptimo.
- Si experimentas artefactos gráficos, pantallas negras o fallos al iniciar (común en GPU antiguas o portátiles híbridas), la documentación oficial de Vinegar recomienda ejecutar `flatpak run org.vinegarhq.Sober config` en la terminal y seleccionar **Force Legacy Rendering** (o establecer `"use_opengl": true` en `config.json`).

</details>

<details>
<summary><strong>Superposición de archivos (texturas y cursores personalizados)</strong></summary>

Sober permite reemplazar los archivos del juego a través del directorio `asset_overlay` ubicado en:
`~/.var/app/org.vinegarhq.Sober/data/sober/asset_overlay`

Los archivos colocados aquí se utilizan con prioridad sobre los predeterminados al reiniciar la aplicación. La estructura de carpetas refleja `packages/*/com.roblox.client/base.apk/assets`.

Ejemplo para cursores de ratón personalizados:
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
Para revertir los cambios, limpia los archivos del directorio `asset_overlay`.

</details>

<details>
<summary><strong>Pantalla completa (F11) y controles de salida</strong></summary>

El botón de pantalla completa integrado en Roblox no funciona en versiones móviles de Android. En Sober, presiona `F11` para alternar la pantalla completa. Sober recuerda el estado de pantalla completa para los siguientes inicios.

Para cerrar automáticamente la aplicación al salir de una experiencia, agrega `"close_on_leave": true` a tu `config.json`.

</details>

## Desbloqueo de FPS

La FastFlag `DFIntTaskSchedulerTargetFps` ya no funciona porque Roblox la eliminó de la lista de permitidos. Para cambiar el límite de fotogramas, debes editar el archivo de configuración XML directamente.

### Pasos:

1. Inicia cualquier experiencia de Roblox en Sober para generar los archivos de configuración.
2. Cierra el cliente por completo.
3. Navega a `~/.var/app/org.vinegarhq.Sober/data/sober/appData/`.
4. Abre `GlobalBasicSettings_13.xml` en un editor de texto.
5. Busca la línea: `<int name="FramerateCap">60</int>`
6. Cambia `60` por la tasa de fotogramas deseada (por ejemplo, `144`, `240`) o `0` para desbloquearla por completo.
7. Guarda, cierra y vuelve a iniciar Roblox.

> [!NOTE]
> Debes cerrar Roblox antes de editar este archivo. El cliente sobrescribe este archivo al salir, por lo que cualquier cambio realizado mientras el juego se ejecuta se perderá.

## Riesgos de seguridad y anti-cheat

Roblox utiliza el sistema anti-cheat Hyperion (Byfron). Configurar las FFlags permitidas en `config.json` es seguro. Intentar eludir la lista de permitidos no lo es.

> [!CAUTION]
> Las siguientes acciones conllevan un alto riesgo de expulsión permanente de la cuenta o del hardware. No las intentes.

- **Manipulación de archivos caché (`IxpSettings.json`):** Inyectar flags no autorizadas en los archivos caché y bloquearlos como de solo lectura se detecta como manipulación por parte de Hyperion.
- **Edición de memoria:** El uso de herramientas para forzar la carga de flags no permitidas (como la manipulación del paso de tiempo de físicas a través de `DFIntTimestepArbiterThresholdCFLThou` o eludir texturas para wallhacks) activa expulsiones automáticas.
- **Programas de elusión de la lista de permitidos:** Cualquier programa o argumento de lanzamiento diseñado para esquivar el filtro de FFlags se clasifica como exploit.

## FFlags obsoletas

Estas flags se encuentran en guías antiguas, pero Roblox las eliminó de la lista de permitidos. Añadirlas a `config.json` no tiene ningún efecto porque el cliente las ignora.

| Flag | Por qué está obsoleta |
| :--- | :--- |
| `DFIntTaskSchedulerTargetFps` | Reemplazada por la edición de `GlobalBasicSettings_13.xml`. |
| `FFlagTaskSchedulerLimitTargetFpsTo2402` | Eliminada de la lista de permitidos. |
| `DFIntConnectionMTUSize` | Las flags de ajuste de red están bloqueadas. |
| `FFlagDebugDisableTelemetryEphemeralCounter` | La supresión de telemetría está bloqueada. |
| `FFlagAdServiceEnabled` | La alternancia del servicio de anuncios está bloqueada. |
| `FFlagMovePrerender` | Las flags de manipulación de hilos están bloqueadas. |
| `DFIntDebugDynamicRenderKiloPixels` | La escala de resolución de renderizado fue rechazada por Roblox. |

## Descargo de responsabilidad y fuentes

> [!NOTE]
> Roblox Corporation mantiene la lista de permitidos de FFlags y esta puede cambiar en cualquier momento con futuras actualizaciones del cliente. Esta guía es precisa a fecha de **julio de 2026**. Verifique siempre con la fuente oficial antes de implementar configuraciones.

**Fuentes oficiales:**
- [Lista de permitidos para Fast Flags en Roblox DevForum](https://devforum.roblox.com/t/allowlist-for-local-client-configuration-via-fast-flags/3966569)
- [Consejos y trucos de configuración en la documentación de Vinegar](https://vinegarhq.org/Sober/Configuration/TipsAndTricks.html)
