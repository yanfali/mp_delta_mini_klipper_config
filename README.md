My monoprice mini delta configuration - WIP

## How to flash klipper firmware using OpenOCD

### OpenOCD Config for RPI 4

```
# Uses RPi pins: GPIO25 for SWDCLK, GPIO24 for SWDIO, GPIO18 for nRST
source [find interface/raspberrypi2-native.cfg]
bcm2835gpio swd_nums 25 24
bcm2835gpio srst_num 18
transport select swd

# Use hardware reset wire for chip resets
reset_config srst_only
adapter srst delay 100
adapter srst pulse_width 100

reset_config none separate

# Specify the chip type
source [find target/stm32f0x.cfg]

# Set the adapter speed
adapter speed 40

# Connect to chip
init
targets
reset halt
```

### Commands to start openOCD

`sudo ~/openocd/install/bin/openocd -f ~/openocd/openocd.cfg`

### Commands to flash

#### Connect to OpenOCD
```
telnet localhost 4444
```

#### halt CPU
```
reset halt
```

### erase flash
stm32f0x mass_erase 0

### flash bootloader back
```
flash write_bank 0 MalyanM300BootLoader.bin 0
```

### flash klipper.bin
```
flash write_bank 0 klipper.bin 0x2000
```

Now The system should be bootable. You will see a udev device created at /dev/serial/by-id with klipper in the name
