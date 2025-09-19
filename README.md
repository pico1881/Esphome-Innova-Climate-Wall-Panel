# Esphome-Innova-Climate-Wall-Panel
The ``innova_climate`` climate platform creates a climate device which can be used
to control a Innova fancoil via modbus communication. 

![test_image](/images/innova_climate.jpg)

This component supports the following functionality:
- Set the operating mode: off, heat, cool
- Set the desired fan speed: auto, high (maximum), medium (silent), low (night)
- Set the desired target temperature

Optional you can set sensors:
- Current temperature
- Target temperature

and a switch:
- Keyboard lock switch for lock the on-board keyboard (same function when you push + and - key on the device)

Hardware setup

You need an RS485 transceiver module:

![rs485_module](/images/rs485_module.jpg)

a DC-DC step down regulator like this with the output voltage regulated at 5V from 12V input:

![voltage_regulator](/images/voltage_regulator.jpg)

Connect all as this schematic:
![connection_schema](/images/connection_schema_2.jpg)

### Software

To use this component in your ESPHome configuration, follow the example below.
NOTE: verify the modbus address of fancoil board, default is 1, refer to the device instruction manual for address settings.

#### Example configuration

```yaml
external_components:
  - source:
      type: git
      url: https://github.com/pico1881/Esphome-Innova-Climate-Wall-Panel
      ref: main
    components: [ innova_climate_wall ]

logger:
  baud_rate: 0  #disable serial logger when use serial hardware pins

uart:
  tx_pin: GPIO01
  rx_pin: GPIO03
  baud_rate: 9600
  stop_bits: 1

climate:
  - platform: innova_climate_wall
    name: Innova         #Required field, set your desired name for the climate
    address: 1           #Set modbus address of Innova board default 1
    update_interval: 60s #The interval that the sensors should be checked. Defaults to 60 seconds.
    
    #optional Air temperature sensor
    air_temperature:
      name: ${name} air temp
    #optional Water temperature sensor
    setpoint:
      name: ${name} setpoint   
    #optional binary sensor for output status of boiler relay 
    key_lock_switch:
      name: ${name} key lock
```
