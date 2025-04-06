# Lab 10: Cracking Wireless Passwords

___

Wireless networks are ubigutious and understanding some of their weaknesses and how to attack them is a vital skill for modern pen testers.   
In this lab we’ll look at how simple it really is to break WEP and join a WLAN, or sniff traffic on the that network.

in the last section of these notes you’ll see a flowchart highlighting each of the steps needed along the way. For this lab we’ll be using the our the aircrack suite of tools on our Kali
Linux distribution, although many other alternatives wireless cracking tools also exists. 

One of the issues you might have, is whether or not your wireless card is supported, if is not there isn’t much you can do.


Scenario:
We’ll be attempting to access a wireless network secured using WEP, for which
we don’t have the key.
1. So we need to first set up a basic wireless network and connect to it from
at least one machine to ensure that everything is working correctly.
2. Our next step is to boot up our backtrack distribution, and enable our
wireless card (Backtrack disables this by default).
3. Next you will need to put the card into monitor mode on the desired channel,
using airmon.sh
airmon.sh is a handy utility for checking interface availability and setting
cards to monitor mode on specific channels. It is included as part of the
aircrack package. simply typing airmon.sh displays the available wireless
interface, chipset and driver:
## In this lab we will: 

In this lab we will:
1. Putting your wireless card in monitor mode
2. Scanning and capturing local wireless traffic
3. Cracking WEP Networks
4. Cracking WPA/WPA2 Networks
5. Extra Tricks

___


### 7.1 Using Crunch to generate custom wordlists

Many times in penetration testing engagements you will discover authentication forms that you will need to bypass in order to gain access to an application or to a remote system. Having a good wordlists always helps, but as a penetration tester you must be able to create your own custom wordlists depending on the situation. There are a variety of tools that can assist with this, but here we'll focus on Crunch.

#### 7.1.1 Create a Sample Wordlist

We can create a simple wordlist by executing the following command  
```crunch 5 5 admin -o pentestlab.txt``` 

This will instruct crunch to create a wordlist that will have minimum length of characters 5,maximum length of characters 5 with the characters of admin and it will save it on a .txt file called pentestlab.  Of course instead of just letters we can create a wordlist that will include only numbers with the command:  
```crunch 5 5 12345 -o numbers.txt```

The same method applies and if we want to create a wordlist mixed with letters and numbers.  
```crunch 5 5 pentestlab123 -o numbersletters.txt```




Its main use however, is in putting interfaces into monitor mode. airmon.sh
start wlan0 10 puts the wireless interface into monitor mode on channel 10.
4. We now have to find the network we want to attack. This is normally visible to
us, and we can use a program such as airodump to view the details.
airodump is an 802.11 packet capture program that is designed to "capture
as much encrypted traffic as possible...each WEP data packet has an
associated 3-byte Initialization Vector (IV): after a sufficient number of data
packets have been collected, run aircrack on the resulting capture file.
aircrack will then perform a set of statistical attacks developed by a talented
hacker named KoreK."
airodump usage:
airodump [interface] [output file prefix] [channel no.] [IVs flag]
 The [channel no.] can be set to a single channel (1 thru 14) or set
to 0 to hop between all channels
 The [IVs flag] can be set to 1 to only save the captured IVs
e.g. airodump eth1 testfile1 6 produced the in progress capture below:
Basics to be aware of from the above screen capture are:
 BSSID = MAC address of the access point (but not always!)
 Beacons = Number of captured beacon packets (of no use!)
P a g e | 3
2011
Breaking WEP Secure Communications Labs
 # Data = Number of IVs captured so far (this is the all important figure!)
 MB = Data Rate '48' mixed mode in the above example. A '.' appears
after the figures if the Data Rate is dedicated e.g. '48.'
 WEP = Network is configured as WEP
 Number of IVs required to break WEP depends on the WEP key length
