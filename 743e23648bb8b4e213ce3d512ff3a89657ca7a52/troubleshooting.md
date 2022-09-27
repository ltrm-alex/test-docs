# General Troubleshooting

> Mux Board Revision: **0.4**  
> MCU: **M5STAMP-C3 K056**

## Glossary

* **DAQ:** **D**ata **A**c**q**uisition
* **DMM:** **D**igital **M**ulti**m**eter
* **DUT:** **D**evice **U**nder **T**est
* **MCU:** **M**icro**c**ontroller **U**nit
* **Mux:** **Mu**ltiple**x**er
  * _Mux Board:_ PCB populated with up to two 16-channel muxes, two 24-pin IDC headers, and two output banana jacks
  * _Mux Controller:_ asymmetrical 22-pin M5Stamp-C3 development board built around the ESP32-C3 MCU
* **PSU:** **P**ower **S**upply **U**nit, DC Power Supply, Keithley 2230G-30-1 Triple Channel DC Power Supply
* **SMU:** **S**ource **M**easurement **U**nit, SourceMeter®, Keithley 2450 SourceMeter®

---

## Current Potential Problems

### Floating voltage readings

_The DMM input is not set correctly._

1. Make sure REAR is selected (pushed in) on the DMM front panel input switch.

### ERROR: Could not open requirements file: [Errno 2] No such file or directory: 'requirements.txt'

_The test program is not being executed in the correct directory._

1. In File Explorer, select the directory containing the test programs (for example, select the `HTOL` folder inside the `LTXXXX` folder).
1. Right-click the selected directory.
1. Click `Open with Code`.

### Supply voltage calibration not working

_The test program has lost its remote control session with the instrument being calibrated (this problem is more likely to occur with the PSU than the SMU)._

1. The current test program instance must be ended (or if a graceful stop is not possible, the Terminal session must be killed), and the program should be restarted.
1. To avoid this problem, do **NOT** exit remote mode by pressing the `Menu` button on the PSU front panel while in remote mode after a running test program has already taken control of the PSU! (The PSU is in remote mode when there is an antenna icon in the top-left corner of the front panel display, and the SMU is in remote mode when the green LED is illuminated in the top-right corner of the front panel.)

### Mux controller connection error

_The MCU is waiting for the DUT to respond over I2C, causing the MCU to hang/become unresponsive._

1. Make sure the `SDA` and `SCL` pins are connected properly, the DUT is inserted properly (if applicable), and the DUT is being powered on before attempting I2C communication.  
1. If the program fails to reconnect when retrying after 10 seconds, press `Ctrl` + `C` to give up on testing the current device and return to the `Command [continue]:` command prompt for the current device.
1. Unplug and reconnect the USB-C cable from the mux controller to restart it.
1. Once the mux controller's onboard LED flashes white to signal it is ready, press `Enter` to try running the test again. The program may throw an error about attempting to use a port that is not open, but this is expected; let it try to reconnect on its own.
1. If the program is still unable to reconnect after retrying twice, unplug the USB-C cable from the mux controller, remove the mux controller from the mux board, and plug the USB-C cable back into the mux controller. Then go to step 3.
1. If the mux controller is not seated in its headers on the mux board, you may now reseat it, press `Ctrl`+`C`, then press `Enter` to restart the test for the current device.

---

## Legacy (Early Development) Problems

### Mux output is unstable/floating

1. Make sure the mux control signals CTRL[4:0] are correct for the desired pin (HIGH at least 3.3V, etc.).
1. Make sure the ground connections are solid; more ground connections may help.
1. Try not to use the GPIO pins on H3; use the power supply or SourceMeter instead.

### MCU is not booting (or boots into the wrong mode)

1. Try powering on the HTOL board with 3.3V **before** powering on the MCU via USB-C.
1. Try powering on the HTOL board with 3.3V **after** powering on the MCU via USB-C.
1. Try unplugging the MCU from the mux board,  powering the MCU on via USB-C, then reseating the MCU in its headers on the mux board.
1. Try powering off the MCU (unplug USB-C), discharging the pins by touching them to GND, and then powering up the MCU.

### I2C communication is flaky

1. Try not to use the GPIO pins on H3; use the power supply or SourceMeter instead.

### Inconsistent voltage drop

1. The voltage drop from the PCB connectors (both banana jack and "gold fingers" edge connectors) to each site are slightly different. Most test programs will perform chip supply voltage calibration steps to mitigate this issue.

### VISA error (Tektronix DMM4040): Connection for given session has been lost

1. Re-run the test script.

### VISA error (Keithley 2450 SourceMeter): Insufficient system resources

1. Power cycle the SourceMeter.

### pyvisa.errors.VisaIOError: VI_ERROR_RSRC_NFOUND (-1073807343): Insufficient location information or the requested device or resource is not present in the system

1. The multimeter's IP address may have changed (its default self-assigned IP is `169.254.105.241`). Power cycle the multimeter by flipping the power switch next to the IEC 320 C13 power plug at the back of the instrument. Once the multimeter has restarted, on the instrument front panel, press the `INSTR SETUP`, `F1`, `F3`, and `F2` buttons in order. Replace the IP address in the variable `tektronix_dmmeter_ip` with the IP address displayed on the multimeter. Press the `BACK` button 4 times to exit the menu.
1. The SourceMeter's IP address may have changed (its default self-assigned IP is `169.254.165.210`). On the front panel, press the `MENU` button. Tap `Communication` under `System`, then tap the `LAN` tab. Replace the IP address in the variable `keithley_sourcemeter_ip` with the IP address displayed on the SourceMeter. Press the `HOME` button to exit the menu.
1. The DC power supply may have a bad USB connection. Unplug and replug the USB connector into the back panel of the PSU.
