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




