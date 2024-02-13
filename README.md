# M5-Bruce
Firmware for Cardputer

## Name and Background
Forked project, same intentions B)
NEMO started a personal project to help me learn more about ESP32 development with the Arduino IDE. I decided to replicate a few common, trending pranks that were getting a lot of attention in the tech community, as a challenge to myself, and to also better understand these attacks.  
NEMO is named after the small, clever and stubborn fish in Finding Nemo. This project stands in contrast to another high-tech gadget that's associated with certain sea-dwelling creatures. I did want to prove that there are a lot of things you can do with a small development kit and some curiosity. I have no delusions of superseding the capabilities of any similar device with this project. It's just for fun, and my own education.  

Hello, my name is Bruce.

![M5Cardputer](https://github.com/pr3y/m5-bruce/blob/main/pic1.png)

![M5Cardputer Main Screen](https://github.com/pr3y/m5-bruce/blob/main/pic2.png)

![M5Cardputer Wifi Attacks](https://github.com/pr3y/m5-bruce/blob/main/pic3.png)

## Features
* [TV B-Gone](http://www.righto.com/2010/11/improved-arduino-tv-b-gone.html) port (thanks to MrArm's [HAKRWATCH](https://github.com/MrARM/hakrwatch)) to shut off many infrared-controlled TVs, projectors and other devices
* [AppleJuice](https://github.com/ECTO-1A/AppleJuice) iOS Bluetooth device pairing spam
* Bluetooth device notification spamming for SwiftPair (Windows) and Android
* WiFi EVIL Portal - A captive portal that tries to social engineer email credentials - saves usernames and passwords to SD Card (if inserted into a supported reader)
* WiFi SSID Scanner - Display 2.4 GHz SSIDs nearby and get information about them
* User-adjustable 24 Hour digital clock backed by the M5 Stick RTC so it holds relatively stable time even in deep sleep and low battery mode
* EEPROM-backed Settings for rotation, brightness and, automatic dimming
* Battery level and credits in settings menu
* SSH Client
* DPWO-ESP32
* Raw Sniffer - Saves PCAP to SD
* BadUSB - Reads payload on SD card /badpayload.txt
* Keyboard - Use as a keyboard USB input

## User Interface
There are three main controls:
* Home - Stops the current process and returns you to the menu from almost anywhere in BRUCE
* Next - Moves the cursor to the next menu option. In function modes, this usually stops the process and returns you to the previous menu.
* Select - Activates the currently-selected menu option, and wakes up the dimmed screen in function modes  

* Cardputer
  * Home: Tap the Esc/~/` key or the Left-Arrow/, key
  * Next/Prev: Tap the Down-Arrow/. key and Up-Arrow/; keys to navigate
  * Select: Tap the OK/Enter key or Right-Arrow/? key  

## EVIL Portal
In EVIL Portal mode, BRUCE reads the keyboard input for the SSID and activates a open WiFi, with DNS, DHCP and Web servers activated. 
* EVIL Portal serves a fake login page that claims to provide internet access if you log in.
* This is a social engineering attack, and will log the username and passwords entered on the page. 
* You can view these credentials by connecting to the portal from your own device and browsing to http://172.0.0.1/creds
* If your device has an SD Card reader with a FAT filesystem formatted card inserted, the usernames and passwords will be logged to evil-portal-creds.txt on the SD Card for you to peruse later. 
* SD Card support is only enabled by default on the M5Stack Cardputer platform. It can be enabled on M5Stick devices but an SD Card reader must be built and attached to the front panel pin header.
* EVIL Portal is only for use on professional engagements with a valid scope of work, educational or demonstration purposes. Storage, sale, or use of personal information without consent is against the law. 🤓

## Templates for EVIL PORTAL
The default EVIL PORTAL if you dont have a SD card attached is the Google template, but now its easier to create you own!
On templates folder of this repository there is a Facebook and Google samples which you can put on the root of your SD card, then go to EVIL PORTAL and select the template file.

## BadUSB
To choose a payload for the BadUSB on Cardputer instead of getting rickrolled, you need to create a file on the SD card root directory called "badpayload.txt".
This will be the raw payload that will be sent when the Cardputer is connected via USB cable.

## Install from M5Burner
This is the absolute easiest way to get BRUCE
* [M5Stick C Plus Quick Start](https://docs.m5stack.com/en/quick_start/m5stickc_plus/uiflow) has links to the M5Burner app for Linux, MacOS and Windows. This is the official tool to install UIFlow and other official firmware. I provide up-to-date binaries for BRUCE there.
* Launch M5Burner
* Select "STAMPS3" from the menu on the left (or StampS3 for Cardputer)
* Use the search at the top of the app to look for "BRUCE". My official builds will be uploaded by "owner" and have photos.
* Click Download
* Click Burn

## Install .bin files manually with esptool.py
* Install ESP-IDF tools per the [Espressif Getting Started Guide](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/)
* Open the esp-idf CMD tool (on Windows) - on Mac or Linux, esp-idf.py and esptool.py should be in the system path.
* esptool.py --port COMPORT -b 115200 write_flash -z 0x0 bruce.bin
  * port may be a COM port e.g. COM4, COM11 on Windows. On Mac and Linux it will usually be in /dev such as /dev/ttyUSB0, /dev/ttyACM0 or /dev/cu.usbserial-3
  * bruce.bin should be a version you downloaded from a GitHub Release -- preferably the latest available.

## Building from Source
If you want to customize BRUCE or contribute to the project, you should be familiar with building BRUCE from source.
* Install Arduino IDE. I've used Arduino 1.8 on Linux and Windows, and Arduino 2.2 on Windows successfully.
* Install the M5Stack boards for Arduino IDE: In File -> Preferences, paste this URL into the "Boards Manager URLs" text box. Use commas between URLs if there are already URLs present.  https://m5stack.oss-cn-shenzhen.aliyuncs.com/resource/arduino/package_m5stack_index.json
* If M5Stack -> M5Stick-C-Plus doesn't show up under Tools -> Boards, then use Tools -> Boards -> Boards Manager and search for M5Stack. This will install support for most of the M5Stack boards including the Stick C Plus.
* Ensure the correct device model (e.g. M5Stick-C, M5Stick-C-Plus or M5Cardputer) is selected in the boards menu.
* Install necessary libraries. In Sketch -> Include Library -> Library Manager, search for and install the following libraries and any dependencies they require:
  * M5StickCPlus, M5StickC or M5Cardputer
  * IRRemoteESP8266
* Un-comment the appropriate `#define` line near the top for your platform (STICK_C, STICK_C_PLUS or CARDPUTER)
* Switch partition schemes. `Tools` -> `Partition Scheme` -> `No OTA (Large APP)` - sometimes this option is labeled `Huge APP` 
* Configuration
  * The code should compile cleanly and work on an M5Stick C Plus out of the box from the master branch or a release tag.
  * Uncomment only the one appropriate `#define` option or compiler errors will occur. 
  * If for some reason the screen jumps from very dim at level 0 to almost fully bright at level 1 and further brightness levels don't affect anything, set the pct_brightness variable to false.
* Compile and upload the project

## Troubleshooting
* Several features output debugging information to the serial monitor. Use the Serial Monitor feature in Arduino IDE or M5Burner to gather this information. It may have useful hints. When filing a bug report, it often helps to include serial monitor output.
* Reset the EEPROM. On models with EEPROM settings support, use "Clear Settings" from the settings menu, or hold the "Next" button (Side key on StickC models, Tab or Down Arrow on Cardputer) while powering on. 
* TV-B-Gone's IR LED can be observed through a smart phone camera, emitting a pale purple beam of light. If it seems to be on constantly, or if it never flashes at all during TV-B-Gone operations, something is wrong. Report a bug. There's a known issue with TVBG not working after using Bluetooth spam or random wifi spam.
* Try viewing wifi lists from several different devices if you suspect wifi spam isn't working. Sometimes, Linux network manager can see networks that smart phones cannot. Please include the results of this testing if reporting wifi spam problems.
* Apple has patched a lot of Bluetooth stuff since summer 2023. If testing AppleJuice, try some of the AppleTV device types, as they tend to be more reliable due to apple not filtering out weaker bluetooth signals for that platform.
## Reporting Bugs
Please report bugs via GitHub Issues. These are easier to track than comments on social media posts, M5Burner entries, etc. If something isn't working, please include:
* Firmware version
* How you installed it (M5Burner, compiled yourself, esptool.py)
* Hardware 
* Specifically what feature and options are not working
* How you determined that it is not working and what testing you did. Include device models and operating systems of what you tested with, and any errors or relevant output from Serial Monitor, if applicable.
* If you figure out how to fix a bug you identified, PRs are welcome!

## Contributing
Contributions are welcome. Please look at the GitHub Issues for ideas to help enhance the project. When submitting a Pull Request, please target the develop branch. The easiest way to do this is to fork ALL branches, or to simply create a "develop" branch in your own fork, then use GitHub to Sync your develop branch.  Take note of how certain hardware (like the LED and RTC) are defined and gated in the code and try to stick to those patterns.

Things I'd like help on:
* A new "Region" of TV-B-Gone that's full of additional IR codes that can turn RGB LED strips, air conditioners, fans, sound bars and the like on and off
* Help refactoring how menus work. The way it was built was fine when it was one main menu with a few options. Now, adding a new sub-menu involves a lot of duplicated code.
* ~~TELNET client~~
* SMB client
* Simple network scan
* ~~BadUSB~~
* ~~Select EVIL portal template from SD card~~

Things I probably won't merge in:
* Bulk wifi deauthentication spamming
* Bluetooth spam that potentially disrupts health and fitness trackers, smart watches, etc.

## References
https://github.com/spacehuhn/ArduinoPcap

https://github.com/n0xa/m5stick-nemo

https://github.com/m5stack/M5Cardputer

https://github.com/caioluders/DPWO
