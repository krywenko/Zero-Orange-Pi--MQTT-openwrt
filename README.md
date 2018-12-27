# Zero-Orange-Pi--MQTT-openwrt
Operwrt based MQTT monitor- based on influx chronograf and  kapacitor 

to conserve space i left out  influx software
 just ssh in 169.254.10.250 ( or use the dhcp assigned address from your router)
 ssh root@169.254.10.250 ( blank password)
 and install influxdb ( using 1.6.2  as they change something in the new version that made it incompatable for some reason)
 -- i forgot install ca-bundle ca-certificates on image please install first then run 
 
 wget https://dl.influxdata.com/influxdb/releases/influxdb-1.6.2_linux_armhf.tar.gz

tar xvfz influxdb-1.6.2_linux_armhf.tar.gz

wget https://dl.influxdata.com/chronograf/releases/chronograf-1.7.5_linux_armhf.tar.gz

tar xvfz chronograf-1.7.5_linux_armhf.tar.gz

wget https://dl.influxdata.com/kapacitor/releases/kapacitor-1.5.2_linux_armhf.tar.gz

tar xvfz kapacitor-1.5.2_linux_armhf.tar.gz

to enable data collection - goto - system > startup > start local and un cmments these lines 

/root/./influxdb-1.6.2-1/usr/bin/influxd  >nul 2>&1 & echo "started" & /root/./chronograf-1.7.5-1/usr/bin/chronograf   >nul 2>&1 & echo "started"

/runfiles/./start.sh  >nul 2>&1 & echo "started" & /root/./starttrans >nul 2>&1 & echo "started" 

/root/./kapacitor-1.5.2-1/usr/bin/kapacitord  >nul 2>&1 & echo "end"

 the format for sending MQTT is as follows
 
 mosquitto_pub -t 'incoming/OpenWrt/mqtt-Energy/power-grid' -m 'N:21.5'

mosquitto_pub -t 'incoming/OpenWrt/mqtt-Temp/temperature-greenhouse' -m 'N:21.5'

mosquitto_pub -t 'incoming/OpenWrt/mqtt-Humidity/humidity-greenhouse' -m 'N:21.5'

mosquitto_pub -t 'incoming/OpenWrt/mqtt-Flow/flow-heatpump' -m 'N:21.5'

mosquitto_pub -t 'incoming/OpenWrt/mqtt-Pressure/pressure-heatpump' -m 'N:21.5'`

 you  can change the "hostname" OpenWrt  to what ever you like  you can use it to develope  sub groups
but if if you do not use at least one that matches the host name of the device - it will no display rrd graphs with in 
the openwrt statistics but they will be captured and sent to influxdb 

mosquitto_pub -t 'incoming/Garage/mqtt-Pressure/pressure-heatpump' -m 'N:21.5'`

mqtt-Energy, mqtt-Temp, mqtt-Flow and mqtt-Pressure  are the the current fixed catagories for displaying in openwrt.
you will need to use one of these unless you add in your own catagory- the only prequisite is that it starts with mqtt-XXXXX

power, temperature, humidity, flow, and  pressure  is the typedb used by collectd you will need to use any of the assigned 
typedb listed in collectd.
grid, greenhouse and heatpump are descriptions you can use whatever you like here..


it  creates a data base based on the MAC of the orange pi  in the influxdb -  then the  value is  stored as Mac-"hostname"- type -description

you can use one orange pi zero as the influxdb server  and other as one as remote nodes  just edit  Sendinflux.pl to have the ip or domain of the  remote location
 these remotes will be  based on thier MAC so  there will not be any data overlapping - every thing is automactic and in new MQTT ot nodes will automatically add it data to the influxdb..

if you have issue you can let me know here https://community.openenergymonitor.org/t/orange-pi-zero-iot-mqtt-monitor-with-onboard-influxdb-chronograf-and-kapacitor/9572
