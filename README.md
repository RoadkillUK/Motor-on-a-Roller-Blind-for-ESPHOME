# Motor-on-a-Roller-Blind-for-ESPHOME
ESPHome controlled blinds.

This setup uses a Robotdyne ESP8266 Wifi D1 Mini and a 28BYJ-48 stepper motor + ULN2003 driver board.

I made this sketch because I would prefer to have my blinds running ESPHome rather than MQTT.

It will run on it's own, without HA as long as you have a Button connected to D7 and GND, (this button is also used for manual operation).

This sketch will replace the original 'Motor on a roller blind' which I have been using for some time myself and it's been almost perfect.

To upload via WIFI, go to https://steve.fi/hardware/ota-upload/ and follow the instructions, took less than one minute to upload and configure!

You should only have to set up the blind once, it stores the open/closed status and position even if you flash the device, it will report open/closed so HA will know where your blind is, even after restarting HA.

 This sketch will add 2 switches named <upper_devicename> Setup Switch and Setup Button
 Use your mobile or tablet and connect to http://<devicename>.local to set up the blind

 1) Turn on the Setup Switch to enter setup mode
 2) Press Setup button to start the blind closing
 3) Press Setup button again when closed and blind starts to open (actually resets the stepper position to 0)
 4) Press Setup button again when blind is fully open
 5) Job Done

 This sketch also includes a momentary button on D7 which can be used in the following way

 1) Press button for > 1 second to enter setup mode
 2) Press button again to start the blind closing
 3) Press button again when closed and blind starts to open (actually resets the stepper position to 0)
 4) Press button again when blind is fully open
 5) Job Done

 The button is also used to open/close the blind (must be fully open/closed first)

 NOTE:  If you find that your shades are going the wrong way, you can change the pin
        settings below or reverse the + and – wires for each of the A and B motor
        pairs on your driver and the motor will spin in the opposite direction.

