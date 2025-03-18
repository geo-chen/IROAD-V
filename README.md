# IROAD V Series

Product: https://iroad.kr/download/IROAD_V9_English_Manual.pdf

Version: v9

After testing the X series, we found that the V series exhibited similar weaknesses.


## Finding 1: Default credentials for SSID [CWE-1393]
The IROAD v9 dashcam broadcasts a fixed SSID with default credentials that cannot be changed. This allows any nearby attacker to connect to the dashcam’s network without restriction. Once connected, an attacker can sniff on connected devices such as the user’s smartphone. The SSID is also always broadcasted.

## Finding 2: Hardcoded credentials in APK (IROAD <= v5.2.5) to ports 9091 and 9092
The IROAD v9 mobile application (version 5.2.5 and below) contains hardcoded credentials that provide unauthorized access to the dashcam's API endpoints on ports 9091 and 9092:

a) Once IRoad SSID is connected to, the attacker sends a crafted authentication command with "TibetList" and "000000" to list settings of the dashcam at port 9091. 

b) There's a separate set of credentials for port 9092 (stream) that is exposed in plaintext as well, "admin" + "tibet". 

c) For settings, it's "adim" + "000000"

## Finding 3: Bypassing of Device Pairing [CWE-798] for IROAD V Series
The IROAD v9 dashcam uses MAC address verification as the sole mechanism for recognizing paired devices, allowing attackers to bypass authentication. By capturing the MAC address of an already-paired device through ARP scanning or other means, an attacker can spoof the MAC address and connect to the dashcam without going through the pairing process. This enables full access to the device.

## Finding 4: Remotely Dump Video Footage and Live Video Stream
The IROAD v9 dashcam exposes API endpoints on ports 9091 and 9092 that allow remote access to recorded and live video feeds. An attacker who connects to the dashcam’s network can retrieve all stored recordings and convert them from JDR format to MP4. Additionally, port 9092's RTSP stream can be accessed remotely, allowing real-time video feeds to be extracted without the owner's knowledge. This vulnerability results in severe privacy risks, including exposure of location data embedded in recordings.

## Finding 5: Managing Settings to Obtain Sensitive Data and Sabotaging Car Battery
The IROAD v9 dashcam allows unauthorized users to modify critical system settings once connected to its network. Attackers can extract sensitive car and driver information, mute dashcam alerts to prevent detection, disable recording functionality, or even factory reset the device. Additionally, they can disable battery protection, causing the dashcam to drain the car battery when left on overnight. These actions not only compromise privacy but also pose potential physical harm by rendering the dashcam non-functional or causing vehicle battery failure.

![image](https://github.com/user-attachments/assets/0a00b49b-39d3-4163-8e05-9d32b159a34f)


## Finding 6: Public Domain Used for Internal Domain Name
The IROAD V dashcams uses an unregistered public domain name as internal domain, creating a security risk. During analysis, it was found that this domain was not owned by IROAD, allowing an attacker to register it and potentially intercept sensitive device traffic. If the dashcam or related services attempt to resolve this domain over the internet instead of locally, it could lead to data exfiltration or man-in-the-middle attacks. The vendor has been contacted regarding the potential impact of this issue.

![image](https://github.com/user-attachments/assets/43458854-9dab-432e-8505-ff9cb285d169)

