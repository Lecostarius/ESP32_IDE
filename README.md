# ESP32_IDE
Steps to create a ESP32 development environment

Of course, there is the Arduino IDE plugin. I have already used it; it is very straightforward to use and completely
OK if you do not want to do anything too special. Since I would like to try the deep-sleep feature of the ESP32, I will
probably have to use the Espressif IDE. Big drawback: the vast majority of internet examples uses Arduino... so, possibly,
it is going to be easier to keep it there. Lets give it a try nonetheless.

I'd like to use the VSCode plugin. Reason: debugging is integrated, and I can use it on my laptop which has no Linux.
Second choice would be the cmd.exe based installation under Windows (to have it on my laptop).

# VSCode plugin vs VS Codium plugin
There is a plugin calles "Espressif IDF", and one called "PlatformIO IDE". The former is the official one from Espressif,
the latter is open source. The Open Source tool has 5 stars rating from 2244 users, and the official one has a 2.5 star
rating from 135000 users... 
Anyway, both are unavailable in VS Codium, since the official marketplace for VS Code can only officially be used by
VS Code. So VS Codium uses open-vsx.org as a marketplace. There are a few ways how VS Codium can access the official
marketplace (an extension to do so is https://dev.to/geopjr/vscodium-upgrading-the-extension-experience-1ko0, the VS Codium
github has an article https://github.com/VSCodium/vscodium/blob/master/DOCS.md#extensions--marketplace).
However, I decided to switch to VS Code in order to try whether the extension is worth it in the first place (and also
whether to use the official Espressif one or the one from PlatformIO).

# Original Espressif IDF

The github page has an explanation how to install: https://github.com/espressif/vscode-esp-idf-extension

First, I installed the extension "Espressif IDF" into my VS Code, using the (Ctrl+Shift+X) keys, searching for "ESP32".
Then, it says, I need to install the prerequisites. Doing so can either be done manually, or by using the all-in-one
Windows installer. The all-in-one way to install the prerequisites is a prerequisites-installer available at:
https://docs.espressif.com/projects/esp-idf/en/stable/esp32/get-started/windows-setup.html which points to
https://dl.espressif.com/dl/esp-idf/?idf=4.4. The idf=4.4 is pointless, you always get to the same location. At the
time of my download (Jan 2022) the Installer version 2.12 was available (22.11.2021). 
This thing installs GCC, binutils, GDB, OpenOCD, and KConfig Frontends.
When running, it complains that I do not have Long Paths Enables in my Windows registry:
![image](https://user-images.githubusercontent.com/11603870/149659874-ad86cbd0-50c3-4b17-981e-2b96603e4bcc.png)
From a administrator cmd.exe, I ran:
powershell -Command "&{ Start-Process -FilePath reg 'ADD HKLM\SYSTEM\CurrentControlSet\Control\FileSystem /v LongPathsEnabled /t REG_DWORD /d 1 /f' -Verb runAs}"
and then said "Apply Fixes" and restarted the installer. This time, it did not complain. I selected release version 4.3.2 (the
latest one) and did not change the suggested path although I did not like it very much (C:\Users\Thomas\Desktop\esp-idf).
It said it would install ESP-IDF tools into C:\Users\Thomas\.espressif.
I selected only ESP32 as Chip Target, chose FTDI Chip and CP210x driver support only, and disabled Eclipse integration. It said:
![image](https://user-images.githubusercontent.com/11603870/149660077-f77e42aa-afe8-443a-afc4-0b18b459a275.png)



