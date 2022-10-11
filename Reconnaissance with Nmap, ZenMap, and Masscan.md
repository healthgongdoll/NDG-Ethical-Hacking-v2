# Namp the "Network Mapper"

## Objective

1. Reconnaissance Using Nmap
2. Reconnaissance Using Zenmap 
3. Reconnaissance Using Masscan 

## Topology

![image](https://user-images.githubusercontent.com/79100627/195123374-e0fb9004-40fc-425a-acc9-91e901322a40.png)

## Reconnaissance Using Nmap 

Nmap is a utility for network discovery and security auditing. There are many options 

```man nmap```

![image](https://user-images.githubusercontent.com/79100627/195123903-dd7e7a49-9794-4d6b-becd-0460e4099d15.png)

Nmap has many options, including its own scripting engine. Review the man pages to get familiar with the switches and options. 

In the terminal initiate a general Nmap scan on the OWASP BWA machine, with no options

```nmap 192.168.68.12```

![image](https://user-images.githubusercontent.com/79100627/195124413-d92c9000-44fd-42ef-9093-318fcb46e985.png)

notice that host only has TCP ports open 

Nmap scans scan be nosiy at times with its default port scanning range. Limit Nmap to only scan the most popular ports by initiating the command 
```nmap -F 192.168.68.12```

![image](https://user-images.githubusercontent.com/79100627/195124846-649bd7a2-a137-429e-a0a0-28e89a64db43.png)

Notice that port 5001 is missing from this output. This is a non standard port 

Use Nmap to try to identify versions of Software running on the ports. 

```nmap -A 192.168.68.12```

![image](https://user-images.githubusercontent.com/79100627/195126196-9f3da290-8eb1-4e28-89e7-4567f2cd2c5e.png)

## Nmap Script 

Nmap has a set of scripts installed we can use to test for vulnerabilities 

```nmap -sC 192.168.68.12```

![image](https://user-images.githubusercontent.com/79100627/195126949-62e430e8-c0f9-469b-ae41-efa79a6d2e9c.png)

## Reconnaissance Using Zenmap 

Zenmap is a GUI version of Nmap and allows you to get a better visual representation 

```zenmap```

![image](https://user-images.githubusercontent.com/79100627/195127377-1e47a45e-28c3-4112-b02e-f3129bf73f58.png)


## Reconnaissance Using Masscan 

Masscan is labeled as the "fastest Internet port scanner" capable of scanning the entire internet in under 6 minuite 

```man masscan``` 

Masscan also has mnay nmap-like features, in the terminal window, review these features by typing the following command, press Enter, and review the output

![image](https://user-images.githubusercontent.com/79100627/195130840-87d865a9-bc9d-43d6-916a-b1a352dc6a0b.png)
