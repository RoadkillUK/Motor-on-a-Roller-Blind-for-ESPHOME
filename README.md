### Please download from the Releases Page [Releases](https://github.com/RoadkillUK/Motor-on-a-Roller-Blind-for-ESPHOME/releases)

# Motor-on-a-Roller-Blind-for-ESPHOME
## ESPHome controlled blinds.

This setup uses a Robotdyne ESP8266 Wifi D1 Mini and a 28BYJ-48 stepper motor + ULN2003 driver board.

Thanks to [PeterG](https://www.thingiverse.com/pgote/designs) for his 3D printed parts and [original code](https://www.thingiverse.com/thing:2392856) for the motor on a roller blind. 

In order to control the position of the blinds from HA, I use [Lovelace Slider Entity Row](https://github.com/thomasloven/lovelace-slider-entity-row) from [Thomas Lovén](https://github.com/thomasloven), use the following in lovelace (must be step: 10)

```  - type: entities
  - type: entities
    name: Roller Blind
    min: 0
    max: 100
    step: 10
    entities:
      - cover.roller_blind
      - type: custom:slider-entity-row
        entity: cover.roller_blind
```

I made this sketch because I would prefer to have my blinds running ESPHome rather than MQTT.

It will run on it's own, without HA as long as you have a Button connected to D7 and GND, (this button is also used for manual operation).

This sketch will replace the original 'Motor on a roller blind' which I have been using for some time myself and it's been almost perfect.

To upload via WIFI, go to https://steve.fi/hardware/ota-upload/ and follow the instructions, took less than one minute to upload and configure!

You should only have to set up the blind once, it stores the open/closed status and position even if you flash the device, it will report open/closed so HA will know where your blind is, even after restarting HA.

I've not mentioned before, but I run my blinds using a 9v input on the VIN pin, been running great for over 3 years.

## How do I set up the blind?

 This sketch will add 2 switches named <upper_devicename> Setup Switch and Setup Button
 Use your mobile or tablet and connect to http://\<devicename>.local to set up the blind

 - Turn on the Setup Switch to enter setup mode
 - Press Setup button to start the blind closing
 - Press Setup button again when closed and blind starts to open
 - Press Setup button again when blind is fully open
 - Job Done

 This sketch also includes a momentary button on D7 which can be used in the following way

 - Press button for > 1 second to enter setup mode
 - Press button again to start the blind closing
 - Press button again when closed and blind starts to open
 - Press button again when blind is fully open
 - Job Done

 The button is also used to open/close the blind

 NOTE:  If you find that your shades are going the wrong way, you can change the pin
        settings in the 'testblind.yaml' or reverse the + and – wires for each of the A and B motor
        pairs on your driver and the motor will spin in the opposite direction.
