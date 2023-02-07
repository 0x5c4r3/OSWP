Wpa_supplicant configuration for a WPA or WPA2-PSK:
```
network={
  ssid="home_network"
  scan_ssid=1
  psk="correct battery horse staple"
  key_mgmt=WPA-PSK
}
```

 EAP-PEAP/MSCHAPv2 configuration for RADIUS servers that use the new peaplabel:
```
network={
	ssid="example"
	key_mgmt=WPA-EAP
	eap=PEAP
	identity="user@example.com"
	password="foobar"
	ca_cert="/etc/cert/ca.pem"
	phase1="peaplabel=1"
	phase2="auth=MSCHAPV2"
	priority=10
}
```

For more configs:
https://w1.fi/cgit/hostap/tree/wpa_supplicant/wpa_supplicant.conf

------------
### Steps to follow:
1) To connect to the network:
`# sudo wpa-supplicant -i wlan0 -c wifi-client.conf` (`-B` if run in background)
When successful, it outputs something like  `wlan0: CTRL-EVENT-CONNECTED - Connection to <bssid> completed`

2) To request a DHCP lease on wlan0
`# sudo dhclient wlan0`

------------------

FOR EMERGENCIES might be worth testing:
```
# Catch all example that allows more or less all configuration modes
network={
	ssid="example"
	scan_ssid=1
	key_mgmt=WPA-EAP WPA-PSK IEEE8021X NONE
	pairwise=CCMP TKIP
	group=CCMP TKIP WEP104 WEP40
	psk="very secret passphrase"
	eap=TTLS PEAP TLS
	identity="user@example.com"
	password="foobar"
	ca_cert="/etc/cert/ca.pem"
	client_cert="/etc/cert/user.pem"
	private_key="/etc/cert/user.prv"
	private_key_passwd="password"
	phase1="peaplabel=0"
}
```
