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

## Original Espressif IDF

The github page has an explanation how to install: https://github.com/espressif/vscode-esp-idf-extension

First, I installed the extension "Espressif IDF" into my VS Code, using the (Ctrl+Shift+X) keys, searching for "ESP32".
Then, it says, I need to install the prerequisites. 

### Installing prerequisites for Espressif IDF (actually: install everything)
Doing so can either be done manually, or by using the all-in-one
Windows installer. The all-in-one way to install the prerequisites is available at:
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

The download is "only" 760 MB instead of the 1.3 GB of the full installation. Yey!

After installation, I confirmed everything but did not register the ESP-IDF tools as Windows Defender exclusions,
to avoid a risk of computer compromise. If things are too slow later, I can run the idf-env tool (look at github.com/espressif/idf-env).
I received two windows, one for cmd.exe and one for powershell, that looked promising:

![image](https://user-images.githubusercontent.com/11603870/149660398-fd2fc8b1-726b-474f-ae6d-02b622e07d8c.png)


![image](https://user-images.githubusercontent.com/11603870/149660405-ee62b7c9-3004-4597-ab41-ace41235db58.png)

The installer essentially does steps 1..4 of the https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/ explanation.
Now, I am ready to start a project right away, without VS Code.

### Detour: running a project without VS Code

**Importantly, the PATH variable is not set system-wide. So, if I just start cmd.exe, the python script "idf.py", which does
basically everything, is unavailable. I have to select the "ESP IDF CMD4.3" shell from the start menu of Windows.**

The environment variables used in the documentation page of Espressif are %IDF_PATH% and %userprofile%. I figured out that
%IDF_PATH% is "C:\Users\Thomas\Desktop\esp-idf\" (default from the installation script), and that %userprofile% is apparently
just the "C:\Users\Thomas". Since the latter is used to store my projects, I am basically free to set it whereever I want,
and to make later deletion easier, I created this "esp" directory in the same directory where the %IDF_PATH is. Later,
I shall rename it into "my-projects" or similar (there are a lot of warnings that some directory names may not contain whitespace,
so I better user "my-projects" and not "my projects").

I did a "xcopy /e /i c:\Users\Thomas\Desktop\esp-idf\examples\get-started\hello_world hello-world" at "C:\Users\Thomas\Desktop\esp-idf\esp".

In Device Manager, under "Ports (COM & LPT)" I checked for the COM port of my ESP32-DevkitC (COM4 in my case). Note that the
system did not find the ESP32 if connected to a USB3 port, but only when it was connected to an old USB2 port!

Then, I launched `idf.py set-target esp32`. This took a long while and created a bunch of build files in my hello-world 
directory.
Then, I launched `idf.py menuconfig`. And yes, as promised, the configurator popped up:

![image](https://user-images.githubusercontent.com/11603870/149661507-c44f6eb9-3ce9-45ca-9afb-2deee18d7112.png)

Since the hello world project does not require any configuration I quit it without changing anything.

Then, I tried `idf.py build`, which took surprisingly long for a program that does essentially nothing (a full minute or so?).
When running it again, it noticed that there was nothing to be done, and finished immediately.

Finally, I flashed it using `idf.py -p COM4 flash`, and then tested using `idf.py -p COM4 monitor` which gives me the info that
my spi flash memory is larger than I believed, and that I have a 2-core ESP32:

![image](https://user-images.githubusercontent.com/11603870/149662538-65bd44a0-6381-4b57-88b3-1567c2a88504.png)

To fix it, I ran `menuconfig` again, and in Serial flasher config I changed the Flash size from 2 MB to 8 MB, and recompiled.
Again, it took ages. After flashing and running again, the warning about the wrong size of the spi flash memory disappeared.
Success!

Finally, I changed the hello world program to say "Hello Lecostarius" instead. The compile process took only a second or so, good.
And the thing actually printed Hello Lecostarius. So, creating all the other files (like bootloader etc) takes 95% of the time
for compilation and if I only change the user code, turnaround times are OK!

### Back to the VS Code plugin









