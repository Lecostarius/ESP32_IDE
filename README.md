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

https://dl.espressif.com/dl/esp-idf/?idf=4.4