o Approximately 300.000 IVs for 40-bit WEP (AKA 64-bit WEP)
o Approximately 1.000.000 IVs for 104-bit WEP (AKA 128-bit
WEP)
Examples:
airodump wlan0 capture1 10 (Interface=wlan0, filename=capture1,
channel=10)
airodump eth1 testfile 6 1 (Interface=eth1, filename=testfile, channel=6, only
captured IVs saved)
airodump ath0 alpha 0 (Interface=ath0, filename=alpha, channel hopping
mode)
Output Files:
An airodump capture with produce the following output
files .txt, .cap and .gps
The .txt file contains:
 BSSID and MAC addresses
 Time/Date info
 Channel Info
 Data rate
 Encryption method
 No. of beacons captured
 No. of IVs captured
 LAN IP
 ESSID
The .cap file contains the packet capture from your session. This is the file
that is input into aircrack for WEP cracking.
The .gps file contains GPS related info if you have a GPS device enabled
P a g e | 4
2011
Breaking WEP Secure Communications Labs
5. Sometimes however, the name of the network is hidden and we see no value
under the ESSID field.
In these cases we need to capture the association handshake of a new client.
To force this we can use aireplay, which is an 802.11 packet injection
program. We use aireplay to force a disassociation; basically we kick the
client off the network and listen when it tries to reconnect.
aireplay -0 1 -a Base_MAC -c Client_MAC [interface]
The hidden ESSID will now appear in the airodump windows
P a g e | 5
2011
Breaking WEP Secure Communications Labs
6. The next step for us is to try capturing packets and weak IVs. How we do this
depends on whether we want to be passive or active, as a passive attacker
we could sit quietly, waiting to capture enough IV to enable us to try break the
key, or if we wanted to be less patient we could become more active and
force more packets to be sent by injecting arp requests onto the network. The
issue is the network may not have heavy usage which would mean we’d have
to wait to capture enough packets, by injecting extra packets it speeds up the
process. So how do we do this.
This injection procedure is required when there is not enough encrypted traffic
being passed across the WLAN to break the WEP key (approximately
300,000 packets are required for breaking 64-bit WEP and approximately
1,000,000 packets for 128-bit WEP).
This process involves:
 DE authenticating a valid client (to increase the chances of acquiring
an ARP packet, this also allowed us to determine the hidden ESSID.
 Capture and replay of a valid ARP packet to speedup/generate the
collection of encrypted packets.
Open another console and initiate a broadcast deauth:
aireplay -0 10 -a AccessPoint -c Client [interface]
Now to produce encrypted packets by collecting and replaying ARP packets:
aireplay -3 -b AccessPoint -h Client [interface]
We now start listening for ARP requests with the -3 option. The -h option is
mandatory and has to be the MAC address of an associated client:
P a g e | 6
2011
Breaking WEP Secure Communications Labs
7. Ok, so far we’ve detected the SSID of the network we want to attack, we’ve
started looking for arp packets that we can replay to increase the number of
IVs we can capture. The last 2 steps are to basically save these IVs and then
use aircrack to break the key based on these IVs.
To save the IV’s we again use airodump
airodump eth1 testfile 6 1 (Interface=eth1, filename=testfile, channel=6, only
captured IVs saved)
leave this running and then in a separate window we can run aircrack
aircrack filename.cap
this will take several minutes to run but should spit out the WEP key.
UPDATE:
Aircrack-ng (next generation) is the tool of choice these days, the process is
basically the same as I’ve outline above, I’ve summarised all the newer ng
commands below.
1. View networks
airodump-ng --channel 1 wlan0
2. Save packets
airodump-ng --channel 1 wlan0 –write filename
3. Detect and replay arp packets
aireplay-ng --arpreply –e SSID wlan0
4. Disconnect client
aireplay-ng --deauth 0 -e ssid wlan0
5. If we need to use a fake MAC
airelay-ng --arpreplay –e ssid –h 00:00:00:00:00:00:00 wlan0
6. Start aircrack
aircrack-ng filename.cap
P a g e | 7
2011
Breaking WEP Secure Communications Labs
P a g e | 8
2011
