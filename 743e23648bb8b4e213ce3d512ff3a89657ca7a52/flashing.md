# Flashing Mux Controller Firmware

> Mux Board Revision: **0.4**  
> MCU: **M5STAMP-C3 K056**

?> The mux controller firmware can also be set to target the Arduino MEGA 2560 for debugging and/or development purposes, but this setup has not been tested and is not documented.

If you do not have **PlatformIO** installed in **Visual Studio Code**:

1. Download, install, and open VS Code.

1. Bring up the Extensions view by clicking on the Extensions icon in the **Activity Bar** on the left side of the VS Code window or the **View: Extensions** command (`Ctrl`+`Shift`+`X`).

  ?> Press `Ctrl`+`Shift`+`P` to show the **Command Palette**. Begin typing `View: Extensions`, then press `Return` when the correct command is highlighted to invoke the command.

1. Begin typing `PlatformIO` in the Extensions search field, then click the `Install` button at the bottom-right of the "PlatformIO IDE" list item once it shows up.

1. Allow PlatformIO some time to set itself up. Click `Reload Now` when prompted to do so by the notification in the bottom-right of the VS Code window after the installation finishes.

---

The following procedures assume you have **PlatformIO** installed in **Visual Studio Code**.

1. In VS Code, open the PlatformIO home screen (switch to the PlatformIO view in the **Activity Bar**, and then click `Open` under `PIO Home`).

1. Click `Open Project`.

1. Navigate to the `~/Documents/PlatformIO/Projects/Mux Controller` folder (click `Projects` under `Places` in the sidebar of the dialog that pops up after you click `Open Project`, then click `Mux Controller`).

1. Click `Open "Mux Controller"`.

  ?> If you recently opened the "Mux Controller" project, you can simply click `Open` under the `Actions` column for the "Projects\Mux Controller" entry of the `Recent Projects` section at the bottom of the `PIO Home` page.

1. Click the `main.cpp` source file under the `src` subfolder to open it in the editor.

1. Edit line 28 to assign a unique hostname to the device being flashed (for example, `#define HOSTNAME "mux3"`). At the time of writing, "mux1" and "mux2" are already in use.

1. (Optional) Click `Default (Mux Controller)` in the **Status Bar** at the bottom of the VS Code window to switch to a different PlatformIO Project Environment and explicitly specify a platform to target (M5STAMP-C3 or Arduino MEGA 2560). The Mux Controller project is configured to target the M5STAMP-C3 by default.

1. Connect the target MCU to the computer via USB.

  !> Remove the M5STAMP-C3 from the mux board before starting the upload procedure! The MCU may not boot into the correct programming mode if it is still seated in its headers on the mux board when the uploader prepares the MCU for flashing, causing the upload to fail.

1. Click the `➜` icon in the **Status Bar** at the bottom of the VS Code window to upload the firmware to the MCU. PlatformIO will automatically recompile any modified source files before uploading.
