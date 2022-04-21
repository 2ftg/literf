## What if hackRF, but cheap?

This repository contains hardware designs and software for the liteRF project,
a low cost, open source Software Defined Radio platform based on the popular HackRF design by Great Scott Gadgets.  

This is a living project and thus not suitable for manufacturing or any use. You have been warned.

We want to reduce the cost and availability problems with the parts chosen for the original HackRF.  
Our target is to at least halve the BOM cost and avoid as many of the unobtainium and obsolete parts.  
Especially those by Maxim.

This whole project started with me finding AT86RF215's on Digikey one night and discovering their IQ interface.   
It was re-invigorated by the [CaribouLite project](https://github.com/cariboulabs/cariboulite/blob/main/docs/smi/README.md) also named Lite and using the AT86RF215.

![HackRF One](https://raw.github.com/mossmann/hackrf/master/doc/HackRF-One-fd0-0009.jpeg)

(photo by fd0 from https://github.com/fd0/hackrf-one-pictures)

This is the original hackRF, want to make it cheaper and to avoid problems with unobtainium parts.

Because all great artists steal, we will consider existing related projects when choosing parts.   
As that gives us access to existing, proven designs with support from the community.

### Component substitutions that have been decided on:
1. Maxim MAX2837+MAX5864 replaced by Microchip AT86RF215. Cheaper, more available.
2. Xilinx XC2C64A-7VQG100C CPLD replaced with Lattice ICE40LP1k-QN84
3. SiLabs/Skyworks Si5351C replaced by Hangzhou Ruimeng Tech MS5351M. Cheaper, AVAILABLE. Might require two or rethinking clocks.

Some links to candidate parts and information on them below.

#### Maxim MAX2837+MAX5864 Transceiver + AFE
Both of these are expensive and hard to find. It might change now that Analog Devices owns Maxim, or get worse.   
Replacing these is the origin of this project. I had a terrible time sourcing them for my hackRF build.   

MAX2837 info:   
https://www.maximintegrated.com/en/products/comms/wireless-rf/MAX2837.html   

MAX5864 info:   

https://www.maximintegrated.com/en/products/analog/data-converters/analog-front-end-ics/MAX5864.html

#### Xilinx XC2C64A-7VQG100C CPLD with Lattice ICE40LP1k-QN84 FPGA
0 stock and 52 week lead time on Digikey, nuff said:   
https://www.digikey.fi/en/products/detail/xilinx-inc/XC2C64A-7VQG100C/949460
Datasheet: https://docs.xilinx.com/v/u/en-US/ds311

In addition to sourcing problems, a 64 macrocell CPLD can't do what we want to the 128Mbit/second 64MHz DDR clocked LVDS interface AT86RF215 has.   
Thus we replace it with Lattice ICE40LP1k-QN84. This has a FOSS toolchain and a free tool chain from the manufacturer.   
It is used in the CaribouLite, so it works with AT86RF215 amd the gateware is under a permissive license. So we are not starting from 100% scratch.  
We need some fast programmable gluelogic here and Lattice iCE40 is a good choise due to low cost, good FOSS community and okay-ish [availability](https://octopart.com/ice40lp1k-qn84-lattice+semiconductor-22303412).   
I might have to reconsider this if I cannot source it or any iCE40 device with the same or larger amount of LUT's. I'm open to suggestions.   
iCE40LP family datasheet:    https://www.latticesemi.com/~/media/LatticeSemi/Documents/DataSheets/iCE/iCE40LPHXFamilyDataSheet.pdf

#### SiLabs/Skyworks Si5351C replaced with Hangzhou Ruimeng Tech MS5351M.

Skyworks Si5351C is in QFN case and has 8 outputs, compared to MS5351M's 3, so some work is likely needed. But they are register compatible and MS5351M 1:1 replaces Si5351B in almost all applications. 



### Component substitutions that are under consideration:
1. NXP LPC MCU substitution with something cheaper and more available.
2. TI TPS62410 dual VREG with two Microchip switchers and point of load low noise LDO's for RF. Due to availability and noise.
3. Skyworks SKY13317 RF switch with Maxcend MXD8641. Due to price and availability. Also for better frontend.

Some links to candidate parts and information on them.

##### WCH CH32V307VCT6 MCU candidate.
I'm unsure if this is fast enough. It's RISC-V! 
Datasheet+SDK https://github.com/openwch/ch32v307   

LQFP100 case https://lcsc.com/product-detail/Microcontroller-Units-MCUs-MPUs-SOCs_WCH-Jiangsu-Qin-Heng-CH32V307VCT6_C2943979.html

Used in [iCEBreaker 1.99 hardware](https://github.com/icebreaker-fpga/icebreaker/tree/hw-v1.99-evaluation-designs/hardware/v1.99a) with Lattice iCE40UP5K.  

##### NXP CH32V307VCT6 MCU candidate.

##### MS5351M clock generator 

Source and datasheet.   
https://lcsc.com/product-detail/Clock-Generators-Frequency-Synthesizers-PLL_Hangzhou-Ruimeng-Tech-MS5351M_C1509083.html   

QRP-Labs investigation that finds MS5351 meeting or exeeding Si5351.   
http://www.qrp-labs.com/synth/ms5351m.html

##### Maxcend MXD8641 SP4T RF switch
Source and data.
https://lcsc.com/product-detail/RF-Switches_Maxscend-MXD8641_C285559.html

NanoVNA v2, they use Maxcend switches and later also MS5351M.   
https://github.com/nanovna-v2/NanoVNA2

Some parts selection logic I have used:   
Needs to be cheaper than what it's replacing.   
If not cheaper, then more available.   
If possible, already used and thus qualified by somebody else. Possibly lower risk of surprises that way.   
Higher performance than what it is replacing.   
Easier to use use than what it is replacing.      
If possible, the case should be easily soldered and inspected. So more QFN and TQFP, less BGA and LGA.   
Available from JLCPCB SMT assembly, LCSC.com , TME.eu, Mouser or Digikey.

##### Other minor motivations and changes:
PROPER REF DES on PCB.


#### Contact
[@2ftg1](https://twitter.com/2ftg1/) on Twitter   
ftg on Ircnet with shell, as ftg on Libera.Chat from time to time. 

