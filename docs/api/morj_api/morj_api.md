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



## IMU : 

### get_error:

<u>Input :</u>

```json
{IMU:{get_error:1}}
```

!!! info "type"
    argument value type  : *ignored*

<u>Output :</u>

Returns status of the access between microcontroller and IMU. Possible values returned are listed [here](https://github.com/sparkfun/SparkFun_ICM-20948_ArduinoLibrary/blob/main/src/ICM_20948.cpp#L264). If everything is ok, you should get : *All is well.*

Returns a string under the key : `IMU_error`



