<h1 align="center">Guia de FastFlags do Roblox para o Sober</h1>
<p align="center">FastFlags verificados, seguros e na allowlist para o cliente Sober no Linux.</p>

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
> Este guia foi traduzido usando IA. Revisões ou contribuições humanas são bem-vindas para melhorar a qualidade da tradução. Fique à vontade para abrir um Pull Request para correções!

---

## Sumário

- [O que é o Sober?](#o-que-é-o-sober)
- [O que são FastFlags?](#o-que-são-fastflags)
- [FFlags Ativos Confirmados](#fflags-ativos-confirmados)
  - [Renderização & Desempenho](#renderização--desempenho)
  - [Estabilidade & VRAM](#estabilidade--vram)
  - [Interface & Ambiente](#interface--ambiente)
- [Predefinições de Configuração](#predefinições-de-configuração)
  - [Predefinição 1: Baixo Desempenho / Correção de Crash de VRAM](#predefinição-1-baixo-desempenho--correção-de-crash-de-vram)
  - [Predefinição 2: Equilibrado / Intermediário](#predefinição-2-equilibrado--intermediário)
  - [Predefinição 3: Fidelidade Gráfica Máxima](#predefinição-3-fidelidade-gráfica-máxima)
- [Tópicos Avançados](#tópicos-avançados)
- [Desbloqueio de Framerate (FPS)](#desbloqueio-de-framerate-fps)
- [Segurança & Riscos de Anti-Cheat](#segurança--riscos-de-anti-cheat)
- [FFlags Obsoletos (Descontinuados)](#fflags-obsoletos-descontinuados)
- [Aviso Legal & Fontes](#aviso-legal--fontes)

---

## O que é o Sober?

[O Sober](https://sober.vinegarhq.org/) é uma camada de compatibilidade que roda o aplicativo Roblox Android (APK) nativamente no Linux. Ele é distribuído como Flatpak (`org.vinegarhq.Sober`) e usa Vulkan como seu backend de renderização principal, com OpenGL como alternativa. A configuração é gerenciada através do arquivo `~/.var/app/org.vinegarhq.Sober/config/sober/config.json`. Você também pode abrir o menu de configurações graficamente via terminal com `flatpak run org.vinegarhq.Sober config` ou clicando com o botão direito no Sober no menu de aplicativos e selecionando **Settings**.

## O que são FastFlags?

FastFlags (FFlags) são variáveis internas da engine do Roblox que controlam renderização, interface, estabilidade e mais. Desde **29 de setembro de 2025**, o Roblox impõe uma lista de permissões (allowlist) rígida — apenas um pequeno subconjunto de flags pode ser substituído localmente por meio de arquivos de configuração. Qualquer flag fora da allowlist é silenciosamente ignorada pelo cliente.

> [!IMPORTANT]
> Este guia cobre apenas as flags confirmadas como parte da allowlist atual. Flags de guias de comunidade mais antigos podem não funcionar mais.

---

## FFlags Ativos Confirmados

### Renderização & Desempenho

Estas flags controlam o nível de detalhes geométricos, anti-aliasing, iluminação e distância de renderização de grama.

| Nome da Flag | Tipo | Intervalo de Valores | O que Faz |
| :--- | :--- | :--- | :--- |
| `DFIntCSGLevelOfDetailSwitchingDistance` | int | `0` - `1000` | Distância master de culling LOD para modelos CSG. Menor = melhor FPS. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL12` | int | `0` - `1000` | Distância LOD para qualidade gráfica 1–2. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL23` | int | `0` - `1000` | Distância LOD para qualidade gráfica 2–3. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL34` | int | `0` - `1000` | Distância LOD para qualidade gráfica 3–4. |
| `FIntDebugForceMSAASamples` | int | `1`, `2`, `4` | Força o anti-aliasing MSAA (bordas mais suaves, consome GPU). |
| `DFIntDebugFRMQualityLevelOverride` | int | `0` - `21` | Sobrescreve o seletor de nível gráfico (vai além do padrão 1–10). |
| `FIntFRMMaxGrassDistance` | int | `0` - `1000` | Distância máxima de renderização da grama do terreno. Defina `0` para desativar a grama. |
| `FIntFRMMinGrassDistance` | int | `0` - `1000` | Distância mínima onde a grama começa a ser renderizada. |

### Estabilidade & VRAM

Estas flags ajudam a prevenir crashes por falta de memória (out-of-memory), especialmente em GPUs com VRAM limitada.

| Nome da Flag | Tipo | Intervalo de Valores | O que Faz |
| :--- | :--- | :--- | :--- |
| `DFFlagTextureQualityOverrideEnabled` | bool | `true` / `false` | Habilita controle manual sobre a resolução de texturas. |
| `DFIntTextureQualityOverride` | int | `0` - `3` | Define a qualidade da textura (0 = mínima, 3 = máxima). |

> [!WARNING]
> Definir `DFIntTextureQualityOverride` como `3` em GPUs com 4GB de VRAM ou menos muito provavelmente causará um crash instantâneo `RBXCRASH: OutOfMemory`. Use `2` or `1` para maior estabilidade.

### Interface & Ambiente

Flags menores que afetam o conforto visual e o comportamento da interface.

| Nome da Flag | Tipo | Intervalo de Valores | O que Faz |
| :--- | :--- | :--- | :--- |
| `FIntGrassMovementReducedMotionFactor` | int | `0` - `1000` | Controla a intensidade da animação de balanço da grama. `0` = completamente estática. |

---

## Predefinições de Configuração

Copie uma das predefinições abaixo e cole-a no seu arquivo de configuração do Sober:

```
~/.var/app/org.vinegarhq.Sober/config/sober/config.json
```

### Predefinição 1: Baixo Desempenho / Correção de Crash de VRAM

Para GPUs com menos de 4GB de VRAM, gráficos integrados ou sistemas com crashes de `OutOfMemory`.

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

### Predefinição 2: Equilibrado / Intermediário

Para GPUs intermediárias (classe GTX 1650, RX 580) com 4–6GB de VRAM. Bom equilíbrio entre visual e desempenho.

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

### Predefinição 3: Fidelidade Gráfica Máxima

Para sistemas de alto desempenho com 8GB+ de VRAM. Força o máximo de detalhes, anti-aliasing e qualidade de textura.

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
> Você pode misturar e combinar flags entre as predefinições. Por exemplo, use as distâncias de LOD da predefinição Equilibrada com a qualidade de textura Máxima se a sua GPU tiver VRAM suficiente, mas sofrer com geometria.

---

## Tópicos Avançados

<details>
<summary><strong>Nível de Detalhe (LOD) & Escalonamento de Geometria</strong></summary>

Os mapas do Roblox costumam usar uniões complexas de Geometria Sólida Construtiva (CSG). Renderizar esses objetos a longas distâncias sobrecarrega bastante a CPU e a GPU.

Ao reduzir o valor de `DFIntCSGLevelOfDetailSwitchingDistance` (ex.: para `150`), você força o jogo a trocar modelos complexos por versões de baixa contagem de polígonos (low-poly) mais perto da câmera. Isso aumenta significativamente o framerate sem alterar os hitboxes físicos de colisão — os objetos se comportam da mesma maneira, mas parecem mais simples de longe.

As variantes em camadas (`L12`, `L23`, `L34`) permitem ajustar isso por nível de qualidade gráfica, fazendo com que as configurações mais baixas façam o culling de forma mais agressiva.

</details>

<details>
<summary><strong>Alocação de VRAM & Crashes de Falta de Memória (OOM)</strong></summary>

O Sober executa o binário Android do Roblox dentro de um ambiente Flatpak no Linux. O binário de Android assume um modelo de memória móvel compartilhada, que não corresponde a como os drivers de GPU no Linux desktop lidam com a VRAM.

**O problema:** Quando a engine solicita texturas de qualidade máxima, ela pode esgotar rapidamente a VRAM dedicada da GPU. No Windows desktop, o driver transferiria o excesso de dados para a memória RAM do sistema. No Linux (especialmente com drivers NVIDIA proprietários), essa alternativa não funciona de forma confiável — causando um crash instantâneo `RBXCRASH: OutOfMemory`.

**A solução:** Defina `DFFlagTextureQualityOverrideEnabled` como `true` e `DFIntTextureQualityOverride` como `2` (médio) ou `1` (baixo). Isso força a engine a solicitar texturas menores do servidor, mantendo o uso de VRAM dentro de limites seguros.

</details>

<details>
<summary><strong>APIs Gráficas: Vulkan vs. OpenGL</strong></summary>

A seleção da API gráfica é gerenciada pelas configurações do Sober, **não** por FFlags internas.

- Por padrão, o Sober usa **Vulkan** para desempenho ideal.
- Se você tiver artefatos visuais, tela preta ou travamentos ao iniciar (comum em GPUs antigas ou notebooks híbridos), force o uso de OpenGL definindo `"use_opengl": true` na raiz do seu `config.json`.

Não use FFlags como `FFlagDebugGraphicsPreferVulkan` ou `FFlagDebugGraphicsPreferOpenGL` para isso — elas podem causar travamentos por incompatibilidade de contexto.

</details>

<details>
<summary><strong>Sobreposição de Arquivos (Texturas e Cursores Personalizados)</strong></summary>

O Sober permite substituir arquivos do jogo através do diretório `asset_overlay` localizado em:
`~/.var/app/org.vinegarhq.Sober/data/sober/asset_overlay`

Os arquivos colocados aqui têm prioridade sobre os arquivos padrão do Roblox ao reiniciar o aplicativo. A estrutura de pastas espelha `packages/*/com.roblox.client/base.apk/assets`.

Exemplo para cursores de mouse personalizados:
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
Para reverter as alterações, basta limpar os arquivos do diretório `asset_overlay`.

</details>

<details>
<summary><strong>Tela Cheia (F11) e Controles de Saída</strong></summary>

O botão de tela cheia nativo do Roblox não funciona em compilações Android móveis. No Sober, pressione `F11` para alternar o modo tela cheia. O Sober lembra do estado de tela cheia nas inicializações futuras.

Para fechar o aplicativo automaticamente ao sair de uma experiência, adicione `"close_on_leave": true` no seu `config.json`.

</details>

---

## Desbloqueio de Framerate (FPS)

A FastFlag `DFIntTaskSchedulerTargetFps` não funciona mais — ela foi removida da allowlist. Para alterar o limite de framerate, você deve editar o arquivo de configurações XML diretamente.

### Passos:

1. Inicie qualquer experiência do Roblox no Sober para gerar os arquivos de configuração.
2. Feche o cliente completamente.
3. Navegue até `~/.var/app/org.vinegarhq.Sober/data/sober/appData/`.
4. Abra o arquivo `GlobalBasicSettings_13.xml` em um editor de texto.
5. Encontre a linha: `<int name="FramerateCap">60</int>`
6. Mude o valor `60` para a sua taxa de quadros desejada (ex.: `144`, `240`) ou `0` para desbloquear.
7. Salve, feche e abra o Roblox novamente.

> [!NOTE]
> Você precisa fechar o Roblox antes de editar este arquivo. O cliente sobrescreve este arquivo ao fechar, portanto, qualquer alteração feita com o jogo aberto será perdida.

---

## Segurança & Riscos de Anti-Cheat

O Roblox usa o sistema anti-cheat Hyperion (Byfron). Configurar FFlags permitidas (allowlisted) no arquivo `config.json` é completamente seguro. Tentar burlar a allowlist não é.

> [!CAUTION]
> As seguintes ações trazem um alto risco de banimentos permanentes de conta ou de hardware. Não tente realizá-las.

- **Manipulação de arquivos de cache (`IxpSettings.json`):** Injetar flags não autorizadas em arquivos de cache e bloqueá-los como somente leitura é detectado como alteração não autorizada pelo Hyperion.
- **Edição de memória:** Usar ferramentas para forçar o carregamento de flags não permitidas (como alteração do timestep de física por meio de `DFIntTimestepArbiterThresholdCFLThou`, ou bypass de textura para wallhacks) dispara banimentos automáticos.
- **Programas para burlar a allowlist:** Qualquer programa ou argumento de inicialização projetado para contornar o filtro de FFlags é classificado como exploit.

---

## FFlags Obsoletos (Descontinuados)

As seguintes flags costumam ser encontradas em guias antigos, mas não fazem mais parte da allowlist. Adicioná-las ao `config.json` terá efeito nulo — elas serão apenas ignoradas silenciosamente pelo cliente.

| Flag | Por que está Obsoleta |
| :--- | :--- |
| `DFIntTaskSchedulerTargetFps` | Substituída pela edição de `GlobalBasicSettings_13.xml`. |
| `FFlagTaskSchedulerLimitTargetFpsTo2402` | Removida da lista de permissões. |
| `DFFlagDebugPauseVoxelizer` | A supressão de iluminação voxel está bloqueada na lista atual. |
| `FFlagDebugSkyGray` | A substituição por céu cinza plano está bloqueada na lista atual. |
| `DFIntConnectionMTUSize` | Flags de ajuste de rede foram bloqueadas. |
| `FFlagDebugDisableTelemetryEphemeralCounter` | Bloqueio de envio de telemetria foi impedido. |
| `FFlagAdServiceEnabled` | Alternância do serviço de anúncios foi bloqueada. |
| `FFlagMovePrerender` | Flags de manipulação de threads foram bloqueadas. |
| `DFIntDebugDynamicRenderKiloPixels` | Ajuste de resolução de renderização foi vetado pela equipe do Roblox. |

---

## Aviso Legal & Fontes

> [!NOTE]
> A allowlist de FFlags é mantida pela Roblox Corporation e pode mudar a qualquer momento com atualizações futuras do cliente. Este guia está atualizado até **julho de 2026**. Sempre verifique na fonte oficial antes de implantar configurações.

**Fontes Oficiais:**
- [Allowlist for local client configuration via Fast Flags — Roblox DevForum](https://devforum.roblox.com/t/allowlist-for-local-client-configuration-via-fast-flags/3966569)
- [Sober Configuration Tips & Tricks — Documentação do Vinegar](https://vinegarhq.org/Sober/Configuration/TipsAndTricks.html)
