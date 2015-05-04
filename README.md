# FPGArduino

Arduino core extensions for FPGA-based 32-bit MIPS and RISC-V 
soft CPU cores (f32c).

Copy or symlink this to existing arduino directory 
(versions 1.6.0 and newer), close the Arduino IDE, open
it again and new FPGA boards, progammers
and examples should appear under pull down menus:

tools->boards->...new mips and riscv boards..

tools->programmers->...ujprog openocd etc...

file->examples->f32c->...

file->examples->...RCswitch...Adafruit GFX/SSD1306..

We had to do small modifications of original examples
for AVR-specific or hardware-dependent stuff in order 
to get them running out of the box on f32c.

This repository doesn't contain required compiler toolchains
for mips or risc-v, so they have to be fetched and installed
separately, see http://www.nxlab.fer.hr/fpgarduino for further
instructions.

We have here both old and new core tree.

New tree arduino/hardware/fpga
contains unified MIPS and RISC-V source trees so both
achitectures will execute the same code. This is recommended
for use.

Old tree arduino/hardware/f32c had separated source copies 
for MIPS and RISC-V it is not maintained anymore and kept here
for some time for historical reference how we started.

# What should work

Blink led :)

Serial (over usb-ttl adapter, some boards need it external)

Timer (millis(); 32-bit CPU core clock counter, glitch-free, good for realtime)

GPIO (digitalWrite(), digitalRead())

Interrupts (MIPS only, attachInterrupt() gpio rising/falling edge, timer)

PWM (analogWrite(), analogWriteResolution(), analogWriteFrequency())

Fade (PWM) works on 2 output pins (LEDs).

Software SPI (bitbang)

# What doesn't work yet

Analog inputs (cheap FPGA used to not to have ADC, 
please contribute some code for new hardware...)

Hardware SPI and I2C. Included are SPI and Wire (i2c)
libraries but they are empty, just a placeholders
for the compiler to not fail for sketches that use them.

Arduino SSD1306 OLED library uses its own bitbang SPI
so it works for SPI OLEDs out of the box, only it doesn't
work yet for I2C OLEDs.

Processor is fast enough to do software SPI and I2C
and this saves FPGA LUTs.

Our FPGA f32c soft core contains already SPI in VHDL source so
adding support for additional SPI ports for those who need
them hardware accelerated should be fairly simple.

CRC-based uploading or upload verify. 
Currently a blind upload of compiled MIPS or RISC-V binary is 
sent to the device so strange bugs may appear due to serial
transmission errors.
