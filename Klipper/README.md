# Config Files for Klipper
See attached files.

# SuperSlicer Custom G-Code
## Start G-Code
```
; set rpi fan to full
SET_FAN_SPEED FAN=rpi SPEED=1

; set bed temperature
M118{"Bed Set to Temp "}{max(first_layer_bed_temperature[0], first_layer_bed_temperature[1])}
M140 S{max(first_layer_bed_temperature[0], first_layer_bed_temperature[1])}

; set standby temp, extruder 0
{if first_layer_temperature[0] > 50}
	M118{"T0 Set to Standby Temp "}{round(first_layer_temperature[0]*.85)}{else}
	M118{"T0 Set to Temp Off"}{endif}
{if first_layer_temperature[0] > 50}
	M104 S{round(first_layer_temperature[0]*.85)} T0{else}
	M104 S0 T0{endif}

; set standby temp, extruder 1
{if first_layer_temperature[1] > 50}
	M118{"T1 Set to Standby Temp "}{round(first_layer_temperature[1]*.85)}{else}
	M118{"T1 Set to Temp Off "}{endif}
{if first_layer_temperature[1] > 50}
	M104 S{round(first_layer_temperature[1]*.85)} T1{else}
	M104 S0 T1{endif}

; set initial temp, initial extruder
M118{"T"}[initial_extruder]{" Initial Extruder Set to Temp "}{first_layer_temperature[initial_extruder]}
M104 S{first_layer_temperature[initial_extruder]} T[initial_extruder]

; heat to temp, bed temperature
M118{"Bed Heating to Temp "}{max(first_layer_bed_temperature[0], first_layer_bed_temperature[1])}
M190 S{max(first_layer_bed_temperature[0], first_layer_bed_temperature[1])}

; heat to temp, initial extruder
M118{"T"}[initial_extruder]{" Initial Extruder Heating to Temp "}{first_layer_temperature[initial_extruder]}
M109 S{first_layer_temperature[initial_extruder]} T[initial_extruder]

; home, tram gantry, and home
G34

; auto bed leveling
;G29

; auto bed leveling, only active print area
BED_MESH_CALIBRATE PRINT_MIN={first_layer_print_min[0]},{first_layer_print_min[1]} PRINT_MAX={first_layer_print_max[0]},{first_layer_print_max[1]} FORCE_NEW_MESH=True

; park extruders
_PARK_extruder1
_PARK_extruder

; initiate extruder
T[initial_extruder]
```
## End G-Code
```
; zero rpi fan
SET_FAN_SPEED FAN=rpi SPEED=0

; turn off heaters
TURN_OFF_HEATERS

; raise z
G91
G0 Z5 F500
G90

; kill fan
M107

; park extruders
_PARK_extruder1
_PARK_extruder
T0

; present print
BED_PRESENT

; disable steppers
M18
```
## Before Layer Change G-Code
```
```
## After Layer Change G-Code
```
```
## Tool Change G-Code
- ##### Under Construction
```
; raise z
G91
G0 Z5 F500
G90

; set standby temp, current extruder
M118{"T"}[current_extruder]{" Current Extruder Set to Standby Temp "}{round(first_layer_temperature[current_extruder]*.85)}
M104 S{round(first_layer_temperature[current_extruder]*.85)} T[current_extruder]

; set temp, next extruder
M118{"T"}[next_extruder]{" Next Extruder Set to Temp "}{first_layer_temperature[next_extruder]}
M104 S{first_layer_temperature[next_extruder]} T[next_extruder]

; park extruders
_PARK_extruder1
_PARK_extruder

; heat to temp, next extruder
M118{"T"}[next_extruder]{" Next Extruder Heating to Temp "}{first_layer_temperature[next_extruder]}
M109 S{first_layer_temperature[next_extruder]} T[next_extruder]

; change extruder
T[next_extruder]
```
## Between Objects G-Code (for Sequntial Printing)
```
```
## Between Extrusion Role Change G-Code
```
```
## Colour Change G-Code
```
```
## Pause Print G-Code
```
PAUSE
```
## Template Custom G-Code
```
```
