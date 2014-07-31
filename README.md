notorious-PID
=============

PID fermentation control for AVR platforms

notorious PID is an open source fermentation temperature control program for homebrew use.  The code has been developed using the arduino IDE.

List of physical components:

  Arduino MEGA 2560
  Adafruit data logging shield
  20x4 character LCD
  Adafruit 24-pulse rotary encoder with pushbutton
  SainSmart 2 channel relay board
  2x DS18B20 Dallas OneWire digital temperature sensor
  3/8" stainless thermowell

Control Overview:

  A standard PID control algorithm outputs the air temperature necessary to maintain a desired fermentation setpoint. Controller output of the main PID cascades into two additional control algorithms for heating and cooling.  Final control elements are the refrigerator compressor and resistive heating element.  Temperature sensing is performed by the Dallas OneWire DS18B20.  The sensor's on-board DAC performs a conversion with up to 12-bit resolution.  Potentially limitless numbers of Dallas OneWire Sensors may be attached via a single digital input in on the AVR.

  Cooling --  The refrigerator compressor is switched by a differential control algorithm with time-based overshoot prediction capabilities.

  Heating --  A second PID instance outputs a duty cycle for time proportioned conrol of a resistive heating element lining the inner chamber walls.

Additonal Features:

  EEPROM storage -- notorious PID stores vital program states and settings in non-volatile EEPROM memory space.  If power is lost or the arduino reboots via the reset button, previous settings can be recalled from EEPROM at startup.

  Data Logging -- Logging functionality is provided by the Adafruit data logging shield.  The shield includes an SD card slot and a real time clock for accurate timestamping of data and files.  Logfiles are formatted as simple CSV with headers.  Logging operations may be enabled/disabled by the end user at any time via the menu.
  
  Temperature Profiles -- The program includes support for end-user created temperature profiles.  Profiles in CSV format may be placed in the /PROFILES/ directory of the SD card used for data logging.  Files use the 8.3 filename format with .PGM file extension and consist of comma separated pairs of setpoint temperature (deg C) and duration (hours).  During profile operation, main PID setpoint is varied according to the pairs included in the .PGM file.
  
  Failsafe -- An infinite loop or other AVR lock-up could lead to a loss of control of the final control elements.  To prevent an AVR failure from leading to unsafe operation, notorious PID makes use of the Watchdog timer feature of arduino (and similar) boards.  The Watchdog is an onboard countdown timer that will reboot the arduino if it has not recieved a reset pulse from the AVR within a set time.
  
Future Features:

  WiFi Connectivity -- Connectivity to be acomplished via the Adafruit wifi breakout with external antenna.  Data will be viewable online via the Xively service.