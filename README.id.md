<h1 align="center">Panduan FastFlags Roblox untuk Sober</h1>
<p align="center">FastFlags yang terverifikasi, aman, dan masuk allowlist untuk Sober di Linux.</p>

<p align="center">
  <b>Translations:</b>
  <a href="README.md">English</a> |
  <a href="README.es.md">Español</a> |
  <a href="README.zh.md">简体中文</a> |
  <a href="README.ru.md">Русский</a> |
  <a href="README.id.md">Bahasa Indonesia</a> |
  <a href="README.pt.md">Português</a>
</p>

## Daftar isi

- [Apa itu Sober?](#apa-itu-sober)
- [Apa itu FastFlags?](#apa-itu-fastflags)
- [FFlags aktif yang terkonfirmasi](#fflags-aktif-yang-terkonfirmasi)
  - [Rendering dan performa](#rendering-dan-performa)
  - [Stabilitas dan VRAM](#stabilitas-dan-vram)
  - [UI dan lingkungan](#ui-dan-lingkungan)
- [Preset konfigurasi](#preset-konfigurasi)
  - [Preset 1: Kelas bawah / Perbaikan crash VRAM](#preset-1-kelas-bawah--perbaikan-crash-vram)
  - [Preset 2: Seimbang / Kelas menengah](#preset-2-seimbang--kelas-menengah)
  - [Preset 3: Kualitas grafis maksimal](#preset-3-kualitas-grafis-maksimal)
- [Topik mendalam](#topik-mendalam)
- [Membuka batas framerate (FPS)](#membuka-batas-framerate-fps)
- [Keamanan dan risiko anti-cheat](#keamanan-dan-risiko-anti-cheat)
- [FFlags yang usang](#fflags-yang-usang)
- [Pernyataan penyangkalan dan sumber](#pernyataan-penyangkalan-dan-sumber)

## Apa itu Sober?

[Sober](https://sober.vinegarhq.org/) adalah lapisan kompatibilitas yang menjalankan aplikasi Roblox Android (APK) secara native di desktop Linux. Aplikasi ini didistribusikan sebagai Flatpak (`org.vinegarhq.Sober`) dan menggunakan Vulkan sebagai backend rendering utama, dengan OpenGL sebagai cadangan. Konfigurasi dikelola melalui `~/.var/app/org.vinegarhq.Sober/config/sober/config.json`. Kita juga dapat membuka menu pengaturan secara grafis menggunakan perintah `flatpak run org.vinegarhq.Sober config` atau dengan mengklik kanan Sober di menu aplikasi kita dan memilih **Settings**.

## Apa itu FastFlags?

FastFlags (FFlags) adalah variabel internal engine Roblox yang mengontrol rendering, UI, stabilitas, dan pengaturan lainnya. Sejak 29 September 2025, Roblox menerapkan allowlist yang ketat: hanya sebagian kecil flag yang dapat diubah secara lokal melalui file konfigurasi. Flag apa pun yang tidak ada dalam allowlist akan diabaikan oleh klien.

> [!IMPORTANT]
> Panduan ini hanya mencakup flag yang terkonfirmasi ada di dalam allowlist saat ini. Flag dari panduan komunitas yang lebih lama mungkin tidak berfungsi lagi.

## FFlags aktif yang terkonfirmasi

### Rendering dan performa

Flag ini mengontrol detail geometri, anti-aliasing, pencahayaan, dan jarak rendering rumput.

| Nama Flag | Tipe | Rentang Nilai | Fungsi / Kegunaan |
| :--- | :--- | :--- | :--- |
| `DFIntCSGLevelOfDetailSwitchingDistance` | int | `0` - `1000` | Jarak master culling LOD untuk model CSG. Semakin rendah = FPS semakin baik. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL12` | int | `0` - `1000` | Jarak LOD untuk Kualitas Grafis 1 hingga 2. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL23` | int | `0` - `1000` | Jarak LOD untuk Kualitas Grafis 2 hingga 3. |
| `DFIntCSGLevelOfDetailSwitchingDistanceL34` | int | `0` - `1000` | Jarak LOD untuk Kualitas Grafis 3 hingga 4. |
| `FIntDebugForceMSAASamples` | int | `1`, `2`, `4` | Memaksa anti-aliasing MSAA (tepi lebih halus, membebani GPU). |
| `DFIntDebugFRMQualityLevelOverride` | int | `0` - `21` | Mengabaikan penggeser tingkat grafis (melampaui batas default 1 hingga 10). |
| `FIntFRMMaxGrassDistance` | int | `0` - `1000` | Jarak render maksimum untuk rumput terrain. Atur ke `0` untuk menonaktifkan rumput. |
| `FIntFRMMinGrassDistance` | int | `0` - `1000` | Jarak minimum rumput mulai dirender. |
| `DFFlagDebugPauseVoxelizer` | bool | `true` / `false` | Menonaktifkan pencahayaan voxel. |
| `FFlagDebugSkyGray` | bool | `true` / `false` | Mengubah warna skybox menjadi abu-abu. |
| `FFlagDebugGraphicsPreferVulkan` | bool | `true` / `false` | Memprioritaskan Vulkan untuk rendering. |
| `FFlagDebugGraphicsPreferOpenGL` | bool | `true` / `false` | Memprioritaskan OpenGL untuk rendering. |

### Stabilitas dan VRAM

Flag ini membantu mencegah crash akibat kehabisan memori (out-of-memory), terutama pada GPU dengan VRAM terbatas.

| Nama Flag | Tipe | Rentang Nilai | Fungsi / Kegunaan |
| :--- | :--- | :--- | :--- |
| `DFFlagTextureQualityOverrideEnabled` | bool | `true` / `false` | Mengaktifkan kontrol manual atas resolusi tekstur. |
| `DFIntTextureQualityOverride` | int | `0` - `3` | Mengatur kualitas tekstur (0 = terendah, 3 = maksimal). |

> [!WARNING]
> Mengatur `DFIntTextureQualityOverride` ke `3` pada GPU dengan VRAM 4 GB atau kurang akan menyebabkan crash instan `RBXCRASH: OutOfMemory`. Gunakan `2` atau `1` untuk stabilitas.

### UI dan lingkungan

Flag minor yang memengaruhi kenyamanan visual dan perilaku antarmuka.

| Nama Flag | Tipe | Rentang Nilai | Fungsi / Kegunaan |
| :--- | :--- | :--- | :--- |
| `FIntGrassMovementReducedMotionFactor` | bool | `true` / `false` | Mengurangi gerakan animasi rumput. *(Catatan: Menggunakan awalan nama `FInt` tetapi membutuhkan nilai boolean `true`/`false`).* |

## Preset konfigurasi

Salin salah satu preset di bawah ini dan tempelkan ke file konfigurasi Sober kita:

```
~/.var/app/org.vinegarhq.Sober/config/sober/config.json
```

### Preset 1: Kelas bawah / Perbaikan crash VRAM

Untuk GPU dengan VRAM kurang dari 4 GB, grafis terintegrasi, atau sistem yang mengalami crash `OutOfMemory`.

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

### Preset 2: Seimbang / Kelas menengah

Untuk GPU kelas menengah (seperti GTX 1650, RX 580) dengan VRAM 4 hingga 6 GB. Keseimbangan yang baik antara visual dan performa.

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

### Preset 3: Kualitas grafis maksimal

Untuk sistem kelas atas dengan VRAM 8 GB+. Memaksa detail maksimal, anti-aliasing, dan kualitas tekstur.

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
> Kita dapat mencampur dan mencocokkan flag di antara berbagai preset. Contohnya, gunakan jarak LOD Preset 2 dengan kualitas tekstur Preset 3 jika GPU kita memiliki cukup VRAM tetapi kesulitan dengan geometri.

## Topik mendalam

<details>
<summary><strong>Level of detail (LOD) dan penskalaan geometri</strong></summary>

Peta Roblox sering kali menggunakan union Constructive Solid Geometry (CSG) yang kompleks. Merender ini pada jarak jauh memberikan beban berat baik pada CPU maupun GPU.

Dengan menurunkan `DFIntCSGLevelOfDetailSwitchingDistance` (misalnya ke `150`), kita memaksa game untuk menukar model yang kompleks dengan versi poligonal rendah (low-poly) yang lebih dekat dengan kamera. Ini meningkatkan framerate tanpa mengubah hitbox tabrakan fisik: objek tetap berperilaku sama, hanya terlihat lebih sederhana dari jauh.

Varian bertingkat (`L12`, `L23`, `L34`) memungkinkan kita menyempurnakan ini per tingkat kualitas grafis, sehingga pengaturan kualitas yang lebih rendah melakukan culling secara lebih agresif.

</details>

<details>
<summary><strong>Alokasi VRAM dan crash out-of-memory (OOM)</strong></summary>

Sober menjalankan file biner Android Roblox di dalam lingkungan Flatpak Linux. Biner Android mengasumsikan model memori seluler bersama, yang tidak cocok dengan cara driver GPU Linux desktop menangani VRAM.

Ketika engine meminta tekstur kualitas maksimal, ia dapat dengan cepat menghabiskan VRAM GPU khusus. Di desktop Windows, driver akan memindahkan data berlebih ke RAM sistem. Di Linux (terutama dengan driver proprietary NVIDIA), fallback ini tidak bekerja secara andal, yang menyebabkan crash instan `RBXCRASH: OutOfMemory`.

Untuk mengatasinya, atur `DFFlagTextureQualityOverrideEnabled` ke `true` dan `DFIntTextureQualityOverride` ke `2` (sedang) atau `1` (rendah). Ini memaksa engine untuk meminta tekstur yang lebih kecil dari server, menjaga penggunaan VRAM dalam batas aman.

</details>

<details>
<summary><strong>API grafis: Vulkan vs. OpenGL</strong></summary>

Pemilihan API Grafis dapat dikonfigurasi melalui `config.json` atau FFlags (`FFlagDebugGraphicsPreferVulkan` / `FFlagDebugGraphicsPreferOpenGL`).

- Secara default, Sober menggunakan Vulkan untuk performa optimal.
- Jika kita mengalami artefak grafis, layar hitam, atau crash saat startup (umum terjadi pada GPU lama atau laptop hybrid), dokumentasi resmi Vinegar menyarankan untuk menjalankan `flatpak run org.vinegarhq.Sober config` di terminal dan memilih **Force Legacy Rendering** (atau mengatur `"use_opengl": true` di `config.json`).

</details>

<details>
<summary><strong>Asset overlay (tekstur dan kursor kustom)</strong></summary>

Sober memungkinkan penggantian aset game melalui direktori `asset_overlay` yang terletak di:
`~/.var/app/org.vinegarhq.Sober/data/sober/asset_overlay`

File yang ditempatkan di sini akan diprioritaskan dibandingkan aset standar Roblox setelah aplikasi di-restart. Struktur folder mencerminkan `packages/*/com.roblox.client/base.apk/assets`.

Contoh untuk kursor mouse kustom:
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
Untuk mengembalikan ke default, hapus file dari direktori `asset_overlay`.

</details>

<details>
<summary><strong>Layar penuh (F11) dan kontrol keluar</strong></summary>

Tombol layar penuh bawaan Roblox tidak berfungsi pada biner Android. Di Sober, tekan `F11` untuk masuk atau keluar dari mode layar penuh. Sober menyimpan status layar penuh untuk peluncuran berikutnya.

Untuk menutup aplikasi secara otomatis saat meninggalkan permainan, tambahkan `"close_on_leave": true` ke file `config.json` kita.

</details>

## Membuka batas framerate (FPS)

FastFlag `DFIntTaskSchedulerTargetFps` tidak lagi berfungsi karena Roblox telah menghapusnya dari allowlist. Untuk mengubah batas framerate, kita harus mengedit file pengaturan XML secara langsung.

### Langkah-langkah:

1. Jalankan pengalaman Roblox apa pun di Sober untuk membuat file konfigurasi.
2. Tutup klien sepenuhnya.
3. Buka folder `~/.var/app/org.vinegarhq.Sober/data/sober/appData/`.
4. Buka file `GlobalBasicSettings_13.xml` dengan editor teks.
5. Cari baris: `<int name="FramerateCap">60</int>`
6. Ubah nilai `60` ke target framerate kita (misalnya `144`, `240`) atau `0` untuk membuka batas.
7. Simpan, tutup, dan jalankan kembali Roblox.

> [!NOTE]
> Kita harus menutup Roblox sebelum mengedit file ini. Klien menimpa file ini saat keluar, jadi perubahan apa pun yang dibuat saat game sedang berjalan akan hilang.

## Keamanan dan risiko anti-cheat

Roblox menggunakan sistem anti-cheat Hyperion (Byfron). Mengonfigurasi FFlags yang masuk allowlist dalam `config.json` sepenuhnya aman. Memaksa allowlist tidaklah aman.

> [!CAUTION]
> Tindakan ini membawa risiko tinggi terkena pemblokiran akun atau perangkat keras secara permanen. Jangan mencobanya.

- **Manipulasi file cache (`IxpSettings.json`):** Menyuntikkan flag tidak resmi ke dalam file cache dan mengunci file menjadi baca-saja terdeteksi sebagai modifikasi ilegal oleh Hyperion.
- **Pengeditan memori:** Menggunakan alat untuk memaksa memuat flag yang tidak diizinkan (misalnya manipulasi cara kerja game via `DFIntTimestepArbiterThresholdCFLThou` atau bypass tekstur untuk wallhack) akan memicu pemblokiran otomatis.
- **Program pemintas allowlist:** Program atau argumen peluncuran apa pun yang dirancang untuk menghindari filter FFlag diklasifikasikan sebagai exploit.

## FFlags yang usang

Flag berikut biasa ditemukan dalam panduan lama tetapi tidak lagi ada dalam allowlist. Menambahkannya ke `config.json` tidak memberikan efek apa pun karena klien mengabaikannya.

| Flag | Alasan Ditinggalkan |
| :--- | :--- |
| `DFIntTaskSchedulerTargetFps` | Digantikan dengan mengedit `GlobalBasicSettings_13.xml`. |
| `FFlagTaskSchedulerLimitTargetFpsTo2402` | Dihapus dari allowlist. |
| `DFIntConnectionMTUSize` | Flag penyetelan jaringan diblokir. |
| `FFlagDebugDisableTelemetryEphemeralCounter` | Penekanan telemetri diblokir. |
| `FFlagAdServiceEnabled` | Peralihan layanan iklan diblokir. |
| `FFlagMovePrerender` | Flag manipulasi thread diblokir. |
| `DFIntDebugDynamicRenderKiloPixels` | Penskalaan resolusi render ditolak oleh tim rekayasa Roblox. |

## Pernyataan penyangkalan dan sumber

> [!NOTE]
> Allowlist FFlag dikelola oleh Roblox Corporation dan dapat berubah kapan saja seiring dengan pembaruan klien di masa mendatang. Panduan ini akurat per **Juli 2026**. Selalu verifikasi dengan sumber resmi sebelum menerapkan konfigurasi.

**Sumber resmi:**
- [Allowlist untuk Fast Flags pada Roblox DevForum](https://devforum.roblox.com/t/allowlist-for-local-client-configuration-via-fast-flags/3966569)
- [Tips dan trik konfigurasi Sober dari Dokumentasi Vinegar](https://vinegarhq.org/Sober/Configuration/TipsAndTricks.html)
