```
DIRECTION_VECTOR
 C is used for movement calculations when the player enters a course (1-9), creating a lookup table for direction vectors where:
 C(direction,1) is the X component
 C(direction,2) is the Y component
 For example:
 Course 1 = (0,1) - North
 Course 2 = (-1,1) - Northeast
 Course 3 = (-1,0) - East ...and so on in a circular pattern.
```

```
KLINGONSTATUS
 Looking at the code, K (3,3) is an array that stores information about up to 3 Klingon ships in the current quadrant, where:
 First dimension (1-3): Can track up to 3 Klingons
 Second dimension (1-3):
 K (i,1) = X sector coordinate of Klingon i
 K (i,2) = Y sector coordinate of Klingon i
 K (i,3) = Remaining shield/health of Klingon i
```

```
LRSBUFFER
 Looking at the code, N (3) is used in the Long Range Sensor scan to temporarily store data for displaying a 3x3 grid around the current quadrant:
 N () acts as a temporary buffer for one row (3 quadrants) of sensor data at a time, where:
 N (1) = left quadrant
 N (2) = center quadrant
 N (3) = right quadrant
```

```
DEVICESTATUS

WARP_DAMAGE = 0          ' D(1) Warp Engines
SENSOR_DAMAGE = 0        ' D(2) Short Range Sensors 
LRSENSOR_DAMAGE = 0      ' D(3) Long Range Sensors
PHASER_DAMAGE = 0        ' D(4) Phaser Control
TORPEDO_DAMAGE = 0       ' D(5) Photon Tubes
DAMAGECONTROL_DAMAGE = 0 ' D(6) Damage Control
SHIELD_DAMAGE = 0        ' D(7) Shield Control
COMPUTER_DAMAGE = 0      ' D(8) Library Computer
```

```
Remaining 1-2 character variables (excluding loop counters I, J, L):

 A$ - Temporary string for map symbols ("<*>", "+K+", ">!<", " * ", " ")
 A - Used in direction calculations (lines 8120-8222)
 C$ - Condition status ("RED", "GREEN", "YELLOW", "DOCKED")
 C1 - Course input (1-9) for navigation
 D1 - Damage report flag (0/1)
 D3 - Repair time calculation
 D4 - Random repair difficulty modifier (line 1326)
 D6 - Warp factor for damage calculation
 G2$ - Device/region name storage (used in messages)
 G5 - Galaxy map display flag
 H1 - Phaser hit calculation intermediate
 H8 - Computer display mode flag
 H - Hit points in combat
 O1$ - Line separator string ("-------------------")
 Q$ - Current quadrant map string (190 characters)
 R1, R2 - Random position coordinates
 S8 - String position calculation
 T8 - Time increment for warp movement
 W1 - Warp factor input
 X$ - Plural suffix ("S" or "")
 X0$ - Verb agreement (" IS " or " ARE ")
 X1, X2 - Movement vector components
 X3, Y3 - Torpedo track coordinates
 X5 - Border crossing flag
 Z$ - Space padding string
 Z1, Z2 - Temporary coordinates
 Z3 - String comparison result
 Z4, Z5 - Quadrant coordinates for name lookup
```


```
 Original	Refactored	            Purpose
-------------------------------------------------------------
 G(8,8)	GALAXY(8,8)	            Galaxy map data
 C(9,2)	DIRECTIONVECTOR(9,2)	Movement vectors
 K(3,3)	KLINGONSTATUS(3,3)	    Klingon ships data
 N(3)	    LRSBUFFER(3)	        Long range sensor buffer
 Z(8,8)	VISITEDQUADRANTS(8,8)	Visited quadrants record
 D(8)	    DEVICESTATUS(8)	        Ship device status
 Q1,Q2	    ENTERPRISEQUADRANTX,Y	Enterprise position
 S1,S2	    ENTERPRISESECTORX,Y	    Enterprise sector position
 T	        STARDATE	            Current game time
 K3	    KLINGONCNT	            Klingons in quadrant
 K9	    TOTALKLINGONCNT	        Total klingons remaining
 B3	    BASECNT	                Bases in quadrant
 B9	    TOTALBASECNT	        Total bases in galaxy
 S3	    STARCNT	                Stars in quadrant
 P	        TORPEDOCNT	            Torpedo count
 S	        SHIELD	                Shield energy
```

