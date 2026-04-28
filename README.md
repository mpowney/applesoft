# applesoft

## TRAFFIC.BAS — Australian 4-Way Traffic Light Simulation

An AppleSoft BASIC program that simulates traffic light sequencing for a
four-way intersection on a left-hand drive (Australian) road system, running
in standard 40×24 text mode.

### Features

* **Four-way intersection** with a north/south road and an east/west road
* **Two protected right-turn arrow phases** — North approach (`<` arrow, turning west) and South approach (`>` arrow, turning east)
* **Australian signal sequencing** including the Red+Amber pre-green phase
* **INVERSE text** used to show illuminated lamps; normal text for unlit lamps
* **DATA-driven configuration** — signal positions and phase sequence are entirely in `DATA` statements; the intersection style can be changed without touching the control logic

### Screen Layout (40×24)

```
         AUST. TRAFFIC LIGHT SIM          <- row 1

N: O  O  O  <                             <- North signal (row 3)
           |   |                          <- N/S road
===========+===+========================  <- E/W road edge (row 8)
O  O  O  W |   | E  O  O  O              <- W & E signals (row 10)
===========+===+========================  <- E/W road edge (row 12)
           |   |                          <- N/S road
S: O  O  O  >                             <- South signal (row 17)

PHASE: NS GREEN                           <- current phase (row 20)
PRESS ANY KEY TO STOP                     <- row 22
```

Capital `O` = lamp (INVERSE when lit). `<`/`>` = right-turn arrow lamps.

### Signal States

| Code | Meaning                        |
|------|-------------------------------|
| 1    | Red only                      |
| 2    | Amber only                    |
| 3    | Green only                    |
| 4    | Arrow (Red + Arrow lamp lit)  |
| 5    | Red + Amber (Australian pre-green) |

### Phase Cycle

| Phase | Name     | N | S | W | E |
|-------|----------|---|---|---|---|
| 1  | NS PREP  | R+A | R+A | R | R |
| 2  | NS GREEN | G   | G   | R | R |
| 3  | NS AMBER | A   | A   | R | R |
| 4  | ALL RED  | R   | R   | R | R |
| 5  | N ARROW  | Arrow | R | R | R |
| 6  | ALL RED  | R   | R   | R | R |
| 7  | S ARROW  | R | Arrow | R | R |
| 8  | ALL RED  | R   | R   | R | R |
| 9  | EW PREP  | R   | R | R+A | R+A |
| 10 | EW GREEN | R   | R   | G | G |
| 11 | EW AMBER | R   | R   | A | A |
| 12 | ALL RED  | R   | R   | R | R |

### Running the Program

Load `TRAFFIC.BAS` into an Apple II emulator (e.g. AppleWin, Virtual II) or
an online AppleSoft BASIC interpreter, type `RUN`, and press any key to stop.

The delay multiplier on BASIC line 5020 (`PD(CP) * 200`) can be adjusted to suit
emulator speed. The phase durations and intersection layout are in the `DATA`
statements starting at line 9000.