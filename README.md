# Installing and Troubleshooting Cursor on Ubuntu
I attempted to install Cursor, and my Ubuntu system broke two times due to issues with installing FUSE. Recovering the system was time-consuming and required considerable effort. I'm writing this guide to help you avoid the same mistakes and save valuable time. By carefully following these steps, hopefully, you'll be able to install Cursor without compromising your Ubuntu system.
## 1. Why the Issue Happens

FUSE (Filesystem in Userspace) allows non-privileged users to create file systems. Ubuntu, by default, uses Fuse3, which is critical for the GNOME desktop environment. Running the command `apt-get install fuse` can break your system as it installs the wrong version, leading to boot failures or a non-responsive desktop. The safer option is to use libfuse2 instead of Fuse3 to avoid conflicts.

## 2. How to Fix It

### If You Can Boot Normally:

Reinstall the Ubuntu Desktop (to reset it to default):

```bash
sudo apt install ubuntu-desktop
```

### Steps to Resolve Boot Failure:

1. Boot into Recovery Mode:
   - Press Esc during boot.
   - In the GRUB menu, select Advanced Options for Ubuntu.
   - Pick a kernel version with (Recovery Mode).
   - Choose Root from the recovery menu for terminal access.

2. Start Network Manager:
   ```bash
   systemctl start NetworkManager
   ```

3. Reinstall the Ubuntu Desktop:
   ```bash
   apt install ubuntu-desktop
   ```

4. Reboot the system:
   ```bash
   reboot
   ```

## 3. Installing Cursor on Ubuntu

1. Download Cursor:
   - Go to [cursor](https://www.cursor.com/pricing) and download the .AppImage file for Linux.
   - Optionally, rename the file to cursor.AppImage for simplicity:
     ```bash
     mv cursor-0.8.5.AppImage cursor.AppImage
     ```

2. Make the AppImage Executable:
   ```bash
   chmod +x cursor.AppImage
   ```

3. Run Cursor:
   ```bash
   ./cursor.AppImage
   ```

## 4. Fixing the FUSE Issue

If you get an error about fuse when running the AppImage:

1. Do not install FUSE, as it may break your system. Instead, install libfuse2:
   ```bash
   sudo apt-get install libfuse2
   ```

2. Move Cursor to a Different Folder:
   ```bash
   sudo mv cursor.AppImage /opt/cursor.appimage
   ```

3. Run Cursor from /opt:
   ```bash
   cd /opt
   ./cursor.appimage
   ```

If it works, proceed to the next steps. If you encounter a sandboxing issue, follow the method below.

## 5. Creating a Desktop Entry for Cursor

1. Download a Logo:
   - Download a logo for Cursor, name it cursor.png, and move it to /opt:
     ```bash
     sudo mv cursor.png /opt/cursor.png
     ```

2. Create a Desktop Entry:
   ```bash
   sudo nano /usr/share/applications/cursor.desktop
   ```

3. Add the Following Details:
   ```
   [Desktop Entry]
   Name=Cursor
   Exec=/opt/cursor.appimage
   Icon=/opt/cursor.png
   Type=Application
   Categories=Development;
   ```
   Save and exit (Press Ctrl + X, then Y, then Enter).

## 6. Fixing the Sandboxing Issue

If you encounter errors related to the sandbox when trying to run the AppImage:

1. Extract the AppImage:
   Navigate to /opt and extract the AppImage:
   ```bash
   cd /opt
   ./cursor.appimage --appimage-extract
   ```
   This creates a directory called squashfs-root containing all necessary files.

2. Navigate to the Extracted Directory:
   ```bash
   cd squashfs-root
   ```

3. Make AppRun Executable:
   ```bash
   chmod +x AppRun
   ```

4. Run the Application:
   ```bash
   ./AppRun
   ```

## 7. Fixing Sandboxing Permissions

If you still get errors related to sandboxing, follow these steps:

1. Change Ownership of chrome-sandbox:
   ```bash
   sudo chown root:root /opt/squashfs-root/chrome-sandbox
   ```

2. Set Correct Permissions:
   ```bash
   sudo chmod 4755 /opt/squashfs-root/chrome-sandbox
   ```

3. Run the Application Again:
   ```bash
   ./AppRun
   ```

## 8. Creating a Desktop Entry for Extracted Cursor

If the app works after fixing sandboxing issues, create a desktop entry for the extracted version:

1. Create a Desktop Entry:
   ```bash
   sudo nano /usr/share/applications/cursor.desktop
   ```

2. Add Entry Details:
   ```
   [Desktop Entry]
   Name=Cursor
   Exec=/opt/squashfs-root/AppRun
   Icon=/opt/cursor.png
   Type=Application
   Categories=Development;
   ```
   Save and exit (Ctrl + X, then Y, then Enter).

Now, you should have a fully functional installation of Cursor on your Ubuntu system! Start developing and enjoy the power of Cursor AI!

## Conclusion:

After facing issues with the installation of Cursor and breaking my Ubuntu system twice, I finally resolved the problems through troubleshooting and learned valuable lessons along the way. Initially, I was on Ubuntu 22.04, but during the recovery process, I upgraded to Ubuntu 24.04.1 LTS. By following the steps outlined in this guide, I was able to successfully install and run Cursor without compromising my system. Hopefully, this guide will help others avoid the same pitfalls and save time when installing Cursor on Ubuntu.  
If you have any questions or feedback, or if you found this documentation helpful, feel free to reach out to me on [LinkedIn](https://www.linkedin.com/in/muhammad-bilal-a75782280/). I’m always happy to connect and assist others in the community!
