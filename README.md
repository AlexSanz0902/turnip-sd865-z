# TurnipZ

Modified Mesa Turnip Vulkan driver focused on boosting emulation performance and stability across Adreno GPUs — primarily A6xx, with secondary support for A7xx.

## Benchmarks

## FAQ

**Status Legend:**
🟢🟢 Optimal | 🟢 Compatible | 🟡 Secondary support or needs more testing | 🟠 Lightly Tested or experimental support | 🔴 Unsupported or broken | ⚪ Untested

### Device Compatibility
Primarily developed for the **Adreno A6xx** architecture, with secondary support for **A7xx**.

| Status | Support Tier | GPU Architecture | Target SoCs / Examples | Verified Handhelds & Devices |
| :---: | :--- | :--- | :--- | :--- |
| 🟢🟢 | **Primary (Tested)** | **Adreno 650** | Snapdragon 865, 865+, 870 | Retroid Pocket 5, RP Mini, RP Flip 2, AYN Thor Lite, etc. |
| 🟢 | **Primary (General)** | **Adreno A6xx** | Snapdragon 845, 855, 860, 888, 7c+, etc.
| 🟡 | **Secondary** | **Adreno A7xx (Gen 1/2)** | Snapdragon 8 Gen 1/2, 7+ Gen 2 | AYN Thor, AYN Odin 2, Retroid Pocket 6 |
| ⚪ | **Secondary (Untested)** | **Adreno A7xx (Gen 3 / G3x)** | Snapdragon 8 Gen 3, 8s Gen 3, G3x Gen 2
| 🔴 | **Unsupported** | **Adreno A8xx  / Others** | Snapdragon 8 Elite, SD 835, Mali, Xclipse

### Emulator Compatibility

| Status | Emulator | Compatibility / Notes |
| :---: | :--- | :--- |
| 🟢🟢 | **Eden / Eden Nightly / Citron / Other Yuzu Forks** | Fully supported; primary target for Switch emulation optimizations. |
| 🟢 | **ARMSX2 / Azahar (and other Citra forks)** | Compatible; stable performance and minor rendering issues. |
| 🟡 | **Vita3k / WatermelonDS / HakuX / WinNative** | Compatible; Needs more testing to move to 🟢 status. |
| 🟠 | **ARMSX3 / Cemu / Dolphin** | Lightly tested; Titles may exhibit rendering glitches. |
| ⚪ | **Winlator / Gamehub / Other Windows Compatibility Layers** | Untested on current builds. |
