# soma:1

Turn a Raspberry Pi into a SomaFm streaming device.

_This is a work in progress_

## Prerequisites

Installation is based on a Raspberry Pi running Raspberry Pi OS Lite. It is assumed you have a system up and running, connected to the Internet and terminal access via console or SSH.

## MQTT

This project uses the MQTT protocol for playback control. You will need access to a broker.

## Out of scope

This does not cover setting up your audio output. A proper DAC is recommended.

## Dependancies

Run the following:

```
sudo apt update -y && sudo apt upgrade -y
sudo apt install mpv
```

## Installation

Clone this repo:

```
cd
git clone https://github.com/mrpjevans/soma1.git
```

## Configuration

Edit the the main file:

```
cd ~/soma1
nano somoa1.py
```

Change these lines to suit your config:

```
broker = '192.168.1.10' // Change your broker's hostname

// Don't change any of these unless you need to
port = 1883
topic = "study/sonos1/somafm"
client_id = "soma1"
```

Now create the service file, so everything runs on boot:

```
sudo nano /usr/lib/systemd/soma1.service
```

Add the text below, changing the paths as required:

```
[Unit]
After=network-online.target
Requires=network-online.target
Description=Soma1

[Service]
WorkingDirectory=/home/pj/soma1
ExecStart=/usr/bin/python3 ./soma1.py
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

```
sudo systemctl enable /usr/lib/systemd/soma1.service
sudo systemctl start soma1.service
```
## Usage

Using the default config, you can control playback as follows:

All published to the MQTT topic `study/sonos1/somamf` :

```
channel:name
```

Commences playback of the given channel (`name`). For a valid list of channels, run `~/soma1/somafm.sh list`

```
stop
```

Stops playback

```
volume:n
```

Sets the volume to the value of n (a percentage)

```
volume:up
```

Ups the volume by 5%

```
volume:down
```

Lowers the volume by 5%

## Credits

This code leans heavily on the pure shell code by Rocky Madden:
https://github.com/rockymadden/somafm-cli