```
  Main Loop Entry Points
-------------------------------------------------
  Line 1980: GOSUB 6430 - Display short range scan (primary loop start)
      This is the start of the main game loop as you noted
      All navigation commands return here to refresh the display
  
  Line 1990: Command input check
      Most commands return here to get the next command
      This skips redrawing the short-range scan when unnecessary
  Line 1320: New quadrant entry
      This is another important entry point
      When moving to a new quadrant, we need initialization logic
```

```
             ┌───────────────────┐
             │Game Initialization│
             └──────────┬────────┘
                        │
                        ▼
  ┌───────────────────────────────────┐
  | New Quadrant (1320)               │
  | - Set up new quadrant             │◄─────────┐
  | - Place objects                   │◄─────────┐
  └─────────────────────┬─────────────┘          │
                        │                        │
                        ▼                        │ Quadrant
  ┌───────────────────────────────────┐          │ change
  │ Display Short Range Scan (1980)   │          │
  │ - Show current quadrant           │◄─────┐   │
  └─────────────────────┬─────────────┘      │   │
                        │                    │   │
                        ▼                    │   │
  ┌───────────────────────────────────┐      │   │
  │ Command Input (1990)              │      │   │
  │ - Get player command              │      │   │
  └─────────────────────┬─────────────┘      │   │
                        │                    │   │
                        ▼                    │   │
  ┌───────────────────────────────────┐      │   │
  │ Command Processing                │      │   │
  │ - NAV (movement) ─────────────────┼──────┘   │
  │ - Fire weapons                    │          │
  │ - Other commands ──────────────────────────┐ │
  │ - Quadrant transition ────────────┼──────────┘
  └───────────────────────────────────┘        │
                        │                      │
                        └──────────────────────┘
                       Return to command input
                       
```


### In the game, the following characters or entities give reports or say things:
- Lt. Sulu - Reports navigation errors.
- Chief Engineer Scott - Reports issues with the warp engines and energy levels.
- Lt. Uhura - Relays messages from Starfleet Command.
- Science Officer Spock - Provides sensor and tactical information.
- Damage Control - Reports on the status of repairs and damage to the ship.
- Starfleet Command - Sends mission-related messages and updates.
- Ensign Chekov - Reports incorrect course data for photon torpedoes.



## Vital Messages and Representations:

### In the game, the following characters or entities give reports or say things:
- Lt. Sulu - Reports navigation errors.
- Chief Engineer Scott - Reports issues with the warp engines and energy levels.
- Lt. Uhura - Relays messages from Starfleet Command.
- Science Officer Spock - Provides sensor and tactical information.
- Damage Control - Reports on the status of repairs and damage to the ship.
- Starfleet Command - Sends mission-related messages and updates.
- Ensign Chekov - Reports incorrect course data for photon torpedoes.

#### Chief Engineer Scott

- **Insufficient Energy**:
  - Message: "ENGINEERING REPORTS   'INSUFFICIENT ENERGY AVAILABLE FOR MANEUVERING AT WARP [WARP FACTOR]!'"
  - Trigger: When the player tries to use more energy than available.
  - Representation: Use a bar graph LED to show energy levels. Flash a red LED labeled "LOW ENERGY."

- **Warp Engine Damage**:
  - Message: "WARP ENGINES ARE DAMAGED. MAXIMUM SPEED = WARP 0.2"
  - Trigger: When the player attempts to exceed the damaged warp engine's speed limit.
  - Representation: Blink an LED labeled "ENGINE DAMAGE."

---

#### Lt. Uhura

- **Starfleet Command Messages**:
  - Message: "LT. UHURA REPORTS MESSAGE FROM STARFLEET COMMAND: 'PERMISSION TO ATTEMPT CROSSING OF GALACTIC PERIMETER IS HEREBY *DENIED*. SHUT DOWN YOUR ENGINES.'"
  - Trigger: When the player attempts to cross the galactic perimeter.
  - Representation: Use a dedicated LED or speaker for audio cues. Acknowledge with a button press.

