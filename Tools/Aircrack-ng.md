
<font style="color:#981f3a">Airmon-ng</font>: to enable and disable monitor mode on various wireless interfaces.
	`# airmon-ng check kill`: will stop processes that interfere with the airmon-ng command.
	`# airmon-ng start wlan0`: will start monitor mode on the specified interface.
	`# airmon-ng stop wlan0mon`: will stop monitor mode on the specified interface.

<font style="color:#981f3a">Airodump-ng</font>: sniffing with monitor mode interface - used to capture handshakes.
	(I.E. With a connected GPS receiver, it can work out location data that can be imported on a DB to provide graph to pause the dumping of the data.
	![[Pasted image 20221202180004.png]]
	<font style="color:#981f3a">AP's list (on the top)</font>:
	`BSSID`: mac address
	`PWR`: signal level (it increases when we get closer)
	`RXQ`: Receive quality (% of successfully received framed over the last 10 seconds)
	`Beacons`: beacons sent by the AP (frames containing network information needed by a station before transmitting an actual dataframe.)
	`Data`: captured data packeges
	`#/s`: number of packet per seconds
	`CH`: channel number
	`MB`: Maximum speed supported
	`ENC`: encryption alg
	`AUTHL`: authentication protocol
	`ESSID`: wireless network name
	<font style="color:#981f3a">Device's list (on the bottom)</font>:
	`Rate`: last used rate between client and AP
	`Lost`: lost frames originated from the client
	`Frames`: dataframe sent by the client
	`Notes`: shows if a particular client has done the handshake
	`Probes`: ESSID probe (wireless networks airodump-ng is trying to connect to)

<font style="color:#981f3a">Aireply-ng</font>: to generate traffic.
	Injection test: # aireplay-ng -9 wlan0mon

<font style="color:#981f3a">Aircrack-ng</font>: it can crack WPA & WPA2 networks that use PMK ID. It works locally on captured packets.

<font style="color:#981f3a">Airdecap-ng</font>: we can use it to decrypt WEP, WPA PSK or WPA2 PSK captured packets.
	`# airdecap-ng -b <MACADDRESS> file.cap`: to check information about the data packages linked to the passed bssid. It creates another file with only the interesting packages.

<font style="color:#981f3a">Airgraph-ng</font>: creates 2 types of graph: `Client Probe` graph and `Client to AP relationship` graph.
	`# airgraph-ng -i dump-01.csv -o capr.png -g CAPR`: to output a graph (-g to choose the type of graph)

<font style="color:#981f3a">Airolib-ng</font>: store and manage essid and password list.