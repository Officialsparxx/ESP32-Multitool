# ESP32-Multitool (WIP)

**ESP32-Multitool** is an open-hardware, open-firmware handheld built around an ESP32-S3.  
Think “Flipper-style Swiss-army gadget” that adds NFC, sub-GHz RF, IR, TFT UI, SD logging,  
I²S audio, and more.


This is a repository for my first ESP32 project. This is device is a multitool that when completely assembled, should offer a wide range of features, from GB and NES emulation to ethical hacking and pen testing. (Even USB HID emulation AKA BadUSB with the power of an ESP32-"S3") Feel free to add or remove modules that you see fit to your own build though. 

Below is a list of the parts and modules I have included in my build: 

| # | Module / Part | Function | Vendor / Datasheet |
|---|---------------|----------|--------------------|
| 1 | **SANXIXING ESP32-S3 Dev Board** | Main MCU, Wi-Fi, BT | [[link]](https://www.amazon.com/dp/B0DB1WK3CW?ref=ppx_yo2ov_dt_b_fed_asin_title) |
| 1 | **DIANN 3.2″ ILI9341 SPI TFT** + touch | UI display & input | [[link]](https://www.amazon.com/dp/B0BNQD38T2?ref=ppx_yo2ov_dt_b_fed_asin_title) |
| 1 | **Dorhea MT3608** boost converter | Li-ion → **5 V** rail | [[link]](https://www.amazon.com/dp/B089JYBF25?ref=ppx_yo2ov_dt_b_fed_asin_title) |
| 1 | **TP4056 USB-C Li-ion charger** (5 V 1 A) | Battery charging & protection | [[link]](https://www.amazon.com/dp/B08X6G26Q8?ref=ppx_yo2ov_dt_b_fed_asin_title) |
| 1 | **CC1101 433 MHz module** | Sub-GHz TX/RX | [[link]](https://www.amazon.com/dp/B0D2TMTV5Z?ref=ppx_yo2ov_dt_b_fed_asin_title) |
| 1 | **AITRIP PN532 NFC/RFID V3** | 13.56 MHz tag read/write | [[link]](https://www.amazon.com/dp/B09BTH8GGJ?ref=ppx_yo2ov_dt_b_fed_asin_title) |
| 1 | **IR pair** – 38 kHz RX module + TX LED | IR control / capture | [[link]](https://www.amazon.com/dp/B092MH7DJH?ref=ppx_yo2ov_dt_b_fed_asin_title) |
| 1 | **PCM5102(A) I²S DAC** | Stereo line / headphone out | [[link]](https://www.amazon.com/dp/B0DNW32Y46?ref=ppx_yo2ov_dt_b_fed_asin_title) |
| 1 | **PAM8403 2-ch class-D amp** | Drives stereo speakers | [[link]](https://www.amazon.com/dp/B00M0F1LJW?ref=ppx_yo2ov_dt_b_fed_asin_title) |
| 1 | **DIANN 2-slot 18650 holder** *(rewired parallel)* | 3 V battery pack | [[link]](https://www.amazon.com/dp/B0BJV9ZL3J?ref=ppx_yo2ov_dt_b_fed_asin_title) |
| 1 | **BOJACK MLCC kit** (0.1 µF → 10 µF) | Bypass & bulk caps | [[link]](https://www.amazon.com/dp/B085RDTCCV?ref=ppx_yo2ov_dt_b_fed_asin_title) |
| 1 strip | **Spare 10 k Ω resistors** | CS pull-ups | salvaged |
| 24 pcs | **Micro tactile switches** (QTEATAK set) | Front-panel buttons | [[link]](https://www.amazon.com/dp/B0BFWK5N8V?ref=ppx_yo2ov_dt_b_fed_asin_title) |

You can pull the ceramic capcitors from the spare motherboard as well, but I decided to opt for the kit because my multimeter lacks the ability to test capacitors. 

In this repo, I've provided an editable photoshop PSD as well as a standard image (PNG) for a (somewhat rough) wiring schematic. I am learning a lot of things myself as I go along so there are a few things to keep in mind when viewing the schematic:

I do not have things correctly color coded except for maybe the power and ground lines. I have these cables color coded to the physcial placement of my own cable managment, and it might not make much sense for someone expecting a more structured color scheme. Keep in mind that this schematic is mostly meant to help see connection points, otherwise feel free to edit and draw your own schematic. 
I have not included the Audio modules in the schematic yet.
In the schematic, you will see that I have split the VCC connections between both 3V3 outputs to all of the modules. I have since decided to dedicate one 3v3 pin for VCC, and the other for the 10k ohm pull ups. Since this is a multitool running multiple modules, every module will require a 10k Ohm resistor between its CS/SS pin and one od the 3V3 pins on the ESP32. CS/SS must idle high so the modules won't interfere with the boot sequence. Here is a list of modules that require the VCC and GND to be jumped by 0.1uf MMLC's: CC1101, PN532 V3, IR receiver & transmitter, & Class-D amp, or opt for a bulkier one like I did for audio reasons. The TFT and DAC should already have one built in. 

Along with the schematic, I have included a text file that directly shows which pins are connected which. Here is a summarized version:

| Function        | GPIO | Notes |
|-----------------|------|-------|
| MOSI            | 11   | Blue wire |
| MISO            | 13   | Orange wire |
| SCK             | 12   | Green wire |
| TFT_CS          | 10   | 10 k pull-up |
| TOUCH_CS        | 9    | 10 k pull-up |
| SD_CS           | 8    | 10 k pull-up |
| CC1101_CS       | 6    | 10 k pull-up |
| CC1101_GDO0     | 5    | IRQ |
| PN532_SS        | 42   | 10 k pull-up |
| PN532_IRQ       | 7    | INPUT_PULLUP |
| IR_RX           | 41   | — |
| IR_TX           | 17   | PWM/RMT |
| TFT_DC          | 21   | — |



