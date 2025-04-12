
# Star Trek Variable Types Analysis

Looking at the variables in the Star Trek code, I can identify which are used as integers and which are used as floating point values. This is important to understand the game mechanics and any potential rounding issues.

## Integer Variables

These variables are always used as whole numbers:

1. **ENTERPRISEQUADRANTX, ENTERPRISEQUADRANTY** - Always whole numbers (1-8)
2. **KLINGONCNT, TOTALKLINGONCNT** - Counts of Klingon ships
3. **BASECNT, TOTALBASECNT** - Counts of starbases
4. **STARCNT** - Count of stars in current quadrant
5. **TORPEDOCNT, INITIALTORPEDOES** - Always whole numbers
6. **ISDOCKED** - Boolean flag (0 or 1)
7. **Z3** - Binary flag used for string comparison results 
8. **MISSIONLENGTH** - Always an integer value
9. **X3, Y3** - Torpedo track coordinates (explicitly converted with INT)
10. **X5** - Border crossing flag (0 or 1)

## Floating Point Variables

These variables can have fractional components:

1. **ENTERPRISESECTORX, ENTERPRISESECTORY** - Used in movement calculations with fractions
2. **ENERGY** - Can have fractional values after calculations
3. **SHIELD** - Shield energy can be fractional
4. **STARDATE** - Incremented by fractional amounts (T8 can be 0.1 times warp factor)
5. **DEVICESTATUS(array)** - Damage values can be fractional
6. **X1, X2** - Direction vector components (often fractional for diagonal movement)
7. **KLINGONSTATUS(i,3)** - Klingon shield strength can be fractional
8. **W1** - Warp factor input (can be 0.2, etc.)
9. **D3, D4** - Repair difficulty modifiers (fractional)
10. **H, H1** - Hit strength calculations (can be fractional)

## Mixed-Use Variables

These variables are sometimes treated as integers, sometimes as floats:

1. **C1** - Course input can be integer (1-8) but is used in fractional calculations
2. **Z1, Z2** - Used both as integer coordinates and in floating point calculations
3. **T8** - Time increment (1 for warp ≥1, fractional for lower warps)
4. **N** - Energy for warp movement (calculated as INT(W1*8+.5))
5. **R1, R2** - Random coordinates (fractionally calculated but often used as integers)

## Arrays

1. **GALAXY(8,8)** - Integer values (encoded as KLINGONCNT*100+BASECNT*10+STARCNT)
2. **VISITEDQUADRANTS(8,8)** - Integer values (copy of GALAXY entries)
3. **DIRECTIONVECTOR(9,2)** - Integer values (-1, 0, 1)
4. **KLINGONSTATUS(3,3)** - Mixed (positions are integers, health can be fractional)
5. **LRSBUFFER(3)** - Integer values from galaxy map

Understanding these variable types helps explain why certain calculations use INT() functions and why some game mechanics work the way they do, especially in combat damage calculations and movement.
