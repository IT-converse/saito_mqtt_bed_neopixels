# Saito MQTT NeoPixel Bed

Very simple mqtt client app in Python that allows control of NeoPixel ring
attached to the my kids bed.

Makes use of Jeremy Garffs neopixel lib for the Rpi. More info at:

* https://learn.adafruit.com/neopixels-on-raspberry-pi?view=all
* https://github.com/jgarff/rpi_ws281x

## Installing rpi_ws281x

```shell
sudo apt-get update
sudo apt-get install build-essential python-dev git scons swig python-pip
cd ; git clone https://github.com/jgarff/rpi_ws281x.git
cd rpi_ws281x
scons
cd python
sudo python setup.py install
```


## Other requirements

You also need jsonschema and paho mqtt libs.

```shell
sudo pip install jsonschema
sudo pip install paho-mqtt
```


## Installing Digital led mqtt

```shell
cd sudo apt-get update
git clone https://github.com/IT-converse/digital_Led_MQTT.git
```


## Hardware

Currently library defaults to GPIO18 (pin 12) on http://www.raspberry-pi-geek.com/howto/GPIO-Pinout-Rasp-Pi-1-Model-B-Rasp-Pi-2-Model-B

## Home Assistant

Just add an MQTT JSON Light with following config:

```
# Enable mqtt
mqtt:
  broker: xxxxxxxxxxxxx
  port: 8000
  client_id: xxxxxxxxxxxxxxxxx
  keepalive: 60

# Example configuration.yaml entry
light:
  platform: mqtt_json
  name: "Saito RGB Light"
  command_topic: "saito/bed/neopixels/set"
  state_topic: "saito/bed/neopixels"
  brightness: true
  rgb: true
  effect: true
  effect_list: [rainbow, rainbowcycle, theaterchaserainbow, colorwipe, theaterchase]
```

and you are good to go.

State and commands are passed using json:

```json
{
  "brightness": 255,
  "color": {
    "g": 255,
    "b": 255,
    "r": 255
  },
  "effect": "rainbow",
  "transition": 2,
  "state": "ON"
}
```

## Autostart on Raspberry Pi

Just use pm2 to start the script

```shell
sudo python app.py
```

## Start after reboot

```shell
sudo nano /etc/rc.local
```

add you path before the `exit 0`

```shell

sudo python /home/pi/Digital_Led_MQTT/app.py &

```

test it with a `sudo reboot`



## Static IP

```shell
sudo nano /etc/dhcpcd.conf
```

```shell


interface eth0

static ip_address=192.168.11.41/24
static routers=192.168.11.1
static domain_name_servers=192.168.11.1

interface wlan0

static ip_address=192.168.11.41/24
static routers=192.168.11.1
static domain_name_servers=192.168.11.1
```

## SSH PI

add to `config.txt`

```
# Enable the serial port
enable_uart=1
​
# Disable bluetooth
dtoverlay=pi3-disable-bt
```

add a file 'ssh'