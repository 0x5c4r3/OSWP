
<font style="color:#981f3a">Open Wireless Networks</font> do not envolve any encryption:

1.  The client sends an authentication request to the AP
2.  The AP sends an authentication response of "successful"
3.  The STA sends an association request to the access point
4.  The AP sends an association response if the capability of the clients meets that of the AP

<font style="color:#981f3a">Wired Equivalent Privacy (WEP)</font> only uses a 24-bit initialization vector (IV) as when the WEP standard was being drafted, the key size was limited due to US government export restrictions on cryptographic technologies. Later on, 128-bit WEP (using the same 24-bit IV) was implemented.

RC4 (symmetric cypher - same key is used to encrypt and decrypt) was chosen for wireless encryption due to its semplicity and speed.
RC4 consists of two key elements:
1.  **Key Scheduling Algorithm (KSA)**: Initializes the state table with the IV and WEP key.
2.  **Pseudo-Random Generation Algorithm (PRGA)**: Creates the keystream.

_WEP Authentication_
WEP can make use of two authentication systems: Open or Shared authentication. Open authentication is trivial and commonly used. Shared authentication, on the other hand, is fairly uncommon and clients will often struggle trying open authentication before switching to shared authentication.

_Open Authentication_
In open authentication, a client does not provide any credentials when authenticating to the Access Point. However, once associated, it must possess the correct key to encrypt and decrypt data frames.

_Shared Authentication_
During authentication, a challenge text is sent to the client. The challenge text must be encrypted with the WEP key by the client and sent back to the AP for verification, which allows the client to prove knowledge of the key. Once the encrypted challenge text is received, the AP attempts to decrypt it. If it is successful and matches the clear text version of the challenge text, the client is allowed to proceed to associate to the access point.

<font style="color:#981f3a">Wired Equivalent Privacy (WEP)</font>

...