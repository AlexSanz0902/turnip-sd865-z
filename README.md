# Turnip SD865 "Z"

A modified Turnip (Mesa) Vulkan driver for **Snapdragon 865 / Adreno 650**, tested on a **Retroid Pocket 5** with Switch emulation.

It is based on Mesa `main` (commit `bffd91947efc`, October 10, 2026) and adds six changes: four for performance and two for stability.

**Download:** see the [latest release](../../releases/latest).

## Results

Same device, same game and same scene, compared against StevenMXZ's Turnip v26.3.0-R5.

| | Stock | Z |
|---|---|---|
| Test scene, 1x docked (1080p) | 49.9 fps | 59.9 fps (game cap) |
| Test scene, 1.25x docked (1350p) | 33 fps | 49.2 fps |
| Real use across 5 games | 40–50 fps | 55–60 fps |
| Crashes or reboots in those games | — | none |

Games tested: *Link's Awakening*, *Super Mario Odyssey*, *Super Smash Bros. Ultimate*, *Dragon Ball FighterZ* and *Super Mario Bros. Wonder*.

<!-- Screenshots: upload them to a "screenshots" folder and uncomment.
![Super Mario Odyssey at 1350p](screenshots/odyssey-1350p.jpg)
![Link's Awakening](screenshots/links-awakening.jpg)
-->

## Performance changes

### 1. Compressed mutable images

- **Why:** on Adreno 6xx, Mesa makes images linear and uncompressed when they can be viewed with a different format. Emulators create nearly all their render targets that way, which is the most expensive way to use them on this GPU.
- **How:** in `tu_image.cc` those images stay tiled and UBWC-compressed, including when they declare storage usage.
- **Effect:** this change accounts for almost all of the gain.

### 2. Cheaper texture filtering

- **Why:** texture sampling is costly on a GPU that is already at its limit.
- **How:** in `tu_sampler.cc` anisotropic filtering is disabled, filtering between mip levels becomes "nearest", and 0.5 is added to the LOD bias.
- **Cost:** distant or steeply angled textures are slightly less sharp. It can be turned off with the system property `debug.mesa.tu.sd865.texfast=0` (needs root or adb).

### 3. LRZ rescue

- **Why:** LRZ rejects hidden pixels before they are shaded. Mesa turns it off when depth is cleared with `vkCmdClearAttachments`, which is exactly how emulators clear, so it never became active.
- **How:** in `tu_lrz.cc`, if that clear is the first thing in the render pass and covers the whole image, it is treated as a pass that starts with a clear and LRZ stays valid.
- **Effect:** +1.3 % measured at 1.25x scale.

### 4. Build tuned for the SoC

- **How:** `-O3`, ThinLTO, `-fno-plt` and `-march=armv8.2-a -mtune=cortex-a77`.
- **Effect:** it only affects the part of the driver that runs on the CPU. On its own it did not move fps in GPU-bound scenes.

## Stability changes

Both come from what the community already uses on Adreno 6xx. They were added together and there have been no crashes since; which of the two is responsible has not been isolated.

### 5. Direct rendering only

- **Why:** community builds for A6xx never use tiled rendering (GMEM). On this device, forcing it was also 2 to 3 times slower.
- **How:** in `tu_cmd_buffer.cc` direct rendering ("sysmem") is always used and the automatic selector is ignored.

### 6. Safe image-to-buffer readback

- **Why:** the NetherSX2-Turnip team linked SD865 reboots to reading compressed images back into a buffer.
- **How:** in `tu_clear_blit.cc`, before every `vkCmdCopyImageToBuffer` all GPU caches are flushed and the GPU is waited on until idle; the same is done after the copy.
- **Cost:** none measurable. It happens 2–3 times per frame.

## Things to know

- **Tested on a single device**, with the GPU overclocked to 905 MHz and one Switch emulator (Eden 0.2.0-rc2). There is no data for other GPUs or emulators.
- **The root cause of the SD865 crashes has not been identified.** What exists is evidence that they stopped happening.
- **Known issue:** in *Dragon Ball FighterZ* some HUD textures show a blue tint.
- **If a game writes from a shader into a compressed image**, that image will look corrupted. This did not happen in any of the games tested.
- **It writes a diagnostic log** to the emulator's data folder: `turnip_sd865.txt` and `turnip_sd865_estado.txt`. It contains no personal data and is not sent anywhere.
- **Development base:** this is Mesa's main branch, not a stable release.
- **No Mesa developer has reviewed this code.** Use it at your own risk and keep a driver you know works.

## Installation

1. Download `Turnip-SD865-26.3.0-Z-Comunidad.zip` from the [latest release](../../releases/latest).
2. In the emulator, open the GPU driver manager and install the zip as it is, without extracting it.
3. Select it and restart the game. Expect stutter for the first few minutes while shaders are recompiled.

SHA-256 of the zip: `965ff72efae4a5db2918aecbd42a7cf1bd5b35d2100a4986389f152227a05ec5`

## Reporting a problem

Open an issue with the game, the resolution scale, what happened, and the `turnip_sd865.txt` file from that session.

## Source

The full change on top of Mesa is in `variant-Z.diff`: 13 files and about 1,350 lines, under the same MIT license as Mesa. `tu_sd865_defaults.h` holds the default values the build uses. The build scripts are not included yet.

## Credits

- **The Mesa project and the freedreno and Turnip developers.** The driver is theirs; this is 1,350 lines on top of their work.
- **[StevenMXZ](https://github.com/StevenMXZ/Adreno-Tools-Drivers).** His Turnip v26.3.0-R5 is the reference for every comparison, and his build settings for NDK r29 are the ones used here.
- **[Weab-chan](https://github.com/Weab-chan/freedreno_turnip-CI).** Author of the `force_sysmem_no_autotuner` patch, the origin of change 5.
- **[nckstwrt and the NetherSX2-Turnip team](https://github.com/nckstwrt/NetherSX2-Turnip).** Their research into the SD865 reboots is the origin of change 6.
- **[The412Banner](https://github.com/The412Banner/Banners-Turnip).** His performance build recipe and his documentation of the KGSL fixes.
- **[K11MCH1](https://github.com/K11MCH1/AdrenoToolsDrivers) and [MrPurple666](https://github.com/MrPurple666/purple-turnip).** Their drivers have been the 865 community's reference for years.
- **[Tranquility6789](https://github.com/Tranquility6789/Turnip-A650-Patched-Drivers).** For documenting in public how the A650 was fixed.
- **libadrenotools, [HomuHomu833](https://github.com/HomuHomu833/android-ndk-custom), chaotic-cx and the Eden team.** For the driver loader, the NDK, the Mesa mirror and the emulator.

Device testing and direction: *[your name or alias]*. Analysis and code changes were AI-assisted (Claude, by Anthropic).
