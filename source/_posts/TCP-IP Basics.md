# TCP-IP Basics

title: TCP-IP Basics
date: 2015-10-20 18:00:00
tags:
- Network
- TCP/IP

---

# What is TCP/IP?
`TCP/IP` (Transmission Control Protocol/Internet Protocol) is the basic communication language or protocol of the Internet. It can also be used as a communications protocol in a private network (either an intranet or an extranet).

<!--more-->


----------


# TCP/IP addressing
IP Address is Logical Address given to each and every device in the network. It is a Network Layer address (Layer 3). There are two versions of IP: `IPv4` and `IPv6`.

IPv4 was developed in 1980's. It is a 32-bit representation with totally 4.3 billion addresses.

To overcome the IPv4's "small" amount of addresses, people come up with IPv6. IPv6 was developed in 1990's. It is a 128-bit representation with approximately 3.4×10^38 addresses.


Besides IPv6, another solution called [NAT (Network Address Translation)](https://en.wikipedia.org/wiki/Network_address_translation#Translation_of_the_endpoint)  was suggested to extend the IPv4 usage . `NAT` allows 60,000 users to go to a single IP address.


----------


# IPv4 Address Classification
In the original Internet routing scheme developed in the 1970s, sites were assigned addresses from one of three classes: Class A, Class B and Class C. The address classes differ in size and number. Class A addresses are the largest, but there are few of them. Class Cs are the smallest, but they are numerous. Classes D and E are also defined, but not used in normal operation.

![IPv4 Address Classification](http://i.imgur.com/ChgCNZD.png)

[Check out this Wikipedia post for more details :)](https://en.wikipedia.org/wiki/Classful_network)


----------


# Network & Host portions
IP address is divided into [Network & Host Portion](https://en.wikipedia.org/wiki/Classful_network).
- **Host**: a specific device in the network
- **Network**: set of devices

For Class A, the network portion is the first number. For Class B and C, their network portions are the first two numbers and the first three numbers respectively.

| CLASS    |    NETWORK & HOST  | START IP  |
| :------: | :--------:| :--: |
| Class A  | **N**.H.H.H | 0.0.0.0 |
| Class B  | **N**.**N**.H.H  | 128.0.0.0  |
| Class C  | **N**.**N**.**N**.H | 192.0.0.0 |



This addressing scheme is illustrated in the following table frow [Wikipedia](https://en.wikipedia.org/wiki/Classful_network):

![addressing scheme for classful network](http://i.imgur.com/TATMJx0.png)

The number of addresses usable for addressing specific hosts in each network is always 2^N - 2 (where N is the number of rest field bits, and the subtraction of 2 adjusts for the use of the all-bits-zero host portion for network address and the all-bits-one host portion as a broadcast address. Thus, for a Class C address with 8 bits available in the host field, the number of hosts is 254.

If two devices in a LAN want to communicate with each other, they have to be in the same network. For example, we have two devices with IP addresses `192.168.1.1` and `192.168.2.1`. They IP addresses suggest that they belong to CLASS C, whose network portions consume the first three numbers in their IPs. Since the first device's network portion is `192.168.1` and the second one's network portion is `192.168.2`, they belong to different networks. They are not logically same even though they are in the same LAN and physically connected.

----------

# Network Address & Broadcast Address & Subnet-mask
## Network Address & Broadcast Address
For each network, as stated above we cannot use the first IP and neither the last one. The first IP is called **Network Address** and the other one is called **Broadcast Address**. For example, in the network `192.168.1.X`, `192.168.1.0` is the Network Address and `192.168.1.255` is the Broadcast Address.

The Network Address is reserved to identify the network. It is useful during routing.
The Boradcast Address is reversed for broadcasting within the same network.

The remaining addresses are valid IP addresses. Only valid IP addresses can be assigned to hosts/clients.

## Subnet-mask
Subnet Mask differentiates Network portion and Host Portion:
- `1` represent network
- `0` represent hosts

| CLASS    |    NETWORK & HOST  | MASK VALUE  |
| :------: | :--------:| :--: |
| Class A  | **N**.H.H.H | 255.0.0.0 |
| Class B  | **N**.**N**.H.H  | 255.255.0.0  |
| Class C  | **N**.**N**.**N**.H | 255.255.255.0 |

Check out this [post](https://www.iplocation.net/subnet-mask) for more information :)

----------

# Private & Public IP
## Private Address
If we design a LAN, we can consider the private IP.
**Private IP**
- Used within the LAN or within the organization
- Not recognized on Internet
- Given by the admin
- Unique within the network or organization
- Free
- Unregistered IP

## Public Address
If we want to go to Internet, we need a public IP.
**Public IP**
- Used on public network (Internet)
- Recognized on Internet
- Given by the service provider (from IANA)
- Globally unique
- Pay to service provider (or IANA)
- Registered

## Network Address Translation
When we connect to the Internet from a host within a LAN, the router will translate our private IP into a public IP, which is called `Network address translation` or `NAT`. So how to identify whether an IP address is a private IP address or a public IP address?

There are certain addresses in each class of IP address are reserved for Private Network. These addresses are called **private address**.

| CLASS    |  START IP  | END IP |
| :------: | :--------:| :--: |
| Class A  | 10.0.0.0 | 10.255.255.255 |
| Class B  | 172.16.0.0 | 172.31.255.255  |
| Class C  | 192.168.0.0 | 192.168.255.255 |


----------

@(Learning Cards)[Marxico|Full-Stack]