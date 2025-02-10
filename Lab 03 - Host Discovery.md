# Lab 3: Host Discovery

Estimated Time: 100 Mins
___


Before you begin this lab, you should have completed both Lab 01, setting up your Kali linux and Lab 02, introducing “TryHackMe.com”. This lab will cover the hugely important topic of 'Host Discovery'

With host discovery we want to map the target network to get a list of active IP addresses. We want to know what IPs on our target network are actually active. The goal at the end of host discovery, is to have a list of Active IPs

> 💡 While I'll suggest tools at each stage of our PenTest you are always free to use whatever tools you prefer, a vital skill of a pentester is researching new tools and techniques, and building their own preferred tool set.
___


## In this lab we will: 

1. Understand the ping command and it's options.  
2. Learn to use the fping command line tool. 
3. Learn when to use an ARP scan, and what tools to use. 
    - Arping 
    - netdiscover 
4. Learn how to use the de facto port scanning tool Nmap.
5. Nmap Live Host Discovery
6. Learning to use RustScan.  
___


### 3.1 Understanding the PING command. 

One of the easiest methods of performing host discovery by simply performing a ping sweep of all the IP addresses in a particular range. If a host replies, then we would know it was active. In this first lab section we’ll look at the basic ping command, which we’re likely all familiar with, but this time we’ll take a detailed look at the command and what it can do.  

Note: For this lab I’m assuming you are using you Kali VM or similar, but you will likely want to try the commands on windows host also. 

> 💡 It might also be worth running the network analyser wireshark in the background so you can look in detail at what exactly is being sent when you use each command or tool.  

 
Arrange yourself into small breakout groups (2 or 3) and work your way through and discuss each of following questions: 

On your attack machine open a terminal and perform each of the following. 

1. Type ifconfig to check your IP address. What is your IP address on the virtual network? 

2. What do you think would happen if you ping your network broadcast address? (Different on windows and Linux) Try it. Why did you get the results you got?

3. Look at the help for ping. What command would you use to send exactly 100 ping packets to your target machine? 
___


### 3.2 Understanding the Fping command. 

This time we'll be using the fping tool on Kali. 

4. Again look at the help for the fping command. What command would you use to send exactly 100 ping packets to your target machine? 

5. Check the timings for both step 3 and 4, which is quicker? Why do you think there is a difference?

6. Again using fping what command would you use to ping the following list of popular targets? microsoft.com ebay.com citibank.com google.com slashdot.org yahoo.com ?

7. Using your command from step 6 above, which of the above listed targets show as being active? Why do you think some are not? 
___
 

### 3.3 When to use ARP scan.  

The Address resolution protocol (ARP) works on a local network to help map IPs and MAC addresses. The basic protocol involves any host broadcasting the ‘Who has IP 1.2.3.4’ message to all connected devices, if any device has that IP they usually respond, and the host records the MAC address of the host that responded. 

If we sat on a network and sent lots of these requests, we could easily map out a local network and list all the active hosts. Because the ARP broadcasts aren’t forwarded by routers/layers 3 switches etc. this technique only ever works for local network sections that we’re directly connected too. 

Have a look at the man pages for both the arping and netdiscover tools and try to perform a scan of your local network or on your THM connection.  

> 💡 You might need to specify which interface to use if you have more than one interface. 

8. What command did you use to perform both your arping scan and your netdiscover scan?
___


### 3.4 Learning to use Nmap.  

One of the go to tools in the world of security is the versatile Nmap scanning tool. Learning how to use it well is crucial as it is a tool you’ll use over and over again with every pentest. 

Sign up for the Nmap room on THM and complete the introduction and exercises listed in the room. 
___

### 3.5 Nmap Live Host Discovery

Again on TryHackMe,search and find the 'Nmap Live Host Discovery' room and complete the full room to learn how to use Nmap to discover live hosts using ARP scan, ICMP scan, and TCP/UDP ping scan.


### 3.6 Learning to use RustScan.  

A very new modern scanning tool is RustScan. It is extremely fast and is designed to be used with Nmap (not as an Nmap replacement) to massively speed up full network scans.  

Sign up for the RustScan room on THM and complete the introduction and exercises listed in the room. 
___


 

 
