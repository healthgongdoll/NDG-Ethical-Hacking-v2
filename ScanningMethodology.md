# Scanning Methodology 

This study will be advanced tool for scanning networks. Advanced IP Scanner 

## Avanced IP Scanner v2
![image](https://user-images.githubusercontent.com/79100627/191632437-0c4240cf-7cad-4716-bcd2-7a539049e6a4.png)

![image](https://user-images.githubusercontent.com/79100627/191632466-8c031c15-a446-4b5a-9aa7-d2188e98e4f2.png)

1. Menu Bar - This contains dropdown lists that allow you to save, change view options, adjust settings, and get help 
2. Scan - This button starts the scanning process
3. Pause - This button pauses the scanning process 
4. IP - This button scans the subnet of the current computer 
5. C - This button scans the class C subnet 
6. Expand and contract all - These buttons allow you to expand and contract the results of the scan 
7. IP Range - This field lists the IP address range to be scanned 
8. Search - This allows you to search the results 
9. Results - This pane lists the results of the scan in column view 
10. Favorites - THis tab lists the IP addresses that were favorited by the user 

Scan IP range 192.168.0.1 - 254 in the IP Range field
![image](https://user-images.githubusercontent.com/79100627/191632747-38e4c103-30a9-48b0-baa6-72422671766f.png)

![image](https://user-images.githubusercontent.com/79100627/191632998-88d669ac-0c97-4be8-a08e-e16e84b9a580.png)

The WinOS.Ethical.local system seen in the list and in the screen is the hostname of Windows OS that you are currently using. There is some additional information that can be gained from one of the IP address in the list. We can determine that one of the systems is running a service by looking for the arrow beside it. As seen in very last 192.168.254. Click the arrow to expand the entry. The running service will appear below the entry (HTTP, pfSense-Login (nginx). This indicates that the system with IP address 192.168.0.254 is running HTTP service. This means you should be able to invoke a webpage by visiting the IP address with a web browser and attempt to log in using the default password for pfSense or any other password you think could work. 

![image](https://user-images.githubusercontent.com/79100627/191633394-37679b22-d1b6-4cd8-96d9-8f0ae755ecf3.png)

As you can see in the exercise, scanning with Advanced IP scanner is great for getting data about systems in the local network. It can be great starting point when identifying a workstation to target 

## Kali Machine (Nmap)

Before starting we need to ensure that the host machine is on the same network as the target machine. Select the Ethernet network connection from the navigation panel and click Wired connection eth1 to enable Kali Linux to configure with the IP address 192.168.0.2 

![image](https://user-images.githubusercontent.com/79100627/191633935-faade4c4-23cb-4e5d-b1b6-f330f2f5d74c.png)

We use Nmap to scan the network and create a map based on the feedback we get. A typical scan of multiple subnet would be ideal in this case; however, due to time constraints we will assume that all the hosts on the network were already discovered. We will list the target IP addresses and perform the scan. 

Once Nmap starts type ``` sudo nmap -sn -Pn --traceroute 192.168.0.2,20,30,100,254 192.168.9.1,2 192.168.68.12,254``` then press Enter 

![image](https://user-images.githubusercontent.com/79100627/191634262-dcb9f24f-f38b-4e1e-839d-b055797c1b71.png)

It is the subnet of followings: 
- Subnet 192.168.0.0/24 (192.168.0.2,20,30,100,254)
- Subnet 192.168.9.0/24 (192.168.9.1,2)
- Subnet 192.168.68.0/24 (192.168.68.12,254)

The command will list the round trip Time (RTT) and the number of hops it takes to get to each host. This will allow us to determine the structure of the network, such as gateways, separate LANs, etc. Each scan can provide a different round-trip time and latency. Take the scan below, for example. As you can see from the screenshot, the IP address 192.168.0.20, 192.168.0.30, and 192.168.0.100 all have a round-trip time between 0.21 and 0.33 milliseconds, as seen in 
![image](https://user-images.githubusercontent.com/79100627/191634652-def64779-c5e0-444c-baa6-86fff807db7c.png)

The node 192.168.0.254 has the shortest response time, 0.17 milliseconds, as seen in the following 
![image](https://user-images.githubusercontent.com/79100627/191634772-e6603b53-78b3-44c2-b51c-894a43057e0f.png)

which suggest that it is the network's gateway. Similarly, your scan should show results where the gateway has the shortest RTT. We also know from previous exercise that this IP address is a pfSense host. The node 192.168.0.2 only returned with the result 

![image](https://user-images.githubusercontent.com/79100627/191635010-9522c074-5fd7-401b-aa3b-34d37f7e8f24.png)
![image](https://user-images.githubusercontent.com/79100627/191635023-0d501116-371d-46c4-92a7-e73bf014bbdf.png)
![image](https://user-images.githubusercontent.com/79100627/191635054-a8922144-af7e-43b3-9d17-5a3b27d0aa83.png)
![image](https://user-images.githubusercontent.com/79100627/191635075-76cec39f-b457-4a2e-b7a2-67d6f9443aa0.png)

Due to the way that Standard Transport Control Protocol (TCP) is defined, it inherently causes variations in RTT. The RTT is the amount of time it takes for a signal to be sent plus the amount of time it takes for it to be acknowledged having received it. Variations are very common in a live environment, and you should examine each on a case by case basis. 

The next section we will look is that the result for the network 192.168.9.0/24. As you can see in item 2, The IP address 192.168.9.2 responded with host is up, which we know is our kali linux host. Since we know that the RoundTrip Time can vary, we will use the example in the screenshot below. The round trip time for the IP address 192.168.9.1 as seen in item 1, is 0.15 milliseconds. This indicative that that node is a gateway as well. We can do further checks to see if it is the same pfSense host being used as a router 
![image](https://user-images.githubusercontent.com/79100627/191635698-e39a4936-472b-4a15-823a-33bd5a7d6693.png)

Kali Host has two network interfaces 
- 192.168.9.2
- 192.168.0.2

To do this, open a new Terminal window and type ```sudo namp -sV -script=banner 192.168.9.1``` and enter 

Look at the results to see what we can learn about this host. The command shows the open ports and the services that are running on the host. It is also supposed to get the operating system but is not always successful at this. If you noticed 
![image](https://user-images.githubusercontent.com/79100627/191635982-ab0b8a1d-2f95-4559-9e32-ef212c732ec6.png)

open HTTP (80) and DNS (53) ports and their versions Nginx and ISC BIND 9.11, respectively. Nginx is a webserver like Apache and is known to be used with pfSense. ISC BIND is an implementation of the Domain Name System (DNS). It performs both main DNS server roles and acts as an authoritative name server for domains. This is enough evidence to determine that it is the same pfSense host that we identified before. 
![image](https://user-images.githubusercontent.com/79100627/191636167-1c5bf0f6-bd36-4bdb-8e3c-21491346fde7.png)

By looking at the Diagram, we can determine that the results of the scans for the network 192.168.9.0/24 coincides with the network diagram for this lab
![image](https://user-images.githubusercontent.com/79100627/191636271-e68e45da-1441-4898-8a3b-4402d6ec3f36.png)

Switch back to the first terminal window to continue reviewing the results of the traceroute scan. The last part of the scan lists the results for the network 192.168.0/24. 

![image](https://user-images.githubusercontent.com/79100627/191636394-382a0174-3279-45a8-95e7-34390fc0395b.png)

As you can see in the picture, the scan shows that there are 2 hops to get to the host at IP address 192.168.68.12. This is normal since we know that the Kali Linux host does not have an interface directly connected to that network. So it must go through a gateway to get there. The next host in 192.168.68.254 
![image](https://user-images.githubusercontent.com/79100627/191636527-224e21da-0397-4c12-8977-d18f75beb91b.png)

It is 1 hop away. This suggest that this is a gateway node. We can confirm this by taking a closer look at the round trip time value. Remember that the round-trip time (RTT) can vary; we will use the example in screenshot. As you can see the response time for 192.168.68.12 is 29 milliseconds and while the time for 192.168.68.254 is 0.15 milliseconds. 

Now switch back to the second terminal window type ```sudo nmap -sV -script=banner 192.168.254``` and press Enter 

The option ```--script=banner``` erfers to the technique used to gain information about a computer system on a network and the services running on its open ports. It is referred to as Banner grabbing```
![image](https://user-images.githubusercontent.com/79100627/191636926-2b5fe82f-b48b-4980-ba99-75111ee645f0.png)

going back to the diagram, we can determine that results of the scan s for the network 192.168.68.0/24 coincides with the network diagram for this lab, as seen in the diagram below. 
![image](https://user-images.githubusercontent.com/79100627/191637103-5c3a9efb-c5f3-4ee2-bcb8-9fb2f331d779.png)

## Angry IP Scanner 

![image](https://user-images.githubusercontent.com/79100627/191638053-933b5af2-e8c2-41b0-853c-877f2088b197.png)

1. Menu Bar - This contains dropdown lists that allow you to save, change view options, adjust settings and get help. 
2. IP Range - This field lists the IP address range to be scanned 
3. IP Feeder menu - This dropdown menu lists the different IP sources. You can type the range or wtich to a text file with IP address in it or choose a random IP.
4. Preferences - This button contains additional settings
5. Hostname - This field contains the target hostname 
6. IP Source - This button allows you to choose IP addresses from the network adapters in the computer
7. Netmasdk - This dropdown menu allows you to choose the subnet mask for the intended target range 
8. Start - This button starts the scanning process
9. Fetchers - This button provides options to recover different types of data from the scanned hosts.
10. List - This pane lists the results of the scan in column view 

Scan address 192.168.0.0 and 192.168.0.255 in the second IP Range field. Once you are done you can click the Fetchers button in 3 
![image](https://user-images.githubusercontent.com/79100627/191638531-2f5ea904-fc15-4f6e-8ccb-f4c48b166173.png)

The Fetchers window will open. The items listed in the left pane, seen in item 1, are the default and currently selected fetchers. You can use the buttons in item 2 to select, remove, reorder and configure the different fetchers. The pane on the right seen in item 3, contains the available fetchers. The results for each fetcher will appear as additional columns in the main GUI results pane

![image](https://user-images.githubusercontent.com/79100627/191638836-c767dae3-6cc5-4e0a-8593-5a855fa4c39c.png)

The last thing we will do is select the subnet mask. To do this, click the dropdown arrow Netmask as seen in the item 1 below. Select /24 from the dropdown 
![image](https://user-images.githubusercontent.com/79100627/191639001-9886416f-6a77-46e1-887b-0768c231d1d3.png)

Let's look at the result. We will look at 1 of the live systems. As you can see, the live hosts appear as a blue circle in the IP column. Let us scroll down using the arrow or scroll bar, seen in item 1, until you get to the IP address 192.168.0.30 
![image](https://user-images.githubusercontent.com/79100627/191641554-a3c955e4-42fb-4669-af91-2f4144688206.png)

![image](https://user-images.githubusercontent.com/79100627/191641769-96dbfcc1-89a6-4b43-aaa4-9d195e079abb.png)

The IP address details window will appear and list all the information that the fetchers collected from the system. The important thing to note in this window is the response time of the ping. This one is 0ms and can give you an idea of the workstation's location on the network. The hostname field provides the name for the system, and the Ports field indicates the open ports, The MAC address and Vendor fields provide the physical address of the network card and the possible manufacturer. Finally, the Filtered Ports field, also seen in item 3, will provide the list of ports that are monitored by a firewall or router. 

## TCP and UDP packet Crafting using HPING3 

Type ```hping3 -c 3 <IP address of the target machine>``` and press enter 

![image](https://user-images.githubusercontent.com/79100627/191642407-f63c6a4a-2bde-461f-9cdc-1579afe87c9b.png)

-c count means stop after sending ( and receiving) count response packets. so -c means that we will only send 3 packets to the target machine

now let's scan the target machine ```hping3 -q --scan 1-3000 -s 192.168.0.20 | grep -v 'Not res'```

![image](https://user-images.githubusercontent.com/79100627/191642731-8bb2c34a-f01c-43f3-ba02-708268cb1c6d.png)

--scan parameter defines the port range to scan and -S represents SYN flag

Now let's perform UDP packet crafting. Packet crafting is a technique that allows the Pen Tester to probe firewall rule sets and find entry points into a target system or network. This is done by manually generating packets to test the network devices instead of using exisiting network traffic 

```hping3 192.168.0.20 --udp --rand-source --data 500``` 

![image](https://user-images.githubusercontent.com/79100627/191643025-d06ea420-25d3-425c-ae50-be3a66ff0646.png)

![image](https://user-images.githubusercontent.com/79100627/191643256-4b8713b8-0c93-4d3d-a2a6-82137f504e52.png)

Next we will send TCP SYN Request to the target machine. To start type
```hping -S 192.168.0.20 -p 80 -c 5 ``` and press Enter 

![image](https://user-images.githubusercontent.com/79100627/191643510-fc77da2c-851d-428d-982a-4ecbc19c8241.png)

-s will perform a TCP SYN request on the target machine -p will pass the traffic through which port is assigned and -c is the count of the packet sent 

![image](https://user-images.githubusercontent.com/79100627/191643630-9d069acd-a94b-40b7-ada5-816e364e3186.png)

## Vulnerability Scanning 

we will now perform vulnerability scan to see what kind of vulenrabilities we can identify on the hosts on this network. 

```sudo nmap -p80,53,22 192.168.0.0/24 192.168.68.0/24 -oG Desktop/list.txt```
![image](https://user-images.githubusercontent.com/79100627/191643913-72dcc26e-7cf8-4346-97f7-63049254c601.png)

Scan is completed and let's see 

![image](https://user-images.githubusercontent.com/79100627/191644010-a6a692b9-1b76-4e21-9c2a-aa876492d146.png)

Enter command ```cat Desktop/list.txt | awk '/Up$/{print $2}' | cat >>Desktop/targets.txt``` This will parse only the IP addresses of the hosts that are up and save the results to a file on the Desktop

![image](https://user-images.githubusercontent.com/79100627/191644292-a6e981cb-3b3a-4d16-9bfa-604edf536774.png)

Next we will use Nikto to scan the hosts in our targets.txt file. To do this type ```nikto -h Desktop/targets.txt``` this scan will take some time to complete 

![image](https://user-images.githubusercontent.com/79100627/191644577-10e83c90-4b69-4f20-87fa-f478062e2a33.png)
