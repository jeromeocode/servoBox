# Servo Box Software

## Description
This servo box uses a Mini Maestro 12-channel USB servo controller and is heavily documented on the Pololu website [https://www.pololu.com/docs/0J40]

## Prerequisites
Follow these steps [https://www.pololu.com/docs/0J40/3] to install the proper drivers and software to interface with the board.

The board also requires a USB Mini B cable.

## Installation
In the case that the board has to be completely reflashed with the script, follow these steps:
1.  Remove the jumper
2. Connect the USB Mini B cable from the board to the PC
3. Open Pololu Maestro Control Center
4. Click "Connected to" dropdown menu and select your device
5. Go to the Channel Settings Tab
6. Ensure that channels 0 to 3 (4 channels in total) have their Mode set to Servo
7. Ensure that channels 10 to 11 have their Mode set to Input
8. Set the Min and Maxes for channels 0 to 3 as Min: 1200, Max: 1808 (They do not have to be exact, the software will round for you)
9. Set the Speed of channels 0 to 3 to 25
	APPLY SETTINGS
10. Go to the Status Tab and ensure channels 0 to 3 and 10 to 11 have their checkboxes checked.
	APPLY SETTINGS
11. Go to the Script Tab and copy and paste the included script into the script text input.
12. Ensure that "Run script on startup" checkbox is checked
	APPLY SETTINGS

With this the board is properly flashed and can be disconnected from the PC. More documentation on how to use the Maestro Control Center can be found here[https://www.pololu.com/docs/0J40/4].

## Code
```
# Sequence 0
begin
  button2 button LOGICAL_AND if sysSeqBoth endif
  button button2 LOGICAL_NOT LOGICAL_AND if sysSeq1 endif
  button2 button LOGICAL_NOT LOGICAL_AND if sysSeq2 endif
  button LOGICAL_NOT button2 LOGICAL_NOT LOGICAL_AND if homeBoth endif  
repeat

sub button2
  11 get_position 500 less_than 
return

sub button
  10 get_position 500 less_than 
return

sub sysSeq1
  500 6000 6000 6000 6000 frameBoth # THIS IS HOME POSITION
  500 6000 7232 6000 6000 frameBoth
  500 4800 7232 6000 6000 frameBoth 
  500 4800 6000 6000 6000 frameBoth 
  500 6000 6000 6000 6000 frameBoth 
  500 6000 4800 6000 6000 frameBoth 
  500 7232 4800 6000 6000 frameBoth 
  500 7232 6000 6000 6000 frameBoth 
return

sub sysSeq2
  500 6000 6000 6000 6000 frameBoth 
  500 6000 6000 6000 7232 frameBoth 
  500 6000 6000 4800 7232 frameBoth 
  500 6000 6000 4800 6000 frameBoth 
  500 6000 6000 6000 6000 frameBoth 
  500 6000 6000 6000 4800 frameBoth 
  500 6000 6000 7232 4800 frameBoth
  500 6000 6000 7232 6000 frameBoth 
return


sub sysSeqBoth
  500 6000 6000 6000 6000 frameBoth 
  500 6000 7232 6000 7232 frameBoth 
  500 4800 7232 4800 7232 frameBoth
  500 4800 6000 4800 6000 frameBoth 
  500 6000 6000 6000 6000 frameBoth 
  500 6000 4800 6000 4800 frameBoth 
  500 7232 4800 7232 4800 frameBoth 
  500 7232 6000 7232 6000 frameBoth 
return

sub homeBoth
  500 6000 6000 6000 6000 frameBoth
return

sub frameBoth
  3 servo
  2 servo
  1 servo
  0 servo
  delay
  return
```
## Code Documentation
SUPPLEMENTAL DOCUMENTATION [https://www.pololu.com/docs/0J40/6]

### button/button2 Subroutine
`11 get_position 500 less_than `

This takes the Logical Voltage value of Channel 11 (0-1023) and checks if it is less than 500. If so, returns true.

### frameboth Subroutine
This subroutine can be used to input a servo position frame for both systems (all 4 servos) for a given period of time.

Example:
`500 6000 7232 5000 8000 frameBoth`

This gives Servo Channel 3: 6000, Servo Channel 2: 7232, Servo Channel 1: 5000, Servo Channel 0: 8000 for 500 ms. 

These position values given to the channels are in units of 0.25 microseconds. (Home position: 1500 microseconds -> 6000)

### homeboth Subroutine
Gives all 4 servos a target position of 6000 for 500 ms.

This is used when SW1 and SW2 is both off.

### sysSeq1
Gives servo channels 3 and 2 (System 1) the sequence loop while servo channels 1 and 0 are kept at home position.

This is used when SW1 is flipped on but SW2 is flipped off.

### sysSeq2
Gives servo channels 1 and 0 (System 2) the sequence loop while servo channels 3 and 2 are kept at home position.

This is used when SW2 is flipped on but SW2 is flipped off.

### sysSeqBoth
Gives both systems of servos the sequence loop.

This is used when both switches are on.


## Citation
Board Documentation [https://www.pololu.com/docs/0J40]
Board Purchase page [https://www.pololu.com/product/1352]
	-Click the Resources tab to find CAD files and other helpful information on the board

## Contact
Project Lead: George (Scott) Wagner [George.Wagner@bendix.com]
Electrical/Software Lead: Jerome Villapando [jerome.villapando@bendix.com]
Mechanical Lead: Jack Mason [Jack.Mason@bendix.com]

*Note that Jerome and Jack will be leaving the company Mid-December
