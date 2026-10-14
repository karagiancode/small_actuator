# Bill of Materials

Unit prices are per piece. **TBD** means the price or quantity is not yet known.

| Category | Item | Specification | Qty | Unit (EUR) | Line (EUR) | Status | Notes |
|---|---|---|---|---|---|---|---|
| Control | Single-board computer | Raspberry Pi 5 | 1 | TBD | TBD | To buy | ROS 2 master |
| Power | Pi 5 power supply | USB-C, 27 W | 1 | TBD | TBD | To buy | Powers the Pi 5 and, through USB, both Picos |
| Control | Microcontroller | Raspberry Pi Pico | 2 | TBD | TBD | To buy | Real-time stepper control, micro-ROS slaves |
| Actuation | NEMA 17 stepper motor | 42BYGH603 | 2 | 15.00 | 30.00 | Owned | Base yaw, shoulder pitch |
| Actuation | Geared stepper motor | 28BYJ-48 | 2 | 5.00 | 10.00 | Owned | Elbow, wrist pitch |
| Actuation | Micro servo | SG90 | 1 | TBD | TBD | Owned (verify) | Gripper |
| Drivers | Stepper driver | DRV8825 | 2 | 2.00 | 4.00 | Owned | One per NEMA 17 |
| Drivers | Stepper driver | ULN2003APG | 2 | 2.00 | 4.00 | Owned | One per 28BYJ-48 |
| Drivers | Breakout / driver board | 12 V input | 1 | TBD | TBD | Owned | Carries 12 V to the motor drivers |
| Feedback | Magnetic encoder | AS5600 module (proposed) | 1 | TBD | TBD | To buy | First closed-loop joint, for PID; I2C, 12-bit |
| Feedback | Diametric magnet | 6 x 2.5 mm (proposed) | 1 | TBD | TBD | To buy | Mounted on the shaft of the encoded joint |
| Transmission | GT2 timing belt | 202 mm | 1 | 2.00 | 2.00 | Owned |  |
| Transmission | GT2 pulley, aluminium | 12T, 5 mm bore | 1 | 2.50 | 2.50 | Owned |  |
| Transmission | GT2 pulley, aluminium | 48T, 5 mm bore | 1 | 3.00 | 3.00 | Owned | 4:1 reduction |
| Bearings | Skateboard bearing | 608-2RS | TBD | TBD | TBD | Owned (qty TBD) | Seen in the CAD |
| Power | Power supply | 12 V, 5 A | 1 | 5.00 | 5.00 | Owned |  |
| Power | Buck converter | 12 V to 5 V, 2 A or more | 1 | TBD | TBD | To buy | 5 V rail for the SG90 gripper only; the Picos are powered over USB from the Pi 5 |
| Fasteners | Screw | M5 x 70 mm | 1 | 0.20 | 0.20 | Owned |  |
| Fasteners | Screw | M5 x 33 mm | 10 | 0.20 | 2.00 | Owned |  |
| Fasteners | Nut | M5 | 20 | 0.20 | 4.00 | Owned |  |

**Known total: EUR 66.70**, excluding items marked TBD.

## Design checks before powering up

- **28BYJ-48 voltage.** Both the 5 V and 12 V versions exist. Confirm the version on the label matches the rail it is driven from.
- **DRV8825 current limit.** Set the reference voltage for the rated current of the 42BYGH603 before connecting the motor, and keep a 47 to 100 uF capacitor across the driver supply pins.
- **Supply split.** 12 V feeds the motor drivers and the 28BYJ-48 boards; the SG90 uses the 5 V buck rail; the Pi 5 and both Picos use the USB-C supply. All grounds must be common.
- **Current budget.** Check the combined peak motor current against the 5 A supply rating before running several joints at once.
- **Encoder mounting.** A magnetic encoder needs a diametrically magnetised magnet on a shaft end that is accessible, either the rear of a dual-shaft motor or the output shaft. Check which is available on the 42BYGH603.
