# Marlin-Firmware-BTT-SKR-E3V2
Marlin Firmware Ender 3 :  BTT SKR E3V2 + BTT TFT E35 V3 E3 + BTT SMART FILAMENT SENSOR V1 + BLTOUCH 3D TOUCH CLONE + DIRECTDRIVE

DirectDrive : https://www.thingiverse.com/thing:3650780 #define NOZZLE_TO_PROBE_OFFSET { -45, -10, 0 } be careful the nozzle fan need to be adjusted on the stl because the size is different. And BLTOUCH is to hight but it work 
BTT SFS Top Mount : https://www.thingiverse.com/thing:5693438
Spool Holder : https://www.printables.com/fr/model/189847-ender-3-angled-spool-holder/remixes
Vslot cover for the cable of the BTT SFS : https://www.printables.com/fr/model/374623-flush-v-slot-rail-covers-parametric-pre-sized-for-/files

BTT SFS check the pinout on the board and the sensor - if using the Touch mode : need to set the sensor on the TFT to : off

Steps : Extruder 99.91
PID : BED : Kp 143.73 Ki 27.64 Kd 498.260
PID : HOTEND : Kp 23.82 Ki 2.13 Kd 66.70

Start Gcode : 
M75
G90 ; use absolute coordinates
M83 ; extruder relative mode
M104 S150 ; set temporary nozzle temp to prevent oozing during homing and auto bed leveling
M140 S{first_layer_bed_temperature[0]} ; set final bed temp
G4 S30 ; allow partial nozzle warmup
G28 ; home all axis
M420 S1 Z10
G1 Z50 F240
G1 X2.0 Y10 F3000
M104 S{first_layer_temperature[0]} ; set final nozzle temp
M190 S{first_layer_bed_temperature[0]} ; wait for bed temp to stabilize
M109 S{first_layer_temperature[0]} ; wait for nozzle temp to stabilize
G1 Z0.28 F240
G92 E0
G1 X2.0 Y140 E10 F1500 ; prime the nozzle
G1 X2.3 Y140 F5000
G92 E0
G1 X2.3 Y10 E10 F1200 ; prime the nozzle
G92 E0
M412 S1

End Gcode :
{if max_layer_z < max_print_height}G1 Z{z_offset+min(max_layer_z+2, max_print_height)} F600 ; Move print head up{endif}
G1 X5 Y{print_bed_max[1]*0.85} F{travel_speed*60} ; present print
{if max_layer_z < max_print_height-10}G1 Z{z_offset+min(max_layer_z+70, max_print_height-10)} F600 ; Move print head further up{endif}
{if max_layer_z < max_print_height*0.6}G1 Z{max_print_height*0.6} F600 ; Move print head further up{endif}
M140 S0 ; turn off heatbed
M104 S0 ; turn off temperature
M107 ; turn off fan
M84 X Y E ; disable motors
M77
