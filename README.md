# Lego Smart Play Reverse Engineering

LEGO® is a trademark of the LEGO Group of companies which does not sponsor, authorize or endorse this site.

If you have any further information than presented here I would be happy about a pull request or message me.

As the Lego Smart Play Ecosystem is just release, any information is naturally quite sparse at the moment. The information here is provided as a starting point for further investigations, with the goal to open up the system to hobbyists in the future.

For introduction to the topic, I suggest the following resources:

- [Video from Brickstory (german) showcasing the user stories](https://www.youtube.com/watch?v=fez3vRo7jfs)
- [Technical Analysis of the technology through openly accessible documents before the launch from heise](https://www.heise.de/en/background/Lego-Smart-Play-Patent-applications-and-FCC-documents-reveal-the-technology-11135038.html)
- [Video from Lego about the development history](https://www.lego.com/en-us/smart-play/article/innovation)

## Current Key Questions

- Does the ASIC contain a processor or is the complete firmware limited to the EM9305?
- Is the application specific code contained in the Smart Tag or the Smart Brick Firmware?

## Smart Brick PEL-1

Smart Bricks are currently (02. March 2026) on [bricklink](https://www.bricklink.com/v2/catalog/catalogitem.page?P=5409c01#T=S) for 20-35 EUR,

The Smart Brick is a highly sophisticated piece of technology that uses advanced manufacturing technology and highly integrated electronics. It consists of a main electronic assembly that is housed in a ABS Lego brick shell, which is ultrasonically welded together, thus making disassembly destructive by design.

The main electronic assembly consists of

- a main plastic mechanical frame
- a plastic light diffusor and speaker holder
- a printed circuit board
- three pairs of coils connected to the PCB and wound around the plastic frame
- a 45 mAh lithium polymer battery
- a speaker
- a 2.4 GHz bluetooth antenna

The heise article suggest the assembly may be performed by [Jabil](https://jabil.com/solutions/manufacturing.html).

The top and bottom coils contain less windings than the other coils. During dissassembly, it was confirmed that the brick still charges with all but the top and bottom coil removed, but not with only the bottom coil present.

The PCB contains the following notable parts

- [em microelectronic "em|bleu" EM9305 BLE SoC](https://www.emmicroelectronic.com/product/standard-protocols/em-bleu-em9305) in QFN-28 package
- "DA000001-04" Play Engine ASIC
- [Bosch Sensortec BMA580 Accerolometer](https://www.bosch-sensortec.com/products/motion-sensors/accelerometers/bma580/) in WLCSP-6 package
- unknown component next to the Accelerometer labelled "4A5R" "JIQ" as a WLCSP style package
- a side mounted RGB-LED and a photodiode with a mirror designed as part of the plastic frame realizing a color sensor
- two RGB LEDs for the UI
- a microphone or pressure/blow sensor

the EM9305 is a BLE MCU designed for ULP applications which "may be easily connected with [...] any custom sensor processing ASIC for customers requiring a simple add-on function" It features

- Synoptic ARC EM7D, 32-bit MCU
- 64 kB RAM
- 512 kB Flash
- 12 GPIO
- various security features including AES-128 hardware en-/decryption and Secure Firmware-OTA

## Smart Tags and Minifigures

The Smart Tags present themselves as ISO15693 compliant tags with a vendor code attributable to em microelectronic and a memory size of 2116 bits. A good fit from em's portfolio would be the [EM4237](https://www.emmicroelectronic.com/product/nfc-high-frequency-ics/em4237) although readout seems to not be disabled. Furthermore, multiple identical coded tags read out the same data.

All Tags and Minifigures seem to have different length of data suggesting the program code is stored on the Tags/Minifigures themselves.

