Reference to create VSCode (win) environment connected to WSL with USB/IP connectivity

- [1. Install WSL distro](#1-install-wsl-distro)
- [2. Install VS Code in Windows](#2-install-vs-code-in-windows)
- [3. Install VS Code Remote Development extension pack](#3-install-vs-code-remote-development-extension-pack)
- [4. Install usbipd-win](#4-install-usbipd-win)
- [5. Setup USB-IP in WSL](#5-setup-usb-ip-in-wsl)
- [6. Add user to dialout group](#6-add-user-to-dialout-group)
- [7. Share USB device with WSL](#7-share-usb-device-with-wsl)
- [8. References](#8-references)


# 1. Install WSL distro
Assume WSL2 already installed.
```
> wsl.exe --install Ubuntu
```

# 2. Install VS Code in Windows

- [VS Code install page](https://code.visualstudio.com/download)

# 3. Install VS Code Remote Development extension pack



- [Remote Development extension pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)
  
This extension pack includes the WSL extension, enabling you to open any folder in WSL (from VS Code running in Windows).

Ref: [Get started using Visual Studio Code with Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode)

# 4. Install usbipd-win
To share USB devices with WSL, both Windows and WSL need changes.

In Windows, install [usbipd-win](https://github.com/dorssel/usbipd-win/releases).

# 5. Setup USB-IP in WSL

Within WSL, install the user space tools for USB/IP and a database of USB hardware identifiers. See [WSL-setup](https://github.com/dorssel/usbipd-win/wiki/WSL-support#wsl-setup).


# 6. Add user to dialout group

To avoid running as root to access USB serial device.

```
$ sudo usermod -a -G dialout $USER
```
Login again.

# 7. Share USB device with WSL
Start wsl to keep VM active
```
> wsl.exe -d Ubuntu
```

In a separate command window (as Administrator)..

```
>usbipd wsl list
BUSID  VID:PID    DEVICE                                                        STATE
2-4    10c4:ea60  CP2104 USB to UART Bridge Controller


>usbipd wsl attach --busid 2-4
usbipd: info: Using default WSL distribution 'Ubuntu'; specify the '--distribution' option to select a different one.
```

Should be visible in WSL now.

```
$ lsusb | grep CP210
Bus 001 Device 002: ID 10c4:ea60 Silicon Labs CP210x UART Bridge
```


# 8. References

- [Setup WSL](https://learn.microsoft.com/en-us/windows/wsl/setup/environment)
- [Connecting USB devices to WSL](https://devblogs.microsoft.com/commandline/connecting-usb-devices-to-wsl/)
- [usbipd-win project on Github](https://github.com/dorssel/usbipd-win)
- [usbipd-win WSL support wiki](https://github.com/dorssel/usbipd-win/wiki/WSL-support)

