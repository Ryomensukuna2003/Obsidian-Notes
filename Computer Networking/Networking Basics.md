- **Client**: Computer software that requests data from a server.
- **Server**: Computer software that serves data to a client.
- **Protocol**: A set of rules/procedures that define how data is transmitted between devices on a network.
- **Data Packets**: The actual requests sent over the internet, often divided into smaller chunks for transmission.
- **IP Address**: Internet Protocol Address, a unique identifier for a device on the internet.
    - Consists of a **host address** and a **port number**.
    - **Host address**: Identifies the device on the internet.
    - **Port number**: Identifies the specific process or service (e.g., port 5132 for a Vite server).

### **Question: How does a client/server know the address of the other?**

- #### **DNS (Domain Name System)**
	- DNS maps human-readable domain names (e.g., `.com`, `.in`, `.gov`) to their corresponding IP addresses.
	- DNS is usually managed by Internet Service Providers (ISPs) like Jio, Airtel, etc.
	- DNS dynamically updates to reflect new domains or changes to existing ones.


- ### **DNS Working**
	1. **PC Cache**: If the requested domain is in the PC's cache, it is resolved locally to avoid redundant DNS queries.
	2. **ISP DNS Server**: If not in the cache, the query is sent to the ISP's DNS server. If the ISP has the record, it resolves the request.
	3. **TLD Query**: If the ISP doesn't have the record, it forwards the query to a **Top-Level Domain (TLD)** server based on the domain suffix (e.g., `.com`, `.gov`, `.in`).
	4. **Authoritative DNS Server**: The TLD server forwards the request to the **authoritative DNS server** responsible for the specific domain, which resolves the query.

### **OSI Model**

The Open Systems Interconnection (OSI) Model consists of 7 layers that computer systems use to communicate over a network:

1. **Application Layer**
2. **Presentation Layer**
3. **Session Layer**
4. **Transport Layer**
5. **Network Layer**
6. **Data-Link Layer**
7. **Physical Layer**

#### **Application Layer**
- So what Application layer dose is - When we put a GET request over a IP address with Port number it adds all relevant data that might me needed for request to be fulfilled like HTTP Headers, Cookies, Content-type etc.
- Acts as the interface for network services to application software, abstracting underlying networking details.
- Provides services like **file transfer**, **email**, and **web browsing**.

#### **Presentation Layer**
- So lets say you posted a GET request using HTTP. What will happen? Nothing. You'll pass through as nothing happened As Presentation layer work is to encrypt data of Application layer and encryption is supported in HTTPS only.
- Responsible for **data encryption and decryption**, ensuring secure communication.
- Translates data from the Application Layer into an intermediary format.
- Handles **data compression** for optimised transfer.

#### **Session Layer**
- Establishes, manages, and terminates sessions between applications.
- Adds **checkpoints** to the data stream, enabling resumption from the last checkpoint if transmission is interrupted.

#### **Transport Layer**
- So after all this Transport layer will say that this Data stream is too big for transportation nigga and let me break it into pieces and I'll attach port number and sequence number in each broken segment so original data can be retrieved from this.   
- Ensures **error checking** and complete data transfer.
- Divides messages into **segments** and reassembles them at the destination.
- Controls the **flow rate** of data for reliable delivery.

#### **Network Layer**
- Network layer is same as transport layer but instead of Port and sequence it will attach Source and Destination IP in each segment.
- Determines the best possible **logical path** for data to travel from source to destination.
- Introduces **logical addressing** (e.g., IP addresses) for unique identification of devices.

#### **Data-Link Layer**
- Now this Data link layer will say, Man this is too big of a data packets here you brought here, Let me break it into even smaller frames with little bit of error detection and it will add Target and Source **MAC Address** in each frame 
- Now Sending part is done. What will it do in Receiving part. So it when it receives a Data frame it will check for its Destination MAC address and will check if this data frame is for this device or not. if not it will discard it.
- ##### Why Should you never connect to public WiFi?
	- Because people can sniff that data packet and instead of discarding them they will accept your data frames as utility like wireshark dose the same. NOTE - If data is encrypted you don't have to worry about this part.
- For finding the MAC Address Computer broadcasts its MAC address requesting Destination MAC address.

```cardlink
url: https://www.youtube.com/watch?v=cn8Zxh9bPio
title: "ARP Explained - Address Resolution Protocol"
description: "What is ARP?  This is an animated video explaining how ARP (address resolution protocol) works.  ARP resolves an IP address to a MAC address."
host: www.youtube.com
favicon: https://www.youtube.com/s/desktop/c01ea7e3/img/logos/favicon_32x32.png
image: https://i.ytimg.com/vi/cn8Zxh9bPio/maxresdefault.jpg
```

- Handles **traffic control** and **error detection** (but **not correction**).
- Responsible for **framing** and managing access to the physical medium.

#### **Physical Layer**
- Bits are transmitted by voltages in physical mediums (Wires/Cables) but the thing with voltage is that it loses its power so now we will transmit bits using radio waves and then as it was kinda slow so people started transmitting raw bits using light waves through optic fibres 
- Transmits raw **bit streams** over a physical medium (e.g., cables, radio frequencies).
- Establishes, maintains, and deactivates the physical connection between devices.


Soruce -> [Fetching Data#tb22](https://www.youtube.com/watch?v=7IS7gigunyI)