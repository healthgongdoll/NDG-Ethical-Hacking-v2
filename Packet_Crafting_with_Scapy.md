# Packet Crafting with Scapy 

![image](https://user-images.githubusercontent.com/79100627/190926439-179fe276-d1d8-4f84-951a-4d184697aa6b.png)

## Introduction 

Building a packet traffic entering or leaving the network. This is how to build packets layer by layer using Scapy, a packet manipulation tool, and implementing the finished packet to perform various network functions 

## Objective 

- Creating Packets with Scapy 
- Sending Crafted Packets 


## Creating Packets with Scapy 

Type command ```Scapy```

![image](https://user-images.githubusercontent.com/79100627/190926809-014df97f-6455-4c37-8e4c-2e880778c8d2.png)

To List out all of the protocols and layer type ```ls ()``` 

![image](https://user-images.githubusercontent.com/79100627/190926834-cb4a120d-c0c5-4e69-ab30-5a1bd13ed630.png)

To command below to list the available commands 

```lsc ()```

![image](https://user-images.githubusercontent.com/79100627/190926904-51629745-ee32-4a6c-b0cb-9608e42cb345.png)

To build a IP packet, you have to know IP protocol.

![image](https://user-images.githubusercontent.com/79100627/190926932-5d79e3cf-42d7-4f92-b9f9-6b70d4870d51.png)

Scapy to set the Time to Live for the IP packet 

```ip=IP(ttl=10)```

![image](https://user-images.githubusercontent.com/79100627/190926967-814d5636-90e2-4163-ac1f-0f5323b66745.png)

To check the previous modification use ```ip``` 

![image](https://user-images.githubusercontent.com/79100627/190926995-c07be459-8830-477a-b33c-8fecb3a3866e.png)

To check the IP destination type ```ip.dst```

![image](https://user-images.githubusercontent.com/79100627/190927030-2ab0810b-453d-48da-a969-19294f4bdd5a.png)

To set the IP destination ```ip.dst="192.168.9.1"

![image](https://user-images.githubusercontent.com/79100627/190927069-d906759f-1648-408a-8a01-7de66e83f46a.png)

To check the IP source address ```ip.src``` and change the IP src = ```ip.src="192.168.9.2```
![image](https://user-images.githubusercontent.com/79100627/190927126-ebba8460-4aba-4943-8dc7-5b79d72111d1.png)

With the TTL, source address, and destination address populated, remove the TTL and set it to the default TTL 
```del(ip.ttl)```

![image](https://user-images.githubusercontent.com/79100627/190933514-a30085da-3f99-44a9-b689-e7588c09c351.png)

Verify TTL with the RFC value of 64 ```ip.ttl```
![image](https://user-images.githubusercontent.com/79100627/190933538-9e7aa287-1fec-4a09-a2da-dc45d02c15b1.png)

Add additional protocol lyaer by adding TCP of top ```ip/TCP()```
![image](https://user-images.githubusercontent.com/79100627/190933578-8943419e-ce4e-4c65-b7d5-1199a8824d6c.png)

Analyze the TCP header from the RFC 793
![image](https://user-images.githubusercontent.com/79100627/190933611-2d8d93a0-b7f0-412c-94f5-b312e28c5ee4.png)

Add some information to the TCP protocol fields ```tcp=TCP(sport=1025, dport=80)```
![image](https://user-images.githubusercontent.com/79100627/190933669-3b2d523f-49ab-4043-9bf6-48c13e145f78.png)

Show the TCP stack ```(tcp/ip).show()```
![image](https://user-images.githubusercontent.com/79100627/190933699-8b3990c6-3d83-4a33-806e-a0fb5b50235b.png)

Add an Ethernet Layer ```Ether()/ip```
![image](https://user-images.githubusercontent.com/79100627/190933722-6b8e18f2-d232-4193-a604-b109950581d8.png)

## Sending Crafted Packets 

Open New Terminal with ```wireshark``` 

Within the Wireshark window, select the eth0 interface from the Capture panel and press CTRL+E to start capturing 
![image](https://user-images.githubusercontent.com/79100627/190933783-6a268f79-32c7-4c7d-9e7d-a7f82ddb275d.png)

Back to the Scapy Terminal and generate a single ICMP packet to be sent to the OWASP machine (```packet=sr1(IP(dst="192.168.68.12")/ICMP()/"XXXXXXXXXXX"```)
![image](https://user-images.githubusercontent.com/79100627/190933868-40306f9f-3f9e-434b-9f6f-7d44a795a5ed.png)

![image](https://user-images.githubusercontent.com/79100627/190933917-e5f07bf1-eb29-4f2a-b549-2a1647c44056.png)

To check the packet you have created ```packet``` 
![image](https://user-images.githubusercontent.com/79100627/190933943-e42dea55-45dd-4662-a0a0-cd8136be16db.png)

simple SYN scan on a single port ```packet=sr1(IP(dst="192.168.68.12")/TCP(dport=80,flags="S"))```
![image](https://user-images.githubusercontent.com/79100627/190934000-bac34c3e-0f05-480c-af00-52a64fef3ff0.png)


![image](https://user-images.githubusercontent.com/79100627/190934018-4372509b-bfe7-4fbf-874c-ef4c585d1b1e.png)