---

#### Science Officer Spock

- **Enemy Presence**:
  - Message: "SCIENCE OFFICER SPOCK REPORTS  'SENSORS SHOW NO ENEMY SHIPS IN THIS QUADRANT'"
  - Trigger: When there are no Klingons in the current quadrant.
  - Representation: Use LEDs to indicate "RED ALERT" or "CLEAR."

- **Sensor Damage**:
  - Message: "*** SHORT RANGE SENSORS ARE OUT ***"
  - Trigger: When short-range sensors are damaged.
  - Representation: Flash an LED labeled "SENSOR DAMAGE."

---

#### Damage Control

- **Device Repair Status**:
  - Message: "DAMAGE CONTROL REPORT: [DEVICE] REPAIR COMPLETED."
  - Trigger: When a damaged device is repaired.
  - Representation: Use individual LEDs for each device (e.g., "WARP," "SENSORS") that light up when repaired.

- **Device Damage Status**:
  - Message: "DAMAGE CONTROL REPORTS [DEVICE] DAMAGED BY THE HIT."
  - Trigger: When a device is damaged during combat.
  - Representation: Flash the corresponding device LED.

---

#### Starfleet Command

- **Mission Updates**:
  - Message: "YOUR ORDERS ARE AS FOLLOWS: DESTROY THE [NUMBER] KLINGON WARSHIPS WHICH HAVE INVADED THE GALAXY BEFORE THEY CAN ATTACK FEDERATION HEADQUARTERS."
  - Trigger: At the start of the game or after significant events.
  - Representation: Use 7-segment displays to show remaining Klingons, bases, and stardate.

---

#### Ensign Chekov

- **Incorrect Torpedo Course**:
  - Message: "ENSIGN CHEKOV REPORTS,  'INCORRECT COURSE DATA, SIR!'"
  - Trigger: When the player inputs an invalid course for torpedoes.
  - Representation: Flash a red LED labeled "TORPEDO ERROR."

- **Torpedo Count**:
  - Message: "ALL PHOTON TORPEDOES EXPENDED."
  - Trigger: When the player runs out of torpedoes.
  - Representation: Use a 7-segment display to show the remaining torpedo count.

---

#### Lt. Sulu

- **Incorrect Course Data**:
  - Message: "LT. SULU REPORTS, 'INCORRECT COURSE DATA, SIR!'"
  - Trigger: When the player inputs an invalid course for navigation.
  - Representation: Flash a red LED labeled "NAV ERROR" or display an error code on a 7-segment display.

- **Warp Engine Speed Issues**:
  - Message: "WARP ENGINES ARE DAMAGED. MAXIMUM SPEED = WARP 0.2"
  - Trigger: When the player attempts to exceed the damaged warp engine's speed limit.
  - Representation: Blink an LED labeled "ENGINE DAMAGE."

---

To add audio effects and samples from the TV series, you would likely need a small set of iconic snippets to enhance the gameplay. Here's an estimate:

1. **Navigation and Alerts:**
 - "Red Alert" sound or siren.
 - "Warp Speed" or engine sound.
 - Total: 2 snippets.

2. **Combat and Weapons:**
 - Phaser firing sound.
 - Photon torpedo launch sound.
 - Klingon ship explosion sound.
 - Total: 3 snippets.

3. **Character Voices:**
 - Lt. Sulu: "Course laid in, Captain."
 - Chief Engineer Scott: "The engines won't take it!"
 - Lt. Uhura: "Message from Starfleet Command."
 - Science Officer Spock: "Sensors show no enemy ships."
 - Total: 4 snippets.

4. **Ambient Sounds:**
 - Bridge background noise.
 - Starbase docking sound.
 - Total: 2 snippets.
 - Total Duration

Each snippet would likely be 2-5 seconds long.
Total duration: 20-30 seconds of audio.

### Floating-Point Variables and Their Ranges

Below is a list of floating-point variables in the game and their potential ranges based on the game's logic:

