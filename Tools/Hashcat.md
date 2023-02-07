`# hashcat -I`  to check OpenCL info.
`# hashcat -b`  to benchmark.

Convert capture file to HCCAPx:
`# /usr/lib/hashcat-utils/cap2hccapx.bin wpa-01.cap output.hccapx`

Passphrase Cracking 
`# hashcat -m 2500 output.hccapx <wordlist>`

