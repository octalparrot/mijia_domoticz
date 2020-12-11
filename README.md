# mijia
Simple script to report temperature, humidity and battery level of Xiaomi
Mijia Bluetooth Temperature and Humidity Sensor. 
Requires gatttool,bc and xxd tools to be installed.

On Ubuntu 18.04 these packages can be installed with 
```apt install xxd bc bluez```

Temperatures returned are Celsius degrees.

You could add it in cron to do this repeatedly.

## Usage

```
./btdomo.sh -a [address] -n [sensor name] -s [domoticz address:port] -id [idx in domoticz]

Mandatory arguments:

-a  | --address           Bluetooth MAC address of sensor.
-n  | --name              Name of the sensor to use.
-s  | --server            Domoticz server address eg. localhost:8080
-id | --idx               Idx in domoticz

Optional arguments:

-r  | --retries           Number of max retry attempts. Default  times.
-d  | --debug             Enable debug printouts.
-h  | --help
```
## Find MJ_HT_V1
```
root@gen8:~/# hcitool lescan
LE Scan ...
58:2D:34:3A:79:EB (unknown)
58:2D:34:3A:79:EB MJ_HT_V1
```
# Example
```
root@gen8:~/# /btdomo.sh  -a 58:2D:34:34:41:18 -s 192.168.10.1:6080 -id 133

{
        "status" : "OK",
        "title" : "Update Device"
}
```

## If you want to use multiple sensors
Create file eg. /home/domo/tempbt.sh
```
#!/bin/bash

/home/domo/btdomo.sh  -a 58:2D:34:34:41:18 -s 192.168.10.1:6080 -id 133
/home/domo/btdomo.sh -a 4C:65:A8:DA:88:A9 -s 192.168.10.1:6080 -id 67
```
### Crontab line to run every 10 minutes
```
*/10 * * * * /home/domo/tempbt.sh
```