1. **STARDATE**:
   - Represents the current in-game date.
   - Range: Likely starts around 2000.0 and increments by small values (e.g., 0.1 per turn).
   - Safe for fixed-point conversion.

2. **ENERGY**:
   - Represents the ship's energy level.
   - Range: Starts at 3000 and decreases during gameplay.
   - Safe for fixed-point conversion.

3. **SHIELD**:
   - Represents the ship's shield energy.
   - Range: Can vary but is typically below 3000.
   - Safe for fixed-point conversion.

4. **WARP FACTOR (W1)**:
   - Represents the warp speed.
   - Range: Typically between 0.0 and 8.0.
   - Safe for fixed-point conversion.

5. **DAMAGE CONTROL VARIABLES (e.g., WARP_ENGINES, SHORT_RANGE_SENSORS)**:
   - Represent the repair state of various ship systems.
   - Range: Typically between -1.0 (fully damaged) and 1.0 (fully repaired).
   - Safe for fixed-point conversion.

6. **REPAIR TIME (D3)**:
   - Represents the estimated time for repairs.
   - Range: Small values (e.g., 0.0 to 1.0).
   - Safe for fixed-point conversion.

7. **PHASER HIT POINTS (H)**:
   - Represents damage dealt to Klingons.
   - Range: Depends on energy allocated to phasers but likely below 1000.
   - Safe for fixed-point conversion.

8. **RANDOM DAMAGE/REPAIR AMOUNTS (DAMAGE_AMOUNT, REPAIR_AMOUNT)**:
   - Represent random values for damage or repair.
   - Range: Small values (e.g., 0.0 to 10.0).
   - Safe for fixed-point conversion.

### Conclusion
All floating-point variables in the game appear to fit within the range of -3,276.8 to +3,276.7 when converted to fixed-point with a single decimal place. A 16-bit signed value should be sufficient for this purpose.

### Next Steps
Would you like me to proceed with converting these variables to fixed-point representation in the code?

### Integer Variables and Their Potential Ranges

Below is a list of integer variables in the game and their potential ranges based on the game's logic:

1. **ENTERPRISEQUADRANTX, ENTERPRISEQUADRANTY**:
   - Represent the quadrant coordinates of the Enterprise.
   - Range: 1 to 8 (unsigned 8-bit).
   - Can fit in an 8-bit unsigned variable.

2. **ENTERPRISESECTORX, ENTERPRISESECTORY**:
   - Represent the sector coordinates of the Enterprise within a quadrant.
   - Range: 1 to 8 (unsigned 8-bit).
   - Can fit in an 8-bit unsigned variable.

3. **KLINGONCNT**:
   - Represents the number of Klingons in the current quadrant.
   - Range: 0 to 3 (unsigned 8-bit).
   - Can fit in an 8-bit unsigned variable.

4. **TOTALKLINGONCNT**:
   - Represents the total number of Klingons in the galaxy.
   - Range: Depends on the game setup but likely below 100.
   - Can fit in an 8-bit unsigned variable.

5. **BASECNT**:
   - Represents the number of starbases in the current quadrant.
   - Range: 0 to 1 (unsigned 8-bit).
   - Can fit in an 8-bit unsigned variable.

6. **TOTALBASECNT**:
   - Represents the total number of starbases in the galaxy.
   - Range: Likely below 10.
   - Can fit in an 8-bit unsigned variable.

7. **STARCNT**:
   - Represents the number of stars in the current quadrant.
   - Range: 0 to 64 (unsigned 8-bit).
   - Can fit in an 8-bit unsigned variable.

8. **TORPEDOCNT**:
   - Represents the number of photon torpedoes available.
   - Range: 0 to 10 (unsigned 8-bit).
   - Can fit in an 8-bit unsigned variable.

9. **DIRECTIONVECTOR (Array)**:
   - Represents movement vectors for navigation.
   - Range: -1 to 1 (signed 8-bit).
   - Can fit in an 8-bit signed variable.

