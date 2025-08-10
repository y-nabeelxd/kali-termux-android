# Kali Linux on a Non-Rooted Android Device

A comprehensive, step-by-step guide to installing a full Kali Linux environment on your Android device without rooting, using Termux and NetHunter KeX.

## Table of Contents

- [Introduction](#introduction)
- [Important Disclaimer](#important-disclaimer)
- [Prerequisites: Before You Begin](#prerequisites-before-you-begin)
- [Installation Guide](#installation-guide)
  - [Part 1: Installing the Necessary Android Apps](#part-1-installing-the-necessary-android-apps)
  - [Part 2: Installing Kali Linux in Termux](#part-2-installing-kali-linux-in-termux)
  - [Part 3: Accessing Your Kali Linux Desktop](#part-3-accessing-your-kali-linux-desktop)
- [Post-Installation: What's Next?](#post-installation-whats-next)
- [Troubleshooting & FAQ](#troubleshooting--faq)
- [Screenshots](#screenshots)
- [Conclusion](#conclusion)

## Introduction

Welcome! This guide will walk you through the entire process of transforming your Android device into a portable hacking station with Kali Linux. By leveraging the power of Termux (a terminal emulator) and NetHunter KeX (a remote desktop client), you can run a full Kali desktop environment directly from your phone, without needing to root it. This gives you access to Kali's powerful suite of security and penetration testing tools on the go.

This process involves installing a full operating system, so please read the instructions carefully and be patient, as some steps may take a while to complete.

## Important Disclaimer

* **Storage Space:** A full Kali Linux installation requires a significant amount of storage (typically 8-12 GB). Ensure you have enough free space before you begin.
* **Battery & Performance:** Running a full Linux desktop is resource-intensive. It will consume more battery and may cause your device to heat up.
* **Data Usage:** The initial download of Kali Linux is large (over 1.5 GB). It is **highly recommended** to be connected to a stable, unmetered Wi-Fi network.
* **For Educational Use:** This setup is intended for educational purposes and security research. Always have permission before performing any network testing.

## Prerequisites: Before You Begin

Make sure you have the following ready before starting the installation:

-   ✅ **A modern Android device:** (Android 7.0 or higher is recommended).
-   ✅ **At least 15 GB of free storage space.**
-   ✅ **A stable and fast Wi-Fi connection.**
-   ✅ **Patience!** The main installation can take anywhere from 30 minutes to over an hour, depending on your device and internet speed.

## Installation Guide

Follow these three parts in order.

### Part 1: Installing the Necessary Android Apps

First, we need to install the applications that will host and display our Kali environment.

1.  **Install the Kali NetHunter App Store:**
    * Using a browser on your Android device, navigate to the official NetHunter Store website: **[store.nethunter.com](https://store.nethunter.com)**.
    * Download the App Store APK file.
    * Open the downloaded APK. You will likely need to grant your browser permission to "install unknown apps". This is safe as it's from the official Kali source.
    * Complete the installation.

2.  **Install Termux and NetHunter KeX:**
    * Open the **NetHunter Store** app you just installed.
    * Use the search bar to find **Termux**. Install the result named **"Termux (terminal emulator with packages)"**.
    * Go back to the search bar and find **NetHunter KeX**. Install it.

> **Note:** It is crucial to install Termux from the NetHunter Store (or F-Droid) and not the Google Play Store, as the Play Store version is outdated and no longer maintained.

### Part 2: Installing Kali Linux in Termux

Now we will use Termux to download and set up the core Kali Linux command-line interface (CLI).

1.  **Update Termux Packages:**
    Open the **Termux** app. A command prompt will appear. First, update and upgrade the built-in packages to ensure everything is current.
    ```bash
    pkg update && pkg upgrade
    ```
    > This command first (`update`) fetches the latest list of available packages and then (`upgrade`) installs the updates for any outdated packages you have. Press `Y` if prompted.

2.  **Grant Storage Access:**
    Allow Termux to access your device's storage. This is necessary for downloading and storing the Kali image.
    ```bash
    termux-setup-storage
    ```
    > A popup will appear asking for permission. Tap "Allow".

3.  **Install `wget`:**
    We need the `wget` utility to download files from the internet.
    ```bash
    pkg install wget
    ```

4.  **Download the NetHunter Installation Script:**
    This command downloads the official script that will install Kali NetHunter for Termux.
    ```bash
    wget -O install-nethunter-termux [https://offs.ec/2MceZWr](https://offs.ec/2MceZWr)
    ```

5.  **Make the Script Executable:**
    By default, downloaded files don't have permission to run. We need to change that.
    ```bash
    chmod +x install-nethunter-termux
    ```

6.  **Run the Installation Script:**
    This is the main installation step. It will download the Kali Linux image and extract it. **This will take a long time.**
    ```bash
    ./install-nethunter-termux
    ```
    > The script will download the rootfs (root file system), which is a few gigabytes. It will then extract it. Just let it run. If it seems stuck, give it time. Once it's finished, you will be back at the Termux prompt.

### Part 3: Accessing Your Kali Linux Desktop

With Kali installed, let's launch the graphical desktop.

1.  **Start the Kali NetHunter CLI:**
    Type the following command in Termux to enter the Kali environment.
    ```bash
    nethunter
    ```
    > Your prompt will change from `$` to `kali@localhost:~$`. You are now inside Kali!

2.  **Create a KeX Password:**
    You need to set a password to secure your desktop session. This is **not** your device password.
    ```bash
    nethunter kex passwd
    ```
    > You will be prompted to enter and then verify a new password. Choose something you will remember.

3.  **Start the KeX Server:**
    This command starts the graphical server that the KeX app will connect to.
    ```bash
    nethunter kex &
    ```
    > The `&` at the end runs the process in the background, so you can still use the terminal. Note the display number and port that appears (it's usually `:1` on port `5901`).

4.  **Connect with the NetHunter KeX App:**
    * Open the **NetHunter KeX** app.
    * The address and port should be pre-filled (e.g., `127.0.0.1` and `5901`). If not, enter them.
    * Enter the KeX password you created in step 2.
    * Tap **Connect**.

**Congratulations!** You should now see the full Kali Linux XFCE desktop environment running on your device.

## Post-Installation: What's Next?

* **Updating Kali:** To keep your tools up-to-date, periodically run these commands inside the `nethunter` CLI:
    ```bash
    sudo apt update
    sudo apt full-upgrade -y
    ```
* **Installing New Tools:** To install a tool (e.g., `neofetch`), use the `apt` package manager:
    ```bash
    sudo apt install neofetch
    ```
* **Stopping the KeX Server:** To safely shut down the graphical desktop and save battery, go to the Termux window where you started the server and run:
    ```bash
    nethunter kex stop
    ```
* **Exiting Kali:** To exit the Kali CLI and return to the normal Termux shell, simply type:
    ```bash
    exit
    ```

## Troubleshooting & FAQ

-   **Problem:** The `install-nethunter-termux` script fails or the download is very slow.
    -   **Solution:** Your internet connection may be unstable. Try switching to a different, more reliable Wi-Fi network. You can try running the script again; it should resume the download if possible.

-   **Problem:** The NetHunter KeX app says "Connection Refused".
    -   **Solution:** Ensure the KeX server is running. Go to Termux and type `nethunter kex &`. If it was already running, try stopping it (`nethunter kex stop`) and starting it again. Also, double-check the port number.

-   **Problem:** Termux closes when running in the background.
    -   **Solution:** Your phone's battery optimization might be killing the app. Go to your Android Settings -> Apps -> Termux -> Battery, and set the battery usage to "Unrestricted".

-   **Question:** Can I delete the downloaded rootfs file after installation?
    -   **Answer:** The script should automatically remove the downloaded archive after successful extraction. If you see a large file ending in `.tar.xz` in your Termux home directory after everything is working, you can manually remove it with `rm filename.tar.xz`.

## Screenshots

Here are placeholders for what you should see during the process:

-   *(Screenshot: The NetHunter App Store main screen)*
-   *(Screenshot: Termux after running the initial update command)*
-   *(Screenshot: The `wget` command downloading the installation script)*
-   *(Screenshot: The installation script running and extracting the rootfs)*
-   *(Screenshot: The NetHunter KeX app connection screen)*
-   *(Screenshot: The final, running Kali Linux desktop on Android)*

## Conclusion

You have successfully installed a powerful and flexible security research tool on your Android device. This setup provides a fantastic platform for learning about cybersecurity, practicing ethical hacking techniques, and exploring the depths of a Linux environment. Remember to use these tools responsibly and continue learning. Happy hacking!
