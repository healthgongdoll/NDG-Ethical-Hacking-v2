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





