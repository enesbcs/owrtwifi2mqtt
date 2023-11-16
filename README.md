A simple shell script to detect presence of Wifi devices (smartphones, tablets, Amazon Dash Buttons, ..) and post the results via MQTT. HA MQTT Autodiscovery capability added, every new MAC will identify itself as "binary_sensor".

Installation
------------

### Install the MQTT client

Install the packages

- `mosquitto-client`
- `coreutils-nohup`

with either luci or opkg.

### Get the script

#### Download

    wget -O /usr/bin/presence_report https://raw.githubusercontent.com/enesbcs/owrtwifi2mqtt/master/presence_report && chmod u+x /usr/bin/presence_report

#### Copy with SCP

Use SCP to copy the presence_report script to `/usr/bin/presence_report` on the target device.
Call `chmod u+x /usr/bin/presence_report` to allow script execution.

### Add the script to rc.local

Place the following lines

    nohup /usr/bin/presence_report 192.168.1.2 >/dev/null 2>&1 &

inside the `/etc/rc.local` file before the `exit 0`. You can to this via command-line or via LuCI in System -> Startup -> Local Startup. The script will be executed after reboot.

ENV variables for configuration are:

- `MQTT_BASETOPIC`: `openwrt/HOSTNAME`
- `MQTT_STATUS_TOPIC`: `$MQTT_BASETOPIC/`
- `MQTT_USER`
- `MQTT_PASSWORD`
- `MQTT_ID`: `$MQTT_BASETOPIC`
- `MQTT_DISCOVERY`: `homeassistant/`

Usage
-----

After installation the following topics will be published for each WiFi device, using the _lowercase_ MAC address:

	openwrt/HOSTNAME/00-00-00-00-00-00/iwevent
	
Messege will be a JSON with "state" : Online/Offline and "name" with DNSName of the device, like this:
{
  "state": "Offline",
  "name": "IAMTEDEVICE"
}


Credits
-------

Original script from [Simon Christmann](https://github.com/dersimn) [owrtwifi2mqtt](https://github.com/dersimn/owrtwifi2mqtt/tree/master)