10. **VISITEDQUADRANTS (Array)**:
    - Represents whether a quadrant has been visited.
    - Range: 0 to 999 (unsigned 16-bit).
    - Requires a 16-bit unsigned variable.

11. **GALAXY (Array)**:
    - Represents the contents of each quadrant (Klingons, bases, stars).
    - Range: 0 to 999 (unsigned 16-bit).
    - Requires a 16-bit unsigned variable.

12. **RANDOM COORDINATES (R1, R2)**:
    - Represent random positions within a quadrant.
    - Range: 1 to 8 (unsigned 8-bit).
    - Can fit in an 8-bit unsigned variable.

### Summary
- **Variables that fit in 8-bit unsigned**:
  - ENTERPRISEQUADRANTX, ENTERPRISEQUADRANTY
  - ENTERPRISESECTORX, ENTERPRISESECTORY
  - KLINGONCNT, TOTALKLINGONCNT
  - BASECNT, TOTALBASECNT
  - STARCNT, TORPEDOCNT
  - RANDOM COORDINATES (R1, R2)

- **Variables that fit in 8-bit signed**:
  - DIRECTIONVECTOR

- **Variables requiring 16-bit unsigned**:
  - VISITEDQUADRANTS
  - GALAXY

### Conclusion
Many variables can fit within 8-bit ranges, but some (like VISITEDQUADRANTS and GALAXY) require 16-bit values. Using 16-bit integers universally might simplify implementation and avoid potential overflows. Let me know if you'd like to proceed with this approach or optimize specific variables.

### Pain Points for Assembly Conversion in `sstr-devices.bas`

Below are potential challenges and considerations when converting the code to assembly:

