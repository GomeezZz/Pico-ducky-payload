# Pico Ducky: Ethical Hacking Payload 🦆

This repository contains a DuckyScript payload designed for a Raspberry Pi Pico configured as a USB Rubber Ducky. The script automates a series of actions on a Windows system for learning and demonstration purposes within a controlled, ethical hacking environment.



---

## ⚠️ Warning! Ethical and Educational Use Only

This script is designed **exclusively for educational and research purposes** in controlled and authorized environments.

* Using this code on systems without the owner's explicit written consent is **illegal and unethical**.
* The author is not responsible for any misuse of this tool.
* Use it at your own risk and always within legal boundaries.

---

## ⚙️ Device Setup

For this payload to work, the Raspberry Pi Pico must be flashed with **CircuitPython firmware** and configured to act as a Human Interface Device (HID), emulating a keyboard.

The entire initial hardware setup is based on the excellent work by dbisu. To prepare your Pico, follow the detailed instructions in their repository:

➡️ **[dbisu/pico-ducky on GitHub](https://github.com/dbisu/pico-ducky)**

---

## 📜 Payload Analysis (`payload.dd`)

The script is written in DuckyScript and performs the following actions sequentially when connected to a target Windows machine:

* **Initial Delay**: Waits for 3 seconds to ensure the operating system has recognized the device.
* **Open PowerShell**:
    * Opens the "Run" dialog (`Win + R`).
    * Types `powershell` and executes it to start a new session.
* **WiFi Password Extraction**:
    * Runs a `netsh` command to list all saved WiFi profiles on the machine.
    * It iterates through each profile and extracts its password in clear text.
    * All the information is saved to a temporary file named `wifi_passwords.txt` in the `$env:TEMP` directory.
* **Data Exfiltration**:
    * Uses `Invoke-WebRequest` to send the contents of the `wifi_passwords.txt` file via a `POST` request to an attacker-controlled server. The example script uses an OAST (Out-of-band Application Security Testing) endpoint.
* **Reverse Shell Download**:
    * Again, uses `Invoke-WebRequest` to download an executable (in this case, `nc64.exe`) from a local web server and saves it to the temp directory.
* **Reverse Shell Execution**:
    * Starts the `nc64.exe` process in a hidden window (`-WindowStyle Hidden`).
    * The process establishes a reverse shell connection back to the attacker's machine, providing access to a `cmd.exe` terminal on the victim's system.
* **Clean Up Traces**:
    * To avoid leaving evidence, it deletes the files created during execution: `wifi_passwords.txt` and `nc64.exe`.
* **Exit Session**:
    * Types `exit` to close the PowerShell window and end the script.

---

## 🚀 Usage

1.  **Set up your Pico**: Make sure you have followed all the steps from the `pico-ducky` repository.
2.  **Customize the Payload**: Edit the `payload.dd` file with your own URLs, IP addresses, and ports.
3.  **Copy the Payload**: Drag and drop the modified `payload.dd` file into the root directory of your Raspberry Pi Pico (it will appear as a USB drive).
4.  **Execute**: Plug the Pico into the target machine. The payload will run automatically.
