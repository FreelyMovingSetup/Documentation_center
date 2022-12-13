## root commands :

### program info

<u>Input :</u>

```json
{info:1}
```

!!! info "type"
    argument value type  : *ignored*

<u>Output :</u>

Returns information about the program file and date of the code running on the device, for traceability, under the keys : `prog_file` and `prog_date`, as strings.



### handshake

<u>Input :</u>

```json
{handshake:1}
```

!!! info "type"
    argument value type  : *ignored*

<u>Output :</u>

Return an handshake acceptation under key `accept_handshake` as boolean with value True.



### reboot :

<u>Input :</u>

```json
{reboot:1}
```

!!! info "type"
    argument value type  : *ignored*

<u>Output :</u>

Returns nothing, activates watchdog timer, and wait 15ms that it triggers a software reboot.



## imu : 

### get_error:

<u>Input :</u>

```json
{imu:{get_error:1}}
```

!!! info "type"
    argument value type  : *ignored*

<u>Output :</u>

Returns status of the access between microcontroller and IMU. Possible values returned are listed [here](https://github.com/sparkfun/SparkFun_ICM-20948_ArduinoLibrary/blob/main/src/ICM_20948.cpp#L264). If everything is ok, you should get : *All is well.*

Returns a string under the key : `IMU_error`







## coupling :

Coupling is separated into two classes that are both attainable with the same key.

First one is responsible of keeping track of position, turns and offsets :

### set_status :

<u>Input :</u>

```json
{coupling:{set_status:1}}
```

!!! info "type"
    Argument value type  : *float*
    Must be an angle value between -180 and +180 degrees

Sets the coupling mode ON (value = 1) or OFF (value=0). If the coupling is off, the position will still be computed but the motor will not be updated by these new positions until coupling is turned ON again.

<u>Output :</u>

Returns nothing.

### set_offset :

<u>Input :</u>

```json
{coupling:{set_offset:35.78}}
```

!!! info "type"
    Argument value type  : *float*
    Must be an angle value between -180 and +180 degrees

<u>Output :</u>

Returns nothing.

### get_offset : 

<u>Input :</u>

```json
{coupling:{get_offset:1}}
```

!!! info "type"
    Argument value type  : *ignored*

<u>Output :</u>

Returns the cur.

### set_zero :

<u>Input :</u>

```json
{coupling:{set_zero:1}}
```

!!! info "type"
    Argument value type  : *ignored*

Sets the offset to correspond to the current instantaneous position, so that it's new value will be 0 after substraction.

<u>Output :</u>

Returns nothing.

### get_pos :

```
{coupling:{get_pos:1}}
```

Returns the position of the coupling in degrees (between -180 and +180)

### get_summed_pos : 

```
{coupling:{get_summed_pos:1}}
```

Returns the position of the coupling in degrees (between max int32 and min int32). This position is the accumulated rotationnal position since the MORJ is turned on. (the position at turn on is 0) Negative direction is counterclockwise. Positive is clockwise.

### add_turn :

```
{coupling:{add_turn:1}}
```

Add turns (one per command max, whatever the value) if supplying +1 : adding a clockwise turn. If supplying -1 adding a counterclockwise turn.

### get_turns :

```
{coupling:{get_turns:1}}
```

Get the number of current turns on the MORJ

### set_sensitivity :

```
{coupling:{set_sensitivity:600}}
```

Set the number of steps that have to be overcomed before the MORJ can issue command to the motor. (in absolute steps) Default is 600 (as 1 turn is 9600, 600 = 1/16th of a turn)

### get_last_cmd :

```
{coupling:{get_last_cmd:1}}
```

Get the last_motor command send to the motor, in steps, relative to the last-last command.

### get_last_scmd :

```
{coupling:{get_last_scmd:1}}
```

Get the last_motor command send to the motor, in steps, accumulated from 0 (0 = position when the MORJ turned ON)

### reset :

```
{coupling:{reset:1}}
```

Set  ``turns``, ` summed_pos`, ``offset``, `last_command`, `last_summed_command` to 0



## motor :

All the commands motor accepts, directly forwarded :

### handshake:

```
{motor:{handshake:1}}
```

Set  ``turns``, ` summed_pos`, ``offset``, `last_command`, `last_summed_command` to 0

### go :

```
{motor:{go:50}}
```

Go to a given position (relative to current position) in steps. can take positive or negative integers.

### go_abs:

DEPRECATED : DO NOT USE. (Was used to send absolute command (from the 0 of the motor) but usually creates very dangerous situations as it is hard for user, as well as software, to keep track of a variable number with extreme certainty across two distributed devices, even more when asynchronously zeroing them or reboots / crashes / disconnections are possible "features")

### get_pos:

```
{motor:{get_pos:1}}
```

DEPRECATED : DO NOT USE. (send absolute command (from the 0 of the motor) usually creates dangerous situations !!!!)

### stop :

```
{motor:{stop:50}}
```

Stop the motor movement, while keeping track of the destination position.

### lock :

```
{motor:{lock:1}}
```

NOT RECOMMANDED TO USE THIS COMMAND. Better use the MORJ specific lock manager by using the command `{lock :{set_status:0}}`  to keep track of the lock status.

### zero:

```
{motor:{zero:1}}
```

Set the OREF of the motor to the current position, at which the lock will come back. (by increments of one full turn)

### set_motor_accel:

```
{motor:{set_motor_accel:1}}
```

Set the motor maximum speed in steps per second. By default, 10000

### set_motor_max_speed:

```
{motor:{set_motor_max_speed:1}}
```

Set the motor maximum acceleration in steps per second per second. By default, 20000

This is usually the variable that have the strongest effect on the ability of the motor to make fast movements. Use with care and try increasing values slowly. If you goo to high, the motor will stall and thus the controller will loose the zero position as there is no rotary encoder on the motor or MORJ shaft.

## lock :

### set_status :

```
{lock :{set_status:0}}
```

Set the lock status of the motor. 0 is unlocked. 1 is locked. This takes priority over the BNC signal momentarily, but if the BNC status changes (Rise or Falls) after this command has been issued, the BNC status takes priority again.



## shutter :

### set_status :

```
{shutter :{set_status:0}}
```

Set the current shutter output BNC status. 0 is shutter deactivated (LOW). 1 is shutter activated. (HIGH). 

Remember that this MORJ output is set to INPUT_PULLUP, and so it can set a HIGH but not a LOW. To set a LOW, the other end of a BNC must be plugged into an input that can pull LOW. (all BNC compliant inputs usually can) This means that is the BNC is unplugged, there may be a RISING state detection at MORJ bootup on the shutter input line.



## bnc_in_1 : 

This line is a RefractoryInterruptInput (subclass of InterruptInput) which gets activated by rising edges (trigger mode can be configured in the code, not in the API)

When activated, it can't be again activated for the refractory period. 

For now the refractory period can't be set in API, but in the code (it's a fast thing to change it if necessary)

