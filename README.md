# rpi-snmp-ups-synology-nas
Using Raspberry Pi with USB connected APC UPS as a "SNMP UPS" server for Synology NAS.

The following assumes you have apcupsd installed, configured and running.

# Install snmpd
```
sudo apt-get update; sudo apt-get install snmp snmpd
```
# Config snmpd

Edit /etc/snmp/snmpd.conf to include the following:
```
rocommunity public
agentaddress 192.168.1.2  # Use IP address of your RPi
pass .1.3.6.1.4.1.318.1.1.1 /bin/sh /etc/snmp/apcupsd.sh
```
Download apcupsd.sh, copy it to /etc/snmp/, and make it executable.
```
sudo chmod +x /etc/snmp/apcupsd.sh
```
# Restart snmpd
```
sudo service snmpd restart
```
# Test snmpd on RPi
```
snmpwalk -v 2c -c public 192.168.1.2 .1.3.6.1.4.1.318.1.1.1  # Change IP address to match your device.
```
# Configure Synology NAS
Change "SNMP UPS IP address" to reflect your device.

![Synology NAS configuration](/synology-nas-config.png)

# That should do it!
Full credit goes to the person who posted [this shell script & write-up](http://zloy.pclovers.ru/2020/11/25/usb-ups-snmp/).
