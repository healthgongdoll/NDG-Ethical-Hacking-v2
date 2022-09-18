# DNS Footprinting

## General Structure of Lab Environmentation

![image](https://user-images.githubusercontent.com/79100627/190925362-554b8795-eea9-445f-ba3e-71ac5b315421.png)

## Two Common Tools for DNS Footprinting

- nslookup 
- dig 

## Objective 

- Footprinting with nslookup
- Comparing nslookup with dig

## Footprinting using nslookup

```man nslookup``` is the nslookup's manual 

![image](https://user-images.githubusercontent.com/79100627/190925496-4e6e1a02-495f-40da-9955-f78df05d7378.png)

In the terminal type nslookup for interactive mode and press enter. If you type server you can see the default server listed </br>
It is the pSense Firewall

![image](https://user-images.githubusercontent.com/79100627/190925524-babbe9c3-4cb7-46fc-ac62-fb1153c69cb7.png)


To change the default server via ```server 192.168.0.254```

![image](https://user-images.githubusercontent.com/79100627/190925627-6ac2bcc5-c0fc-445c-a743-930f2a4c18c3.png)

In the lab envrioment, there is a domain name called ```mylab.com``` and if we want to see what the root domain is we type 
```mylab.com```

![image](https://user-images.githubusercontent.com/79100627/190925669-54256a95-5d37-42b7-bd1b-9e532518fc83.png)

## DNS record types 

- A- Host Address for IPv4 
- AAAA - Host Address for IPv6 
- CNAME - Canonical Name: Maps an alias to the canonical name. Often used to point multiple systems to one IP without assigning an A record to each host name. The DNS lookup will continue by retrying the lookup with the new name
- MX - Mail Exchanger - Maps a domain name to a list of message transfer agents 
- NS - Nameserver - Delegates a DNZ zone to use the given authroative name servers 
- PTR - Pointer - Points to a canonical name. Unlike CNAME the DNS lookup processs stops, and just the name is returned 
- SOA - Start of Authority - Specifies authroative information about a DNS zone 
- SRV - Service Location - General service location 
- TXT - text - Originally for arbitrary human-readable text in a DNS record, it is now used for several other machine-readable data types. 

## Setting the command 

```set type=ns``` - set the record type 

![image](https://user-images.githubusercontent.com/79100627/190925869-d48ea210-dd82-4783-ab91-d804373491d8.png)

![image](https://user-images.githubusercontent.com/79100627/190925927-1a821d74-d75d-4a10-8f27-fdf36b4f4e5d.png)

```set type=any``` - not on the list is any 

![image](https://user-images.githubusercontent.com/79100627/190925983-077dba91-ae89-4c55-802f-9b2980f3c79f.png)

```set type=axfr``` - in order to see everything (Transfer Record) Usually this is not availabe 
![image](https://user-images.githubusercontent.com/79100627/190926046-82d46b79-f1a7-460b-aa66-b83a821e2f22.png)

OpenSUSE - allow transfer records (Another OS)

![image](https://user-images.githubusercontent.com/79100627/190926126-cd100c69-67ea-4785-95db-47c4af2ae5eb.png)

## Comparing nslookup with dig 

Dig is similar tool. 

```dig @192.168.0.254 mylab.com ns```

![image](https://user-images.githubusercontent.com/79100627/190926280-2af5cd53-b76e-4a43-ac3b-3dedd59c2884.png)

- dig - The program you are using
- @192.168.0.254 - The server to lookup from
- mylab.com - The name you are looking for 
- ns - The type of record
