This is a C++ library for Arduino, built and tested on version 1.8.10, and based on Arduino's public Test library
and 9555 expander examples.

Installation
--------------------------------------------------------------------------------

To install this library, download the .zip file and with Arduino open, go 
'Sketch->Include Library->Add .ZIP Library...' and open the downloaded file. Or, 
just place the extracted library as a subfolder in your Arduino/libraries folder.

When installed, this library should look like:

Arduino/libraries/Plex64                (this library's folder)
Arduino/libraries/Plex64/Plex64.cpp     (the library implementation file)
Arduino/libraries/Plex64/Plex64.h       (the library description file)
Arduino/libraries/Plex64/keywords.txt   (the syntax coloring file)
Arduino/libraries/Plex64/examples       (the examples in the "open" menu)
Arduino/libraries/Plex64/readme.txt     (this file)

Building
--------------------------------------------------------------------------------

To use this library in a sketch, go to the Sketch | Import Library menu and
select Plex64.  This will add a corresponding line to the top of your sketch:
#include <Plex64.h>

To stop using this library, delete that line from your sketch.

Usage
--------------------------------------------------------------------------------

Constructor: Plex64(uint8_t address, uint8_t pinE, uint8_t pinF, uint8_t pinG, uint8_t pinH);
  Parameter address: the I2C address of the 9555 IO expander. Board defaults to
  0x20 but can be changed to 0x20-0x27 with solder jumpers A0/A1/A2.

  Parameters pinE, pinF, pinG, pinH: the Arduino analog pin numbers that are 
  connected to the shield E/F/G/H channels. Default are A0, A1, A2 and A3 
  respectively when shield is plugged directly into a standard Arduino.

void begin(void);
  No parameters or return, this must be run inside setup(). Performs startup 
  tasks for I2C interface, analog inputs and IO expander.

void setAllChannels(uint8_t input);
  Sets all 4 channels to the same input.
  Parameter input: the input number (0-15) that will be used to set all 4 channels.

void setChannel(uint8_t pin);
  Sets one single channel to a given input. First it checks the _outputRegister private
  variable and does not update the IO expander if it should already be set.
  
  Parameter pin: must be either an integer from 0-63, or a pin name E0-15, F0-15,
  G0-15, H0-15 which will be translated to 0-63 int using enum table.

uint16_t readAnalog(uint8_t pin);
  Sets one single channel to a given input (if not set already), and then performs 
  and returns analogRead from the same input. analogRead will be affected by
  standard Arduino functions such as analogReadResolution().

  Parameter pin: must be either an integer from 0-63, or a pin name E0-15, F0-15,
  G0-15, H0-15 which will be translated to 0-63 int using enum table.

uint16_t getAllChannels(void);
  Gets and returns full 16-bit contents of IO expander output register, which
  can be used to confirm that the expander is being set correctly.