# MQTT2Dream(screen)

## What?

It's a simple script to bridge mqtt messages to switch a Dreamscreen (4k) on and off.

## Why?

in combination with [homebridge-mqtt](https://github.com/hobbyquaker/homekit2mqtt) this enables you to control your Dreamscreen with Homekit

### Config?
sure

```
    "Dreamscreen": {
        "name": "Dreamscreen",
        "service": "Switch",
        "topic": {
            "statusOn": "dream/status",
            "setOn": "dream/set"
        },
        "model": "Switch",
        "payload": {
            "onFalse": "standby",
            "onTrue": "on"
        },
        "manufacturer": "vendor 4346"
    }
```
only tested with homebride-mqtt version 0.6.5 ... it's running on my machine

## How?

### Install

1. `curl https://raw.githubusercontent.com/lad1337/mqtt2dream/master/mqtt2dream.py -o /usr/local/bin/mqtt2dream`
2. `chomd +x /usr/local/bin/mqtt2dream`
3. `sudo pip install paho-mqtt`

### Run

`mqtt2dream <mqtt-host> <dreamscreen-host>`  
e.g.
`mqtt2dream.py 10.0.13.39 10.0.13.62`

Note: port of MQTT is hardcoded to `8888` ... change that in the file for now

### Service file template

```
# /etc/systemd/system/mqtt2dream.service
[Unit]
Description=mqtt dreamscreen bridge
After=syslog.target network.target

[Service]
Type=simple
PIDFile=/var/mqtt2dream/mqtt2dream.pid
ExecStart=/usr/local/bin/mqtt2dream <mqtt-host> <dreamscreen-host>
Restart=on-failure

[Install]
WantedBy=multi-user.target
```