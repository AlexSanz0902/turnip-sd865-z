# Turnip SD865 "Z"

A modified Turnip (Mesa) Vulkan driver for **Snapdragon 865 / Adreno 650**, tested on a Retroid Pocket 5 with Switch emulation (Eden). Built on Mesa `main` (commit `bffd91947efc`), it keeps emulator render targets compressed, uses cheaper texture filtering and direct rendering, and fixes swapped red and blue colors. In the games tested it ran 15-35% fps faster than stock Turnip, with less crashes.

**Download:** [latest release](../../releases/latest). Install the zip as it is from the emulator's GPU driver manager.

Tested on Retroid Pocket 5; use at your own risk.


## FAQ

### What is the best way to use it?
It was tested on Eden version 0.2.0-rc2 with the GPU overclocked to 905 MHz. 
Keep in mind that certain games require specific emulator settings, engine patches, or workarounds to boot and render properly; 
that has nothing to do with the driver.

### Does it work on other devices?
It is built for the Adreno 650 (Snapdragon 865). Other devices with this GPU should work; other Adreno architectures and revisions are untested.

### Is it suitable for non-Switch emulators or Windows layers (Winlator, GameHub, Dolphin, etc.)?
No. Do not use it outside of Eden (version 0.2.0-rc2 highly recommended) . It was neither designed nor verified for PC translation layers (Winlator, GameHub) or other console emulators (Vita3K, Azahar, Dolphin). Using this build elsewhere will likely result in crashes or rendering regressions.
