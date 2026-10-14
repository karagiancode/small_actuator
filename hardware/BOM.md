# Bill of Materials

Costs are the figures from the original parts list. Where the original list gave one price for a pair, that price covers the pair. Items marked **To confirm** were visible in the CAD or are required for operation but were not on the original list.

| Category | Item | Specification | Qty | Cost (EUR) | Notes |
|---|---|---|---|---|---|
| Actuation | NEMA 17 stepper | 42BYGH603 | 2 | 15.00 | Base yaw, shoulder pitch |
| Actuation | Geared stepper | 28BYJ-48 | 2 | 5.00 | Elbow, wrist pitch |
| Actuation | Micro servo | SG90 | 1 | to confirm | Gripper |
| Drivers | Stepper driver | DRV8825 | 2 | 2.00 | For the NEMA 17 motors |
| Drivers | Stepper driver | ULN2003APG | 2 | 2.00 | For the 28BYJ-48 motors |
| Transmission | GT2 timing belt | 202 mm | 1 | 2.00 | |
| Transmission | GT2 pulley, aluminium | 12T, 5 mm bore | 1 | 2.50 | |
| Transmission | GT2 pulley, aluminium | 48T, 5 mm bore | 1 | 3.00 | 4:1 reduction |
| Bearings | Skateboard bearing | 608-2RS | to confirm | to confirm | Seen in the CAD |
| Power | Power supply | 12 V, 5 A | 1 | 5.00 | |
| Power | Buck converter | 12 V to 5 V | 1 | to confirm | SG90 and logic supply |
| Control | Microcontroller | Raspberry Pi Pico | to confirm | to confirm | |
| Fasteners | Screw | M5 x 70 mm | 1 | 0.20 | |
| Fasteners | Screw | M5 x 33 mm | 10 | 0.20 | |
| Fasteners | Nut | M5 | 20 | 0.20 | |

**Listed total: EUR 37.10**, excluding the unconfirmed items.

## Design checks before powering up

- **28BYJ-48 voltage.** The common version is rated 5 V; a 12 V version also exists. Check the label. A 5 V motor must not be driven from the 12 V rail.
- **DRV8825 current limit.** Set the reference voltage for the rated current of the 42BYGH603 before connecting the motor, and add a 47 to 100 uF capacitor across the driver supply pins.
- **Supply split.** The 12 V rail feeds the DRV8825 drivers. The SG90, the logic and, if applicable, the 5 V 28BYJ-48 motors need a separate regulated 5 V rail with a common ground.
- **Current budget.** Add the peak current of all motors against the 5 A supply rating before running all joints at once.
