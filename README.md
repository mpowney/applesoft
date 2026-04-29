## TRAFFIC.BAS — Australian 4-Way Traffic Light Simulation

An AppleSoft BASIC program that simulates traffic light sequencing for a
four-way intersection on a left-hand traffic (Australian) road system, running
in standard 40×24 text mode.

### Features

* **Four signal heads side-by-side** across the screen (no road representation)
* **Lamps stacked vertically** — Red (top), Amber (middle), Green (bottom) — matching standard Australian traffic light design
* **Arrows on the same row as their colour lamp** — green arrow on the green row (9), amber arrow on the amber row (7)
* **Two protected right-turn arrow heads** — North approach (`<` arrow, turning west) and South approach (`>` arrow, turning east)
* **Random vehicle-detection loop** — each cycle, each arrow head independently has a 50% chance of a vehicle being detected in its activation loop
  - If neither loop triggers: the arrow section is skipped entirely
  - If both loops trigger: both N and S get protected green arrow only (state 4), no straight-through
  - If only one loop triggers: that direction gets green + green arrow simultaneously (state 7), allowing straight-through AND right-turn concurrently, then transitions to amber arrow
* **Amber arrow phase** — each active arrow transitions green arrow → amber arrow → all-red
* **Australian pre-green signal** — Red+Amber phase before every green phase
* **INVERSE text** for illuminated lamps; normal text for installed-but-unlit lamps
* **Detection loop status** displayed on row 21 each cycle

### Screen Layout (40×24)

```
         AUST. TRAFFIC LIGHT SIM          <- row 1

  NORTH     SOUTH     WEST      EAST      <- row 3

    O         O         O         O       <- row 5  RED lamps
    O  <      O  >      O         O       <- row 7  AMBER lamps + AMBER ARROW (N,S)
    O  <      O  >      O         O       <- row 9  GREEN lamps + GREEN ARROW (N,S)

PHASE: NS GREEN                           <- row 20 current phase
N-LOOP:-- S-LOOP:--                       <- row 21 detection loop status
PRESS ANY KEY TO STOP                     <- row 22
```

Capital `O` = circular lamp. `<`/`>` = arrow lamps.  
Square-bracket highlight (`[O]`, `[<]`, `[>]`) = lamp illuminated (INVERSE on Apple II).  
Normal text = lamp installed but not lit.

### Signal States

| Code | Meaning                                               |
|------|-------------------------------------------------------|
| 1    | Red only                                              |
| 2    | Amber only                                            |
| 3    | Green only                                            |
| 4    | Green Arrow — red main + green arrow lit (protected turn only) |
| 5    | Red + Amber (Australian pre-green)                    |
| 6    | Amber Arrow — red main + amber arrow lit              |
| 7    | Green + Arrow — green main + green arrow lit (straight-through AND turn permitted) |

### Phase Sequence (procedural, one cycle)

| Phase        | N         | S         | W     | E     | Condition         |
|--------------|-----------|-----------|-------|-------|-------------------|
| NS PREP      | R+A       | R+A       | R     | R     | always            |
| NS GREEN     | G         | G         | R     | R     | always            |
| NS AMBER     | A         | A         | R     | R     | always            |
| ALL RED      | R         | R         | R     | R     | always            |
| NS ARROW     | Grn+Arw   | Grn+Arw   | R     | R     | NF=1 AND SF=1     |
| NS ARW AMB   | Amb Arw   | Amb Arw   | R     | R     | NF=1 AND SF=1     |
| N ARROW      | Grn+Arw   | R         | R     | R     | NF=1 AND SF=0     |
| N ARW AMB    | Amb Arw   | R         | R     | R     | NF=1 AND SF=0     |
| S ARROW      | R         | Grn+Arw   | R     | R     | NF=0 AND SF=1     |
| S ARW AMB    | R         | Amb Arw   | R     | R     | NF=0 AND SF=1     |
| ALL RED      | R         | R         | R     | R     | if arrow ran      |
| EW PREP      | R         | R         | R+A   | R+A   | always            |
| EW GREEN     | R         | R         | G     | G     | always            |
| EW AMBER     | R         | R         | A     | A     | always            |
| ALL RED      | R         | R         | R     | R     | always            |

_NF = North detection flag (0/1), SF = South detection flag (0/1), set randomly each cycle._

### DATA-Driven Configuration

**Signal heads** (lines 9025–9065): `NH`, then per head: `COL, ARROW_TYPE`  
(`ARROW_TYPE`: 0 = no arrow, 1 = `<` west, 2 = `>` east)

Lamp rows are global constants set on line 15: `RR=5` (red), `AR=7` (amber), `GNR=9` (green).  
`GNR` is used instead of `GR` because `GR` is a reserved keyword in AppleSoft BASIC (low-res graphics command).

### Running the Program

Load `TRAFFIC.BAS` into an Apple II emulator (e.g. AppleWin, Virtual II) or
an online AppleSoft BASIC interpreter, type `RUN`, and press any key to stop.

The delay multiplier on BASIC line 5020 (`PD*200`) can be adjusted to suit emulator speed.
The detection probability can be changed on line 50 by adjusting `INT(RND(1)*2)` —
e.g. `INT(RND(1)*3)` gives a 2-in-3 chance instead of 1-in-2.
