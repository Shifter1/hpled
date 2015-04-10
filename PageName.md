# Introduction #

This wiki serves as documentation for the HP LED driver project that began in the DIY forum at Reef Central:

http://reefcentral.com/forums/showthread.php?t=1759758


# Goals #

This project strives to design drivers for HP LEDs that meet the following goals.

  * Perform similarly to Buckpucks and other commercially-available LED drivers with respect to number of LEDs driven and drive currents possible:
  * Easy assembly for novices
  * High efficiency
  * Low cost

# NCP3066 Dual-driver #

The current release of the "dual driver" is based on the NCP3066 chip from OnSemi. The driver is implemented in a dual format, such that each PCB contains two independent driver circuits. Each driver circuit can run up to 8 LEDs and needs an input voltage less than the required LED forward voltage at the selected drive current. For 8 typical HP LEDs (Cree XR-E, Luxeon Rebel, etc) this means a 24v supply works well.

Each driver circuit has a PWM pin for dimming control. If PWM functionality is not desired, this pin MUST have voltage. You can apply the input voltage to this pin if it's 25v or less.

The downloads tab above has complete Eagle projects for two versions of the dual-driver:

  * dual\_driver: This version is intended for production at a board house.
  * dual\_driver\_homebrew: This version is intended for home production (etching or milling). The traces has been rerouted, to the back side where possible, making assembly easier if your drill holes are not through-plated, since you'll be able to solder on the back side for most components.

There are two resistors on each driver - connected to the COMP pin and the IPK pin - which are "tricky" values. In order to accommodate hobbyists that might not be able to easily find the exact values for these resistors, multiple pads have been provided. On the dual\_driver version, there are two spots for through-hole resistors and one for an SMT resistor. On the homebrew version, there are two spots for through-hole resistors. The idea is that people implementing this design will be able to mix resistors in parallel to get the correct value, if needed.

The BOM file with a version number matching the Eagle project zip contains a spreadsheet with names, values, notes, and digikey part numbers.

# ZXLD1366 Triple Driver #

The current beta release of the triple driver is based on the ZXLD1366 IC from Diodes Inc. Each IC will drive 12 LEDs up to 1A from 48v. The "common dimming" version of the driver has been designed such that a single PWM signal will dim all three strings of LEDs. Users who wish for separate dimming of each string can duplicate the base resistor and NPN transistor on the ADJ pin for each IC. Or, the IC can be fed an analog 0-2.5v DC signal on the ADJ pin for analog dimming. Please see the device's datasheet for information on dimming, as the sense resistor will need to be resized for analog dimming if the signal goes above 1.25v.

This ZXLD1366 was not fully tested because it proved slightly less cost effective than the other options described in this project.

# CAT4101 Triple Driver #

This is currently regarded as the optimal solution for most needs. The CAT4101 chip provides the best combination of features, efficiency, and cost effectiveness. The one "drawback" is that the IC is surface mount, but it's a fairly easy surface mount package, so don't be scared!

The CAT4101 IC is manufactured by OnSemi and acts as a current sink regulating up to 25v of HP LEDs at currents up to 1A. Current is set via a sense resistor. The only other components are some bypass capacitors. The IC is a linear regulator, so efficiency is good only if the voltage drop across the circuit is optimized by carefully matching the LED supply voltage to the total Vf of the LED string. The IC has a minimum drop of .5v - operated with a .5v drop, the circuit is reasonably efficient and stays cool. If the circuit requires a drop larger than several volts, it's likely that the IC will shut down due to thermal overload - so be sure to match the power supply to your LED string.

The design requires +5v input to run the IC circuit itself, and a PWM signal shared by all three ICs on the PCB. The IC performs fine with low-frequency 5v PWM signals, i.e. from an Arduino or other similar microcontroller.

# General Notes #

This project has been designed using the free version of CadSoft's Eagle program, available here:

http://www.cadsoft.de/