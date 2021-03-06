---
title: Tracker SoM datasheet
layout: datasheet.hbs
columns: two
order: 4
description: Datasheet for the Particle Tracker SoM Cellular GNSS module
---

# Tracker SoM Datasheet <sup>(002)</sup>

{{#unless pdf-generation}}
{{downloadButton url="/assets/pdfs/datasheets/tracker-som-datasheet.pdf"}}
{{/unless}} {{!-- pdf-generation --}}


![SoM](/assets/images/t523-som.svg)


## Functional description

### Overview

The AssetTracker SoM is a System-on-a-Module (SoM) with:

- LTE Cat 1 (EMEAA) or LTE Cat M1 (North America) cellular modem
- GNSS (supports GPS, SBAS, QZSS, GLONASS, BeiDou, and Galileo) with up to 1.8m accuracy and untethered dead-reckoning 
- Support for CAN bus and 5V power for CAN devices
- Built-in Inertial Measurement Unit (IMU)
- Castellated module can be reflow soldered to your base board, and is available on an evaluation board or carrier board

---

### Features

 * GNSS u-blox Neo M8U for GNSS with on-board dead-reckoning for up to 1.8m CEP50 GPS accuracy
  * Supports GPS L1C/A, SBAS L1C/A, QZSS L1C/A, QZSS L1-SAIF, GLONASS L1OF, BeiDou B1I, Galileo E1B/C
  * Support for battery-backup for almanac and ephemeris data
 * Quectel BG96-NA modem
  * LTE Cat M1 module for North America (United States, Canada, and Mexico) 
  * 3GPP E-UTRA Release 13
  * LTE FDD bands supported: 2, 4, 12, 13
 * Quectel EG91-EX modem
  * LTE Cat 1 module for EMEAA region 
  * 3GPP E-UTRA Release 13
  * Cat 1 bands supported: 1, 3, 7, 8, 20, 28
  * Support for Europe only at this time
 * Nordic Semiconductor nRF52840 SoC 
  * ARM Cortex-M4F 32-bit processor @ 64MHz 
  * 1MB flash, 256KB RAM in SoC
  * Bluetooth 5: 2 Mbps, 1 Mbps, 500 Kbps, 125 Kbps 
  * Supports DSP instructions, HW accelerated Floating Point Unit (FPU) calculations 
  * ARM TrustZone CryptoCell-310 Cryptographic and security module 
  * Up to +8 dBm TX power (down to -20 dBm in 4 dB steps) 
  * NFC-A tag
 * Wi-Fi location: on-board ESP32 offers SSID scanning for using third-party Wi-Fi location services
 * PMIC (Power Management IC) and Fuel Gauge
 * On-module additional 8MB SPI flash
 * CAN Bus: on-board, integrated CAN Bus controller and transceiver making it ideal for fleet and micromobility
 * Boost Converter to power 5V CAN devices from a 3.6V battery
 * RTC: Battery-backed external real-time clock
 * Watchdog Timer: integrated hardware WDT
 * 10 Mixed signal GPIO (8 x Analog, 10 x Digital, UART, I2C, SPI)
 * USB 2.0 full speed (12 Mbps)
 * JTAG (SWD) pins
 * Support for external RGB status LED 
 * Support for external Reset and Mode buttons
 * On-module MFF2 Particle SIM 
 * Bluetooth chip antenna on module, switchable to use U.FL connector in software.
 * Five on-module U.FL connectors for cellular, GNSS, BLE, Wi-Fi, and alternative GNSS.
 * Castellated module designed to be reflow soldered to your own custom base board, or pre-populated on a Particle Evaluation Board or Carrier Board.
 * FCC, IC, and CE certified 
 * RoHS compliant (lead-free)


## Interfaces

### Block diagram

<div align="center"> <a href="/assets/images/at-som/at-som-block-diagram.png" target="_blank"> <img src="/assets/images/at-som/at-som-block-diagram.png" class="full-width"></a></div> 

### Power
The Tracker SoM can be powered via the VIN (3.88V-12VDC) pin, over USB, or a LiPo battery.

#### VIN
The input voltage range on VIN pin is 3.88VDC to 12VDC. When powering from the VIN pin alone, make sure that the power supply is rated at 10W (for example 5 VDC at 2 Amp). If the power source is unable to meet this requirement, you'll need connect the LiPo battery as well.  An additional bulk capacitance of 470uF to 1000uF should be added to the VIN input when the LiPo Battery is disconnected.  The amount of capacitance required will depend on the ability of the power supply to deliver peak currents to the cellular modem.

#### LiPo
This pin serves two purposes. You can use this pin to connect a LiPo battery (either directly or using a JST connector), or it can be used to connect an external DC power source (and this is where one needs to take extra precautions). When powering it from an external regulated DC source, the  recommended input voltage range on this pin is between 3.6V to 4.4VDC. Make sure that the supply can handle currents of at least 3Amp. This is the most efficient way of powering the module since the PMIC bypasses the regulator and supplies power to the module via an internal FET leading to lower quiescent current.

When powered from a LiPo battery alone, the power management IC switches off the internal regulator and supplies power to the system directly from the battery. This reduces the conduction losses and maximizes battery run time. The battery provided with the module is a Lithium-Ion Polymer battery rated at 3.7VDC 1,800mAh. You can substitute this battery with another 3.7V LiPo with higher current rating. Remember to never exceed this voltage rating and always pay attention to the polarity of the connector. A LiPo battery with internal protection circuits is recommended.

Typical current consumption is around 180mA and up to 1.8A transients at 5VDC. In deep sleep mode, the quiescent current is 130uA [this value may change] (powered from the battery alone).

The MAX17043 fuel gauge is only compatible with single cell lithium-ion batteries. The state-of-charge (SoC) values will not be accurate with other battery chemistries.

A battery temperature sensor can be added if desired. Connect a negative temperature coefficient thermistor to the TS pin and GND. Charge suspends when the TS pin is out of range. A 103AT-2 thermistor is recommended.

#### VBUS

VBUS is connected to the USB detect pin of nRF52840 to enables the USB interface. The recommended input voltage range is between 4.35V to 5.5V DC. It is also connected to the bq24195 PMIC to allow for DPDM, detection of the power capacity of the USB port.

#### 3V3 Pin
This pin is the output of the on-board 3.3V switching regulator that powers the microcontroller and the peripherals. This pin can be used as a 3.3V power source with a max load of 800mA. Unlike the Photon, this pin _CANNOT_ be used as an input to power the module.


#### RTC_BAT
This is the supply to the real-time clock battery backup. 1.4 to 3.6V. 

| Voltage | Typical Current | Maximum Current | Unit |
| :---:   | ---:            | ---:            | :--- |
| 3.0V    | 56              | 330             | nA   |
| 1.8V    | 52              | 290             | nA   |

If the RTC battery is not used, connect RTC_BAT to ground.

#### GNSS_BAT
This is the supply for maintaining the u-blox GNSS ephemeris and almanac data when removing power. This can use the same battery as RTC_BAT, can be a super-capacitor, or can be omitted. 1.5 to 3.6V. Typical current is 15 uA.

If you are not powering GNSS\_BAT with a battery or super-capacitor, connect GNSS\_BAT to 3V3.

- Saving the ephemeris and almanac data can improve fix/lock time.
- It won't make a difference on completely cold boot, where is no previously saved data.
- It does not make a difference if the GNSS is constantly powered or is using a software power save mode.

---

#### PMID
This pin is the output of the internal boost regulator of the PMIC that can source 5.1VDC from the battery in OTG (On The Go) mode. This feature is useful when your circuitry needs a 5V source from the module when powered by the battery alone.

The confusing bit about this pin is that it will continue to provide 5.1VDC but only when the input voltage (VIN) is between 3.6V to 5.1VDC. As soon as the input voltage exceeds this limit, the PMID starts tracking _that_ voltage. For example if VIN = 9VDC, the PMID will be 9VDC and _NOT_ 5.1VDC. So you need to be careful when using it as a source for powering your external circuitry. The max current draw on this pin is 2.1A but is not recommended due to thermal limitations of the circuit board.

### Antennas

There are a number of U.FL antenna connectors on the Tracker SoM:

| Label | Purpose | 
| :---: | :--- |
| GNSS | u-blox GNSS antenna (GPS) |
| CELL | Quectel cellular modem antenna |
| WIFI | Wi-Fi antenna for Wi-Fi geolocation (optional)<sup>1</sup> |
| BLE  | External Bluetooth (optional)<sup>2</sup> |
| GNSS/DIV | Quectel GNSS antenna (optional)<sup>1</sup> |

<sup>1</sup>Not supported in initial release.

<sup>2</sup>There is a BLE chip antenna on the module, the external BLE antenna is optional.

There is no U.FL connector for NFC. If you wish to use the NFC tag feature, you'll need to add an antenna or antenna connector on your base board.

- The antenna placement needs to follow some basic rules, as any antenna is sensitive to its environment. Mount the antenna at least 10mm from metal components or surfaces, ideally 20mm for best radiation efficiency, and try to maintain a minimum of three directions free from obstructions to be able to operate effectively.
- Needs tuning with actual product enclosure and all components.
- For the BLE antenna, it is recommended to use a 2.4 GHz single-frequency antenna and not a 2.4 GHz + 5 GHz antenna, so as to avoid large gain at the frequency twice of 2.4 GHz which can cause the second harmonic radiation of 2.4 GHz to exceed standards.
 
---

### Peripherals and GPIO

There are 10 exposed GPIO lines labeled A0-A7, TX, and RX. These multi-function pins can be configured for use as GPIO or other interfaces like SPI and I2C.

| Shared Peripherals | Qty | Input(I) / Output(O) |
| :---:|:---:|:---:|
| Digital | 10 (max) | I/O |
| Analog (ADC) | 8 (max) | I |
| UART | 1 | I/O |
| SPI  | 1 | I/O |
| I2C  | 1 | I/O |
| PWM  | 10 (max)<sup>1</sup> | O |


| Peripheral Type | Qty | Input(I) / Output(O) |
| :---:|:---:|:---:|
| USB  | 1 | I/O |
| NFC Tag  | 1 | O |
| CAN Bus | 1 | I/O | 

<sup>1</sup>PWM is divided into three PWM groups. Each group must share the same frequency, but can have different periods.

**Note:** All GPIO are only rated at 3.3VDC max. CAN bus has a [higher voltage rating](#can-specifications).



### JTAG (SWD) 

The AssetTracker SoM exposes the nRF52 SWD interface on the following pins. The Evaluation Board connects these pins to the 2x5 connector used on the Argon and Boron to easily connect the Particle Debugger.

| # |	Pin	 | Function | Connected To |	Description |
| :---: | :---: | :---: | :---: | --- |
| 22 | SWDIO | JTAG | nRF52 | nRF52 MCU SWDIO |
| 23 | SWDCLK | JTAG | nRF52 | nRF52 MCU SWDCLK |
| 24 | SWO | JTAG | nRF52 | nRF52 MCU SWO |

This interface can be used to debug your code or reprogram your bootloader, device OS, or the user firmware. 

## Memory map

### nRF52840 Flash Layout Overview

 - Bootloader (48KB, @0xF4000)
 - User Application (128KB, @0xD4000)
 - System (656KB, @0x30000)
 - SoftDevice (192KB)

### External SPI Flash Layout Overview (DFU offset: 0x80000000)

 - OTA (1500KB, @0x00689000)
 - Reserved (420KB, @0x00620000)
 - FAC (128KB, @0x00600000)
 - Reserved (2MB @0x00400000)
 - LittleFS (4MB, @0x00000000)


## Pins and connectors

<div align="center"> <a href="/assets/images/at-som/at-som-labeled.png" target="_blank"> <img src="/assets/images/at-som/at-som-labeled.png" class="full-width"></a></div> 

Circular labels are as follows:

| Label | Purpose | 
| :---: | :--- |
|  1 | Quectel cellular modem antenna |
|  2 | Wi-Fi antenna for Wi-Fi geolocation (optional) |
|  3 | External Bluetooth (optional) |
|  4 | Built-in Bluetooth chip antenna |
|  5 | Quectel GNSS antenna (optional) |
|  6 | u-blox GNSS antenna (GPS) |
|  7 | u-blox Neo M8 GNSS (GPS) |
|  8 | Quectel cellular modem |

---

### SoM Pin description

| # |	Pin	 | Function | Connected To |	Description |
| :---: | :---: | :---: | :---: | --- |
|   |   |  |  | Right Side |
| 1 | GND | POWER | | Ground |
| 2 | GNSS_BAT | POWER IN | GNSS | Battery backup for GNSS |
| 3 | GNSS_RESET | IO | GNSS & IOEX | GNSS hardware reset. Can be controlled by this pin or software. |
| 4 | GNSS_VBUS | USB PWR | GNSS | GNSS USB power. Optional. |
| 5 | GNSS_P | USB D+ | GNSS | GNSS USB interface D+. Optional. |
| 6 | GNSS_N | USB D- | GNSS | GNSS USB interface D-. Optional. |
| 7 | GNSS_PULSE | OUT | GNSS | GNSS time pulse output. Can be used for a GNSS fix LED.<sup>2</sup> |
| 8 | GND | POWER | | Ground |
| 9 | NC |  | | Leave unconnected. |
| 10 | GND | POWER | | Ground |
| 11 | WIFI_EN | IO | WIFI & IOEX | ESP32 enable. Can be controlled by this pin or software.  |
| 12 | WIFI_BOOT | IO | WIFI & IOEX | ESP32 boot mode. Can be controlled by this pin or software.  |
| 13 | WIFI_TXD | OUT | WIFI | ESP32 serial TX |
| 14 | WIFI_RXD | IN | WIFI | ESP32 serial TX |
| 15 | CELL_VBUS | USB PWR | CELL | Cellular modem USB power. Optional. |
| 16 | CELL_D+ | USB D+ | CELL | Cellular modem USB interface D+. Optional. |
| 17 | CELL-D- | USB D- | CELL | Cellular modem USB interface D-. Optional. |
| 18 | NC SOM18 | | | Leave unconnected. |
| 19 | NC SOM19 | | | Leave unconnected. |
| 20 | NC SOM20 | | | Leave unconnected. |
| 21 | NC SOM21 | | | Leave unconnected. |
| 22 | SWDIO | JTAG | nRF52 | nRF52 MCU SWDIO |
| 23 | SWDCLK | JTAG | nRF52 | nRF52 MCU SWDCLK |
| 24 | SWO | JTAG | nRF52 | nRF52 MCU SWO |
| 25 | GND | POWER | | Ground |
| 26 | NFC2 | NFC | nRF52 | nRF52 NFC antenna. Supports NFC tag mode only. Optional. |
| 27 | NFC1 | NFC | nRF52 | nRF52 NFC antenna. Supports NFC tag mode only. Optional. |
| 28 | RGB_BLUE | RGB LED | nRF52 | Common anode RGB status LED, blue. Optional. |
| 29 | RGB_GREEN | RGB LED | nRF52 | Common anode RGB status LED, green. Optional. |
| 30 | RGB_RED | RGB LED | nRF52 | Common anode RGB status LED, red. Optional. |
| 31 | GND | POWER | | Ground |
| 32 | MODE | INPUT | nRF52 | External MODE button input, active low. Optional. |
| 33 | RESET | INPUT | nRF52 | External RESET button input, active low. Optional. |
| 34 | NC SOM34 | | | Leave unconnected. |
| 35 | NC SOM35 | | | Leave unconnected. |
| 36 | NC SOM36 | | | Leave unconnected. |
| 37 | NC SOM37 | | | Leave unconnected. |
| 38 | A7 | IO | nRF52 | A7, D7, SS, WKP |
| 39 | A6 | IO | nRF52 | A6, D6, SPI SCK  |
| 40 | A5 | IO | nRF52 | A5, D5, SPI MISO |
| 41 | A4 | IO | nRF52 | A4, D4, SPI MOSI |
| 42 | GND | POWER | | Ground |
|   |   |  |  | Top Side |
| 43 | GND | POWER | | Ground |
| 44 | NC SOM44 | | | Leave unconnected. |
| 45 | 3V3 | POWER OUT | TPS62291 | 3.3V power output. 1000 mA maximum include nRF52 and other peripheral use. |
| 46 | TS | IN | PMIC | Battery temperature sensor |
| 47 | PMID | POWER OUT | PMIC | PMIC power output in OTG mode. |
| 48 | GND | POWER | | Ground |
| 49 | VIN | POWER IN | PMIC | Power input 3.88VDC to 12VDC. |
| 50 | STAT | OUT | PMIC | PMIC charge status. Can be connected to an LED. Active low. Optional. | 
| 51 | VBUS | POWER IN | PMIC & nRF52 | nRF52 USB power input. Can be used as a power supply instead of VIN. |
| 52 | GND | POWER | | Ground |
| 53 | LI+ | POWER | PMIC | Connect to Li-Po battery. Can power the device or be recharged by VIN or VBUS. |
|   |   |  |  | Left Side |
| 54 | GND | POWER | | Ground |
| 55 | A0 | IO | nRF52 | A0, D0, Wire SDA, Thermistor<sup>1<sup> |
| 56 | A1 | IO | nRF52 | A1, D1, Wire SCL, User button<sup>1<sup> |
| 57 | A2 | IO | nRF52 | A2, D2, Serial1 CTS, GNSS lock indicator<sup>1<sup> |
| 58 | A3 | IO | nRF52 | A3, D3, Serial1 RTS, M8 GPIO<sup>1<sup> |
| 59 | NC SOM59 | | | Leave unconnected. |
| 60 | NC SOM60 | | | Leave unconnected. |
| 61 | NC SOM61 | | | Leave unconnected. |
| 62 | NC SOM62 | | | Leave unconnected. |
| 63 | AGND | POWER | nRF52 | nRF52 analog ground. Can connect to regular GND. |
| 64 | CAN_N | CAN | CAN | CAN Data- |
| 65 | CAN_P | CAN | CAN | CAN Data+ |
| 66 | CAN_5V |  | XCL9142F40 | 5V power out, 0.8A maximum. Can be controlled by software. |
| 67 | GND | POWER | | Ground |
| 68 | MCU-D- | USB D- | nRF52 | MCU USB interface D-. Optional. |
| 69 | MCU_D+ | USB D+ | nRF52 | MCU USB interface D+. Optional. |
| 70 | GND | POWER | | Ground |
| 71 | MCU_RX | IO | nRF52 | Serial RX, GPIO D9, Wire3 SDA |
| 72 | MCU_TX | IO | nRF52 | Serial TX, GPIO D8, Wire3 SCL |
| 73 | RTC_BAT | POWER | AM18X5 | RTC/Watchdog battery +. Connect to GND if not using. |
| 74 | RTC_BTN | IN | AM18X5 | RTC EXTI. Can use as a wake button. |
| 75 | GND | POWER | | Ground |
| 76 | NC SOM76 | | | Leave unconnected. |
| 77 | NC SOM77 | | | Leave unconnected. |
| 78 | NC SOM78 | | | Leave unconnected. |
| 79 | NC SOM79 | | | Leave unconnected. |
| 80 | NC SOM80 | | | Leave unconnected. |
| 81 | NC SOM81 | | | Leave unconnected. |
| 82 | NC SOM82 | | | Leave unconnected. |
| 83 | CELL_GPS_RX | IN | CELL | Cellular modem GPS serial RX data. |
| 84 | CELL_GPS_TX | OUT | CELL | Cellular modem GPS serial TX data. |
| 85 | CELL_RI | OUT | CELL | Cellular modem ring indicator output. |
| 86 | GND | POWER | | Ground |
| 87 | CELL_GPS_RF | RF | CELL | Cellular modem GPS antenna. Optional. |
| 88 | GND | POWER | | Ground |
| 89 | GND | POWER | | Ground |
| 90 | GNSS_BOOT |  | GNSS | u-blox GNSS boot mode |
| 91 | GNSS_ANT_PWR |  | GNSS | u-blox GNSS antenna power |
| 92 | GNSS_LNA_EN |  | GNSS | u-blox GNSS LNA enable or antenna switch |
| 93 | GND | POWER | | Ground |
| 94 | GNSS_RF |  | GNSS | GNSS antenna. |
| 95 | GND | POWER | | Ground |

Note: All GPIO, ADC, and peripherals such as I2C, Serial, and SPI are 3.3V maximum and are **not** 5V tolerant.

Pin numbers match the triangular numbers in the graphic above.

<sup>1</sup>Pin usage on the Tracker One.

<sup>2</sup>The GNSS_PULSE pin can be used for a hardware GPS lock indicator, however the Tracker One controls the GNSS Lock indicator in software and connects the LED to pin A2.

### nRF52 pin assignments

| SoM Pin | GPIO  | Analog | Other       | PWM     | nRF Pin |
| :-----: | :---: | :----: | :---------: | :-----: | :-----: |
| 55      | D0    | A0     | Wire SDA<sup>1</sup>    | Group 0 | P0.03   |
| 56      | D1    | A1     | Wire SCL<sup>1</sup>    | Group 0 | P0.02   |
| 57      | D2    | A2     | Serial1 CTS | Group 0 | P0.28   |
| 58      | D3    | A3     | Serial1 RTS | Group 0 | P0.30   |
| 41      | D4    | A4     | SPI MOSI    | Group 1 | P0.31   |
| 40      | D5    | A5     | SPI MISO    | Group 1 | P0.29   |
| 39      | D6    | A6     | SPI SCK     | Group 1 | P0.04   |
| 38      | D7    | A7     | SPI SS, WKP | Group 1 | P0.05   |
| 72      | D8    |        | Serial1 TX, Wire3 SCL  | Group 2 | P0.06   |
| 71      | D9    |        | Serial1 RX, Wire3 SDA  | Group 2 | P0.08   |

<sup>1</sup>Pull-up resistors are not included. When using as an I2C port, external pull-up resistors are required.

---

#### System peripheral GPIO

| Name | Description | Location | 
| :---: | :--- | :---: |
| BTN | MODE Button | P1.13 | 
| PMIC_INT | PMIC Interrupt | P0.26 | 
| LOW_BAT_UC | Fuel Gauge Interrupt | IOEX 0.0 |
| RTC_INT | Real-time clock Interrupt | P0.27 |
| BGRST | Cellular module reset | P0.7 |
| BGPWR | Cellular module power | P0.8 |
| BGVINT | Cellular power on detect  | P1.14 |
| BGDTR | Cellular module DTR | IOEX 1.5 |
| CAN_INT | CAN interrupt | P1.9 |
| CAN_RST | CAN reset | IOEX 1.6 |
| CAN_PWR | 5V boost converter enable | IOEX 1.7 |
| CAN_STBY | CAN standby mode | IOEX 0.2 |
| CAN_RTS0 | CAB RTS0 | IOEX 1.4 |
| CAN_RTS1 | CAN RTS1 | IOEX 1.2 |
| CAN_RTS2 | CAN RTS2 | IOEX 1.3 |
| SEN_INT | IMU interrupt | P1.7 |
| ANT_SW1 | BLE antenna switch | P1.15 |
| GPS_PWR | u-blox GNSS power | IOEX 0.6 | 
| GPS_INT | u-blox GNSS interrupt | IOEX 0.7 | 
| GPS_BOOT | u-blox GNSS boot mode | IOEX 1.0 | 
| GPS_RST | u-blox GNSS reset | IOEX 1.1 | 
| WIFI_EN | ESP32 enable | IOEX 0.3 |
| WIFI_INT | ESP32 interrupt | IOEX 0.4 |
| WIFI_BOOT | ESP32 boot mode | IOEX 0.5 |



### Status LED

The Tracker SoM does not have an on-module RGB system status LED. We have provided its individual control pins for you to connect an LED of 
your liking. This will allow greater flexibility in the end design of your products.

Device OS assumes a common anode RGB LED. One common LED that meets the requirements is the 
[Cree CLMVC-FKA-CL1D1L71BB7C3C3](https://www.digikey.com/product-detail/en/cree-inc/CLMVC-FKA-CL1D1L71BB7C3C3/CLMVC-FKA-CL1D1L71BB7C3C3CT-ND/9094273 CLMVC-FKA-CL1D1L71BB7C3C3) 
which is inexpensive and easily procured. You need to add three current limiting resistors. With this LED, we typically use 1K ohm current limiting resistors. 
These are much larger than necessary. They make the LED less blinding but still provide sufficient current to light the LEDs. 
If you want maximum brightness you should use the calculated values - 33 ohm on red, and 66 ohm on green and blue.

A detailed explanation of different color codes of the RGB system LED can be found [here](/tutorials/device-os/led/).

## Technical specifications

### Absolute maximum ratings <sup>[1]</sup> <i class="icon-attention"></i>

| Parameter | Symbol | Min | Typ | Max | Unit |
|:---|:---|:---:|:---:|:---:|:---:|
| Supply Input Voltage | V<sub>IN-MAX</sub> |  |  | +17 | V |
| Supply Input Current | I<sub>IN-MAX-L</sub> |  |  | 1 | A |
| Battery Input Voltage | V<sub>LiPo</sub> |  |  | +6 | V |
| Supply Output Current | I<sub>3V3-MAX-L</sub> |  |  | 800 | mA |
| Storage Temperature | T<sub>stg</sub> | -30 |  | +75 | °C |
| ESD Susceptibility HBM (Human Body Mode) | V<sub>ESD</sub> |  |  | 2 | kV |
| CAN Supply Current | | | 500 | mA |

<sup>1</sup> Stresses beyond those listed under absolute maximum ratings may cause permanent damage to the device. These are stress ratings
only, and functional operation of the device at these or any other conditions beyond those indicated under recommended operating
conditions is not implied. Exposure to absolute-maximum-rated conditions for extended periods may affect device reliability.


#### Supply voltages

| Parameter | Symbol | Min | Typ | Max | Unit |
|:---|:---|:---:|:---:|:---:|:---:|
| **Supply voltages** | | | | | |
| Supply Input Voltage | VIN | -2.0 |  | +22.0 | V |
| VBUS USB supply voltage | VUSB | -0.3 |  | +5.8 | V |
| Supply Output Voltage | V<sub>IN</sub> |  | +4.8 |  | V |
| Supply Output Voltage | V<sub>3V3</sub> |  | +3.3 |  | V |
| LiPo Battery Voltage | V<sub>LiPo</sub> | -0.5 |  | +6.0 | V |
| CAN Supply Voltage | | 5 | | V |
| CAN Supply Current | | | 500 | mA |
| **I/O pin voltage** | | | | | | 
| VI/O | IO | -0.3 |  | +3.6 | V |
| **NFC antenna pin current** | | | | | |
| I<sub>NFC1/2</sub> | NFC1/NFC2 | | | 80 | mA |
| **Radio**| | | | | |
| BT RF input level (52840)| | | | 10 | dBm |
| **Environmental**| | | | | |
| Storage  temperature | | -40 | | +85 |°C |


<sup>1</sup> Stresses beyond those listed under absolute maximum ratings may cause permanent damage to the device. These are stress ratings
only, and functional operation of the device at these or any other conditions beyond those indicated under recommended operating
conditions is not implied. Exposure to absolute-maximum-rated conditions for extended periods may affect device reliability.


---

### Recommended operating conditions

| Parameter | Symbol | Min | Typ | Max | Unit |
| :---|:---|:---:|:---:|:---:|:---:
| **Supply voltages** |
| Supply Input Voltage | VIN | 3.9 |  | 17.0 | V |
| VBUS USB supply voltage | VUSB | 4.35 | 5.0 | 5.5 | V |
| LiPo Battery Voltage | V<sub>LiPo</sub> | 3.6 |  | 4.3 | V |
| **Environmental** |
| Normal operating temperature<sup>1</sup> | | -20 | +25 | +75<sup>3</sup> | °C |
| Extended operating temperature<sup>2</sup> | | -40 |  | +85 | °C |
| Humidity Range Non condensing, relative humidity | | | | 95 | % |

**Notes:**

<sup>1</sup> Normal operating temperature range (fully functional and meet 3GPP specifications).

<sup>2</sup> Extended operating temperature range (RF performance may be affected outside normal operating range, though module is fully functional)

<sup>3</sup> The maximum operating temperature is 75°C on the B523 (Quectel) but is 65°C on the B402 (u-blox LTE M1). For compatibility across modules, limit this to 65°C. 

---

### GNSS specifications

- u-blox NEO-M8U untethered dead reckoning module including 3D inertial sensors
- SPI Interface 
- Supports GPS L1C/A, SBAS L1C/A, QZSS L1C/A, QZSS L1-SAIF, GLONASS L1OF, BeiDou B1I, and Galileo E1B/C

| Parameter | Specification |
| :--- | :--- |
| Dynamics operational limit<sup>1</sup> | &le; 4g | 
| Altitude operational limit<sup>1</sup> | 50000 m | 
| Velocity operational limit<sup>1</sup> | 500 m/s | 
| Velocity accuracy<sup>2</sup> | 0.5 m/s |
| Heading accuracy<sup>2</sup> | 1 degree |
| Max navigation update rate<sup>3</sup> | 30 Hz |
| Max navigation latency<sup>3</sup> | < 10 ms | 

| Parameter |                                       | GPS & GLONASS | GPS      | GLONASS  | BeiDou   | Galileo  |
| :--- | :---                                       | :---          | :---     | :---     | :---     | :---     |
| Time-To-First Fix<sup>5</sup> | Cold start        | 26s           | 30s      | 31s      | 39s      | 57s      |
|                               | Hot start         | 1.5s          | 1.5s     | 1.5s     | 15.s     | 1.5s     |
|                  | Aided start<sup>6</sup>        | 3s            | 3s       | 3s       | 7s       | 7s       |
| Sensitivity <sup>78</sup> | Tracking & Navigation | -160 dBm      | -160 dBm | -157 dBm | -160 dBm | -154 dBm |
|                           | Reacquisiton          | -160 dBm      | -159 dBm | -156 dBm | -155 dBm | -152 dBm |
|                           | Cold Start            | -148 dBm      | -147 dBm | -145 dBm | -143 dBm | -133 dBm |
|                           | Hot Start             | -157 dBm      | -156 dBm | -155 dBm | -155 dBm | -151 dBm |
| Horizontal positioning accuracy | Autonomous <sup>9</sup> | 2.5m  | 2.5m     | 4.0m     | 3.0m     | TBC<sup>10</sup> |
|                         | With SBAS<sup>11</sup>  | 1.5m          | 1.5m     | -        | -        | -        |
| Altitude accuracy       | With SBAS<sup>12</sup>  | 3.5m          | 3.0m     | 7.0m     | 5.0m     | -        |

<sup>1</sup> Configured for Airborne < 4g platform

<sup>2</sup> 50% at 30 m/s

<sup>3</sup> High navigation rate mode

<sup>5</sup> All satellites at -130 dBm, except Galileo at -127 dBm

<sup>6</sup> Dependent on aiding data connection speed and latency

<sup>7</sup> Demonstrated with a good external LNA

<sup>8</sup> Configured min. CNO of 6 dB/Hz, limited by FW with min. CNO of 20 dB/Hz for best performance 

<sup>9</sup> CEP, 50%, 24 hours static, -130 dBm, > 6 SVs

<sup>10</sup> To be confirmed when Galileo reaches full operational capability

<sup>11</sup> CEP, 50%, 24 hours static, -130 dBm, > 6 SVs

<sup>12</sup> CEP, 50%, 24 hours static, -130 dBm, > 6 SVs

GNSS GPIO:

| Name | Description | Location | 
| :---: | :--- | :---: |
| GPS_PWR  | u-blox GNSS power | IOEX 0.6 | 
| GPS_INT  | u-blox GNSS interrupt | IOEX 0.7 | 
| GPS_BOOT | u-blox GNSS boot mode | IOEX 1.0 | 
| GPS_RST  | u-blox GNSS reset | IOEX 1.1 | 
| GPS_CS   | CAN SPI Chip Select | CS Decoder 4 | 


---

### CAN Specifications

- Microchip MCP25625 CAN Controller with Integrated Transceiver
- SPI Interface
- Implements CAN2.0B (ISO11898-1)
- Implements ISO-11898-2 and ISO-11898-5 standard physical layer requirements
- Up to 1 Mb/sec operation
- 3 transmit buffers with prioritization and abort features
- 2 receive buffers
- 6 filters and 2 masks with optional filtering on the first 2 data bytes
- CAN bus pins are disconnected when device is unpowered
- High-ESD protection on CANH and CANL, meets IEC61000-4-2 up to ±8 kV
- Very low standby current, 10 uA, typical
- 5V step-up converter (XCL9142F40CER), 400 mA maximum
- CAN terminator resistor is not included

CAN GPIO:

| Name | Description | Location | 
| :---: | :--- | :---: |
| CAN_INT | CAN interrupt | P1.9 |
| CAN_RST | CAN reset (LOW = reset for 100 milliseconds) | IOEX 1.6 |
| CAN_PWR | 5V boost converter enable (HIGH = on) | IOEX 1.7 |
| CAN_STBY | CAN standby mode (HIGH = standby) | IOEX 0.2 |
| CAN_RTS0 | CAB RTS0 | IOEX 1.4 |
| CAN_RTS1 | CAN RTS1 | IOEX 1.2 |
| CAN_RTS2 | CAN RTS2 | IOEX 1.3 |
| CAN_CS   | CAN SPI Chip Select | CS Decoder 7 | 


CANH, CANL Absolute Maximum Ratings:

| Parameter | Maximum |  
| :--- | :--- | 
| DC Voltage at CANH, CANL | -58V to +58V |
| Transient Voltage on CANH, CANL (ISO-7637) | -150V to +100V |
| ESD Protection on CANH and CANL Pins (IEC 61000-4-2) | ±8 kV |
| ESD Protection on CANH and CANL Pins (IEC 801; Human Body Model) | ±8 kV |

CAN Tranceiver Characteristics

| Parameter | Symbol | Min | Typ | Max | Unit | Conditions |
|:---|:---|:---:|:---:|:---:|:---:| :--- |
| Supply Input Voltage | V<sub>DDA</sub> |  | 5.0 |  | V | |
| Supply Current | I<sub>DD</sub> | | 5 | 10 | mA | Recessive; V<sub>TXD</sub> = V<sub>DDA</sub> |
| | | | 45 | 70 | mA | Dominant; V<sub>TXD</sub> = 0V |
| Standby Current | I<sub>DDS</sub> | | 5 | 15 | µA |  Includes I<sub>IO</sub> |
| CANH, CANL Recessive Bus Output Voltage | V<sub>O(R)</sub> | 2.0 | 2.5 | 3.0 | V | V<sub>TXD</sub> = V<sub>DDA</sub> |
| CANH, CANL Bus Output Voltage in Standby | V<sub>O(S)</sub> | -0.1 | 0.0 | +0.1 | V | STBY = V<sub>TXD</sub> = V<sub>DDA</sub>; No load |
| Recessive Output Current | I<sub>O(R)</sub> | -5 | | +5 | mA | -24V < V<sub>CAN</sub> < +24V |
| CANH: Dominant Output Voltage | V<sub>O(D)</sub> | 2.75 | 3.5 | 4.5 | V | T<sub>XD</sub> =0; R<sub>L</sub> = 50 to 65&#8486; |
| CANL: Dominant Output Voltage | V<sub>O(D)</sub> | 0.5 | 1.5 | 2.25 | V | R<sub>L</sub> = 50 to 65&#8486; |
| Dominant: Differential Output Voltage | V<sub>O(DIFF)</sub> | 1.5 | 2.0 | 3.0 | V | T<sub>XD</sub> = V<sub>SS</sub>; R<sub>L</sub> =50 to 65&#8486; | 
| Recessive: Differential Output Voltage | | -120 | 0 | 12 | mV | T<sub>XD</sub> = V<sub>DDA</sub>; R<sub>L</sub> =50 to 65&#8486; | 
|  |  | -500 | 0 | 50 | mV | T<sub>XD</sub> = V<sub>DDA</sub>; No load | 
| CANH: Short-Circuit Output Current | I<sub>O(SC)</sub> | -120 | 85 | | mA | V<sub>TXD</sub> = V<sub>SS</sub>; V<sub>CANH</sub> = 0V; CANL: floating |
| CANL: Short-Circuit Output Current | | | 75 | 120 | mA | V<sub>TXD</sub> = V<sub>SS</sub>; V<sub>CANL</sub> = 18V; CANH: floating |
| Recessive Differential Input Voltage | V<sub>DIFF(R)(I)</sub> | -1.0 | | +0.5 | V | Normal mode; -12V < V<sub>(CANH, CANL)</sub> < +12V| 
|  | | -1.0 | | +0.4 | V | Standby mode; -12V < V<sub>(CANH, CANL)</sub> < +12V| 
| Dominant Differential Input Voltage | V<sub>DIFF(D)(I)</sub> | 0.9 | | 5.0 | V | Normal mode; -12V < V<sub>(CANH, CANL)</sub> < +12V| 
|  | | 1.0 | | 5.0 | V | Standby mode; -12V < V<sub>(CANH, CANL)</sub> < +12V| 


---

### Other components

#### IMU (Inertial Measurement Unit)

- Bosch Sensortec BMI160
- SPI Interface connected to SPI1 (MISO1, MOSI1, SCK1) 
- Chip Select: SEN_CS (CS Decoder 2)
- Can wake nRF52 MCU on movement (SEN_INT1)

- 16 bit digital, triaxial accelerometer and triaxial gyroscope
- Very low power consumption: typically 925 μA with accelerometer and gyroscope in full operation
- Allocatable FIFO buffer of 1024 bytes (capable of handling external sensor data)
- Hardware sensor time-stamps for accurate sensor data fusion
- Integrated interrupts for enhanced autonomous motion detection


#### PMIC

- Texas Instruments bq24195
- I2C interface (Wire1 address 0x6B)
- Can interrupt nRF52 MCU on charge status and fault

- Handles switching between USB, VIN, and battery power
- LiPo battery charger
- Charge safety timer, thermal regulation, and thermal shutdown
- Optional connection for battery thermistor

#### Fuel Gauge

- MAX17043
- I2C interface (Wire1 address 0x36)
- Can interrupt nRF52 MCU on low battery

- Fuel-gauge system for single cell lithium-ion (Li+) batteries
- Precision voltage measurement ±12.5mV Accuracy to 5V
- Accurate relative capacity (RSOC) Ccalculated from ModelGauge algorithm
- No offset accumulation on measurement
- No full-to-empty battery relearning necessary


#### RTC/Watchdog

- Ambiq Micro AM18X5 Real-Time Clock with Power Management
- 55 nA power consumption
- Crystal oscillator
- I2C interface (Wire1 address 0x68)
- Can wake MCU from hibernate (SLEEP_MODE_DEEP) at a specific time using `RTC_INT`.
- Programmable hardware watchdog
- RTC powered by XC6504 ultra-low consumption regulator so the main TPS62291 can be shut down from RTC

---

#### Wi-Fi Geolocation

The Wi-Fi module is intended for Wi-Fi geolocation only. It cannot be used as a network interface instead of using cellular. 
An external service provider such as the Google Geolocation Service is required for mapping Wi-Fi networks to a location.

- ESP32-D2WD
- SPI Interface 
- Connected to SPI1 (MISO1, MOSI1, SCK1) 
- Chip Select: WIFI_CS (CS Decoder 3)
- Interrupt: ESP32 IO4 is connected to MCP23517T I/0 Expander GPA4.

The SoM connector has several pins dedicated to Wi-Fi:

| # |	Pin	 | Function | Connected To |	Description |
| :---: | :---: | :---: | :---: | --- |
| 11 | WIFI_EN | IO | WIFI & IOEX | ESP32 enable. Can be controlled by this pin or software.  |
| 12 | WIFI_BOOT | IO | WIFI & IOEX | ESP32 boot mode. Can be controlled by this pin or software.  |
| 13 | WIFI_TXD | OUT | WIFI | ESP32 serial TX |
| 14 | WIFI_RXD | IN | WIFI | ESP32 serial TX |

The WIFI_EN pin turns on the Wi-Fi module. LOW=Off, HIGH=On. The default is off (with a 100K weak pull-down). It can be turned on from Pin 11 on the SoM 
connection, or in software from the MCP23S17 I/0 Expander 0.3.

The WIFI_BOOT pin enables programming mode. 


#### 3.3V Regulator

- Texas Instruments TPS62291
- 1.0A at 3.3V
- Powers nRF52840 MCU and ESP32 Wi-Fi module
- Can be used by your base board to power 3.3V components
- 3.3V supply can be powered down from the RTC/Watchdog

### Radio specifications

#### nRF52840
- Bluetooth® 5, 2.4 GHz
  - 95 dBm sensitivity in 1 Mbps Bluetooth® low energy mode
  - 103 dBm sensitivity in 125 kbps Bluetooth® low energy mode (long range)
  - 20 to +8 dBm TX power, configurable in 4 dB steps

#### 4G LTE cellular characteristics for EG91-EX

| Parameter | Value |
| --- | --- |
| Protocol stack | 3GPP Release 13 |
| RAT | LTE Cat 1 |
| LTE FDD Bands | Band 28 (700 MHz) |
| | Band 20 (800 MHz)  |
| | Band 8 (900 MHz)  |
| | Band 3 (1800 MHz)  |
| | Band 1 (2100 MHz)  |
| | Band 7 (2600 MHz)  |
| WCDMA Bands | Band 8 (900 MHz) | 
| | Band 1 (2100) |
| GSM Bands | EGSM900 (900 MHz) |
| | DCS1800 (1800 MHz) |
| Power class | Class 4 (33dBm ± 2dB) for EGSM900 |
| | Class 1 (30dBm ± 2dB) for DCS1800 |
| | Class E2 (27dBm ± 3dB) for EGSM900 8-PSK |
| | Class E2 (26dBm ± 3dB) for DCS1800 8-PSK |
| | Class 3 (24dBm ± 3dB) for WCDMA bands |
| | Class 3 (23dBm ± 2dB) for LTE FDD bands |

#### 4G LTE cellular characteristics for BG96-NA

| Parameter | Value |
| --- | --- |
| Protocol stack | 3GPP Release 13 |
| RAT | LTE Cat M1 |
|     | EGPRS |
| LTE FDD Bands | Band 12 (700 MHz) |
| | Band 13 (700 MHz)  |
| | Band 4 (1700 MHz)  |
| | Band 2 (1900 MHz)  |
| GSM Bands | EGSM850 (850 MHz) |
| | DCS1900 (1900 MHz) |

#### ESP32

Espressif Systems ESP32 for Wi-Fi geolocation:

| Feature | Description|
| :-------|:-----------|
| WLAN Standards | IEEE 802.11b/g/n |
| Antenna Port | Single Antenna |
| Frequency Band | 2412 to 2484 MHz |

---

### I/O Characteristics 

These specifications are based on the nRF52840 datasheet.


| Symbol | Parameter | Min | Typ | Max | Unit |
| :---------|:-------|:---:|:---:|:---:|:---: |
| VIH | Input high voltage | 0.7 xVDD |  | VDD | V |
| VIL | Input low voltage | VSS |  | 0.3 xVDD | V | 
| VOH,SD | Output high voltage, standard drive, 0.5 mA, VDD ≥1.7 | VDD - 0.4 |  | VDD | V | 
| VOH,HDH | Output high voltage, high drive, 5 mA, VDD >= 2.7 V | VDD - 0.4 |  | VDD | V | 
| VOH,HDL | Output high voltage, high drive, 3 mA, VDD >= 1.7 V  | VDD - 0.4 |  | VDD | V | 
| VOL,SD | Output low voltage, standard drive, 0.5 mA, VDD ≥1.7 | VSS |  | VSS + 0.4 | V | 
| VOL,HDH | Output low voltage, high drive, 5 mA, VDD >= 2.7 V | VSS |  | VSS + 0.4 | V | 
| VOL,HDL | Output low voltage, high drive,3 mA, VDD >= 1.7 V | VSS  |  | VSS + 0.4 | V |  
| IOL,SD | Current at VSS+0.4 V, output set low, standard drive, VDD≥1.7 | 1 | 2 | 4 | mA | 
| IOL,HDH | Current at VSS+0.4 V, output set low, high drive, VDD >= 2.7V | 6 | 10 | 15 | mA | 
| IOL,HDL | Current at VSS+0.4 V, output set low, high drive, VDD >= 1.7V | 3 |  |  | mA | 
| IOH,SD | Current at VDD-0.4 V, output set high, standard drive, VDD≥1.7 | 1 | 2 | 4 | mA | 
| IOH,HDH | Current at VDD-0.4 V, output set high, high drive, VDD >= 2.7V | 6 | 9 | 14 | mA | 
| IOH,HDL | Current at VDD-0.4 V, output set high, high drive, VDD >= 1.7V | 3 |  |  | mA | 
| tRF,15pF | Rise/fall time, standard drivemode, 10-90%, 15 pF load<sup>1</sup> |  | 9 |  | ns | 
| tRF,25pF | Rise/fall time, standard drive mode, 10-90%, 25 pF load<sup>1</sup> |  | 13 |  | ns |  
| tRF,50pF | Rise/fall time, standard drive mode, 10-90%, 50 pF load<sup>1</sup> |  | 25 |  | ns | 
| tHRF,15pF | Rise/Fall time, high drive mode, 10-90%, 15 pF load<sup>1</sup> |  | 4  | | ns | 
| tHRF,25pF | Rise/Fall time, high drive mode, 10-90%, 25 pF load<sup>1</sup> |  | 5 |  | ns | 
| tHRF,50pF | Rise/Fall time, high drive mode, 10-90%, 50 pF load<sup>1</sup> |  | 8 |  | ns | 
| RPU | Pull-up resistance | 11 | 13 | 16 | kΩ | 
| RPD | Pull-down resistance | 11 | 13 | 16 | kΩ | 
| CPAD | Pad capacitance |  | 3 |  | pF | 
| CPAD_NFC | Pad capacitance on NFC pads  |  | 4 |  | pF | 
| INFC_LEAK | Leakage current between NFC pads when driven to different states |  | 1 | 10 | μA |  


<sup>1</sup>Rise and fall times based on simulations

## Mechanical specifications

### Dimensions and Weight

| Parameter | Value | Units |
| :-------- |  ---: | :---- |
| Width     |    28 | mm    |
| Length    |    93 | mm    |
| Thickness |     4 | mm    |
| Weight    |       | g     |

Weight will be provided at a later date.

### Mechanical drawing

Will be provided at a later date.

Dimensions are in millimeters.

---

### Schematics

#### MCU

<div align="center"> <a href="/assets/images/at-som/mcu.png" target="_blank"> <img src="/assets/images/at-som/mcu.png" class="full-width"></a></div> 

#### Cellular

<div align="center"> <a href="/assets/images/at-som/cell.png" target="_blank"> <img src="/assets/images/at-som/cell.png" class="full-width"></a></div> 

---

#### GNSS

<div align="center"> <a href="/assets/images/at-som/gnss.png" target="_blank"> <img src="/assets/images/at-som/gnss.png" class="full-width"></a></div> 

#### Wi-Fi

<div align="center"> <a href="/assets/images/at-som/wifi.png" target="_blank"> <img src="/assets/images/at-som/wifi.png" class="full-width"></a></div> 

---

#### CAN

<div align="center"> <a href="/assets/images/at-som/can.png" target="_blank"> <img src="/assets/images/at-som/can.png" class="full-width"></a></div> 

#### User I/O

<div align="center"> <a href="/assets/images/at-som/user-io.png" target="_blank"> <img src="/assets/images/at-som/user-io.png"></a></div> 

---

#### PMIC

<div align="center"> <a href="/assets/images/at-som/pmic.png" target="_blank"> <img src="/assets/images/at-som/pmic.png" class="full-width"></a></div> 

#### Fuel Gauge

<div align="center"> <a href="/assets/images/at-som/fuel.png" target="_blank"> <img src="/assets/images/at-som/fuel.png"></a></div> 

#### Cell Control

<div align="center"> <a href="/assets/images/at-som/cell-control.png" target="_blank"> <img src="/assets/images/at-som/cell-control.png"></a></div> 

---

#### I/O Expander

<div align="center"> <a href="/assets/images/at-som/ioex.png" target="_blank"> <img src="/assets/images/at-som/ioex.png"></a></div> 

#### QSPI Flash

<div align="center"> <a href="/assets/images/at-som/flash.png" target="_blank"> <img src="/assets/images/at-som/flash.png"></a></div> 

#### RTC/Watchdog

<div align="center"> <a href="/assets/images/at-som/rtc.png" target="_blank"> <img src="/assets/images/at-som/rtc.png"></a></div> 

---

#### IMU

<div align="center"> <a href="/assets/images/at-som/imu.png" target="_blank"> <img src="/assets/images/at-som/imu.png"></a></div> 

#### 3V3 Regulator

<div align="center"> <a href="/assets/images/at-som/3v3-regulator.png" target="_blank"> <img src="/assets/images/at-som/3v3-regulator.png"></a></div> 


### Layout Considerations

Will be provided at a later date.

---

## Product Handling

### ESD Precautions
The Tracker SoM contains highly sensitive electronic circuitry and is an Electrostatic Sensitive Device (ESD). Handling an module without proper ESD protection may destroy or damage it permanently. Proper ESD handling and packaging procedures must be applied throughout the processing, handling and operation of any application that incorporates the module. ESD precautions should be implemented on the application board where the B series is mounted. Failure to observe these precautions can result in severe damage to the module!

### Connectors

The U.FL antenna connectors are not designed to be constantly plugged and unplugged. The antenna pin is static sensitive and you can destroy the radio with improper handling. A tiny dab of glue (epoxy, rubber cement, liquid tape or hot glue) on the connector can be used securely hold the plug in place.

### Disposal

![WEEE](/assets/images/weee.png)


## Default settings

The AssetTracker SoM comes pre-programmed with a bootloader and a user application called Tinker. This application works with an iOS and Android app also named Tinker that allows you to very easily toggle digital pins, take analog and digital readings and drive variable PWM outputs.

The bootloader allows you to easily update the user application via several different methods, USB, OTA, Serial Y-Modem, and also internally via the Factory Reset procedure. All of these methods have multiple tools associated with them as well.

## Ordering Information

| SKU  | Description | Packaging |
| :--- | :--- | :--- |
| | T523 Family (Europe) | |
| T523MEA  | Tracker SoM LTE CAT1/3G/2G (Europe), [x1] | Each |
| T523MTY  | Tracker SoM LTE CAT1/3G/2G (Europe), Tray [x50] | Tray (50) |
| T523MKIT | Tracker SoM LTE CAT1/3G/2G (Europe) Evaluation Kit, [x1] |	Each |
| | T402 Family (North America) | |
| T402MEA | Tracker SoM LTE M1 (NorAm), [x1]	| Each |
| T402MTY | Tracker SoM LTE M1 (NorAm), Tray [x50]	| Tray (50) |
| T402MKIT | Tracker SoM LTE M1 (NorAm) Evaluation Kit, [x1]	| Each |

## Revision history

| Revision | Date | Author | Comments |
|:---------|:-----|:-------|:---------|
| pre1     | 31 Mar 2020 | RK | Preview Release 1 |
| pre2     | 12 May 2020 | RK | Added partial dimensions |
| 001      | 29 Jun 2020 | RK | First release |
| 002      | 10 Jul 2020 | RK | Updated absolute maximum ratings, schematics |