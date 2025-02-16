# Lab 4: Footprinting & Reconnaissance

Estimated Time: 100 Mins
___

In this lab we’ll look at just a small subset of the vast array of recon tools available. We’ll focus on tools listed for the PenTest+ certification exam and some google dorking. Our Practical exam at the end of term will also have some related questions so it's a topic you should'nt ignore. Let all topics in pentesting, it requires lots of practice, time and testing to find the right tools to suit your own needs. We will quickly look at how to get started with the tools listed and complete some of the related rooms on the TryHackMe platform.
___


## In this lab we will: 

In this lab we will:
1.	Understanding search engines and Google dorking. 
2.	Introduction to shodan.io 
3.	Introduction to Censys.io 
4.	Whois and nslookup
5.	TheHarvester
6.	Recon-ng
7.	Foca
8.	Maltego
9.	OSINT Framework
10. Related THM Rooms
___


### 4.1 Understanding Search engines and Google dorking.

![google dorking THM Room](images/dorking.png)

Search engines can reveal huge amounts of information, and learning to use the more advanced options can help you utilise the full power of some of the search engines. To get started we will looking at the google dorking room on THM. Join the room and watch the video walkthrough of the problems.  

While Google is by fair the most popular, other search engines such as Bing offer similar capabilities so its worth checking multiple search engines and understanding the advanced options available on each of them.

- Complete the Google Dorking Room on THM
___


### 4.2 Introduction to Shodan.io

![Shodan THM Room](images/shodan.png)

Shodan.io is a search engine for the Internet of Things. Ever wondered how you can find publicly accessible CCTV cameras? What about finding out how many Pi-Holes are publicly accessible? Or whether your office coffee machine is on the internet? Shodan.io is the answer!  

For this lab section we are going to complete the shodan.io room on THM. The room with introduce the Shodan website and some of the capabilities of the platform. So join the room now and complete the Shodan questions.  

> 💡 Shodan scans the whole internet and indexes the services run on each IP address. You can purchase full accounts, but they often do great black Friday sales etc. doing lifetime memberships and cheap $5 memberships. So if its something your interested in keep an eye out for deals.

- Complete the shodan.io Room on THM
___


### 4.3 Introduction to Censys.io

![Censys.io website](images/censys.png)

The popularity of shodan has caused a number of other similar sites to pop up in the past few years, Scan.io, Zmap.io and Censys.io. With the Censys site, probably the best known after the main shodan site, and offers some nice features.  

Check out the Censys.io website and some their demos and case studies, and try perform some of the same tasks that you performed during the earlier Shodan exercises, explore the site and try get an understanding of the capabilities on offer. Check out the query syntax help section on the Censys site to get a better understanding of the full power and option available.  

- Explore the Censys.io website 
___


> ❗ For the next few sections, we’ll very briefly introduce a few different tools, you should spend some time learning these tools.


### 4.4 whois and NsLookup

Discovering domain names, public-facing websites, and email addresses is an important part of footprinting an organization during an assessment. Pentesters are able to gather public information about organisations, computer systems, and who they belong to. Querying information about the organisation’s domain is the first step in the process. The WHOIS directory service was developed back in the 1980s to look up domain registration information from registry databases administered by multiple registries and registrars around the world. Each region has it's own registry for IP addresses.  

