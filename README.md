# Debian-on-QNAP-TS-451DeU
The QNAP TS-451DeU NAS server is only capable for operating in a headless mode.  All configuration and maintenance is performed throught the web interface.  The server has a graphics processor but does not have a port.  To install an alternate OS on the hardware some additional steps are required.  The following details the procedure for installation of the Debian operating system on this hardware.

## Hardware
1. QNAP TS-451DeU or similar QNAP hardware
2. USB Serial interface[^1]
3. JST PH 4-pin female connector[^2]
4. USB drive
   
## Procedure
### Backup the QNAP
1. Open an SSH connection to the QNAP.
   You may need to enable SSH in the QNAP Control Panel.
3. Backup the firmware.[^4]
   ```bash
   cat /dev/mmcblk0boot0 > mmcblk0boot0
   cat /dev/mmcblk0boot1 > mmcblk0boot1
   cat /dev/mmcblk0rpmb > mmcblk0rpmb
   ```
   Store these files for recovery.

### Create Debian installer on USB drive
1. Download iso
2. Copy to USB[^3]
   ```bash
   cp debian.iso /dev/sdX
   sync
   ```

### Connect Serial Interface
1. Power down the QNAP
2. Connect serial interface to the QNAP
   1. Remove the top cover of the QNAP.
   2. The serial interface header (COM1) is located at the back near the power supply and in front of the middle fan.  Connect the JST PH 4-pin female cable to the header.
      ![QNAP Serial Header)](QNAP-serial-header.png)
      Use the following pin-out. _Do not connect the 5V power wire from the USB cable since the QNAP is already powered._
      ![QNAP Serial Connection (Breadboard)](QNAP-serial_bb.png)
3. Open a serial console
   ```bash
   sudo screen /dev/ttyUSB0 115200
   ```
### Install OS
1. Power on the QNAP
2. After system beep press `ESC` to enter the BIOS setup

## References
- https://www.reddit.com/r/qnap/comments/ttm5db/gaining_access_to_the_ts451deu/
- https://www.cyrius.com/debian/kirkwood/qnap/ts-219/install/
- https://www.desgehtfei.net/serielle-konsole-fuer-qnap-mit-arduino/
- https://wiki.qnap.com/wiki/System_Recovery_Mode
- https://wiki.qnap.com/wiki/Debian_Installation_On_QNAP
- https://www.reddit.com/r/qnap/comments/s1p1g8/truenas_core_on_the_qnap_ts451deu
- https://www.reddit.com/r/qnap/comments/fwg2in/replace_qts_with_linux_server_distro_on_451/
- https://teklager.se/en/knowledge-base/installing-debian-over-serial-console-apu-board/

[^1]: https://www.adafruit.com/product/954
[^2]: https://www.adafruit.com/product/3955 
[^3]: https://www.debian.org/releases/stable/amd64/ch04s03.en.html
[^4]: https://wiki.qnap.com/wiki/Debian_Installation_On_QNAP#Backup_your_data_/_partitions_on_flash_memory

