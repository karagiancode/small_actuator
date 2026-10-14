# Wiring

![Wiring overview](wiring/wiring-diagram.svg)

## Architecture

| Controller | Drives | Notes |
|---|---|---|
| Raspberry Pi 5 | ROS 2 master | Own USB-C supply. Connects to each Pico over USB (micro-ROS). |
| Pico A | Base yaw (NEMA 17), elbow and wrist (28BYJ-48) | Powered over USB |
| Pico B | Shoulder pitch (NEMA 17), gripper (SG90), AS5600 encoder | Powered over USB |

Both Picos use the same pin map for the DRV8825 (GP10 to GP15), so the same firmware can run on either.

## Pico A pinout

| Function | GPIO | Physical pin | Connects to |
|---|---|---|---|
| STEP | GP10 | 14 | DRV8825 #1 STEP |
| DIR | GP11 | 15 | DRV8825 #1 DIR |
| ENABLE | GP12 | 16 | DRV8825 #1 ENABLE (active low) |
| M0 | GP13 | 17 | DRV8825 #1 M0 |
| M1 | GP14 | 19 | DRV8825 #1 M1 |
| M2 | GP15 | 20 | DRV8825 #1 M2 |
| IN1 to IN4 | GP6, GP7, GP8, GP9 | 9, 10, 11, 12 | ULN2003 #1 (elbow) |
| IN1 to IN4 | GP16, GP17, GP18, GP19 | 21, 22, 24, 25 | ULN2003 #2 (wrist) |
| GND | | 38 | Common ground |

## Pico B pinout

| Function | GPIO | Physical pin | Connects to |
|---|---|---|---|
| STEP | GP10 | 14 | DRV8825 #2 STEP |
| DIR | GP11 | 15 | DRV8825 #2 DIR |
| ENABLE | GP12 | 16 | DRV8825 #2 ENABLE (active low) |
| M0, M1, M2 | GP13, GP14, GP15 | 17, 19, 20 | DRV8825 #2 M0, M1, M2 |
| I2C0 SDA | GP4 | 6 | AS5600 SDA |
| I2C0 SCL | GP5 | 7 | AS5600 SCL |
| 3V3 (OUT) | | 36 | AS5600 VCC |
| Servo PWM | GP20 | 26 | SG90 signal |
| GND | | 38 | Common ground |

## DRV8825 connections (per driver)

- **VMOT / GND:** 12 V and ground, with a 100 uF electrolytic capacitor across them, close to the driver.
- **RESET and SLEEP:** join the two pins together and connect them to 3V3, or the driver stays off.
- **ENABLE, STEP, DIR, M0 to M2:** to the Pico pins above. Logic is 3.3 V compatible.
- **Motor coils:** 1A and 1B to one coil, 2A and 2B to the other. Find the coil pairs with a multimeter (low resistance between the two wires of one coil) before connecting.
- **Current limit:** set the reference voltage before connecting the motor. The formula depends on the sense resistor of your module, so check it first.

## ULN2003 and 28BYJ-48

- IN1 to IN4 to the four Pico pins listed above, in order.
- Power the board from the 12 V rail only if the motor is the 12 V version. A 5 V motor needs 5 V.
- Drive with the half-step sequence for smoother motion.

## SG90 servo

- Red to 5 V from the buck converter, brown to ground, orange to GP20.
- Add a 470 uF capacitor across the 5 V rail near the servo to absorb current spikes.

## AS5600 encoder (shoulder)

- VCC to Pico B 3V3, GND to ground, SDA to GP4, SCL to GP5. Tie the DIR pin of the module to ground.
- I2C address is fixed at 0x36. Each Pico has its own bus, so there is no address conflict.
- The magnet is a 6 x 2.5 mm diametrically magnetised disc, glued centred on the flat rear end of the motor shaft. A small printed jig helps centre it.
- The sensor sits 1 to 3 mm from the magnet. Read the magnet status bits of the AS5600 to confirm the field strength is in range.
- The encoder measures the motor shaft angle. If the shoulder is driven through the GT2 belt, the joint angle is the motor angle divided by the belt ratio.

## Power and ground

- 12 V rail: PSU to both DRV8825 VMOT pins and the ULN2003 boards.
- 5 V rail: buck converter to the SG90 only.
- The Pi 5 uses its own USB-C supply and powers both Picos over USB.
- Join all grounds: PSU negative, buck negative, both DRV8825 GND, both ULN2003 GND, SG90 and both Pico GND pins.

## First power-up checklist

1. Verify the 28BYJ-48 voltage version from the label.
2. Set the DRV8825 current limit with the motor disconnected.
3. Power off, connect one motor, power on, and step it slowly in both directions.
4. Repeat joint by joint before connecting everything.
5. Add the encoder only after the shoulder motor runs open loop.
