First, check the first 3 groups of digits on the bssid of the AP and google them to see the manufacturer.

You can use John the Ripper, Crunch, RSMangler.

<font style="color:#981f3a">Crunch</font>
	`# crunch 8 9 abc123`
	`# crunch crunch 13 -t Password@,%^`: will create a list of 13 digits passwords like "Password" followed by a lowercase char (@), an uppercase char (,), a number (%) and a symbol (^).
	`# crunch 1 1 -p dog cat bird`: will create a mix of the words you specify (i.e. dogbirdcat, catdogbird...).


<font style="color:#981f3a">RSMangler</font>
	`# rsmangler --file wordlist.txt`: will create a wordlist from the words that are in the wordlist.txt.