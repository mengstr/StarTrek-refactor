'DIRECTION_VECTOR
' C is used for movement calculations when the player enters a course (1-9), creating a lookup table for direction vectors where:
' C(direction,1) is the X component
' C(direction,2) is the Y component
' For example:
' Course 1 = (0,1) - North
' Course 2 = (-1,1) - Northeast
' Course 3 = (-1,0) - East ...and so on in a circular pattern.

'KLINGONSTATUS
' Looking at the code, K (3,3) is an array that stores information about up to 3 Klingon ships in the current quadrant, where:
' First dimension (1-3): Can track up to 3 Klingons
' Second dimension (1-3):
' K (i,1) = X sector coordinate of Klingon i
' K (i,2) = Y sector coordinate of Klingon i
' K (i,3) = Remaining shield/health of Klingon i

'LRSBUFFER
' Looking at the code, N (3) is used in the Long Range Sensor scan to temporarily store data for displaying a 3x3 grid around the current quadrant:
' N () acts as a temporary buffer for one row (3 quadrants) of sensor data at a time, where:
' N (1) = left quadrant
' N (2) = center quadrant
' N (3) = right quadrant

'DEVICESTATUS
' WARP_DAMAGE = 0          ' D(1) Warp Engines
' SENSOR_DAMAGE = 0        ' D(2) Short Range Sensors 
' LRSENSOR_DAMAGE = 0      ' D(3) Long Range Sensors
' PHASER_DAMAGE = 0        ' D(4) Phaser Control
' TORPEDO_DAMAGE = 0       ' D(5) Photon Tubes
' DAMAGECONTROL_DAMAGE = 0 ' D(6) Damage Control
' SHIELD_DAMAGE = 0        ' D(7) Shield Control
' COMPUTER_DAMAGE = 0      ' D(8) Library Computer

' Here are the remaining 1-2 character variables (excluding loop counters I, J, L):

' A$ - Temporary string for map symbols ("<*>", "+K+", ">!<", " * ", " ")
' A - Used in direction calculations (lines 8120-8222)
' C$ - Condition status ("RED", "GREEN", "YELLOW", "DOCKED")
' C1 - Course input (1-9) for navigation
' D1 - Damage report flag (0/1)
' D3 - Repair time calculation
' D4 - Random repair difficulty modifier (line 1326)
' D6 - Warp factor for damage calculation
' G2$ - Device/region name storage (used in messages)
' G5 - Galaxy map display flag
' H1 - Phaser hit calculation intermediate
' H8 - Computer display mode flag
' H - Hit points in combat
' O1$ - Line separator string ("-------------------")
' Q$ - Current quadrant map string (190 characters)
' R1, R2 - Random position coordinates
' S8 - String position calculation
' T8 - Time increment for warp movement
' W1 - Warp factor input
' X$ - Plural suffix ("S" or "")
' X0$ - Verb agreement (" IS " or " ARE ")
' X1, X2 - Movement vector components
' X3, Y3 - Torpedo track coordinates
' X5 - Border crossing flag
' Z$ - Space padding string
' Z1, Z2 - Temporary coordinates
' Z3 - String comparison result
' Z4, Z5 - Quadrant coordinates for name lookup


' Original	Refactored	            Purpose
'-------------------------------------------------------------
' G(8,8)	GALAXY(8,8)	            Galaxy map data
' C(9,2)	DIRECTIONVECTOR(9,2)	Movement vectors
' K(3,3)	KLINGONSTATUS(3,3)	    Klingon ships data
' N(3)	    LRSBUFFER(3)	        Long range sensor buffer
' Z(8,8)	VISITEDQUADRANTS(8,8)	Visited quadrants record
' D(8)	    DEVICESTATUS(8)	        Ship device status
' Q1,Q2	    ENTERPRISEQUADRANTX,Y	Enterprise position
' S1,S2	    ENTERPRISESECTORX,Y	    Enterprise sector position
' T	        STARDATE	            Current game time
' K3	    KLINGONCNT	            Klingons in quadrant
' K9	    TOTALKLINGONCNT	        Total klingons remaining
' B3	    BASECNT	                Bases in quadrant
' B9	    TOTALBASECNT	        Total bases in galaxy
' S3	    STARCNT	                Stars in quadrant
' P	        TORPEDOCNT	            Torpedo count
' S	        SHIELD	                Shield energy
'   
' 
'   Main Loop Entry Points
' -------------------------------------------------
'   Line 1980: GOSUB 6430 - Display short range scan (primary loop start)
'       This is the start of the main game loop as you noted
'       All navigation commands return here to refresh the display
'   
'   Line 1990: Command input check
'       Most commands return here to get the next command
'       This skips redrawing the short-range scan when unnecessary

'   Line 1320: New quadrant entry
'       This is another important entry point
'       When moving to a new quadrant, we need initialization logic
'   
'                        ┌───────────────────┐
'                        │ Game Initialization│
'                        └──────────┬────────┘
'                                   │
'                                   ▼
'   ┌──────────────────────────────────────────────┐
'   │ New Quadrant (1320)                          │
'   │ - Set up new quadrant                        │
'   │ - Place objects                              │◄───────────┐
'   └─────────────────────┬────────────────────────┘            │
'                         │                                     │
'                         ▼                                     │ Quadrant
'   ┌──────────────────────────────────────────────┐            │ change
'   │ Display Short Range Scan (1980)              │            │
'   │ - Show current quadrant                      │◄───────┐   │
'   └─────────────────────┬────────────────────────┘        │   │
'                         │                                 │   │
'                         ▼                                 │   │
'   ┌──────────────────────────────────────────────┐        │   │
'   │ Command Input (1990)                         │        │   │
'   │ - Get player command                         │        │   │
'   └─────────────────────┬────────────────────────┘        │   │
'                         │                                 │   │
'                         ▼                                 │   │
'   ┌──────────────────────────────────────────────┐        │   │
'   │ Command Processing                           │        │   │
'   │ - NAV (movement) ────────────────────────────┼────────┘   │
'   │ - Fire weapons                               │            │
'   │ - Other commands ───────────────────────────────────────┐ │
'   │ - Quadrant transition ───────────────────────┼────────────┘
'   └──────────────────────────────────────────────┘          │
'                         │                                   │
'                         └───────────────────────────────────┘
'                        Return to command input
'                        


