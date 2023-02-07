Versatile wireless capture tool
- Wifi  <---
- Bluetooth
- nrf
- radio
- ...

#### Config file

/etc/kismet
You can create a `kismet_site.conf` to overwrite configurations.

---

##### Logging
Kismet log files are sqlite3 databases.

`# sqlite3 /var/log/kismet/log_file.kismet`
`.tables`to check tables
`.schema <table_name>` to check column names
`.headers on` to enable column headers

Query example:
`select type, devmac from devices;`
or as oneliner:
`# sqlite3 /var/log/kismet/log_file.kismet "select type,devmac from devices;"`

Use logfile as data source (also ,pcap and .pcam-ng)


---

### Commands

Start kismet
`# kismet -c wlan0` (`--no-ncurses` to get all the output on new lines in the console)
`# kismet --daemonize` to run kismet as daemon and access it through web interface (webapp on port 2501)

#### Remote Capture
If Kismet is installed and running, it will listen on port _3501_.

`# ssh user@machine -L 8000:localhost:3501`
`# kismet_cap_linux_wifi --connect 127.0.0.1:8000 --source wlan0` <-- should send the data to our kali host. This way, the kismet instance on kali remote captures wifi data and sends it to our kali host throught the ssh tunnel.



