# applesoft

## TRAFFIC.BAS — Australian 4-Way Traffic Light Simulation

An AppleSoft BASIC program that simulates traffic light sequencing for a
four-way intersection on a left-hand drive (Australian) road system, running
in standard 40×24 text mode.

### Features

* **Four signal heads side-by-side** across the screen (no road representation)
* **Lamps stacked vertically** — Red (top), Amber (middle), Green (bottom) — matching standard Australian traffic light design
* **Two protected right-turn arrow phases** — North approach (`<` arrow, turning west) and South approach (`>` arrow, turning east)
* **Amber arrow phase** — each protected turn transitions through green arrow → amber arrow → red
* **Australian signal sequencing** including the Red+Amber pre-green phase
* **INVERSE text** used to show illuminated lamps; normal text for unlit lamps
* **DATA-driven configuration** — signal positions and phase sequence are entirely in `DATA` statements; the intersection style can be changed without touching the control logic

### Screen Layout (40×24)

```
         AUST. TRAFFIC LIGHT SIM          <- row 1

  NORTH     SOUTH     WEST      EAST      <- row 3 direction labels

    O         O         O         O       <- row 5  RED lamps
    O         O         O         O       <- row 7  AMBER lamps
    O         O         O         O       <- row 9  GREEN lamps
    <         >                           <- row 11 GREEN ARROW lamps (N and S only)
    <         >                           <- row 13 AMBER ARROW lamps (N and S only)

PHASE: NS GREEN                           <- row 20 current phase
PRESS ANY KEY TO STOP                     <- row 22
```

Capital `O` = round lamp (INVERSE when lit). `<`/`>` = right-turn arrow lamps.  
INVERSE text = lamp illuminated; normal text = lamp installed but not lit.

### Signal States

| Code | Meaning                                   |
|------|------------------------------------------|
| 1    | Red only                                 |
| 2    | Amber only                               |
| 3    | Green only                               |
| 4    | Green Arrow (Red + green arrow lit)      |
| 5    | Red + Amber (Australian pre-green)       |
| 6    | Amber Arrow (Red + amber arrow lit)      |

### Phase Cycle (14 phases)

| Phase | Name      | N       | S       | W   | E   |
|-------|-----------|---------|---------|-----|-----|
| 1     | NS PREP   | R+A     | R+A     | R   | R   |
| 2     | NS GREEN  | G       | G       | R   | R   |
| 3     | NS AMBER  | A       | A       | R   | R   |
| 4     | ALL RED   | R       | R       | R   | R   |
| 5     | N ARROW   | Grn Arw | R       | R   | R   |
| 6     | N ARW AMB | Amb Arw | R       | R   | R   |
| 7     | ALL RED   | R       | R       | R   | R   |
| 8     | S ARROW   | R       | Grn Arw | R   | R   |
| 9     | S ARW AMB | R       | Amb Arw | R   | R   |
| 10    | ALL RED   | R       | R       | R   | R   |
| 11    | EW PREP   | R       | R       | R+A | R+A |
| 12    | EW GREEN  | R       | R       | G   | G   |
| 13    | EW AMBER  | R       | R       | A   | A   |
| 14    | ALL RED   | R       | R       | R   | R   |

### DATA-Driven Configuration

**Signal heads** (lines 9025–9065): `NH`, then per head: `COL, ARROW_TYPE`  
**Phases** (lines 9095–9235): `NP`, then per phase: `DURATION, "NAME", N, S, W, E`

Signal column positions and arrow types can be changed without touching any
control logic. New approaches or phases can be added by updating the counts
(`NH`, `NP`) and appending the corresponding DATA values.

### Running the Program

Load `TRAFFIC.BAS` into an Apple II emulator (e.g. AppleWin, Virtual II) or
an online AppleSoft BASIC interpreter, type `RUN`, and press any key to stop.

The delay multiplier on BASIC line 5020 (`PD(CP) * 200`) can be adjusted to suit
emulator speed.