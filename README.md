# ESPHome TCA9555 External Component

This repository provides an external ESPHome component for the TCA9555 GPIO
expander. It addresses the issue described in
[esphome/issues#6438](https://github.com/esphome/issues/issues/6438), where
binary sensors using GPIO pins from the TCA9555 do not report input changes
correctly. This component ensures accurate GPIO state reading by invalidating
the cache when necessary.

The code was written mainly by [@mobrembski](https://github.com/mobrembski), I myself contributed the two lines necessary for the update function. Sice this PR is still not merged yet, I decided to put it up here for easy use for anyone wanting a functional TCA9555 that is able to read its GPIOs in the millisecond timeframe (hence the need for invalidating the cache).

## Features
- Fixes the issue where TCA9555 GPIO inputs do not update correctly.
- Adds a public `update()` method to invalidate the cache, ensuring fresh GPIO state reads.
- Fully compatible with ESPHome's external components system.

## Repository URL
[https://github.com/lk-ek/esphome-tca9555](https://github.com/lk-ek/esphome-tca9555)

## Installation

Locally: Clone this repository or download it as a ZIP file. 
Place the `esphome-tca9555` folder in your ESPHome project's `components` directory.

### Using Local Components
```yaml
substitutions:
  device_name: test
  upper_devicename: Test

esphome:
  name: $device_name
  platform: ESP8266
  board: esp12e

i2c:
  sda: D2
  scl: D1
  scan: true

external_components:
  - source:
      type: local
      path: components/esphome-tca9555

tca9555:
  - id: exp0
    address: 0x20

binary_sensor:
  - platform: gpio
    name: "${upper_devicename} In 1"
    pin:
      tca9555: exp0
      number: 2
      mode: INPUT
```

Or directly from Github:

### Using Components Directly from GitHub
```yaml
substitutions:
  device_name: test
  upper_devicename: Test

esphome:
  name: $device_name
  platform: ESP8266
  board: esp12e

i2c:
  sda: D2
  scl: D1
  scan: true

external_components:
  - source:
      type: git
      url: https://github.com/lk-ek/esphome-tca9555
      ref: main
    components: ["tca9555", "gpio_expander"]

tca9555:
  - id: exp0
    address: 0x20

binary_sensor:
  - platform: gpio
    name: "${upper_devicename} In 1"
    pin:
      tca9555: exp0
      number: 2
      mode: INPUT
```

## Usage Notes
- Replace `0x20` with the correct I2C address of your TCA9555 device.
- Use the `update()` method in your ESPHome automation or loop to ensure accurate GPIO state reads when timing is critical.

## Acknowledgments
This component is based on the work of
[@mobrembski](https://github.com/mobrembski) and contributors in
[esphome/esphome#8003](https://github.com/esphome/esphome/pull/8003). Thank you!


## Related Issues
- [esphome/issues#6438](https://github.com/esphome/issues/issues/6438)
- [esphome/esphome#8003](https://github.com/esphome/esphome/pull/8003)

## Contributing
I'd rather delete this repository once the PR is merged.

## License
This project is licensed under the same license as the ESPHome project (MIT style and GPLv3), see LICENSE
