<h1 align="center">Panduan FastFlags Roblox untuk Sober</h1>
<p align="center">FastFlags yang terverifikasi, aman, dan masuk allowlist untuk Sober di Linux.</p>

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

## Daftar Isi

- [Apa itu Sober?](#apa-itu-sober)
- [Apa itu FastFlags?](#apa-itu-fastflags)
- [FFlags Aktif yang Terkonfirmasi](#fflags-aktif-yang-terkonfirmasi)
  - [Rendering & Performa](#rendering--performa)
  - [Stabilitas & VRAM](#stabilitas--vram)
  - [UI & Lingkungan](#ui--lingkungan)
- [Preset Konfigurasi](#preset-konfigurasi)
  - [Preset 1: Kelas Bawah (Low-End) / Perbaikan Crash VRAM](#preset-1-kelas-bawah-low-end--perbaikan-crash-vram)
  - [Preset 2: Seimbang (Balanced) / Kelas Menengah (Mid-Range)](#preset-2-seimbang-balanced--kelas-menengah-mid-range)
  - [Preset 3: Kualitas Grafis Maksimal](#preset-3-kualitas-grafis-maksimal)
- [Topik Mendalam](#topik-mendalam)
- [Membuka Batas Framerate (FPS)](#membuka-batas-framerate-fps)
- [Keamanan & Risiko Anti-Cheat](#keamanan--risiko-anti-cheat)
- [FFlags yang Usang (Deprecated)](#fflags-yang-usang-deprecated)
- [Pernyataan Penyangkalan (Disclaimer) & Sumber](#pernyataan-penyangkalan-disclaimer--sumber)

---

## Apa itu Sober?

[Sober](https://sober.vinegarhq.org/) adalah lapisan kompatibilitas (compatibility layer) yang menjalankan aplikasi Roblox Android (APK) secara native di desktop Linux. Aplikasi ini didistribusikan sebagai Flatpak (`org.vinegarhq.Sober`) dan menggunakan Vulkan sebagai backend rendering utama, dengan OpenGL sebagai cadangan (fallback). Konfigurasi dikelola melalui `~/.var/app/org.vinegarhq.Sober/config/sober/config.json`.

## Apa itu FastFlags?

FastFlags (FFlags) adalah variabel internal engine Roblox yang mengontrol rendering, UI, stabilitas, dan banyak lagi. Sejak **29 September 2025**, Roblox menerapkan allowlist yang ketat — hanya sebagian kecil flag yang dapat diubah secara lokal melalui file konfigurasi. Flag apa pun yang tidak ada dalam allowlist akan diabaikan secara diam-diam oleh klien.

> [!IMPORTANT]
> Panduan ini hanya mencakup flag yang **terkonfirmasi ada di dalam allowlist saat ini**. Flag dari panduan komunitas yang lebih lama mungkin tidak berfungsi lagi.

---

## FFlags Aktif yang Terkonfirmasi

### Rendering & Performa

Flag ini mengontrol detail geometri, anti-aliasing, pencahayaan, dan jarak rendering rumput.

| Nama Flag | Tipe | Rentang Nilai | Fungsi / Kegunaan |
| :--- | :--- | :--- | :--- |
| `DFIntCSGLevelOfDetailSwitchingDistance` | int | `0` - `1000` | Jarak master culling LOD untuk model CSG. Semakin rendah = FPS semakin baik. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL12` | int | `0` - `1000` | Jarak LOD untuk Kualitas Grafis 1–2. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL23` | int | `0` - `1000` | Jarak LOD untuk Kualitas Grafis 2–3. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL34` | int | `0` - `1000` | Jarak LOD untuk Kualitas Grafis 3–4. |
| `FIntDebugForceMSAASamples` | int | `1`, `2`, `4` | Memaksa anti-aliasing MSAA (tepi lebih halus, membebani GPU). |
| `DFFlagDebugPauseVoxelizer` | bool | `true` / `false` | Menjeda pencahayaan voxel, bayangan, dan ambient occlusion. Peningkatan FPS yang besar. |
| `FFlagDebugSkyGray` | bool | `true` / `false` | Mengganti skybox dengan warna abu-abu datar. Menghilangkan beban shader langit. |
| `DFIntDebugFRMQualityLevelOverride` | int | `0` - `21` | Mengabaikan penggeser tingkat grafis (melampaui batas default 1–10). |
| `FIntFRMMaxGrassDistance` | int | `0` - `1000` | Jarak render maksimum untuk rumput terrain. Atur ke `0` untuk menonaktifkan rumput. |
| `FIntFRMMinGrassDistance` | int | `0` - `1000` | Jarak minimum rumput mulai dirender. |

### Stabilitas & VRAM

Flag ini membantu mencegah crash akibat kehabisan memori (out-of-memory), terutama pada GPU dengan VRAM terbatas.

| Nama Flag | Tipe | Rentang Nilai | Fungsi / Kegunaan |
| :--- | :--- | :--- | :--- |
| `DFFlagTextureQualityOverrideEnabled` | bool | `true` / `false` | Mengaktifkan kontrol manual atas resolusi tekstur. |
| `DFIntTextureQualityOverride` | int | `0` - `3` | Mengatur kualitas tekstur (0 = terendah, 3 = maksimal). |

> [!WARNING]
> Mengatur `DFIntTextureQualityOverride` ke `3` pada GPU dengan VRAM 4GB atau kurang kemungkinan besar akan menyebabkan crash instan `RBXCRASH: OutOfMemory`. Gunakan `2` atau `1` untuk stabilitas.

### UI & Lingkungan

Flag minor yang memengaruhi kenyamanan visual dan perilaku antarmuka.

| Nama Flag | Tipe | Rentang Nilai | Fungsi / Kegunaan |
| :--- | :--- | :--- | :--- |
| `FIntGrassMovementReducedMotionFactor` | int | `0` - `1000` | Mengontrol intensitas animasi lambaian rumput. `0` = benar-benar beku. |

---

## Preset Konfigurasi

Salin salah satu preset di bawah ini dan tempelkan ke file konfigurasi Sober Anda:

```
~/.var/app/org.vinegarhq.Sober/config/sober/config.json
```

### Preset 1: Kelas Bawah (Low-End) / Perbaikan Crash VRAM

Untuk GPU dengan VRAM kurang dari 4GB, grafis terintegrasi (integrated graphics), atau sistem yang mengalami crash `OutOfMemory`.

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

### Preset 2: Seimbang (Balanced) / Kelas Menengah (Mid-Range)

Untuk GPU kelas menengah (seperti GTX 1650, RX 580) dengan VRAM 4-6GB. Keseimbangan yang baik antara visual dan performa.

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

### Preset 3: Kualitas Grafis Maksimal

Untuk sistem kelas atas dengan VRAM 8GB+. Memaksa detail maksimal, anti-aliasing, dan kualitas tekstur.

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
> Anda dapat mencampur dan mencocokkan flag di antara berbagai preset. Contohnya, gunakan jarak LOD Preset 2 dengan kualitas tekstur Preset 3 jika GPU Anda memiliki cukup VRAM tetapi kesulitan dengan geometri.

---

## Topik Mendalam

<details>
<summary><strong>Level of Detail (LOD) & Penskalaan Geometri</strong></summary>

Peta Roblox sering kali menggunakan union Constructive Solid Geometry (CSG) yang kompleks. Merender ini pada jarak jauh memberikan beban berat baik pada CPU maupun GPU.

Dengan menurunkan `DFIntCSGLevelOfDetailSwitchingDistance` (misalnya ke `150`), Anda memaksa game untuk menukar model yang kompleks dengan versi poligonal rendah (low-polygon) yang lebih dekat dengan kamera. Ini secara signifikan meningkatkan framerate tanpa mengubah hitbox tabrakan fisik — objek tetap berperilaku sama, hanya terlihat lebih sederhana dari jauh.

Varian bertingkat (`L12`, `L23`, `L34`) memungkinkan Anda menyempurnakan ini per tingkat kualitas grafis, sehingga pengaturan kualitas yang lebih rendah melakukan culling secara lebih agresif.

</details>

<details>
<summary><strong>Alokasi VRAM & Crash Out-of-Memory (OOM)</strong></summary>

Sober menjalankan file biner Android Roblox di dalam lingkungan Flatpak Linux. Biner Android mengasumsikan model memori seluler bersama (shared mobile memory model), yang tidak cocok dengan cara driver GPU Linux desktop menangani VRAM.

**Masalahnya:** Ketika engine meminta tekstur kualitas maksimal, ia dapat dengan cepat menghabiskan VRAM GPU khusus. Di desktop Windows, driver akan memindahkan data berlebih ke RAM sistem. Di Linux (terutama dengan driver proprietary NVIDIA), fallback ini tidak bekerja dengan andal — menyebabkan crash instan `RBXCRASH: OutOfMemory`.

**Solusinya:** Atur `DFFlagTextureQualityOverrideEnabled` ke `true` and `DFIntTextureQualityOverride` ke `2` (sedang) atau `1` (rendah). Ini memaksa engine untuk meminta tekstur yang lebih kecil dari server, menjaga penggunaan VRAM dalam batas aman.

</details>

<details>
<summary><strong>API Grafis: Vulkan vs. OpenGL</strong></summary>

Pemilihan API Grafis dikelola oleh konfigurasi wrapper Sober, **bukan** oleh FFlags internal.

- Secara default, Sober menggunakan **Vulkan** untuk performa optimal.
- Jika Anda mengalami artefak grafis, layar hitam, atau crash saat startup (umum terjadi pada GPU lama atau setup laptop hybrid), paksa penggunaan OpenGL dengan mengatur `"use_opengl": true` di tingkat root file `config.json` Anda.

Jangan gunakan FFlags engine seperti `FFlagDebugGraphicsPreferVulkan` or `FFlagDebugGraphicsPreferOpenGL` untuk hal ini — flag tersebut dapat menyebabkan crash ketidakcocokan konteks (context mismatch) saat konflik dengan pemilihan API tingkat wrapper Sober.

</details>

---

## Membuka Batas Framerate (FPS)

FastFlag `DFIntTaskSchedulerTargetFps` tidak lagi berfungsi — flag ini telah dihapus dari allowlist. Untuk mengubah batas framerate Anda, Anda harus mengedit file pengaturan XML secara langsung.

### Langkah-langkah:

1. Jalankan pengalaman Roblox apa pun di Sober untuk membuat file konfigurasi.
2. Tutup klien sepenuhnya.
3. Buka folder `~/.var/app/org.vinegarhq.Sober/data/sober/appData/`.
4. Buka file `GlobalBasicSettings_13.xml` dengan editor teks.
5. Cari baris: `<int name="FramerateCap">60</int>`
6. Ubah nilai `60` ke target framerate Anda (misalnya `144`, `240`) atau `0` untuk membuka batas (uncap).
7. Simpan, tutup, dan jalankan kembali Roblox.

> [!NOTE]
> Anda harus menutup Roblox sebelum mengedit file ini. Klien menimpa file ini saat keluar, jadi perubahan apa pun yang dibuat saat game sedang berjalan akan hilang.

---

## Keamanan & Risiko Anti-Cheat

Roblox menggunakan sistem anti-cheat Hyperion (Byfron). Mengonfigurasi FFlags yang masuk allowlist dalam `config.json` sepenuhnya aman. Memaksa allowlist tidaklah aman.

> [!CAUTION]
> Tindakan ini membawa risiko tinggi terkena blokir (ban) akun atau perangkat keras secara permanen. Jangan mencobanya.

- **Manipulasi file cache (`IxpSettings.json`):** Menyuntikkan flag tidak resmi ke dalam file cache dan mengunci file menjadi baca-saja (read-only) akan terdeteksi sebagai modifikasi ilegal oleh Hyperion.
- **Pengeditan memori:** Menggunakan alat (tools) untuk memaksa memuat flag yang tidak diizinkan (misalnya manipulasi cara kerja game via `DFIntTimestepArbiterThresholdCFLThou`, atau bypass tekstur untuk wallhack) akan memicu pemblokiran otomatis.
- **Program pemintas allowlist:** Program atau argumen peluncuran apa pun yang dirancang untuk menghindari filter FFlag diklasifikasikan sebagai exploit.

---

## FFlags usang

Flag berikut biasanya ditemukan dalam panduan yang lebih lama tetapi tidak lagi ada dalam allowlist. Menambahkannya ke `config.json` tidak akan memberikan efek apa pun — flag tersebut diabaikan secara diam-diam.

| Flag | Alasan Ditinggalkan |
| :--- | :--- |
| `DFIntTaskSchedulerTargetFps` | Digantikan dengan mengedit `GlobalBasicSettings_13.xml`. |
| `FFlagTaskSchedulerLimitTargetFpsTo2402` | Dihapus dari allowlist. |
| `DFIntConnectionMTUSize` | Flag penyetelan jaringan diblokir. |
| `FFlagDebugDisableTelemetryEphemeralCounter` | Penekanan telemetri diblokir. |
| `FFlagAdServiceEnabled` | Peralihan layanan iklan diblokir. |
| `FFlagMovePrerender` | Flag manipulasi thread diblokir. |
| `DFIntDebugDynamicRenderKiloPixels` | Penskalaan resolusi render ditolak oleh tim rekayasa Roblox. |

---

## Pernyataan Penyangkalan (Disclaimer) & Sumber

> [!NOTE]
> Allowlist FFlag dikelola oleh Roblox Corporation dan dapat berubah kapan saja seiring dengan pembaruan klien di masa mendatang. Panduan ini akurat per **Juli 2026**. Selalu verifikasi dengan sumber resmi sebelum menerapkan konfigurasi.

**Sumber Resmi:** [Allowlist for local client configuration via Fast Flags — Roblox DevForum](https://devforum.roblox.com/t/allowlist-for-local-client-configuration-via-fast-flags/3966569)
