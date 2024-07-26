# Multi stepper usage

Packages which could be used to control multiple blinds from single MCU board

## Packages

---

### Controller info

File: `blind_controller_info.yaml`

#### Required variables

- `upper_devicename`: Friendly name of device to show on frontend (Homeassistant,...)

#### Creates:

Sensors:

- ESPHome Version installed on MCU
- IP Address of MCU
- SSID of wifi network
- MAC address of MCU
- Uptime of MCU

---

### Blind with button

File: `blind_with_button.yaml`

#### Required variables

- `mystepper`: Id of the stepper motor
- `name`: Firendly name (used for logging and frontend strings)
- `buttonpin`: Pin of the physical button
- `speed`: Max speed of the stepper motor in steps/s
- `pina`: First pin of the stepper motor
- `pinb`: Second pin of the stepper motor
- `pinc`: Third pin of the stepper motor
- `pind`: Fourth pin of the stepper motor
- `reportin`: Report blind position while moving ('0'/'1')

#### Creates:

All the necessary things to setup and control the blind

---

### Blind without button

File: `blind_without_button.yaml`

#### Required variables

- `mystepper`: Id of the stepper motor
- `name`: Firendly name (used for logging and frontend strings)
- `speed`: Max speed of the stepper motor in steps/s
- `pina`: First pin of the stepper motor
- `pinb`: Second pin of the stepper motor
- `pinc`: Third pin of the stepper motor
- `pind`: Fourth pin of the stepper motor
- `reportin`: Report blind position while moving ('0'/'1')

#### Creates:

All the necessary things to setup and control the blind without the physical button connected to the MCU (There is still a frontend setup button to use from Homeassistant to setup the blind)

---

## How to use

1. Put `packages` folder somewhere beside your main yaml config (e.g. for Homeassistant to `config/esphome/`)
2. Include one or more files as `package` om yout main yaml config file

### Example

#### Parts

- ESP32 dev MCU ([I used this board, but could be any](https://cdn.myshoptet.com/usr/www.laskakit.cz/user/shop/big/6723-3_la100057p-esp32-lpkit-axonometric-2.jpg?63f72a51))
- 2x [28BYJ-48 Stepper motors](https://cdn.myshoptet.com/usr/www.laskakit.cz/user/shop/big/347_krokovy-motor-28byj-48.jpg?61d95cd9)
- 2x [ULN2003 Stepper motor drivers](https://cdn.myshoptet.com/usr/www.laskakit.cz/user/shop/big/359-1_radic-uln2003-pro-krokovy-motor.jpg?61d95d26)

#### Wiring

Connect power supply to the ESP32 board and both ULN2003 steppers
Connect one stepper motor to the one ULN2003 driver and another stepper motor to the other ULN2003 driver.
Then connect drivers to the ESP32 board like this:

| Driver for Stepper 1 | ESP32 board |
| -------------------- | ----------- |
| `IN1`                | `GPIO26`    |
| `IN2`                | `GPIO27`    |
| `IN3`                | `GPIO14`    |
| `IN4`                | `GPIO13`    |

| Driver for Stepper 2 | ESP32 board |
| -------------------- | ----------- |
| `IN1`                | `GPIO19`    |
| `IN2`                | `GPIO18`    |
| `IN3`                | `GPIO5`     |
| `IN4`                | `GPIO17`    |

#### Code

`blind.yaml`

```yaml
esp32:
  board: esp32dev
  framework:
    type: arduino

esphome:
  name: Blinds

logger:

api:
  encryption:
    key: 'SOME_SUPER_SECRET_API_KEY'

ota:
  password: 'SOME_SUPER_SECRET_KEY_FOR_OTA_UPDATES'

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

ap:
  ssid: 'Blinds Fallback Hotspot'
  password: 'SOME_SUPER_SECRET_HOTSPOT_PASSWORD'

captive_portal:

packages:
  controller_info: !include
    file: packages/blind_controller_info.yaml
    vars:
      upper_devicename: Blinds ESP32
  blind_left: !include
    file: packages/blind_without_button.yaml
    vars:
      mystepper: stepper1
      name: 'Blind Left'
      speed: 300 steps/s
      pina: GPIO26
      pinb: GPIO27
      pinc: GPIO14
      pind: GPIO13
      reportin: '0'
  blind_right: !include
    file: packages/blind_without_button.yaml
    vars:
      mystepper: stepper2
      name: 'Blind Right'
      speed: 300 steps/s
      pina: GPIO19
      pinb: GPIO18
      pinc: GPIO5
      pind: GPIO17
      reportin: '0'
```
