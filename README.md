# build-brighton-cnc

A repository of code and information about the CNC machine at Build Brighton

You must be inducted to use this machine.


## Machine Description

#### Type

The machine is an ISEL 4433 with a FluidNC controller and a water cooled spindle.

#### Size

The maximum coordinate volume of the machine is 440 x 330 x 160 mm.
The water cooled spindle collides with the door, so the maximum usable depth is 430 mm
The bed is about 300 mm wide.
The X axis is from left to right, Y axis is from front to back, and Z axis is from top to bottom.

#### Spindle

The spindle is rated at 2.2kW and has a speed range of 250 - 24000 rpm.
Speed is controlled by the control board via pwm, start and reverse signals.

#### Spindle Coolant

Coolant for the spindle is stored in a tank under the machine. The pump is located inside the tank. The coolant is tap water and dettol floor cleaner.

#### Control

The control board is an MKS-ESP32. Connection to the controller is either by direct USB or wifi. A PC is setup for use with the machine. It can be controlled directly from a phone or tablet, but the PC is recommended. The PC is dual boot Windows 7 or Ubuntu 16.04 LTS, both of which are out of date and should be upgraded. The PC only has 2GB of ram, which is barely adequate.

## Usage

***DO NOT RUN THE MACHINE WITH THE DOOR OPEN*** Or you will damage yourself.

***DO NOT CUT DIRECTLY ON THE BED*** Or you will damage the bed.

***DO NOT CUT STEEL*** Or you may damage the bearings, ballscrews etc.

***DO NOT DROP TOOLS*** The cutting edges will be damaged and it wont cut well, or at all.

***TOOLS ARE SHARP*** They will slice your skin if handled without care.

***USE A VACUUM IF CUTTING WOOD*** Prefereably, don't cut wood. Use the Carvey for that.

#### Setting up

1. Remove any boxes, equipment or material stored in the machine cabinet. Ensure nothing will get in the way of the moving parts. Don't leave anything in there you down want covered in chips and cutting fluid.

2. Check spindle cooland level and condition. Top up or clean out and replace as required. The level must be at least above the pump, but no more than about 60% full. Make sure the return hose is pointing into the tank and that the tank is covered.

3. Connect the machine, PC, monitor to a power extension cable. Be sure not to cause a trip hazard.

4. Turn on the PC and boot into Linux or Windows. Windows is recommended for now.

5. Connect the water pump to power and check for steady flow of coolant from the return hose.

6. If air cooling is required (recommended), follow procedure for turning on the compressor and run the hose to the air connector on the left front leg.

7. Turn on the machine using the power switch on the right hand side. Release the Emergency Stop button.

8. Connect to the controller. On Windows, this can be done over USB using GRBLPlotter. However, GRBLPlotter is not fully compatible with FluidNC. Alternatively any terminal emulator can connect for a text only interface. FluidNC's web interface is not set up for DNS and is dynamically allocated, so it be found using a tool such as Ning (Android) to list all ips on the the network and connecting to each one until the web interface appears. Make sure to connect with http:// and not https://. If anyone knows how to setup a static ip address or configure the DNS, please do.

9. Home all axes using the 'Home All' button next to the jog control, or enter the command '$H'. The machine should gently move into the top left rear corner and back off, one axis at a time.

#### Running a program

1. Upload your gcode to the control board. Use a folder with your name to store your files.

2. Affix you stock material to the bed. For thin sheet material, place a sacrificial block of material under your stock to protect the machine bed. NEVER CUT DIRECTLY ON THE BED. Bar clamps can be used to hold the stock and sacrifical layer to the bed. Bar clamps must have roughly equal height material on both sides of the bolt to function correctly. For thicker and smaller peices, use the clamp/support blocks fixed to the near left corner. The cam grip should be on the right hand side so that its clamping force does not cause it to slide along the T slot. Use shim blocks if the stock doesnt reach the clamp. Make sure that your stock has enough clearance at the edges so as not to cut into the blocks.

3. Change the tool. 
  1. Press the white button on the left hand side. The machine will move the spindle over the tool height probe.
  2. Open the door and use an 18 and 22 mm spanner to remove the collet nut.
  3. Replace the collet with the correct size for your tool. The tool can be up to approx 10% smaller than the collet size and still work.
  4. Insert the collet into the nut *first*, then insert the tool, making sure that the shank of the tool reaches the rear of the collet's gipping surface.
  5. Thread the nut back on to the spindle until finger tight. Use the spanners with *reasonable* force to securely tighten the nut.
  6. **Close the door** and press the *'Start'* button. The machine will move down until the tool touches the tool height probe.
  7. Read the second tool probe Z position. Subtract from 150. Enter the command *'G43.1.Z{result}'* replacing *{result}* with the value just calcuated. eg, probe return -124.5, value entered is 150 - 124.5 = 25.5
  8. Validate the tool height offset with the command *'$#'* and check that TLO matches the value entered.

4. Locate the stock. This can be done by eye, or by gently jogging the tool (with the spindle running) into the stock untill it starts cutting. There is also a set of wigglers in the red toolchest in the metal shop that can do the same without cutting into the stock, and is potentially more accurate. Wigglers require an extra tool change. Generally, either the top centre or top near left corner are used for location.

5. Select and zero the work coordinate offset with the commands *'G54'* and *'G92 X0 Y0 Z0'*. Each axis can be zeroed independantly after locating for convenience, eg *'G92 X0'*.

6. If required, open the door and apply cutting fluid to the stock and tool. **Close the door** when done.

6. Press the play button next to your gcode in the web interface, or start sending gcode if using GRBLPlotter or similar.

7. Observe the machine. Do not let it run unattended. You will almost certainly need to adjust feeds and speeds with the override controls, make sure chips are clearing correctly, and that generally everything is ok. The amchine should sing with good settings. Bad settings will make it sound like a wood chipper.

8. Press the red *'Stop'* button at any time to hold the program. Press the green *'Start'* button to resume. To stop the program completely use the stop button in the web interface or gcode sender. In an **emergency**, use the *emergency stop* button on the right hand side near the power switch.

9. Your gcode should include a *'G28'* command at the end to return the machine to the home position. If you stop for any other reason, enter the command *'G28'* to move the machine back to home and out of the way.

10. When using multiple tools, save each tool operation as a seperate g-code. Repeat steps 3 and 6 through 9 for each tool. When using multiple setups (eg, flipping the stock upside-down, or setting it on its side) repeats steps 2 and  4 through 9 for each setup.

#### Finishing

1. Remove the tool from the collet. Clean chips and cutting fluid from the tool. Put the tool safely back in its protective packaging and return it neatly to where it belongs.

2. Ensure the machine has returned home with *'G28'*, that the spindle is stopped with *'M5'*, and that air cooling is turned off with *'M9'*.

3. Unclamp your work and remove it and any fixturing, like a sacrificial board or clamp shim blocks.

4. Clean out ***all*** the chips, cleanup any cutting fluid and degrease.

5. Turn off the pump, the machine and this PC.

6. Stash whatever machine related things you pulled out of the enclosure back inside and close the door.

7. Carefully roll up the power extension cable and return it.