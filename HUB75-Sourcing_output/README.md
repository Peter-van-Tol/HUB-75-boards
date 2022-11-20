# HUB-75 Sourcing output 
Break-out board with 12 channel sourcing output. The FPGA is galvanically separated from field power using opto-couplers. To enhance the power each channel can deliver, the loads are being switched with a MOSfet (``). 

TODO: images

## Features

- 2 HUB-75 input connectors
- 12 channel sourcing output using JST connectors
- 5\~24 V Field power with reverse polarity protection
- rated 300 mA power per channel
- indicating LED for each output
- indicated switching frequency 1 kHz

## Applications

- Driving LEDs (optional dimmable by using PWM)
- Driving solenoids with current lower then rated current
- Driving relays for larger loads 

> **NOTE** <br> The `PC827` optocouplers have a maximum rated frequency of 80 kHz. When using this board in combination with PWM, the chosen frequency should be well below this limit. It is recommended to start at a frequency of 1 kHz and work up from there. PDM with LitexCNC will not work with this board, because the frequency cannot be set and will be too high. 

## Example wiring

TODO: example

## Litex-CNC example configuration
Let's assume that the two HUB-745 connectors on this board are connected to J1 and J2 of the FPGA. To configure the pins for output, you can use the configuration-block below as a starting point. Optionaly the pin names can also be set in the configuration, making the HAL clearer.

``` json
{
    ...,
    "gpio_out": [
        {"pin": "j1:0"},
        {"pin": "j1:1"},
        {"pin": "j1:2"},
        {"pin": "j1:4"},
        {"pin": "j1:5"},
        {"pin": "j1:6"},
        {"pin": "j2:0"},
        {"pin": "j2:1"},
        {"pin": "j2:2"},
        {"pin": "j2:4"},
        {"pin": "j2:5"},
        {"pin": "j2:6"}
    ],
    ...
}
```

Alternatively, the BOB can also be used for PWM output. In that case the configuration may look something like below. In this example it is assumed that outputs J10 through J12 on the BOB are used for PWM.

``` json
{
    ...,
    "gpio_out": [
        {"pin": "j1:0"},
        {"pin": "j1:1"},
        {"pin": "j1:2"},
        {"pin": "j1:4"},
        {"pin": "j1:5"},
        {"pin": "j1:6"},
        {"pin": "j2:0"},
        {"pin": "j2:1"},
        {"pin": "j2:2"}
    ],
    "pwm": [
        {"pin": "j2:4"},
        {"pin": "j2:5"},
        {"pin": "j2:6"}
    ]
    ...
}
```

When using PWM, the HAL pin `pwm.###.pwm_freq` should be set to a value smaller then 1000 (1 kHz). Higher frequencies can be tested using the board, by increasing this value. The uppper limit is 80 kHz, which is the maximum frequency of the `PC817` opto-coupler. 

## Bill of materials

The table below gives the bill of materials used for this BOB and an indication of its price. 

| Part. number | Package | Description                | Datasheet | Amount | Price* |
|--------------|---------|----------------------------|-----------|--------|--------|
| LTV827       | DIP-8   | Dual-channel opto-coupler  |           | 3      | €      |
|              | SOT-23  | P-channel MOSFET, 30V      |           | 12     | €      |


**NOTES**: 
1. The price is based on availability in the Netherlands with a single shop for all parts. Parts might be sourced cheaper depending on your location, available sources and the time you want to wait before receiving the components.

