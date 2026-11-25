# SOAR Challenge 2026 Template
hihi, here's a cool code template for SOAR Challenge '26 I think

**Note**: the instructions for development via CLI are included for reference only. They should be accurate, but we cannot help you troubleshoot during the software workshop; be prepared to debug on your own. Use Visual Studio Code + the ESP-IDF extension if you want support.

## Prerequisites
### Visual Studio Code (VSCode)
- PC (Windows, macOS or Linux)
- Visual Studio Code (duh)
- 1x robot kit
- 1x micro-USB cable

### Command Line (CLI)
- PC (Windows, macOS or Linux)
- Any code or text editor
- 1x robot kit
- 1x micro-USB cable

## Installing ESP-IDF
### VSCode
Follow the steps in [this guide](https://docs.espressif.com/projects/vscode-esp-idf-extension/en/latest/installation.html#).

### CLI
Follow the steps as per your operating system:
- [Windows](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/get-started/windows-setup.html)
- [macOS](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/get-started/macos-setup.html)
- [Linux](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/get-started/linux-setup.html)

### After installing ESP-IDF
Once done, **check** that Git and Python were installed during this process; if not, you will need to do it manually. (Python 3.10+ is required to work with ESP-IDF.)

## Flashing to ESP32
### VSCode
1. Clone this repository from GitHub and open it in Visual Studio Code.
2. Modify `robot_main.cpp` to add the following:
    1. Autonomous code in `auton()`.
    2. Driver control code in `opcontrol()`.
3. Open the SDK Configuration editor in the ESP-IDF extension menu.
4. Enable `WebSocket server support` (under Component config > HTTP Server) and press Save.
5. Open the ESP-IDF Terminal in the ESP-IDF extension menu.
6. Once you've entered the virtual environment (you should see `(venv)` in your command-line prompt), run `idf.py flash`.

### CLI
1. Clone this repository from GitHub and note the path.
2. Open the IDF Terminal.
    1. For EIM GUI,
        1. Open the ESP-IDF Installation Manager `eim`.
        2. Under `Manage Installations`, click `Open Dashboard`.
        3. Select your desired ESP-IDF version (ideally the latest available) and click `Open IDF Terminal` to launch a terminal session with the ESP-IDF environment activated.
    2. For EIM CLI,
        1. On Windows, click the `IDF_vX.Y.Z_Powershell` shortcut on your desktop to open a PowerShell session with the environment activated (where `X.Y.Z` is the version number).
        2. On Linux or macOS, you should see a command to activate the ESP-IDF environment after successfully installing EIM CLI, such as `source "/Users/username/.espressif/tools/activate_idf_vX.Y.Z.sh"`. Run said command in a terminal session.
3. Navigate to the directory where you cloned this repository.
4. Run `idf.py set-target esp32s3` to set your build target to ESP32-S3.
5. Run `idf.py menuconfig` to open the SDK Configuration menu.
6. Navigate to `Component config` > `HTTP Server`.
7. Ensure `WebSocket server support` is enabled (it should read `[*] WebSocket server support`, with the asterisk between the square brackets).
8. Save and quit the SDK Configuration menu (`s` > `q`), returning to your terminal window.
9. Run `idf.py flash` to build and flash the ESP32-S3 binary image to your board.
    1. If you plugged in the USB cable to your ESP32 (in the **USB** port on the board), then specifying the port isn't necessary; `idf.py` will automatically detect it for you.


## Troubleshooting
#### How do I check if the ESP-IDF extension installed correctly?
Open Command Palette (shortcut `F1`, or View > Command Palette) and run `ESP-IDF: Doctor Command`. 

#### My board is flashing different colors and won't let me flash!
Hold the **BOOT** button and press **RESET**, then release **BOOT**. Once you've confirmed the light has stopped, try flashing again.

#### It says 'no serial ports found'!
Check the USB connection between your computer and the ESP32 board. You should connect to the **USB** port on the board, not the UART port, even though they both use micro-USB connectors.

Try not to connect multiple boards at a time, else you'll have to specify the port since `idf.py` won't know which board you want to flash to.

#### My code builds and flashes, but doesn't run properly on the board!
Reconnect your ESP32 to your computer via the USB cable and monitor your board's output logs.
1. For Visual Studio Code users, navigate to your ESP-IDF extension menu and click `Monitor Device`. (You can also run `ESP-IDF: Monitor Device` via the Command Palette.)
2. For EIM GUI/CLI users, open an IDF terminal and run `idf.py monitor`.

The issue should become apparent in the logs. For this, we recommend you write clear messages and use the appropriate method for information, warning and error logs.

#### How do I exit the monitor session?
Press `Ctrl+]` (`Ctrl+T` then `Ctrl+X` if your hand can't span that far) or mash your keyboard interrupt key combination (`Ctrl+C` on Windows) until the logs stop printing.

#### In my code editor, CMake shows an error in the main folder's CMakeLists.txt!
It should read similar to the following:

```
    cmake_minimum_required(VERSION 3.16)
    include($ENV{IDF_PATH}/tools/cmake/project.cmake)
    project(soar_challenge_26)
```

`soar_challenge_26` may be replaced by any project name and isn't important. If your CMakeLists follows this, then check if the error is caused by `IDF_PATH` not resolving. You will see a message after hovering on the red squiggle, similar to:

```
    CMake Error at CMakeLists.txt:4 (include):
    include could not find requested file:

    /tools/cmake/project.cmake
```

If so, ignore the error. This will not affect you.

(*If you're curious, IDF_PATH doesn't exist in your system's environment variables until you manually add it; CMake isn't wrong. But the necessary variables are injected when we activate our ESP-IDF venv or shell profile, allowing us to build and flash.*)