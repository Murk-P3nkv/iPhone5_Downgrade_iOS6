# Custom iOS 6.1.4 IPSW for iPhone 5,2 (A6) Without SHSH

> **Disclaimer:**  
> This guide is experimental and intended for educational purposes only. Modifying and flashing custom firmware may brick your device. Use at your own risk.

## Overview

This guide explains the steps to create a custom iOS 6.1.4 IPSW that bypasses SHSH checks. The process involves:
1. Unpacking the original iOS 6.1.4 IPSW.
2. Extracting and patching the boot chain components (iBoot and iBEC).
3. Reassembling the custom IPSW.
4. Flashing the custom IPSW using pwned DFU.

## Requirements

- **Device:** iPhone 5,2 (A6)
- **OS:** macOS or Linux (Ubuntu/Debian). Windows is not recommended.
- **Tools:**  
  - [ipwndfu](https://github.com/axi0mX/ipwndfu) (for entering pwned DFU)  
  - [libimobiledevice](https://libimobiledevice.org/), `idevicerestore`, and `irecovery`  
  - [img4tool](https://github.com/tihmstar/img4tool)  
  - [xpwntool](https://github.com/planetbeing/xpwntool)  
  - [iBootPatcher](https://github.com/sbingner/iBoot64Patcher) (or a similar patcher for iBoot)  
- **Original IPSW:** iPhone5,2_6.1.4_10B350_Restore.ipsw

## Installation of Required Tools

### On macOS

1. **Install Homebrew** (if not already installed):
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

