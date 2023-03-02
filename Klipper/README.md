# Config Files for Klipper


# SuperSlicer Custom G-Code
## Start G-Code
```
; set rpi fan to full
SET_FAN_SPEED FAN=rpi SPEED=1
; set standby temp, extruder 0
{if first_layer_temperature[0] > 50}M104 S175 T0{endif}
; set standby temp, extruder 1
{if first_layer_temperature[1] > 50}M104 S175 T1{endif}
; set bed temperature
M140 S{max(first_layer_bed_temperature[0], first_layer_bed_temperature[1])}
; park extruders
_PARK_extruder1
_PARK_extruder
T0
; set and wait for bed temperature
M190 S{max(first_layer_bed_temperature[0], first_layer_bed_temperature[1])}
; home, tram gantry, and home
G34
; auto bed leveling
;G29
;auto bed leveling, only active pint area
BED_MESH_CALIBRATE PRINT_MIN={first_layer_print_min[0]},{first_layer_print_min[1]} PRINT_MAX={first_layer_print_max[0]},{first_layer_print_max[1]} FORCE_NEW_MESH=True
; heat to temp, initial extruder
{if first_layer_temperature[initial_extruder] > 50}M109 S{first_layer_temperature[initial_extruder]} T[initial_extruder]{endif}
; park extruders
_PARK_extruder1
_PARK_extruder
; initiate extruder
T[initial_extruder]
```
## End G-Code
```
;zero rpi fan
SET_FAN_SPEED FAN=rpi SPEED=0
;turn off heaters
TURN_OFF_HEATERS
;raise z
G91
G0 Z5 F500
G90
; park extruders
_PARK_extruder1
_PARK_extruder
T0
; present print
BED_PRESENT
; disable steppers
M18
; kill fan
M107
```
## Before Layer Change G-Code
## After Layer Change G-Code
## Tool Change G-Code
##### Under Construction
```
; heat to temp, next extruder
{if first_layer_temperature[next_extruder] > 50}M109 R{first_layer_temperature[next_extruder]} T[next_extruder]{endif}
; park extruder
M125
; set standby temp, current extruder
{if first_layer_temperature[current_extruder] > 50}M104 S175 T[current_extruder]{endif}
; change extruder
T[next_extruder]
```
## Between Objects G-Code (for Sequntial Printing)
## Between Extrusion Role Change G-Code
## Colour Change G-Code
## Pause Print G-Code
```
PAUSE
```