The American Registry for Internet Numbers (ARIN: https://www.arin.net) is the Regional Internet Registry (RIR) for North America. You can find additional information about the various regional registries on the ARIN website. The whois command, which is a client for the WHOIS directory service, is available in Kali Linux. The figure below is an example command output using the whois command to query example.com.

![whois screenshot](/images/whois.png)

The output can provide useful information that can help identify domain creation date, when it was last updated, associate a company and business location for the domain, DNSSEC information, and in some cases, contact information of the registrar. The nslookup command (shown below) can be used to resolve the name of the domain to an IP address—this is called a forward DNS lookup. A reverse DNS lookup is the opposite—this process resolves the IP address to the domain name. A tool that can aid with discovering domains, subdomains, and email addresses is called theHarvester, which is also included in Kali Linux.  

![nslookup screenshot](/images/nslookup.png)

DNS forward and reverse lookups are common practices executed during information gathering. One command-line tool you may see on the exam is dig. This tool is used to interrogate DNS servers and is sometimes used by administrators to troubleshoot DNS-related problems. This is another tool worth playing around with if you intend to try the actual PenTest+ Exam at some stage.
___


### 4.5 TheHarvester 

The latest version of the tool can be found on the developer’s GitHub page at https://github.com/laramies/theHarvester.  TheHarvester is a Python-based framework, is simple to use, and includes options to allow either passive or active queries to gather target information. Passive sources include various search engines and social media accounts such as
•	Google   www.google.com
•	Dogpile   www.dogpile.com
•	Yahoo   www.yahoo.com
•	Bing   www.bing.com
•	LinkedIn   Google search for user accounts
•	Twitter   Google search for user accounts
•	Shodan   Uses Shodan search engine to discover open ports/banners for hosts

![harvester screenshot](/images/harvester.png)

As with all tools, results from theHarvester may not be totally accurate. During an engagement, it is important to analyzse this information carefully and in some cases vet the information with the customer prior to executing further testing. However, information obtained from theHarvester can prove extremely effective during a black box assessment. For instance, email addresses discovered during the collection process can be used for executing spear phishing attacks. theHarvester is an extremely powerful framework that helps automate the information collection process, using passive and active collection methods.
 
There are a few modules that require API keys to work, such as GoogleCSE, Shodan, and bing api. You will need to register for a key through the vendor’s website. The API key needs to be added to the appropriate discovery script in theHarvester before you can use it. Refer to theHarvester GitHub page for more information.
___


### 4.6 Recon-ng 

Another powerful web reconnaissance framework, very similar to theHarvester and written in Python, is Recon-ng. The environment is very Metasploit-like (Metasploit is covered in a later lecture), in that it includes independent modules, a database for storing engagement information, and much more. Recon-ng is open source and included in the installation of Kali. Launch a terminal window from Kali and follow the steps here to get started with using Recon-ng.  

The tool is executed from the command line by typing recon-ng. To get a list of commands execute help from the framework prompt. The splash screen that is generated during initial startup shows the total number of modules that are supported for each. 

The recon-ng module categories are:
•	Recon modules Reconnaissance modules
•	Reporting modules Compile a report in various formats
•	Import modules Import target listing using supported formats
•	Exploitation modules Supported exploitation modules
•	Discovery modules Informational discovery modules category. 

To get a list of supported modules, execute show modules from the framework prompt. The first thing to do is create a workspace to manage information collection. The workspace and information gathered during module execution will be stored in the database.  

[recon-ng][default] > workspaces add example  
[recon-ng][example]>

To run a simple WHOIS query and pull contact information for a domain, define the appropriate recon module whois_pocs at the framework prompt with the use command.  

Once the module is defined, configure the target source using the set command. Then execute the modules using the run command. The discovery process could take a few minutes, as Internet speeds vary  
___


### 4.7 FOCA

![foca screenshot](/images/foca.png)

Another interesting way of conducting open-source intelligence gathering is through metadata analysis. This type of analysis can be conducted by searching through documents hosted on websites for hidden information. Files created in Office products store hidden properties within the file that may contain sensitive information, such as the author name (username), email address, etc. If you open up a Microsoft Word document, then click Info (or the File tab, depending on the version of Word you are using) you will see the property information stored for the document, similar to what’s shown below. Fingerprinting Organizations with Collected Archives, or FOCA for short, is a Microsoft Windows–based tool used to automate this discovery process. The latest version of FOCA can be downloaded from the developer’s GitHub page at https://github.com/ElevenPaths/FOCA. FOCA uses the Google, Bing, and DuckDuckGo search engines to find and analyse common document types, such as Microsoft Office, Open Office, and Adobe PDF.
___


### 4.8 Maltego 

The last tool on our list is the community edition of Maltego that ships with Kali Linux. For more information about the versions of Maltego that are available, please reference the company website: https://www.paterva.com. Here are a few steps to get started learning some data mining and visualization features that Maltego CE has to offer.  

Before using Maltego CE, create and register for a free community edition account, using the Community Registration page from the company’s website. Once you’ve completed registration, Paterva provides an official user guide for Maltego, video tutorials, and other useful documentation on their website at https://docs.paterva.com. Check out how to get started with Maltego by following some of the videos tutorials.   
___


### 4.9 OSINT Framework

OSINt is a huge area with new tools and techniques popping up daily. You eed to be smart and combine data and tools to chain together your attacks. A great regularly updated source, and inspiration when stuck is the OSINT Framework website, it should be regular site you visit and have bookmarked in your OSINT tools folder.


### 4.10 Additional THM Related Rooms

When most of the tools listed above don't have there own full room, most of them are used in the additional THM rooms listed below.  

Complete all of these additional THM related rooms
- Search Skills
- Active Reconnaissance
- Passive Reconnaissance

 

 