The refractory period is by default 1300000 microseconds, so 1.3 seconds, a bit more than the duration of the 500 pulses sent to the camera for one trial. This allows this line to generate one single trigger at the very start of a 500 Hz pulse train. After 1.3 sec, it doesn't generate anything, but simply can be activated again. This class is used to set and count the status of a trial recordings, and help the PC to save and split recordings of 1.3 sec per trial.

### trigger:

```
{bnc_in_1  :{trigger:1}}
```

Fakes a single trigger (here rising edge from code config) on the bnc_in_1input line. A trigger off will be happening 1.3 seconds after.

### info :

```
{bnc_in_1  :{info:1}}
```

Gets some info about the possible commands this class takes.





## bnc_in_2 : 

This line is a PollingInput class, and thus checks regularly in the loop if it has been activated. This is a bit unprecise and can miss very short pulses, but bnc_in_2 is used to detect the start of the session, and the stop of the session, from the maze.

It does so by detecting the rising edge (session start) of a very long pulse (a pulse the length of the session) and the falling edge (session_stop) of this pulse. Hence , precise timing is not an issue.

### info :

```
{bnc_in_2 :{info:1}}
```

Gets some info about the possible commands this class takes.

### trigger_rising

```
{bnc_in_2 :{trigger_rising:1}}
```

Fakes a single rising edge on the bnc_in_2  input line.

### trigger_falling :

```
{bnc_in_2 :{trigger_falling:1}}
```

Fakes a single falling edge on the bnc_in_2  input line.



## records :

### set_select:

```
{records:{set_select:{ "quat":1 , "turns" : 1 }}}
```

sets the status of data sent into packets for the subset of data attributes you pass into the sub-json object.

Possible keys are :

- "quat"
- "accel"
- "gyro"
- "mag"
- "yaw_quat"
- "summed_yaw_coupling"
- "yaw_coupling"
- "command"
- "summed_command"
- "turns"
- "euler_quat"

### get_select:

```
{records  :{get_select:1}}
```

Gets the current selection of data into packets

### start:

```
{records  :{start:1}}
```

Starts the sending of data packets at a regular interval.

### stop:

```
{records:{stop:1}}
```

Stops the sending of data packets at a regular interval.

### set_interval:

```
{records :{set_interval:50}}
```

Set interval (in milliseconds) between to data packets. (default, 50 milliseconds)

### set_trial_duration:

```
{records :{set_trial_duration:1500}}
```

The time interval (in milliseconds) after a bnc_in_1 trigger to declare the end of the trial (in recording) and increase the trial counter to 1. Set to 1500 milliseconds by default.

### set_quat_ref :

```
{records  :{set_quat_ref :1}}
```

Saves a quaternion reference with the current position, to remember a "zero" 3D orientation position that can be substracted later in python to all the data of the session.

### get_quat_ref :

```
{records  :{get_quat_ref :1}}
```

Gets the quaternion reference saved.

### info :

```
{records  :{info :1}}
```

Gets some info about the possible commands this class takes.



