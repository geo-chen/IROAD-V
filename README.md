# IROAD Q Series

Product: https://iroad.kr/download/IROAD_Q9_EN_Manual.pdf / [IROAD_Q9_EN_Manual.pdf](https://github.com/user-attachments/files/19409711/IROAD_Q9_EN_Manual.pdf)


Version: Q9

After testing the X series, we found that the Q series exhibited similar weaknesses.


## Finding 1 - CVE-2025-30106: Default credentials for SSID [CWE-1393]
The IROAD Q9 dashcam broadcasts a fixed SSID with default credentials that cannot be changed. This allows any nearby attacker to connect to the dashcam’s network without restriction. Once connected, an attacker can sniff on connected devices such as the user’s smartphone. The SSID is also always broadcasted.

## Finding 2 - CVE-2025-30109: Hardcoded credentials in APK (IROAD <= v5.2.5) to ports 9091 and 9092
The IROAD Q9 mobile application (version 5.2.5 and below) contains hardcoded credentials that provide unauthorized access to the dashcam's API endpoints on ports 9091 and 9092:

a) Once IRoad SSID is connected to, the attacker sends a crafted authentication command with "TibetList" and "000000" to list settings of the dashcam at port 9091. 

b) There's a separate set of credentials for port 9092 (stream) that is exposed in plaintext as well, "admin" + "tibet". 

c) For settings, it's "adim" + "000000"

## Finding 3 - CVE-2025-30110: Bypassing of Device Pairing [CWE-798] for IROAD Q Series
The IROAD Q9 dashcam uses MAC address verification as the sole mechanism for recognizing paired devices, allowing attackers to bypass authentication. By capturing the MAC address of an already-paired device through ARP scanning or other means, an attacker can spoof the MAC address and connect to the dashcam without going through the pairing process. This enables full access to the device.

## Finding 4 - CVE-2025-30111: Remotely Dump Video Footage and Live Video Stream
The IROAD Q9 dashcam exposes API endpoints on ports 9091 and 9092 that allow remote access to recorded and live video feeds. An attacker who connects to the dashcam’s network can retrieve all stored recordings and convert them from JDR format to MP4. Additionally, port 9092's RTSP stream can be accessed remotely, allowing real-time video feeds to be extracted without the owner's knowledge. This vulnerability results in severe privacy risks, including exposure of location data embedded in recordings.

## Finding 5 - CVE-2025-30107: Managing Settings to Obtain Sensitive Data and Sabotaging Car Battery
The IROAD Q9 dashcam allows unauthorized users to modify critical system settings once connected to its network. Attackers can extract sensitive car and driver information, mute dashcam alerts to prevent detection, disable recording functionality, or even factory reset the device. Additionally, they can disable battery protection, causing the dashcam to drain the car battery when left on overnight. These actions not only compromise privacy but also pose potential physical harm by rendering the dashcam non-functional or causing vehicle battery failure.

![image](https://github.com/user-attachments/assets/0a00b49b-39d3-4163-8e05-9d32b159a34f)


## Finding 6 - CVE-2025-30132: Public Domain Used for Internal Domain Name
The IROAD Q dashcams uses an unregistered public domain name as internal domain, creating a security risk. During analysis, it was found that this domain was not owned by IROAD, allowing an attacker to register it and potentially intercept sensitive device traffic. If the dashcam or related services attempt to resolve this domain over the internet instead of locally, it could lead to data exfiltration or man-in-the-middle attacks. The vendor has been contacted regarding the potential impact of this issue.

![image](https://github.com/user-attachments/assets/43458854-9dab-432e-8505-ff9cb285d169)


## Finding 7 - CVE-2025-30108: Exposed FTP Administrator Credentials
**Description**: IROAD APK 5.2.5 contains hardcoded, plaintext credentials that provide admin access to a publicly
accessible FTP server, dvrdns.net, exposing non-public firmware, Google Map API keys, and other potentially-sensitive information.

**Vulnerability Type**: Incorrect Access Control

**Vendor of Product**: IROAD

**Affected Product Code Base**: IROAD APK v5.2.5

**Affected Component**: Exposed FTP Administrator Credentials

**Attack Type**: Remote

**Impact Code execution**: False

**Impact Information Disclosure**: True

**Attack Vectors**: An attacker can log in as "administrator" onto a FTP server hosting dashcam firmware.

**Has vendor confirmed or acknowledged the vulnerability?**: No

Truncated screenshot:

<img width="294" alt="image" src="https://github.com/user-attachments/assets/d7db90c9-0ab3-40a1-9dff-7195475bf5bc" />
<br/>

<img width="390" alt="image" src="https://github.com/user-attachments/assets/d2805b83-711b-438a-9c52-2d88bdde2c6b" />
<br/>

<img width="311" alt="image" src="https://github.com/user-attachments/assets/bf7f6fc0-a3db-4c73-858b-3f1ee51e412b" />

## Finding 8 - MFA Spam to Induce Device-Pairing Fatigue
**Description**: The IROAD Q Series dashcams, which have fixed default wifi passwords, are vulnerable to device-pairing fatigue attacks because of a lack of rate limiting on the device-pairing process.

An attacker connected to the network can flood the dashcam with pairing requests, repeatedly trigging device-pairing voice messages and inducing MFA fatigue. This can pressure users into pressing the "Wi-Fi" button, granting the attacker full access for unauthorized control or data extraction. 

In the POC screenshot below, the device-pairing request was replayed 5 times over a short window, with each replay triggering 3 voice messages broadcasting "Press the WiFi button to register the smartphone".
This is then replayed indefinitely until the owner presses the wifi button.

![image](https://github.com/user-attachments/assets/dc8144ed-0608-48c0-a0ae-a1bbdfad8393)


**Vulnerability Type**: Other - Lack of rate controls leading to MFA fatigue

**Vendor of Product**: IROAD

Affected Product Code Base: Q9

Affected Component: Device-pairing mechanism

Attack Type: Remote

Impact Code execution: True

Impact Information Disclosure: True

Attack Vectors: An attacker can spam device-pairing voice messages at a high frequency, without encountering any rate limits, until MFA fatigue sets in and dashcam owner presses the wifi pairing button to give the attacker full dashcam access.

Has vendor confirmed or acknowledged the vulnerability?: No

## Disclosure Timeline

11 Feb 2025 - Disclosed to IROAD

15 Feb 2025 - Follow-up email sent to IROAD

1 Mar 2025 - Final follow-up email sent

14 Mar 2025 - Public disclosure via CVEs


