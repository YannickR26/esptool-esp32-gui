# ESP Flasher Programming Tool

A standalone GUI application for ESP firmware flashing compatible with Windows and macOS.

**Note:** Currently using esptool v4.5

![gui](/wingui.png "Description goes here")


## Installing

You can download prebuilt executable applications for Windows from the [releases section](https://github.com/YannickR26/esptool-esp32-gui/releases). These are self-contained applications and have no prerequisites on your system. They have been tested with Windows 11

## Usage

If you compile your project using make, the App and partition table binaries will be put in your /build directory. The bootloader binary is under /build/bootloader.bin

If the partition table has not been changed, it only needs to be reflashed when the ESP32 has been fully erased. Likewise the bootloader binary will not change between edits to your personal app code. This means only the App needs to be flashed each time

## Running From Source

**Note:** Currently using esptool v4.5

1. Install the project dependencies using your python3 package manager
2. Run the doayee_dfu.py script in python3

## Development

Python package:
- wxPython
- pyserial
- esptool
- pyinstaller

## Generate exe file

### Fix esptool path

Update the loader.py file line 100 with this:
```
if getattr(sys, 'frozen', False):
    # If the application is run as a bundle, the PyInstaller bootloader
    # extends the sys module by a flag frozen=True and sets the app 
    # path into variable _MEIPASS'.
    application_path = sys._MEIPASS
else:
    application_path = os.path.dirname(os.path.abspath(__file__))

STUBS_DIR = os.path.join(application_path, "./targets/stub_flasher/")
```

Change the `DEFAULT_RESET_DELAY` to 0.2 in the reset.py file

To build exe go to the folder with the .py files and run

```
pyinstaller --icon=logo.ico --windowed --add-data=".venv/Lib/site-packages/esptool/targets/stub_flasher/*;targets/stub_flasher/" --onefile --name "ESP_Flasher" doayee_dfu.py
```

The exe file will be created in a folder "/dist"
