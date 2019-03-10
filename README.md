# esp-micropython
ESP32/ESP8266 micropython complete installation guide/ usuage

This guide is to install micropython on esp32/esp8266

Requirments

The first thing you require is a board with an esp8266/esp32 chip.the micropython supports esp8266/esp32 chip itself so any board should work.
The minimum requirment for flash size is 1 Mbyte.There is no support for boards with flash size 512KB.

Names of pins will be given in this tutorial using the chip names (eg GPIO)) and it should be known which pi corresponds to on your pirticular board.

Getting the frimware

The first thing you need to do is download the most recent MicroPython firmware .bin file to load onto your ESP8266/ESP32 device. You can download it from the http://micropython.org/download . From here, you have 3 main choices

Stable firmware builds for 1024kb modules and above.
Daily firmware builds for 1024kb modules and above.
Daily firmware builds for 512kb modules.

If you are just starting with MicroPython, the best bet is to go for the Stable firmware builds. If you are an advanced, experienced MicroPython ESP8266 user who would like to follow development closely and help with testing new features, there are daily builds (note: you actually may need some development experience, e.g. being ready to follow git history to know what new changes and features were introduced).

Support for 512kb modules is provided on a feature preview basis. For end users, it’s recommended to use modules with flash of 1024kb or more. As such, only daily builds for 512kb modules are provided.

Easy Installation
You will need either Python 2.7 or Python 3.4 or newer installed on your system.

The latest stable esptool.py release can be installed from pypi via pip:

$ pip install esptool
With some Python installations this may not work and you'll receive an error, try python -m pip install esptool or pip2 install esptool.

After installing, you will have esptool.py installed into the default Python executables directory and you should be able to run it with the command esptool.py.

For more information or troubleshooting you can visit https://github.com/espressif/esptool/blob/master/README.md

For using esptool for installation of micropython frimware 

Using esptool.py you can erase the flash with the command:

esptool.py --port /dev/ttyUSB0 erase_flash

And then deploy the new firmware using:

esptool.py --port /dev/ttyUSB0 --baud 460800 write_flash --flash_size=detect 0 esp8266-20170108-v1.8.7.bin

You might need to change the “port” setting to something else relevant for your PC. You may also need to reduce the baudrate if you get errors when flashing (eg down to 115200). The filename of the firmware should also match the file that you have.

For some boards with a particular FlashROM configuration (e.g. some variants of a NodeMCU board) you may need to use the following command to deploy the firmware:

esptool.py --port /dev/ttyUSB0 --baud 460800 write_flash --flash_size=detect -fm dio 0 esp8266-20170108-v1.8.7.bin

note: before using esptool you should be inside the installed directiory of esptool and the doenloaded firmware (.bin file) should be in the same directory (esptool directory)

This will complete your firmware installation on ESP board.

USUAGE

Accessing the file system and running your first prgram on firmware you installed

Adafruit Ampy

You should have python installed. To check you can just type:
python

to install adafruit ampy using pip you can type:

pip install adafruit-ampy

Note on some Linux and Mac OSX systems you might need to install as root with sudo:

sudo pip install adafruit-ampy 

Alternatevely if you have python both 2x and 3x installed to install adafruit ampy in python3 you can use:

windows

pip3 install adafruit-ampy
 
Mac

sudo pip3 install adafruit-ampy

Usuage:

ampy --help

You should see usage information for the tool printed, like what commands it has and options for using them.

Once you install micropython firmware in your ESP board it has boot.py as a initial boot setup file.
If you want to run your program on boot you can modify boot.py or you can load multiple .py files and configure them to run on boot using boot.py

IMPORTANT

To list files inside your ESP board you can use command like:

ampy -p PORT -b BOUDRATE -d 1 ls

Here PORT is to which your ESP board is connected to.
BAUDRATE is the baud you use for serial communication (eg: 9600) which you used while flashing firmware.

-d (delay) is important because we need to synchronize your ESP board clock with youur terminal emulator.(here we are using 1 second delay) 


















Execution/Output

Teraterm (windows OS/Mac OS)

You can download Tera Term from here https://ttssh2.osdn.jp/index.html.en .	

Using Tera term:

Before you begin you need to configure the COM port and baud rate depending up on your configuration







Picocom (linux distribution)

Compilation/installation:

Change into picocom's source directory and say:

make

This will be enough to compile picocom for most modern Unix-like systems. If you want, you can then strip the resulting binary like this:

strip picocom

Striping the binary is not required, it just reduces its size by a few kilobytes. Then you can copy the picocom binary, as well as the man-page, to wherever you put your binaries and man-pages. For example:

cp picocom ~/bin
cp picocom.1 ~/man/man1

Again, this is not strictly necessary. You can run picocom and read its man-page directly from the source directory.

If something goes wrong and picocom can't compile cleanly, or if it's lacking a feature you need, take a look at the included Makefile. It's very simple and easy to understand. It allows you to select compile-time options and enable or disable some compile-time features by commenting in or out the respective lines. Once you edit the Makefile, to recompile say:

make clean
make

If your system's default make(1) command is not GNU Make (or compatible enough), find out how you can run GNU Make on your system. For example:

gmake clean
gmake

Alternatively, you might have to make some trivial edits to the Makefile for it to work with your system's make(1) command.


Using picocom

If your computer is a PC and has the standard on-board RS-233 ports (usually accessible as two male DB9 connectors at the back) then under Linux these are accessed through device nodes most likely named: /dev/ttyS0 and /dev/ttyS1. If your computer has no on-board serial ports, then you will need a USB-to-Serial adapter (or something similar). Once inserted to a USB port and recognized by Linux, a device node is created for each serial port accessed through the adapter(s). These nodes are most likely named /dev/ttyUSB0, /dev/ttyUSB1, and so on. For other systems and other Unix-like OSes you will have to consult their documentation as to how the serial port device nodes are named. Lets assume your serial port is accessed through a device node named /dev/ttyS0.

You can start picocom with its default option values (default serial port settings) like this:

picocom /dev/ttyS0
If you have not installed the picocom binary to a suitable place, then you can run it directly from the source distribution directory like this:

./picocom /dev/ttyS0
If this fails with a message like:

FATAL: cannot open /dev/ttyS0: Permission denied
This means that you do not have permissions to access the serial port's device node. To overcome this you can run picocom as root:

sudo picocom /dev/ttyS0

for more information please visit https://github.com/npat-efault/picocom .

