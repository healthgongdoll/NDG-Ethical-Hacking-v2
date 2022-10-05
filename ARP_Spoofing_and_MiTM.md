# ARP Spoofing and MiTM

This lab you will be conducting ethical hacking practices using various tools 

1. ARP Spoofing with Ettercap 
2. Capturing web username and passwords 
3. Manipulating HTTP images 

## Topology 

![image](https://user-images.githubusercontent.com/79100627/193972047-31c848c2-f16d-4893-9477-093c500a27dd.png)


## ARP Spoofing with ettercap 

Open Terminal and confirm your MAC and IP address on the eth1 by typing the following command 

```ip addr```

![image](https://user-images.githubusercontent.com/79100627/193972515-defb5d42-63bf-4344-9e50-7aa526f532bc.png)

Review eth1 and notice MAC add 00:50:56:99:d5:96 and IP adr. 192.168.0.2/24

![image](https://user-images.githubusercontent.com/79100627/193972733-486f599f-f263-4f64-bced-551f93ca1d32.png)

Now login into OpenSUSE and execute ```sudo arp -a```. observe 

![image](https://user-images.githubusercontent.com/79100627/193973354-7f131e4a-fdd2-4998-8e7a-04a8b45f2e60.png)

ARP stands for Address Resolution Protocol, which is used to find the address of a network neighbor for a given IPv4 address.

-a Displays the entries of the specified hosts. If the hostname parameter is not used, all entries display. Hostnames are displayed using alternate BSD-style output format (with no fixed columns).

## Ettercap

```ettercap -G```

![image](https://user-images.githubusercontent.com/79100627/193973572-0bae8b27-56a5-4291-98d4-19326bf8d2f5.png)

Change the primary interface with eth1

CLick checkmark in the upper right to accpet changes

Ettercap is now sniffing traffic. However, in order to mount the ARP MitM attack, we need to select a target that we want to spoof. To scan for hosts on the same network click the magnifying glass in the upperleft 

![image](https://user-images.githubusercontent.com/79100627/193973788-0f0be20b-0aa5-4ae5-a503-e5edae7cf64f.png)

![image](https://user-images.githubusercontent.com/79100627/193973844-4788cb05-314d-46cf-9194-12e1699c4d96.png)

To view the host list, click the icon to the right of the magnifying glass. 

![image](https://user-images.githubusercontent.com/79100627/193973932-4198b05a-fd3f-405f-9659-911e8b7bf04d.png)

You want to target the OpenSUSE machine (i.e., assoicate OpenSUSE's IP with Kali's MAC address), so click on 192.168.0.30. Then add to target 1

You also want to target only the pfsense firewall. If you do not select the firewall, ettercap will end up poisoning all hosts in the list. Click on 192.168.0.254 and click on Add to Target 2

![image](https://user-images.githubusercontent.com/79100627/193974651-4ab3fa54-1f7b-43b1-93e6-7c3b2dbacb17.png)

You will now able to sniff traffic but have not acutally poisoned ARP. To start ARP poisoning, clikc on the MiTM icon at the top right

![image](https://user-images.githubusercontent.com/79100627/193974728-206ba967-3ca6-4558-8e3f-9de9798290b4.png)

Select ARP poisoning 

![image](https://user-images.githubusercontent.com/79100627/193974791-6d714e0a-ee9b-4e35-924b-af2e8f11c063.png)

Leave default setting. At this point, you have poisoned the ARP table. Traffic from OpenSUSE is being directed to the Kali machine

![image](https://user-images.githubusercontent.com/79100627/193974960-06bc394f-8e0b-4153-b31e-040073b35e17.png)

Review the output at the bottom of the screen. You can confirm the two victims you selected 

![image](https://user-images.githubusercontent.com/79100627/193975426-d1e01ce6-e47b-4e44-8c9f-84f111edbe25.png)

if you look at OpenSUSE then you can see two IP addresses and MAC address. One is Kali and One is OpenSUSE

## Capturing Web Username and Passwords

1. By default, Ettercap will look for people logging into HTTP websites and record username/passwords. To test this, in OpenSUSE, open the Firefox web browser.
2. From this browser, you will be logging (remotely) into pFsense web interface - the way a system administrator in charge of the gateway router would do. To do that, in Firefox address bar, type the following press enter 

```http://192.168.254```

![image](https://user-images.githubusercontent.com/79100627/193977881-914188b1-ee20-40ba-b02e-a0b52eaa30d8.png)

If you now go to the Kali Machine

![image](https://user-images.githubusercontent.com/79100627/193977969-0ae7cb44-1c7a-4a1f-a3a8-c7f0bdf6a0a7.png)

You can see the password and username that OpenSUSE used 

Notice that it captured the username and password. In this current configuration, this will only work for unencrypted HTTP websites. To capture HTTPS traffic requires more configuration and is beyond the scope of this lab


## Manipulating HTTP Images 

Open OpenSUSE and type ```192.168.68.12```

![image](https://user-images.githubusercontent.com/79100627/193978717-aadf1b9b-0376-4a04-868b-7a00e4e390de.png)

Go back to Kali and filter folder -> inside this folder, you will see files that end with .ef and .filter. The .ef files are the complied versions of the filter. You will use these later. The first one you want to look at is logo.filter, and you can do that by double clicking on it. 

![image](https://user-images.githubusercontent.com/79100627/193978989-c0a95e67-1033-4071-8e64-cc786df05bde.png)

![image](https://user-images.githubusercontent.com/79100627/193979141-5bf09df8-d502-4e5c-8ff3-ac1ca6dd4bda.png)

The first if statement is looking for traffic that has a protocol of TCP and where it is destined for port 80. Here the filter will trash the Accept-Encoding, which allows the filter to modify HTML.

The second if statement is then looking for traffic that has a protocol of TCP and a source of port 80. The filter will look for <img src> tags in the html and replace them with http://192.168.0.2/logo,jpg. This is file we will host locally on the Kali system. You can close the Mousepad window and the File Manager window. 

To start, you need to launch the webserver on kali to host the image. Open a new terminal window by clicking on icon.

To start the Apache webserver, type the following and press Enter. 

```service apache2 start```
```service apache2 status```

![image](https://user-images.githubusercontent.com/79100627/193979581-cbe09c1f-125c-4021-b25e-00e70a28407b.png)

There is no confirmation the service started. To confirm, you can type service apache2 status and note the active line shows it is running. Press q to exit the status window 

Go to Kali and Ettercap and click Filters 

![image](https://user-images.githubusercontent.com/79100627/193979716-51f9c316-3afd-4fe5-b353-bd33dbc6f7b0.png)

