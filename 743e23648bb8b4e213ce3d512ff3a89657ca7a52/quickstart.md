# User Guide

## Automated DAQ System

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

## Running an Automated Benchtop Test Program

1. Navigate to the `Test Engineer` user's `Documents` folder from the `Start Menu` or by using the `File Explorer`.

1. Double-click the folder named for the part number of the DUT (for example, `LT1234`).

1. **Right-click** the folder named for the test to run (for example, `ESD`).

1. Near the top of the right-click context menu, select `Open with Code`.

1. Looking at the root of the current working directory (this would be `~/Documents/LT1234/ESD` following the previous examples) in the left sidebar of Visual Studio Code, you might see more than one Python script/file with a `.py` extension; the file named in a similar format to `LT1234_Test_CLI.py` is the main test program. Click the main test program item in the left sidebar to open the program code in the editor.

1. Make any necessary changes to test parameters such as `dut_qty` and `start_dut` (if applicable) under the `BEGIN CONFIGURATION` section of the code (if this code block is not immediately visible in the editor window, please scroll down). For more detailed configuration help, please refer to the part-specific guides.

1. Prepare the testbench according to the testbench connection map or `SETUP.md` file in the `docs` subfolder (the full path for which would be `~/Documents/LT1234/ESD/docs` following the previous examples). Make sure the mux controller and all relevant benchtop instruments are connected and powered on.

    > 💡 You can quickly reveal a folder or file in `File Explorer` from `Visual Studio Code` by **right-clicking** the desired folder or file in `Visual Studio Code` and then clicking `Reveal in Explorer`.

1. Click the `▶` icon in the top-right of the `Visual Studio Code` window to begin running an instance of the test program. All ESD and HTOL test program instances will automatically create a new Excel workbook named in a similar format to `LT1234 2022-11-30 15-20.xlsx` under the `results` subfolder to store the acquired data.

    > ⚠️ Since results workbooks are named by the minute during which the test program instance is created, if the instance is stopped or killed and another instance of the test program is run within the same minute, the new instance will overwrite all data saved by the previous instance!

1. Once the prompt for command input appears in the `Terminal` (for example, `"Press return to continue"` or `"Command [continue]:"`), follow the onscreen instructions to test each DUT.

    > 💡 For certain parts' test programs, you may press `Ctrl`+`C` at any time while the program is running to cancel data acquisition for the current DUT, return to the command input prompt, and retry the test for that DUT. Please refer to the part-specific guides for whether this applies to you.  
    ⚠️ If your part's test program does not have this retry testing functionality, pressing `Ctrl`+`C` instantly kills the test program instance, effectively doing the same thing as killing the Terminal. If the instance is forcefully stopped while it is writing data to the results file, the results file may be corrupted and data will be lost!

1. When the test program is either commanded to `stop` or the number of devices tested has reached the configured `dut_qty` (if applicable), the test program will proceed to write a `Statistics` sheet and `All Devices` summary sheet and may reset or power off the benchtop instruments before the test program instance ends.

    > ⚠️ Since results workbooks are named by the minute during which the test program instance is created, if the instance is stopped or killed and another instance of the test program is run within the same minute, the new instance will overwrite all data saved by the previous instance!