1. **Mathematical Operations**:
   - **`SQRT()`**:
     - The square root function is computationally expensive in assembly.
     - **Solution**: Replace with a small lookup table for common values or use an iterative approximation algorithm (e.g., Newton's method).

   - **Division and Multiplication**:
     - Division and multiplication are not natively supported on the 8080 and require software routines.
     - **Solution**: Optimize by using shifts for powers of 2 or precomputing constants where possible.

2. **String Handling**:
   - The game uses many strings for messages and labels (e.g., "DAMAGE CONTROL REPORT").
   - **Challenge**: The 8080 has no native string handling, so you'll need to implement routines for printing and managing strings.
   - **Solution**: Store strings in ROM and use a pointer-based approach to print them.

3. **Array Management**:
   - Arrays like `GALAXY(8,8)` and `KLINGONSTATUS(3,3)` require careful indexing.
   - **Challenge**: The 8080 lacks direct support for multi-dimensional arrays.
   - **Solution**: Flatten arrays into a single dimension and calculate offsets manually.

4. **Floating-Point Arithmetic**:
   - Variables like `STARDATE`, `ENERGY`, and `SHIELD` use floating-point arithmetic.
   - **Challenge**: The 8080 has no native floating-point support.
   - **Solution**: Convert to fixed-point arithmetic with a single decimal place (e.g., multiply all values by 10).

5. **Control Flow**:
   - The BASIC code uses `GOTO` and `GOSUB` extensively.
   - **Challenge**: Translating these into assembly requires careful management of jump instructions and the stack.
   - **Solution**: Use labels for `GOTO` and the 8080's `CALL`/`RET` instructions for `GOSUB`.

6. **Random Number Generation**:
   - The game uses `RND()` for randomness.
   - **Challenge**: The 8080 has no built-in random number generator.
   - **Solution**: Implement a simple pseudo-random number generator (e.g., linear congruential generator).

7. **Device Status Updates**:
   - The code frequently checks and updates device statuses (e.g., `WARP_ENGINES`, `SHORT_RANGE_SENSORS`).
   - **Challenge**: These involve conditional logic and arithmetic.
   - **Solution**: Use efficient branching and lookup tables where possible.

8. **Damage and Repair Logic**:
   - The game calculates random damage and repair amounts.
   - **Challenge**: These calculations involve randomness and floating-point arithmetic.
   - **Solution**: Precompute damage/repair values or use fixed-point arithmetic.

9. **Sector and Quadrant Mapping**:
   - The game uses `GALAXY` and `VISITEDQUADRANTS` arrays to track the state of the game world.
   - **Challenge**: Managing these arrays in assembly requires careful memory management.
   - **Solution**: Use a fixed memory layout and calculate offsets manually.

### Recommendations
- **Lookup Tables**: Use lookup tables for `SQRT()` and other repetitive calculations.
- **Fixed-Point Arithmetic**: Replace floating-point operations with fixed-point arithmetic.
- **String Optimization**: Store strings in ROM and use pointers for efficient access.
- **Flatten Arrays**: Simplify array management by flattening multi-dimensional arrays.
- **Random Number Generator**: Implement a simple pseudo-random number generator.

Would you like me to assist with creating lookup tables or optimizing specific parts of the code for assembly?

---
mengstr: Can you identify any pain points and problems for porting/rewriting the sstd-devices.bas to 8080 assembly. Assume all floating point veriables and operations are handled in fixed point (single decimal) 16 bit variables.

GitHub Copilot: Porting this BASIC code to 8080 assembly, even with fixed-point arithmetic, presents several challenges:

1.  **Fixed-Point Arithmetic:**
    *   **Implementation:** Implementing 16-bit fixed-point (one decimal place) addition, subtraction, multiplication, division, and comparison requires custom assembly routines. Multiplication and division are particularly complex, needing shifts to handle the decimal point and potentially 32-bit intermediate results.
    *   **Range and Precision:** Ensuring all calculations stay within the -3276.8 to +3276.7 range and that the single decimal place provides sufficient precision for game logic (e.g., damage calculation, probabilities) is critical.
    *   **`RND(1)`:** Replacing the floating-point `RND(1)` requires implementing a 16-bit pseudo-random number generator (PRNG) and scaling its output to the fixed-point format (0.0 to 1.0 range, represented as 0 to 10).
    *   **`SQR()`:** The square root function (line 8460) needs a dedicated fixed-point algorithm.
    *   **`INT()`:** Integer truncation needs careful handling for both positive and negative fixed-point numbers.

2.  **Array Management:**
    *   **Addressing:** Calculating the memory address for multi-dimensional array elements (e.g., `GALAXY(I,J)`, `VISITEDQUADRANTS(I,J)`, `KLINGONSTATUS(I,J)`) in assembly is manual and error-prone. It involves base address + row index * row size + column index calculations, scaled by the element size (2 bytes for 16-bit fixed-point).
    *   **Memory Layout:** Allocating sufficient contiguous memory for all arrays (`GALAXY`, `VISITEDQUADRANTS`, `DIRECTIONVECTOR`, `KLINGONSTATUS`, `LRSBUFFER`) needs careful planning.

3.  **String Handling:**
    *   **Representation:** Strings need to be stored as byte arrays, likely null-terminated or length-prefixed.
    *   **Manipulation:** Functions like `LEFT$`, `MID$`, `RIGHT$`, concatenation (`+`), `LEN`, and comparisons require dedicated assembly routines for byte manipulation, copying, and searching. The complex update to `Q$` (line 8700) will be intricate.
    *   **Number-to-String:** Converting fixed-point numbers to decimal strings for `PRINT` statements (`STR$`, implicit in `PRINT`) requires a non-trivial routine, including handling the decimal point.

4.  **Control Flow:**
    *   **`GOTO`/`GOSUB`:** While mapping to `JMP`/`CALL` is direct, the heavy use of `GOTO` (e.g., lines 850, 860, 1100, 1390, etc.) can lead to complex and hard-to-maintain "spaghetti code" in assembly.
    *   **`ON...GOTO`:** Computed jumps (lines 2140, 7352) require implementing jump tables or sequential compare-and-jump logic.
    *   **`FOR...NEXT`:** Loops need manual implementation using registers/memory for counters, increment/decrement, comparison, and conditional jumps. Nested loops require careful register management.

5.  **Input/Output:**
    *   **`PRINT`:** Requires calling low-level character output routines repeatedly. Formatting (`TAB`) needs manual implementation (e.g., looping to print spaces).
    *   **`INPUT`:** Requires routines to read character input, buffer it, handle basic editing (like backspace), and parse the resulting string into the 16-bit fixed-point format.

6.  **Variable Management:**
    *   The 8080 has a limited number of registers. Managing the numerous BASIC variables (`I`, `J`, `KLINGONCNT`, `TOTALKLINGONCNT`, `R1`, `BASECNT`, `ENTERPRISEQUADRANTX`, `ENERGY`, `SHIELD`, `W1`, `N`, etc.) will require extensive use of RAM and potentially the stack, increasing complexity and execution time.

7.  **Code Size and Complexity:** The translation from high-level BASIC to low-level assembly will result in a significantly larger codebase that is much harder to read, debug, and maintain. The original BASIC code is already quite dense and uses many `GOTO`s.

---

## FLOATING POINT OPERATIONS

## Random Number Generation & Comparisons
- `R1 = RND(1)` (line 840)
- `IF R1>.98 THEN...` (line 850)
- `IF R1>.95 THEN...` (line 860)
- `IF R1>.80 THEN...` (line 870)
- `IF RND(1)>.96 THEN...` (line 981)
- `IF RND(1)>.2 THEN...` (line 2801)
- `IF RND(1)>.6 OR H/SHIELD <= .02 THEN...` (line 6120)

## Galaxy Generation
- `INT(RND(1)*7.98+1.01)` (lines 1040, 1162, 1163)
- `INT(RND(1)*20+20)*100` (line 370)
- `INT(RND(1)*10)` (line 372)

## Direction Vector Interpolation
- `X1=DIRECTIONVECTOR(C1,1)+(DIRECTIONVECTOR(C1+1,1)-DIRECTIONVECTOR(C1,1))*(C1-INT(C1))`
- `X2=DIRECTIONVECTOR(C1,2)+(DIRECTIONVECTOR(C1+1,2)-DIRECTIONVECTOR(C1,2))*(C1-INT(C1))`

## Ship & Torpedo Movement
- `ENTERPRISESECTORX=ENTERPRISESECTORX+X1` (line 3171)
- `ENTERPRISESECTORY=ENTERPRISESECTORY+X2` (line 3172)
- `X=X+X1` and `Y=Y+X2` (lines 4920-4921)
- `X3=INT(X+.5)` and `Y3=INT(Y+.5)` (lines 4922-4923)
- `T8=.1*INT(10*W1)` (line 3430)

## Damage Control
- `D4=.5*RND(1)` (line 1326)
- `IF D3>=1 THEN D3=.9` (line 5782)
- `DAMAGE_AMOUNT = RND(1)*5+1` (line 2921)
- `REPAIR_AMOUNT = RND(1)*3+1` (line 3001)
- `INT(WARP_ENGINES*100)*.01` (lines 5912-5919)

## Combat Calculations
- `H=INT((H1/FND(0))*(RND(1)+2))` (line 4480)
- `IF H>.15*KLINGONSTATUS(I,3) THEN...` (line 4481)
- `KLINGONSTATUS(I,3)=KLINGONMAXSHIELDS*(0.5+RND(1))` (line 1783)
- `KLINGONSTATUS(I,3)=KLINGONSTATUS(I,3)/(3+RND(0))` (line 6062)
- `DAMAGE_AMOUNT = H/SHIELD + .5*RND(1)` (line 6142)

## Display and Reporting
- `.1*INT((STARDATESTART+MISSIONLENGTH-STARDATE)*10)` (line 7960)
- `INT(STARDATE*10)*.1` (line 6850)

## Status & Energy Management
- `IF X<=ENERGY+SHIELD THEN...` (line 5590)
- `IF ENERGY<INITIALENERGY*.1 THEN...` (line 6661)

## Direction & Distance Calculations
- `C1+(ABS(A)/ABS(X))` (line 8330)
- `C1+(((ABS(X)-ABS(A))+ABS(X))/ABS(X))` (line 8430)
- `SQR(X^2+A^2)` (line 8460)

## Efficiency Rating
- `1000*(INITIALKLINGONCNT/(STARDATE-STARDATESTART))^2` (line 6400)
```

