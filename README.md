# PRTG Network Monitoring Home Lab

## End-to-End Installation, Configuration & Monitoring

A hands-on infrastructure monitoring home lab documenting the deployment of **PRTG Network Monitor** on a Windows Server 2022 virtual machine running inside VMware Workstation.

The project covers the complete monitoring lifecycle, from installation and initial security hardening to SNMP, WMI, LDAP, NetFlow, alerting, network maps, reporting, ticketing, and monitoring-system backups.

The goal was not simply to install a monitoring platform, but to build a functional monitoring environment capable of providing continuous visibility into network infrastructure, servers, storage, security appliances, traffic flows, and critical Windows services.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Why I Built This](#why-i-built-this)
- [What I Built](#what-i-built)
- [Lab Environment](#lab-environment)
- [Technologies and Protocols](#technologies-and-protocols)
- [Monitoring Architecture](#monitoring-architecture)
- [Project Objectives](#project-objectives)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Initial Security Hardening](#initial-security-hardening)
- [Notification Configuration](#notification-configuration)
- [Configuration Backups](#configuration-backups)
- [Device Organisation](#device-organisation)
- [Cisco Switch Monitoring with SNMP v2c](#cisco-switch-monitoring-with-snmp-v2c)
- [Synology NAS Monitoring](#synology-nas-monitoring)
- [WatchGuard Firewall Monitoring with SNMP v3](#watchguard-firewall-monitoring-with-snmp-v3)
- [NetFlow Traffic Analysis](#netflow-traffic-analysis)
- [Windows Server Monitoring](#windows-server-monitoring)
- [Active Directory Monitoring](#active-directory-monitoring)
- [Windows Service Monitoring](#windows-service-monitoring)
- [Custom PowerShell Monitoring](#custom-powershell-monitoring)
- [Network Maps](#network-maps)
- [Reports](#reports)
- [Ticketing](#ticketing)
- [Backup and Disaster Recovery](#backup-and-disaster-recovery)
- [Testing and Verification](#testing-and-verification)
- [Security Practices](#security-practices)
- [Troubleshooting Approach](#troubleshooting-approach)
- [Key Lessons Learned](#key-lessons-learned)
- [Skills Demonstrated](#skills-demonstrated)
- [Repository Structure](#repository-structure)
- [Future Improvements](#future-improvements)
- [Project Status](#project-status)
- [Final Reflection](#final-reflection)

---

# Project Overview

Network monitoring is an essential part of maintaining reliable IT infrastructure.

A server can be powered on and still have a failed service.

A switch can be reachable while experiencing abnormal CPU utilisation.

A firewall can be online while its WAN interface is saturated.

A storage device can respond to ping while a disk is beginning to fail.

Because of this, effective monitoring needs to go beyond simply checking whether a device is online.

For this project, I deployed **PRTG Network Monitor** and built a monitoring environment capable of collecting information about:

- Network availability
- CPU utilisation
- Memory utilisation
- Storage health
- Network traffic
- Interface utilisation
- Windows services
- Active Directory
- DNS
- Windows Updates
- Firewall health
- NetFlow traffic
- Device uptime
- Hardware health
- Historical performance

PRTG also provided:

- Alerts
- Notifications
- Historical graphs
- Network maps
- Reports
- Ticketing
- Configuration backups

The final environment demonstrated how monitoring can move an IT team from reactive troubleshooting toward proactive infrastructure management.

---

# Why I Built This

Before building this lab, I understood that network monitoring was important, but I wanted practical experience with how monitoring actually works.

I wanted to answer questions such as:

- How does a monitoring platform communicate with network devices?
- How does SNMP work?
- What is the difference between SNMP v2c and SNMP v3?
- How can Windows servers be monitored?
- What does WMI provide that SNMP does not?
- How can network traffic be analysed?
- How can monitoring detect a real outage?
- How should alerts be configured?
- How can monitoring data be presented visually?
- How should the monitoring platform itself be backed up?

Rather than simply reading about these technologies, I built and tested them inside my home lab.

The project therefore followed the principle:


Deploy
   ↓
Secure
   ↓
Organise
   ↓
Monitor
   ↓
Test
   ↓
Alert
   ↓
Analyse
   ↓
Document
   ↓
Back Up
   ↓
Verify Recovery

Introduction 
Before I started building this lab, I understood that network monitoring was important, but I hadn't fully appreciated why until I imagined a realistic 
scenario. It is 2:00 a.m., a critical server has silently gone offline, and the first indication that something is wrong comes from an angry customer 
who cannot access a service. At that point, the outage has already affected the business, and the IT team is reacting instead of preventing the 
problem. That is exactly the situation an effective monitoring solution is designed to avoid. 
As I learned more about enterprise networking, I realised that monitoring is not simply about displaying graphs or collecting statistics. It provides 
continuous visibility into the health, performance, and availability of network devices and servers. More importantly, it alerts administrators when 
something begins to fail, allowing issues to be investigated and resolved before users are affected. 
For this lab, I chose PRTG Network Monitor, developed by Paessler AG. PRTG is one of the most widely used infrastructure monitoring platforms 
in the industry because it combines enterprise-level capabilities with a relatively straightforward deployment process. It supports a wide range of 
monitoring technologies, including SNMP, WMI, NetFlow, Packet Sniffing, SSH, HTTP, VMware, Hyper-V, Active Directory, SQL databases, 
cloud services, and many others. It can monitor almost every component commonly found in a modern IT environment. 
One of the reasons I selected PRTG for my home lab was its generous free edition. The free version supports up to 100 sensors, which is more than 
enough to build a realistic monitoring environment while learning the platform. The installation process is straightforward, the interface is intuitive, 
and it allows me to work with many of the same monitoring techniques used in production enterprise networks. 
In this lab, I documented every stage of the deployment exactly as I completed it. I started by installing PRTG on a Windows Server virtual machine 
running in VMware Workstation before securing the installation with basic hardening measures. From there, I configured monitoring for several 
different types of infrastructure, including a Cisco Catalyst switch, a Synology NAS, a WatchGuard firewall, and a Windows Server. Along the way, 
I worked with multiple monitoring protocols, including SNMP versions 2 and 3, WMI, LDAP, and NetFlow, giving me practical experience with 
technologies commonly used by network and systems administrators. 
All of the work in this guide was completed using my own home lab running on an Intel Core i5 computer with 8 GB of RAM, a 256 GB SSD, 
Windows 11, VMware Workstation, and Windows Server 2022. No enterprise hardware was required beyond the virtual infrastructure and the 
network devices being monitored. 
By the end of this lab, I had built a fully functional monitoring environment capable of tracking network performance, server health, storage 
utilisation, authentication services, and traffic flows in real time.

Section 1: Downloading and Installing PRTG 
7. 
Installing PRTG turned out to be one of the simplest parts of this entire lab. Although PRTG is an enterprise-grade monitoring platform 
capable of monitoring thousands of devices, the installation process is surprisingly straightforward. Paessler has clearly designed the setup 
experience so that administrators can get a monitoring server up and running quickly without having to work through pages of complex 
configuration. 
8. 
For this lab, I installed PRTG directly on my Windows Server 2022 virtual machine running in VMware Workstation. Installing it inside the 
virtual machine meant everything related to the monitoring platform remained in one place, making future management, backups, and 
updates much easier. 
Step 1  Download the Installer 
9. 
The first task was to download the latest version of PRTG Network Monitor. 
10. 
I logged into my Windows Server virtual machine and opened a web browser. From there, I navigated to the official Paessler website and 
selected Products > PRTG Network Monitor. 
11. 
On the download page, I clicked Free Download. At the time of writing, this provides a fully featured 30-day trial licence. Once the 
evaluation period expires, the installation automatically reverts to the free edition, which supports monitoring of up to 100 sensors. No 
payment information or credit card is required to begin using the software. 
12. 
While the installer was downloading, I noticed that the website displayed a unique licence key associated with my Paessler account. 
Although the installer often detects this licence automatically, I copied the key and saved it in a secure location as a precaution. Having the 
licence key readily available can save time if the installer later requests manual activation. 
13. 
Note: The installer is approximately 400 MB in size. On a typical home broadband connection, I found the download completed in around 3 
to 8 minutes, depending on network speed. 
14. 
One habit I have developed when working with virtual machines is to download software directly inside the VM whenever possible. Doing 
so avoids having to copy large installation files from the host computer into the virtual machine, making the deployment process much 
simpler. 
Step 2 Run the Installer 
Once the download completed, I located the PRTG installer in the Downloads folder on my Windows Server virtual machine and launched it. 
The installation wizard guided me through the setup process with only a few decisions to make. 
1. 
I double-clicked the PRTG Network Monitor installer to begin the installation. 
2. 
The first window prompted me to choose a language. I selected English and clicked OK. 
3. 
The welcome screen appeared next. After reviewing the licence agreement, I accepted the terms and clicked Next to continue. 
4. 
Because I had downloaded the installer directly from the Paessler website while signed into my account, the installer automatically detected 
my licence information. If this does not happen, the licence key displayed on the download page can be entered manually. 
5. 
The installer then prompted me for an email address. This address becomes the primary notification account that PRTG uses when sending 
alerts, reports, and system notifications. I entered my email address and continued. 
6. 
PRTG then contacted Paessler's licensing servers to validate the licence. In my experience, this process took approximately 15 to 30 seconds 
before the installation options became available. 
Step 3 Choose the Installation Type 
At this stage, PRTG presented two installation options: 
Express
and 
Custom
. Before choosing, I wanted to understand what each option 
offered. 
Feature 
Installation location 
Express Installation 
Uses the default installation directory 
Custom Installation 
Allows a custom installation path 
Device discovery 
Automatically scans the network after installation 
Configuration 
Best suited for 
Minimal user input required 
Discovery can be configured manually later 
Greater control over installation settings 
Home labs, demonstrations, and small environments 
Production environments with specific deployment requirements 
For this lab, I selected Express Installation. 
Since my goal was to build a functional home lab quickly, I wanted PRTG to begin discovering devices on my network immediately after 
installation. Automatic discovery would also allow me to verify that the monitoring server was communicating with the rest of my lab without 
having to configure every device manually from the beginning. 
After making my selection, I clicked Next to begin the installation. 
The installation process copied the application files, installed the required Windows services, configured the built-in web server, and prepared the 
monitoring database. On my virtual machine, this process took approximately 3 to 10 minutes, depending on system performance. 
Once the installation completed successfully, PRTG automatically created a desktop shortcut on the Windows Server. 
Step 4  Verify the PRTG Services 
Before opening the web interface, I wanted to confirm that the services responsible for running PRTG had started correctly. 
To do this, I opened the Windows Services console by pressing Windows + R, typing services.msc, and pressing Enter. 
I verified that the following services were running: 
PRTG Core Server Service 
This is the main service that powers PRTG. It manages the web interface, stores monitoring data, processes alerts, generates reports, schedules 
sensor scans, and coordinates communication between all connected probes. 
PRTG Probe Service 

PRTG Network Monitoring  |  Home Lab Project Documentation 
The Probe Service performs the actual monitoring work. It polls devices, collects performance data from sensors, executes monitoring protocols 
such as SNMP and WMI, and sends the collected information back to the Core Server for processing and display. 
Both services were configured with an Automatic startup type and showed a status of Running. 
Note: If either service is stopped after installation, it can usually be started manually by right-clicking the service and selecting Start. 
Best Practice: I avoid restarting PRTG services unless it is genuinely necessary. Restarting the Core Server interrupts sensor polling and can create 
temporary gaps in monitoring data, historical graphs, and alert processing. In a production environment, service restarts should normally be 
performed during a planned maintenance window. 
Step 5 Log In to PRTG for the First Time 
With the services running successfully, I was ready to access the PRTG web interface. 
I double-clicked the PRTG Network Monitor shortcut on the desktop, which automatically opened the PRTG web console in my default browser. 
The default login credentials were: 
• 
Username: 
prtgadmin
• 
Password: 
prtgadmin
After entering the credentials, I clicked Log In. 
Within a few moments, the PRTG dashboard loaded successfully. 
One of the first things I noticed was that PRTG had already started discovering devices on my network. Because I had selected the Express 
Installation, the automatic discovery process had begun immediately after installation. Even before I had configured any devices manually, PRTG 
was already identifying systems such as my default gateway, DNS servers, and several virtual machines on my lab network. 
Watching the dashboard populate in real time was reassuring. It confirmed that the Core Server and Probe Service were functioning correctly and 
that the monitoring platform was already communicating with devices across my environment. It also demonstrated one of PRTG's greatest 
strengths—its ability to begin providing visibility into a network almost immediately after installation, with very little manual configuration 
required.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/308c0cea-1768-4e86-8cc5-3c30af5221de" />

Section 2: Initial Setup and Security Hardening 
The first time I logged into PRTG, I resisted the temptation to immediately start adding devices. One lesson I've learned from working with 
infrastructure is that the first few minutes after installing any management platform should be spent securing it rather than using it. 
As soon as the dashboard loaded, two warning banners immediately caught my attention. One informed me that I was still using the default 
administrator password, while the other warned that I was accessing the web interface over HTTP instead of HTTPS. 
Neither of these warnings should ever be ignored. A monitoring platform often contains credentials, device information, network topology, and 
operational data that would be extremely valuable to an attacker. Before trusting PRTG to monitor my infrastructure, I wanted to make sure I could 
trust the platform itself. 
Rather than rushing ahead, I decided to address every security recommendation before configuring my first monitored device. 
Step 1 Cleaning Up the Auto-Discovered Devices 
Because I selected the Express Installation during setup, PRTG immediately began scanning my local network as soon as the installation completed. 
Within a few minutes it had already discovered several devices, including my gateway, DNS server, Linux machine, and a handful of other systems. 
Although this demonstrated how quickly PRTG can begin monitoring an environment, I deliberately chose not to keep these automatically 
discovered entries. 
My goal for this lab was to understand every configuration step myself rather than relying on automation. 
I wanted to manually configure each monitored device later so I would understand exactly: 
• 
how credentials are stored, 
• 
how sensors are created, 
• 
which monitoring protocol each device uses, 
• 
and how PRTG communicates with different operating systems and network equipment. 
To prepare for that, I removed the devices that PRTG had already discovered. 
I navigated to Devices, located each automatically discovered device that I planned to configure manually later, right-clicked it, selected Delete, and 
confirmed the removal. 
The devices I removed included: 
• 
DNS Server 
• 
Gateway / Firewall 
• 
Linux Machine 
I deliberately kept the default monitoring objects that PRTG creates for itself. 
These included: 
• 
Local Probe 
• 
Core Server 
• 
Internet Connectivity Sensor 
• 
DNS Health Sensor 
These built-in sensors continuously monitor the health of the monitoring server itself, which makes them genuinely useful. 
Deleting them would only mean recreating them later. 
One thing I appreciated here was that PRTG separates its own infrastructure from the devices it monitors, making it very easy to distinguish between 
the monitoring platform and the monitored environment. 
Step 2 Replacing the Default Administrator Password 
The very first security warning PRTG displayed concerned the default administrator password. 
By default, every new installation uses: 
Username 
prtgadmin 
Password 
prtgadmin 
This is perfectly acceptable during installation because everyone begins with the same credentials. 
However, leaving these unchanged would effectively mean leaving the front door unlocked. 
Anyone who could reach the PRTG web interface would already know the administrator credentials. 
Changing the password immediately was therefore my highest priority. 
To do this, I clicked my username in the upper-right corner of the interface and opened Account Settings. 
The page displayed several account fields, including: 
• 
Login Name 
• 
Display Name 
• 
Primary Email Address 
• 
Password 
After entering the current password, I created a new password using: 
• 
uppercase letters, 
• 
lowercase letters, 
• 
numbers, 
• 
special characters, 
and ensured it exceeded fourteen characters in length. 
Once I confirmed the new password and clicked Save, the warning banner immediately disappeared. 
That simple change significantly improved the security of the monitoring platform. 
Why This Matters 
One best practice I've noticed throughout enterprise environments is the use of named administrator accounts instead of shared credentials. 

PRTG Network Monitoring  |  Home Lab Project Documentation 
Rather than everyone logging in as Administrator, each engineer receives their own account. 
This provides several important advantages: 
• 
complete audit logs showing who changed what, 
• 
support for multi-factor authentication, 
• 
easier permission management, 
• 
straightforward account removal when staff leave the organisation. 
For this home lab, a single administrator account is perfectly sufficient. 
However, if I were deploying PRTG inside a real organisation, individual named accounts would be mandatory. 
Step 3 Switching from HTTP to HTTPS 
The second warning highlighted something equally important. 
Although I had changed the administrator password, I was still accessing PRTG over ordinary HTTP. 
HTTP sends information across the network without encryption. 
That means usernames, passwords, cookies, session tokens, and monitoring information could potentially be intercepted by anyone capable of 
monitoring network traffic. 
Since a monitoring server often contains credentials for switches, routers, firewalls, Windows servers, and storage devices, protecting that traffic is 
essential. 
I clicked the HTTPS warning banner, which took me directly to the SSL configuration page. 
PRTG offered to automatically generate a self-signed certificate. 
Because this lab exists entirely inside my isolated home environment, I accepted this option. 
After enabling HTTPS, PRTG restarted its internal web service. 
When the browser reconnected, it displayed a certificate warning. 
This behaviour is completely expected. 
Because the certificate was generated locally rather than issued by a trusted Certificate Authority, my browser had no external authority to verify its 
authenticity. 
I selected Advanced, chose Proceed, and continued to the secure HTTPS version of the interface. 
After logging in again using my newly created password, the connection was fully encrypted. 
Home Lab vs Production 
For an internal lab, a self-signed certificate is perfectly acceptable. 
If this server were deployed in production, however, I would replace it with a certificate issued by a trusted Certificate Authority such as: 
• 
DigiCert 
• 
GlobalSign 
• 
Let's Encrypt 
• 
or my organisation's internal Active Directory Certificate Services. 
Using a trusted certificate eliminates browser warnings while ensuring encrypted communication between administrators and the monitoring 
platform. 
Step 4 Enabling Browser Notifications 
The next prompt appeared from my web browser rather than from PRTG itself. 
It asked whether I wanted to allow notifications. 
I chose Allow. 
Although email remains the primary notification method for most monitoring systems, browser notifications provide an additional layer of 
awareness. 
If I'm working elsewhere in PRTG or even in another browser tab, important alerts can immediately appear on my desktop without requiring me to 
constantly watch the dashboard. 
For a home lab this is convenient. 
In a production environment it provides another communication channel alongside email, SMS, Microsoft Teams, Slack, or dedicated mobile push 
notifications. 
Step 5 Reviewing Notification Templates 
With the platform now secured, I moved on to the alerting system. 
Monitoring without notifications has very limited value. 
A monitoring platform should not simply collect information. 
Its real purpose is to notify administrators whenever something requires attention. 
I navigated to: 
Setup → Notifications 
PRTG had already created a default email notification template using the email address I supplied during installation. 
Rather than creating a completely new template, I spent time reviewing how the existing one behaved. 
The default configuration sends notifications whenever: 
• 
a monitored object first goes offline, 
• 
or when it comes back online. 
I found this sensible because it immediately tells me both when a problem begins and when normal service has been restored. 
While reviewing the template, I also considered how I would design alerting in a production environment. 
For critical infrastructure such as: 
• 
domain controllers, 
• 
firewalls, 

PRTG Network Monitoring  |  Home Lab Project Documentation 
• 
Internet gateways, 
• 
backup servers, 
I would configure multiple notification methods simultaneously. 
For example: 
• 
Email 
• 
SMS 
• 
Push notification 
Less important devices could remain email-only. 
I also reviewed alert schedules. 
For this lab, continuous monitoring made perfect sense, so I left notifications enabled twenty-four hours a day. 
In a business environment, however, planned maintenance windows would normally suppress non-critical alerts to prevent unnecessary notifications 
while engineers intentionally perform maintenance. 
An Important Lesson About Alert Fatigue 
One idea that stood out to me while configuring notifications was the danger of alert fatigue. 
It is surprisingly easy to create hundreds of alerts. 
It is much harder to create alerts that people actually pay attention to. 
If every tiny fluctuation generates a warning, administrators eventually stop reading them. 
When that happens, the truly important alert can easily be missed. 
I would much rather receive ten meaningful alerts than one thousand unnecessary ones. 
Effective monitoring is about quality, not quantity.

Step 6 Learning My Way Around the Interface 
Before adding any devices, I spent several minutes simply exploring the PRTG interface. 
Understanding where information lives is almost as important as understanding the sensors themselves. 
Across the top of the interface, I familiarised myself with each major section: 
• 
Devices — where monitored infrastructure is organised into groups, devices, and sensors. 
• 
Sensors — a complete list of every measurement being collected. 
• 
Alarms — active warnings and failures requiring attention. 
• 
Maps — custom dashboards and graphical network maps. 
• 
Reports — historical reports and availability statistics. 
• 
Logs — configuration changes, events, and system activity. 
• 
Tickets — built-in incident tracking and integrations with external service desks. 
• 
Setup — administrative configuration for the entire platform. 
Spending time understanding the interface early saved me repeatedly searching for options later in the lab. 
Step 7 Reviewing Automatic Updates 
Next, I visited: 
Setup → Auto-Update 
PRTG allows updates to be installed automatically or manually. 
For my home lab, I left automatic updates enabled because downtime affects only me. 
If this were monitoring a production network, however, I would almost certainly choose manual updates. 
Monitoring systems are too important to update blindly. 
Before applying any upgrade in production, I would first validate it inside a lab or staging environment. 
If an update introduces a bug into the monitoring platform itself, administrators lose visibility into the rest of the network precisely when they may 
need it most. 
That is not a risk worth taking. 
Step 8 Creating My First Configuration Backup 
Before moving on to monitoring actual devices, I created my first configuration backup. 
I navigated to: 
Setup → Administrative Tools 
and selected Save Configuration to File. 
PRTG generated a ZIP archive containing its configuration database. 
Creating this backup took only a few moments, but it gave me an immediate recovery point should I accidentally misconfigure the platform later. 
As I continued building this lab, I made it a habit to create another backup before making any significant configuration changes. 
One important distinction became clear to me during this step. 
A PRTG configuration backup protects: 
• 
device settings, 
• 
sensor configuration, 
• 
notification templates, 
• 
historical monitoring data. 
A virtual machine backup protects something entirely different: 
• 
the Windows operating system, 
• 
installed applications, 
• 
system files, 

PRTG Network Monitoring  |  Home Lab Project Documentation 
• 
registry, 
• 
and recovery from hardware failure or ransomware. 
Both forms of backup are necessary. 
Neither replaces the other. 
That was an important lesson for me because true disaster recovery depends on protecting both the application and the platform it runs on.
<img width="1402" height="1122" alt="image" src="https://github.com/user-attachments/assets/5067437c-37ec-4891-99cb-70b8e306c855" />

Section 3: Organising Devices with Groups
Before adding my first monitored device, I took a step back to think about how I wanted to organise my monitoring environment. It can be tempting to immediately start adding switches, servers, firewalls, and storage devices as soon as PRTG is installed, especially because the software makes it so easy. However, I quickly realised that spending a few extra minutes planning the structure at the beginning would save me a great deal of time later.
One thing I appreciate about PRTG is that it organises everything using a clear hierarchy:
Groups → Devices → Sensors
Understanding this hierarchy is essential because every object in PRTG inherits settings from the level above it. For example:
•	A Group can contain multiple devices.
•	Each Device can contain dozens or even hundreds of sensors.
•	Settings such as credentials, scanning intervals, notification templates, and SNMP configuration can all be inherited automatically.
That inheritance model is one of PRTG's biggest strengths. Rather than configuring every device individually, I can configure a setting once at the appropriate level and allow every child object beneath it to inherit those settings automatically. As the monitoring environment grows, this dramatically reduces administrative effort and helps maintain consistency.
I found myself thinking about how quickly a monitoring platform can become difficult to navigate if devices are added without any organisational plan. Even a modest network of fifty devices can contain several hundred sensors. Without a logical structure, locating a particular server or switch during an outage becomes frustrating, and troubleshooting takes longer than it should.
For that reason, I decided to establish a clean organisational structure before adding any monitored devices
Understanding Different Grouping Strategies
One thing I learned while researching PRTG is that there isn't a single "correct" way to organise devices. The ideal structure depends entirely on the environment being monitored.
A small business with one office will naturally organise things differently from a multinational organisation with hundreds of locations around the world.
As I explored different deployment examples, I noticed several organisational strategies that appear repeatedly in production environments.
Organising by Function
This is probably the simplest and most common approach for smaller organisations. Devices are grouped according to what they do rather than where they are located. Typical groups include:
•	Network Infrastructure
•	Servers
•	Storage
•	Firewalls
•	Wireless
•	Virtualisation
This approach works particularly well when one IT team manages the entire environment.
Because my home lab is relatively small and I'm responsible for every device myself, this organisational model makes the most sense.
Organising by Location
Larger organisations often separate infrastructure according to physical location. For example:
•	London
•	Manchester
•	Birmingham
•	Edinburgh
or
•	New York
•	Chicago
•	Dallas
Each office contains its own switches, servers, printers, and firewalls.
This structure makes it much easier for regional IT teams to focus only on the equipment they support.
Organising by Technology
Some organisations organise devices according to technology rather than business function. For example:
•	Cisco
•	VMware
•	Microsoft Windows
•	Linux
•	NetApp
•	Synology
This approach is useful when specialist teams exist.
A Windows infrastructure team may only care about Windows servers, while the networking team focuses exclusively on Cisco equipment. Separating technologies gives each team a cleaner operational view.
Organising by Criticality
Another approach is organising infrastructure based on business importance.
For example:
Tier 1
Mission-critical systems such as:
•	Internet firewalls
•	Domain Controllers
•	Core switches
•	Storage arrays Tier 2
Important business systems such as:
•	departmental application servers
•	print servers
•	backup infrastructure Tier 3
Lower-priority equipment such as:
•	lab devices
•	development systems
•	test environments
This structure is especially useful when alert prioritisation matters more than device type
Hybrid Organisation
Large enterprises frequently combine multiple strategies. For example:
London
├── Network
├── Servers
├── Storage
Manchester
├── Network
├── Servers
├── Storage
This hybrid approach scales extremely well because administrators can quickly drill down by both location and function.
Choosing the Best Structure for My Lab
Since my lab represents a single-site environment, I decided that organising by function would provide the clearest and simplest structure.
I wanted similar devices to sit together so they would be easy to locate, compare, and manage. My initial group structure looked like this:
Local Probe
│
├── Network Infrastructure
│	└── Switching
│
├── Servers
│
├── Storage
│
└── Firewalls
Although this hierarchy appears simple now, it leaves plenty of room for future expansion. If I later decide to monitor:
•	wireless access points,
•	routers,
•	VMware hosts,
•	backup servers,
•	cloud infrastructure,
I can simply create additional groups without needing to reorganise everything from scratch. Planning ahead like this avoids unnecessary administrative work later.
Creating the Group Structure
With my design decided, I began creating the groups inside PRTG.
From the Devices page, I located the Local Probe, which acts as the root of the monitoring tree. Every monitored object ultimately belongs beneath this probe.
I right-clicked Local Probe and selected Add Group. The first group I created was called:
Network Infrastructure
During creation, PRTG asked whether I wanted to enable automatic discovery. For this lab, I deliberately left auto-discovery disabled.
My goal was to add every device manually so I could understand exactly how each monitoring protocol was configured.
Once the first group had been created successfully, I repeated the same process to create three additional top-level groups:
•	Servers
•	Storage
•	Firewalls
Within Network Infrastructure, I wanted another level of organisation. Not every network device performs the same role.
Some route traffic. Some switch traffic.
Others provide wireless connectivity.
To reflect this, I created a subgroup specifically for switching equipment.
I right-clicked Network Infrastructure, selected Add Group, and created a new subgroup named: Switching
This is where my Cisco Catalyst switch will be added later in the lab.
As my environment grows, I could easily create additional subgroups such as:
•	Routing
•	Wireless
•	Load Balancers
•	WAN
•	VPN Appliances
Having this flexibility means the monitoring structure can evolve naturally alongside the network itself.
Understanding Inheritance
While creating these groups, I discovered what is probably one of PRTG's most useful features: inheritance. Inheritance allows child objects to automatically receive settings from their parent group.
For example, if every Cisco switch in my organisation uses the same SNMP community string, there is no need to enter that information repeatedly for every individual device.
Instead, I can configure the SNMP credentials once at the Switching group level. Every switch placed inside that group automatically inherits those credentials.
If I later add ten more Cisco switches, they immediately inherit the same settings without requiring any additional configuration. The same inheritance model applies to many other settings, including:
•	scanning intervals,
•	credentials,
•	notification templates,
•	schedules,
•	dependency settings,
•	and security options.
This dramatically reduces repetitive work while ensuring consistency across the monitoring environment.
It's one of those features that may not seem particularly exciting at first, but I can already see how valuable it becomes in larger deployments where hundreds or even thousands of devices are being monitored.
What I Learned from This Section
Before working with PRTG, I assumed groups were simply folders used to keep the interface tidy. After spending time building my own hierarchy, I realised they are much more than that.
Groups are the foundation upon which the entire monitoring platform is built.
A well-designed group structure improves navigation, reduces configuration time through inheritance, simplifies future expansion, and makes troubleshooting significantly easier during an outage.
Taking the time to organise devices before adding them may not feel like the most exciting part of setting up a monitoring system, but I found it to be one of the smartest investments I could make. As my monitoring environment continues to grow, this structure will allow me to manage it efficiently without constantly reorganising devices or duplicating configuration work.

Section 4: Monitoring a Cisco Switch with SNMP Version 2 
Adding my Cisco switch was the first time I connected a real network device to PRTG, and it marked an important milestone in building my 
monitoring environment. Up until this point, I had focused on installing, securing, and organising the monitoring platform itself. Now it was time to 
begin collecting live operational data from the network. 
I deliberately chose to start with my Cisco Catalyst switch because it represents one of the most common devices found in enterprise environments. 
Almost every organisation relies on switches to connect users, servers, printers, wireless access points, and countless other devices. If a switch fails, 
the impact can range from a single disconnected workstation to an entire building losing network connectivity. 
By the end of this section, I wanted PRTG to automatically monitor my switch every sixty seconds, continuously collecting information about its 
availability, uptime, processor utilisation, memory usage, hardware health, and interface traffic. More importantly, I wanted PRTG to immediately 
alert me if the switch became unreachable or if any critical hardware component began to fail. 
This is where PRTG starts moving beyond being a simple dashboard and becomes a proactive monitoring solution. 
Understanding SNMP 
Before configuring the switch, I wanted to properly understand the protocol that makes network monitoring possible. 
SNMP stands for Simple Network Management Protocol, and despite its name, it is one of the most important protocols in network administration. 
Almost every enterprise-grade network device supports SNMP, including: 
• 
routers,  
• 
switches,  
• 
firewalls,  
• 
wireless controllers,  
• 
printers,  
• 
storage arrays,  
• 
UPS systems,  
• 
environmental sensors,  
• 
and even many IoT devices.  
The basic idea behind SNMP is straightforward. 
Instead of waiting for users to report problems, the monitoring server regularly asks devices how they are performing. 
This communication follows what is known as a manager-agent model. 
The monitoring server—in this case, PRTG—acts as the SNMP Manager. 
Every monitored network device runs an SNMP Agent. 
At regular intervals, the manager sends requests to the agent asking questions such as: 
• 
What is your CPU usage?  
• 
How much memory are you using?  
• 
How long have you been running?  
• 
Which network interfaces are active?  
• 
Are your cooling fans operating normally?  
• 
Has your power supply failed?  
The device simply responds with the current value for each requested metric. 
PRTG stores these values in its database, builds historical graphs, compares them against thresholds, and generates alerts whenever something falls 
outside expected behaviour. 
I like thinking of SNMP as a routine health check. 
Rather than waiting for a patient to collapse before seeing a doctor, PRTG performs regular check-ups every minute, allowing me to identify 
warning signs before they develop into major outages. 
Key SNMP Concepts 
While learning SNMP, I came across several terms that appear repeatedly throughout networking documentation. 
Understanding these concepts made everything else much easier. 
Object Identifier (OID) 
Every measurable value inside an SNMP-enabled device has its own unique numerical address known as an Object Identifier, or OID. 
An OID works very much like a file path. 
Instead of pointing to a file, however, it points to a specific piece of information stored inside the device. 
For example: 
1.3.6.1.2.1.1.3.0 
represents the system uptime on almost every SNMP-capable device. 
Whenever PRTG wants to know how long the switch has been running, it queries this OID. 
Thousands of other OIDs exist for values such as: 
• 
CPU utilisation,  
• 
interface traffic,  
• 
memory usage,  
• 
fan status,  
• 
power supply health,  
• 
temperature,  
• 
and many vendor-specific features.  
Management Information Base (MIB) 
Remembering long numerical OIDs would be nearly impossible. 
Page 15 of 52 
PRTG Network Monitoring  |  Home Lab Project Documentation 
Fortunately, manufacturers provide something called a Management Information Base (MIB). 
A MIB acts as a dictionary that translates those long numerical OIDs into meaningful names. 
Instead of remembering: 
1.3.6.1.2.1.1.3.0 
the monitoring software understands that it means: 
System Uptime 
Cisco, Synology, WatchGuard, Dell, HP, and countless other vendors publish their own MIB files so monitoring software can understand both 
standard SNMP values and manufacturer-specific features. 
Without MIBs, SNMP would be far more difficult to work with. 
Community Strings 
Since SNMP Version 2 does not use usernames or passwords in the traditional sense, authentication is handled using a community string. 
A community string functions somewhat like a shared password. 
If the monitoring server knows the correct community string, the device responds to its SNMP requests. 
If the community string is incorrect, the device simply ignores the request. 
For monitoring purposes, I configured Read-Only (RO) access. 
This means PRTG can read information from the switch but cannot change any configuration. 
This is considered best practice because monitoring systems rarely need write access. 
SNMP Traps 
Everything I've described so far involves polling. 
Every sixty seconds, PRTG asks the switch for information. 
However, SNMP also supports something called traps. 
Instead of waiting to be asked, the device immediately sends a message whenever an important event occurs. 
Examples include: 
• 
a network interface failing,  
• 
a power supply fault,  
• 
a fan failure,  
• 
excessive temperature,  
• 
or a device reboot.  
Polling provides continuous monitoring. 
Traps provide immediate notification. 
Using both together creates a much more responsive monitoring solution. 
Step 1 Configuring SNMP on the Cisco Switch 
With the concepts understood, I connected to my Cisco Catalyst 2960X switch using SSH. 
After entering privileged EXEC mode, I entered global configuration mode and configured SNMP. 
Switch> enable 
Switch# configure terminal 
! Configure a read-only SNMP community 
Switch(config)# snmp-server community danmilltraining RO 
! Enable SNMP trap generation 
Switch(config)# snmp-server enable traps 
! Optional but useful administrative information 
Switch(config)# snmp-server contact lab-admin@corp.local 
Switch(config)# snmp-server location Home Lab - Switch Rack 
Switch(config)# end 
Switch# copy running-config startup-config 
Surprisingly, very little configuration was required. 
The switch was now ready to respond to SNMP queries. 
The community string I configured would later be used by PRTG to authenticate every polling request. 
Security Considerations 
While configuring SNMP, I learned that many devices still ship with default community strings such as: 
• 
public  
• 
private  
These values are widely known and should never be used in production. 
Anyone who knows the community string can potentially query information about the device. 
In a production environment I would also configure an SNMP access list limiting requests exclusively to the IP address of the PRTG server. 
That way, even if someone somehow discovered the community string, the switch would ignore requests originating from any other system. 
Step 2 Adding the Switch to PRTG 
With SNMP enabled on the switch, I returned to PRTG. 
Since I had already organised my monitoring environment into groups, adding the device felt very straightforward. 
I navigated to: 
Page 16 of 52 
PRTG Network Monitoring  |  Home Lab Project Documentation 
Devices → Network Infrastructure → Switching 
Inside the Switching group, I right-clicked and selected Add Device. 
The device configuration page requested several pieces of information. 
I entered: 
Device Name 
DMT-SW-01 
I intentionally use a consistent naming convention throughout my lab. 
Breaking the name apart: 
• 
DMT – organisation abbreviation  
• 
SW – device type (Switch)  
• 
01 – device number  
Using a standard naming convention makes locating devices much easier as the environment grows. 
Next, I selected: 
• 
IPv4  
• 
the switch's management IP address  
• 
and the Cisco device icon so it would be visually recognisable throughout the interface.  
When I reached the SNMP settings, I disabled inheritance because I wanted to configure this device individually for learning purposes. 
I selected: 
• 
SNMP Version 2c  
• 
Community String: danmilltraining  
• 
UDP Port: 161  
• 
Timeout: 5 seconds  
Finally, I disabled auto-discovery temporarily. 
I preferred to trigger it manually once the device had been fully configured. 
After clicking OK, the switch appeared beneath the Switching group. 
Step 3 Running Auto-Discovery 
With the device successfully added, I right-clicked the switch and selected: 
Run Auto-Discovery 
This is where PRTG began demonstrating one of its most useful capabilities. 
Rather than asking me to manually create dozens of sensors, it immediately queried the switch via SNMP and analysed which metrics were 
available. 
After approximately one to two minutes, PRTG automatically populated a collection of recommended sensors. 
My Cisco Catalyst switch produced sensors including: 
• 
Ping  
• 
SNMP System Uptime  
• 
SNMP CPU Load  
• 
SNMP Memory  
• 
Fan Health  
• 
Interface Traffic  
• 
SSL Certificate  
Each sensor represented a different aspect of the switch's health. 
The Ping sensor simply verified that the device was reachable. 
The System Uptime sensor showed exactly how long the switch had been running since its last reboot. 
The CPU and Memory sensors allowed me to monitor resource utilisation over time. 
The Fan Health sensor monitored hardware status. 
Meanwhile, interface sensors were automatically created for every active network port, allowing me to monitor traffic flowing through each 
connection individually. 
I found it impressive how much useful information became available after only a few minutes of configuration. 
Step 4 Exploring the Monitoring Data 
To better understand how PRTG presents information, I opened the SNMP CPU Load sensor. 
The sensor page displayed far more than just a single percentage value. 
At the top of the page I could immediately see the current processor utilisation. 
Below that was a historical graph showing CPU usage over the previous twenty-four hours. 
Opening the Live Data tab revealed a much larger interactive graph. 
I could zoom into specific time periods and examine exactly when processor usage increased and how long those spikes lasted. 
This historical visibility immediately stood out as one of PRTG's greatest strengths. 
If someone were to report that "the network was slow yesterday afternoon," I would no longer have to rely on guesswork. 
Instead, I could simply examine the graphs for: 
• 
CPU utilisation,  
• 
interface traffic,  
• 
memory usage,  
• 
or device uptime  
during that exact period and determine whether the switch was experiencing unusually high utilisation. 
Rather than troubleshooting based on opinions, I would have objective historical evidence. 
Page 17 of 52 
PRTG Network Monitoring  |  Home Lab Project Documentation 
Step 5 Testing the Monitoring System 
A monitoring system is only useful if it actually detects failures. 
Rather than assuming everything was working, I decided to perform a controlled test. 
I disconnected the network cable connecting the switch to the firewall. 
Then I simply waited. 
Within approximately one polling cycle, the dashboard changed dramatically. 
The device icon turned red. 
The Ping sensor immediately reported complete packet loss. 
Every SNMP sensor changed state because PRTG could no longer communicate with the SNMP agent running on the switch. 
Shortly afterwards, an email notification arrived in my inbox informing me that the device had become unavailable. 
This confirmed that both monitoring and alerting were functioning exactly as intended. 
I then reconnected the cable. 
After another polling cycle, every sensor gradually returned to a healthy green state. 
A second email notified me that the switch had recovered. 
Watching the entire process happen automatically was incredibly satisfying because it demonstrated the true purpose of infrastructure monitoring. 
Without PRTG, I would only discover the outage when a user reported losing connectivity. 
With PRTG, the monitoring platform identified the problem first, documented it, alerted me, and then confirmed when service had been restored. 
That shift—from reacting to problems after users notice them, to identifying issues before users even realise something is wrong—is one of the 
biggest reasons organisations invest in enterprise monitoring platforms. 
What I Learned from This Section 
This lab gave me my first practical experience with SNMP and helped me understand why it remains the foundation of network monitoring decades 
after its introduction. 
I learned that configuring SNMP on a Cisco switch requires surprisingly little effort, yet it unlocks a huge amount of operational visibility. Within 
minutes, PRTG was collecting live performance metrics, building historical graphs, and continuously checking the health of the switch without any 
manual intervention. 
Perhaps the biggest lesson, however, was the importance of verification. Rather than simply trusting that the configuration was correct, I deliberately 
simulated a network failure to confirm that PRTG detected the outage, generated alerts, and recognised when the device recovered. That simple test 
gave me confidence that my monitoring platform was functioning as expected and would reliably notify me of real issues in the future. From this 
point onward, I wasn't just monitoring my network—I was actively verifying that my monitoring system itself could be trusted.
<img width="1402" height="1122" alt="image" src="https://github.com/user-attachments/assets/9ffc95fc-3b57-4098-b465-a0b146c4edd2" />

Section 5: Monitoring a Synology NAS with Manual Sensors
When I reached the point of monitoring my Synology NAS, I deliberately chose not to rely on PRTG's automatic discovery feature. Auto-discovery 
is incredibly useful and saves a lot of time, but I wanted to understand exactly what I was monitoring and why each sensor mattered. Rather than 
letting PRTG make those decisions for me, I decided to add each sensor manually. 
This turned out to be one of the most educational parts of the lab. 
By manually selecting each sensor, I gained a much better appreciation of how PRTG collects data, what information is available through SNMP, 
and which metrics are genuinely useful in a production environment. It also reinforced an important lesson I've come to appreciate throughout these 
labs: good monitoring isn't about collecting as much data as possible—it's about collecting the right data. 
Understanding Why a NAS Should Be Monitored
A Network Attached Storage (NAS) device is often one of the most important systems in an organisation. Even in a small business, it may store 
shared company files, departmental documents, virtual machine backups, surveillance footage, or user home folders. 
If a switch fails, users may temporarily lose network connectivity. 
If a router fails, users may lose internet access. 
But if a storage device fails without warning, organisations can lose years of important business data. 
That is why monitoring storage devices isn't simply about knowing whether they're online. It's about identifying warning signs long before they 
become disasters. 
Questions I wanted PRTG to answer included: 
• 
Are the hard drives healthy? 
• 
Are any disks beginning to overheat? 
• 
Is the RAID array healthy? 
• 
Is memory usage becoming excessive? 
• 
Is the NAS becoming unreachable? 
• 
Is network traffic unusually high? 
• 
Is storage capacity approaching its limit? 
Those questions shaped the sensors I decided to configure. 
Step 1 Enable SNMP on the Synology NAS
The first task was enabling SNMP on the NAS itself. 
Unlike Cisco switches where configuration is completed through the command line, Synology DSM provides a straightforward graphical interface. 
After opening my web browser, I entered the IP address of my NAS and logged into Synology DiskStation Manager (DSM) using my administrator 
account. 
From the desktop interface, I navigated to: 
Control Panel → Terminal & SNMP → SNMP 
Inside the SNMP tab, I completed the following configuration: 
1. 
Tick Enable SNMP Service. 
2. 
Select SNMP Version: SNMPv2c. 
3. 
Enter the community string: 
danmilltraining 
4. 
Click Apply. 
The community string must exactly match the value that PRTG will later use when polling the device. Even a single incorrect character would cause 
every SNMP request to fail. 
Reflection 
One thing I noticed is how much simpler Synology makes SNMP configuration compared to traditional networking equipment. On Cisco devices I 
had to configure everything through IOS commands, whereas Synology exposes the same functionality through a graphical interface. 
Although the interface is easier, the underlying protocol is exactly the same. 
Step 2 Create the Storage Group and Add the Device
With SNMP enabled, I switched back to PRTG. 
One thing I've started appreciating about PRTG is that organisation matters almost as much as monitoring itself. 
If every monitored device sits in one large list, troubleshooting quickly becomes frustrating as the environment grows. 
To keep everything organised, I first created a dedicated group for storage devices. 
Inside PRTG I: 
1. 
Right-clicked Local Probe. 
2. 
Selected Add Group. 
3. 
Named the group: 
Storage 
4. 
Disabled Auto Discovery. 
5. 
Clicked OK. 
Within that new group, I added my NAS. 
The device configuration looked like this: 
Setting 
Device Name 
IP Version 
IP Address 
Value 
DMT-NAS-01 
IPv4 
NAS management IP 
Page 20 of 52 
PRTG Network Monitoring  |  Home Lab Project Documentation 
Setting 
Device Icon 
Value 
Generic Storage 
Since there isn't a Synology-specific icon available, I selected the generic storage icon, which still makes the device easy to recognise in the device 
tree. 
Further down the page, I configured SNMP. 
I disabled Inherit from Parent, selected: 
• 
SNMP Version 2c 
and entered the community string: 
danmilltraining 
Finally, I clicked OK. 
The NAS now appeared in the Storage group, ready for monitoring. 
Step 3 Add Sensors Manually
Instead of running Auto Discovery, I clicked Add Sensor. 
This was intentional. 
I wanted complete control over every metric being collected. 
One of the things that surprised me about PRTG was just how many sensor types exist. 
Searching for "Synology" alone produced several specialised sensors designed specifically for Synology hardware. 
Synology System Health
The first sensor I added was: 
Synology System Health 
This acts almost like an overall health dashboard for the NAS. 
Rather than monitoring one individual component, it combines several hardware checks together, including: 
• 
overall system status 
• 
fan health 
• 
power supply status 
• 
system temperature 
• 
hard drive condition 
If any of these components begin reporting problems, the sensor immediately changes from green to warning or error. 
I like this sensor because it gives me a quick "at-a-glance" overview before I begin investigating more detailed metrics. 
Synology Physical Disk
Next, I added: 
Synology Physical Disk 
After selecting the sensor, PRTG asked which physical drives I wanted to monitor. 
Since my NAS contains two disks, I selected both. 
Each disk now reports: 
• 
operational status 
• 
temperature 
• 
health 
• 
read/write condition 
This quickly became one of my favourite sensors. 
Hard drives rarely fail instantly. 
Instead, they usually begin showing subtle warning signs first. 
Temperature gradually increases. 
SMART health values begin changing. 
Read errors become more frequent. 
Monitoring these trends allows an engineer to replace a drive before users ever notice a problem. 
Synology Logical Disk
Physical disks are only part of the picture. 
Most NAS devices combine multiple drives into RAID arrays. 
Users don't interact with the physical disks—they interact with logical storage volumes. 
To monitor those, I added: 
Synology Logical Disk 
This sensor reports information including: 
• 
RAID health 
• 
available capacity 
• 
used capacity 
• 
overall logical volume status 
If the RAID array were ever to degrade because of a failed disk, this sensor would immediately alert me. 
Step 4 Add Generic SNMP Sensors
Although Synology-specific sensors are excellent, I also wanted to monitor resources that exist on almost every network device. 
Page 21 of 52 
PRTG Network Monitoring  |  Home Lab Project Documentation 
SNMP Memory 
I searched for: 
SNMP Memory 
and selected: 
• 
Physical Memory 
• 
Virtual Memory 
• 
Swap Space 
After the first polling cycle, PRTG began graphing memory utilisation. 
Watching memory usage over time is far more useful than simply seeing one percentage value. 
If memory slowly increases every day without dropping, that could indicate: 
• 
a memory leak 
• 
runaway processes 
• 
insufficient RAM 
• 
poorly performing applications 
Historical trends are often far more valuable than snapshots. 
Ping Sensor 
Next, I added the most basic sensor of all. 
Ping 
Although it's simple, it's arguably the most important sensor in the entire monitoring system. 
If the NAS stops responding to ICMP requests, I immediately know something fundamental has happened. 
The device could be: 
• 
powered off 
• 
disconnected 
• 
experiencing network failure 
• 
completely crashed 
Every other sensor depends on the device being reachable first. 
SNMP Traffic 
Finally, I added: 
SNMP Traffic 
for the NAS's network interface. 
This sensor measures: 
• 
inbound bandwidth 
• 
outbound bandwidth 
• 
utilisation 
• 
historical traffic patterns 
One of the biggest advantages of monitoring network traffic is identifying abnormal behaviour. 
For example, if a backup job suddenly begins transferring several hundred gigabytes during business hours, the graph immediately reveals exactly 
when the spike occurred. 
That historical visibility becomes invaluable during troubleshooting. 
Step 5 Verify the Sensors
Once the sensors had completed a few polling cycles, every one of them changed to a healthy green status. 
That simple colour change told me two important things immediately: 
• 
PRTG could successfully communicate with the NAS. 
• 
The SNMP configuration was correct. 
I clicked into the Physical Disk sensor to explore the available data. 
PRTG displayed information including: 
• 
disk status 
• 
current temperature 
• 
historical temperature graphs 
• 
long-term health trends 
The historical graph was particularly interesting. 
Rather than showing only the current temperature, it plotted changes over time. 
If a drive slowly increased from 34°C to 49°C over several weeks, I would notice the trend long before the hardware actually failed. 
That is exactly what proactive monitoring is designed to achieve. 
What I Learned From This Section
This lab completely changed how I think about monitoring storage systems. 
Initially, I assumed monitoring simply meant checking whether a device was online. 
By manually building each sensor, I realised that effective monitoring is really about identifying subtle warning signs before they become outages. 
A healthy NAS isn't just one that responds to pings. It's one whose disks remain cool, whose RAID array stays healthy, whose memory usage 
remains stable, and whose network traffic follows expected patterns. 
Page 22 of 52 
PRTG Network Monitoring  |  Home Lab Project Documentation 
Perhaps the biggest lesson I took away from this section was the value of historical data. Looking at a device's current status tells me how it's 
performing right now, but looking at its graphs over days or weeks tells me whether it's getting healthier or slowly drifting towards failure. That 
ability to spot trends before users are affected is what transforms monitoring from a reactive tool into a proactive operational strategy.
<img width="1402" height="1122" alt="image" src="https://github.com/user-attachments/assets/d6f7ae10-bdb7-4661-9858-00cacc21487e" />

Section 6: Monitoring a Firewall with SNMP Version 3
Up to this point in my lab, every device I monitored had used SNMP version 2c. It worked well, was simple to configure, and was perfectly 
adequate for a home lab. However, I knew that if I were monitoring a production firewall—the device responsible for protecting an organisation's 
entire network—I would need a much more secure approach. 
This is where SNMP version 3 becomes essential. 
Unlike SNMP v2c, which relies on a simple community string sent without encryption, SNMP v3 introduces proper authentication and encryption. It 
doesn't just verify that the monitoring server is authorised to request information—it also protects the monitoring traffic itself from being intercepted 
or read by someone else on the network. 
That distinction is especially important when monitoring firewalls. Firewalls expose information about interfaces, VPNs, CPU usage, memory 
consumption, traffic patterns, and other operational data that could be valuable to an attacker. Protecting that information is just as important as 
monitoring it. 
This lab gave me my first practical experience configuring secure SNMP communications, and it reinforced how security should be built into 
management protocols rather than added afterwards. 
Understanding SNMP v2c vs SNMP v3
Before configuring anything, I wanted to understand exactly what improvements SNMP version 3 provides. 
Feature 
Authentication 
SNMP v2c 
Community string (plaintext) 
SNMP v3 
Username with authentication password 
Encryption 
Security 
Credentials 
Best Use 
None 
Low 
Single community string 
DES, AES-128 or AES-256 (device dependent) 
High 
Username, authentication password and encryption password 
Home labs and trusted internal networks 
Production environments and security-critical devices 
The biggest takeaway for me was this: 
With SNMP v2c, anyone who discovers the community string can query the device. 
With SNMP v3, an attacker would need: 
• 
the username 
• 
the authentication password 
• 
the encryption password 
and would still be unable to read the traffic because it is encrypted. 
That's a significant improvement. 
⚠️
Best Practice 
SNMP version 1 should never be used. 
SNMP version 2c is acceptable only inside trusted internal environments. 
Whenever monitoring routers, firewalls, VPN concentrators or any security-sensitive infrastructure, SNMP version 3 should always be the preferred 
choice. 
Step 1 Configure SNMP Version 3 on the WatchGuard Firewall
The first task was configuring the firewall itself. 
I logged into my WatchGuard Firebox management interface and navigated to: 
System → SNMP 
From there, I enabled SNMP and selected Version 3. 
Unlike SNMP v2, several additional configuration options appeared. 
I created a dedicated monitoring account using the following settings. 
Setting 
Username 
Authentication Protocol 
Authentication Password 
Privacy Protocol 
Privacy Password 
Configuration 
snmpv3user 
SHA-1 
Strong unique password 
DES 
Different strong password 
Although DES was the only encryption option available on the WatchGuard appliance in my lab, I learned that many enterprise devices now support 
AES-128 or AES-256, which are considered significantly stronger and should always be chosen whenever available. 
One thing I found particularly interesting was that SNMP v3 separates authentication from encryption. 
The authentication password proves who I am. 
The privacy password encrypts the communication itself. 
Using different passwords for these two purposes improves overall security. 
After creating the user, I configured SNMP traps. 
Rather than waiting for PRTG to poll every sixty seconds, traps allow the firewall to proactively send important events to the monitoring server the 
moment they occur. 
I configured the trap destination as the IP address of my PRTG server before saving the configuration. 
Reflection 
Although the interface varies between vendors like Cisco, Palo Alto, Fortinet and WatchGuard, the underlying concepts remain almost identical. 
Every platform asks for: 
• 
an SNMP username 
 
PRTG Network Monitoring  |  Home Lab Project Documentation 
• 
an authentication protocol 
• 
an authentication password 
• 
an encryption protocol 
• 
an encryption password 
• 
the monitoring server's IP address 
Learning those concepts is more valuable than memorising any single vendor's interface. 
Step 2 Configure SNMP Version 3 at the Group Level in PRTG
One of the features I appreciate most about PRTG is its inheritance model. 
Rather than configuring credentials individually for every device, I can configure them once at the group level and allow every device inside that 
group to inherit those settings automatically. 
To organise my environment, I first created a new group. 
I right-clicked Local Probe and selected: 
Add Group 
I named it: 
Firewalls 
Inside the group's settings, I scrolled down to: 
Credentials for SNMP Devices 
I disabled Inherit from Parent because the default credentials were configured for SNMP version 2. 
I then entered my SNMP version 3 settings. 
Setting 
Version 
Authentication 
Username 
Authentication Password 
Encryption 
Encryption Key 
Value 
SNMP v3 
SHA 
snmpv3user 
My configured password 
DES 
Privacy password 
After saving the group, every future firewall I place inside it will automatically inherit these credentials. 
This may seem like a small feature now, but I can already see how valuable it would become in a larger organisation. 
Imagine monitoring twenty firewalls. 
If the SNMP password changes, I only need to update it once at the group level instead of editing twenty individual devices. 
That significantly reduces administrative effort and the chance of configuration mistakes. 
Step 3 Add the WatchGuard Firewall
With the credentials already configured, adding the firewall itself became straightforward. 
Inside the Firewalls group, I selected: 
Add Device 
I completed the configuration using: 
Setting 
Device Name 
IP Version 
IP Address 
Device Icon 
Value 
DMT-FW-01 
IPv4 
WatchGuard management IP 
WatchGuard firewall icon 
Because the SNMP credentials were inherited from the parent group, I didn't need to enter them again. 
This confirmed that the inheritance model was working exactly as intended. 
Step 4 Run Auto-Discovery
Instead of manually selecting sensors this time, I decided to let PRTG perform automatic discovery. 
WatchGuard devices are well supported, and PRTG already understands many of the metrics they expose. 
I right-clicked the firewall and selected: 
Run Auto-Discovery 
Over the next few minutes, PRTG queried the firewall's Management Information Base (MIB) and automatically added a comprehensive collection 
of sensors. 
The list included: 
• 
Ping 
• 
CPU Usage 
• 
Memory Usage 
• 
Interface Traffic 
• 
Disk Free sensors 
• 
System Uptime 
The interface traffic sensors immediately became some of the most useful. 
Each interface displayed separate graphs for: 
• 
inbound bandwidth 
• 
outbound bandwidth 

PRTG Network Monitoring  |  Home Lab Project Documentation 
• 
utilisation over time 
If I were troubleshooting a slow internet connection, these graphs would quickly show whether the WAN interface was saturated or whether another 
interface was carrying unusually heavy traffic. 
Step 5 Review the Sensors
After discovery completed, I spent some time reviewing each sensor. 
The Ping sensor immediately caught my attention because it serves as the foundation for everything else. 
If Ping fails, every other sensor eventually begins reporting errors because the monitoring server can no longer communicate with the firewall. 
The CPU Usage sensor reports processor utilisation over time. 
A sudden spike might indicate: 
• 
unusually heavy VPN traffic 
• 
denial-of-service attacks 
• 
excessive logging 
• 
overloaded firewall policies 
The Memory Usage sensor provides similar insight. 
If available memory steadily decreases over days or weeks without recovering, it could indicate a memory leak or another software issue requiring 
investigation. 
I also noticed that PRTG created several Disk Free sensors. 
Because the WatchGuard operating system is Linux-based, each filesystem mount point appears separately. 
Some of these partitions are system partitions that rarely change. 
Monitoring every one of them would consume unnecessary sensors without providing meaningful operational value. 
To keep my sensor count efficient, I deleted the partitions that weren't useful and retained only those relevant to ongoing monitoring. 
This reminded me that good monitoring isn't about collecting everything—it's about collecting information that helps me make decisions. 
Step 6 Understanding Sensor Priorities
As I explored the sensors, I noticed that PRTG assigns each one a priority rating represented by stars. 
Initially I assumed this was cosmetic. 
After reading the documentation, I realised these priorities influence which sensors receive the greatest visibility throughout the dashboard. 
I decided to prioritise my sensors based on operational importance. 
I assigned five-star priority to: 
• 
Ping 
• 
CPU Usage 
• 
Primary WAN Interface 
• 
Trusted LAN Interface 
These are the sensors I would want to notice immediately if something went wrong. 
Less critical sensors, such as secondary filesystem usage, remained at three stars. 
This small amount of organisation makes the dashboard significantly easier to interpret during an incident. 
Instead of searching through dozens of sensors, the most important information naturally rises to the top. 
Security Best Practices I Learned
This lab reinforced that monitoring systems must be secured just as carefully as the infrastructure they monitor. 
Some of the practices I'll continue following include: 
• 
Using long, unique authentication and privacy passwords. 
• 
Never reusing SNMP credentials across unrelated environments. 
• 
Restricting SNMP access so only the PRTG server can communicate with monitored devices. 
• 
Choosing SHA instead of MD5 whenever authentication options are available. 
• 
Choosing AES encryption instead of DES whenever the hardware supports it. 
• 
Reviewing SNMP users periodically and removing unused accounts. 
• 
Keeping both monitoring software and firewall firmware fully updated. 
These measures reduce the attack surface while still allowing the monitoring platform to perform its job effectively. 
What I Learned From This Section
This lab showed me that monitoring is not just about visibility—it is also about protecting the visibility itself. 
Before this exercise, I viewed SNMP simply as the protocol PRTG used to collect statistics from network devices. After configuring SNMP version 
3, I realised that the protocol has evolved considerably to address the security weaknesses of its earlier versions. 
Perhaps the biggest lesson I took away was the importance of balancing usability with security. SNMP v3 is undeniably more complex than SNMP 
v2c, requiring usernames, authentication methods, encryption settings, and multiple passwords. However, that additional complexity is justified 
when monitoring devices that sit at the edge of the network and protect an organisation's most valuable assets. 
Finally, configuring credentials at the group level demonstrated how thoughtful organisation can simplify administration. As monitoring 
environments grow from a handful of devices to hundreds or even thousands, small design decisions like credential inheritance become significant 
time savers and help maintain consistency across the entire monitoring platform.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/22997a65-104c-4230-8957-0d7e3205ef8a" />

Section 7: NetFlow Traffic Analysis
After configuring SNMP monitoring, I realised there was still one important question I couldn't answer. 
PRTG could tell me whether a device was online. 
It could tell me how busy its CPU was. 
It could tell me how much bandwidth was passing through an interface. 
But it couldn't tell me who was actually using that bandwidth. 
If my internet connection suddenly became saturated, SNMP would show the WAN interface running at 95% utilisation—but it wouldn't tell me 
whether that traffic was caused by a Windows update, a file backup, someone streaming high-definition video, or even malicious activity. 
That's exactly the problem NetFlow solves. 
NetFlow goes beyond simply measuring traffic volumes. It provides visibility into the conversations taking place across the network, allowing me to 
identify which devices are communicating, which applications they're using, how much bandwidth each conversation consumes, and how traffic 
patterns change over time. 
For a network administrator, this level of visibility is incredibly valuable because it transforms troubleshooting from educated guesswork into 
evidence-based investigation. 
Understanding NetFlow
NetFlow was originally developed by Cisco to give administrators detailed insight into network traffic without having to capture every individual 
packet. 
Instead of recording every packet like Wireshark does, NetFlow summarises traffic into flows. 
This makes it much more efficient for continuous monitoring. 
Before configuring it, I familiarised myself with a few important concepts. 
Flow 
A flow represents a conversation between two devices. 
Packets belong to the same flow when they share characteristics such as: 
• 
source IP address 
• 
destination IP address 
• 
protocol 
• 
source port 
• 
destination port 
For example, if my laptop is copying files from my Synology NAS using SMB, every packet involved in that transfer belongs to a single flow. 
Rather than storing thousands of individual packets, NetFlow records a summary of that conversation. 
Flow Export 
Network devices periodically package these flow summaries and send them to a monitoring server. 
This process is called flow export. 
The exporting device could be: 
• 
a router 
• 
a firewall 
• 
a Layer 3 switch 
In my lab, the WatchGuard firewall performs this role. 
Collector 
The monitoring server that receives these exported flow records is called the collector. 
In my environment, the PRTG server acts as the collector. 
It receives every exported flow, stores the information, and converts it into graphs, reports, and dashboards that are much easier to interpret than raw 
network statistics. 
Top Talkers 
One of my favourite NetFlow features is Top Talkers. 
This simply answers the question: 
Which devices are consuming the most bandwidth? 
If one workstation suddenly starts transferring hundreds of gigabytes, I can identify it almost immediately. 
Top Protocols 
NetFlow can also categorise traffic by protocol. 
Instead of just knowing that 500 Mbps is crossing the WAN connection, I can see whether that traffic consists primarily of: 
• 
HTTPS 
• 
SMB 
• 
DNS 
• 
FTP 
• 
SSH 
• 
VPN traffic 
or any other recognised protocol. 
This provides valuable context during troubleshooting. 
Why NetFlow Version 9? 
Page 29 of 52 
PRTG Network Monitoring  |  Home Lab Project Documentation 
There are several versions of NetFlow. 
For this lab, I used NetFlow Version 9. 
Compared to Version 5, Version 9 introduces several improvements, including: 
• 
IPv6 support 
• 
extensible templates 
• 
support for additional traffic information 
• 
improved compatibility with modern devices 
Most current enterprise equipment supports Version 9, although some older hardware may still require Version 5. 
Step 1 Configure NetFlow on the WatchGuard Firewall
The first step was enabling NetFlow on the firewall itself. 
Inside the WatchGuard management interface, I navigated to: 
System → NetFlow 
From there, I enabled NetFlow and selected Version 9. 
I then configured the collector settings. 
Setting 
NetFlow Version 
Collector Address 
Collector Port 
Active Flow Timeout 
Value 
9 
IP address of my PRTG server 
UDP 9996 
1 minute 
The collector address tells the firewall exactly where to send its flow exports. 
The collector port must match the listening port that PRTG will use later. 
I also configured the traffic that should be exported. 
I enabled monitoring for: 
• 
traffic generated by the firewall itself 
• 
traffic destined for the firewall 
Although these flows represent management traffic rather than user traffic, they can provide useful security information if unusual activity occurs. 
Finally, I selected the interfaces that should export flow information. 
For this lab, I chose the external WAN interface because I wanted visibility into internet traffic entering and leaving the network. 
I enabled both: 
• 
Ingress (incoming traffic) 
• 
Egress (outgoing traffic) 
before saving the configuration. 
�
�
Reflection 
One thing I found interesting is that enabling NetFlow doesn't noticeably affect normal firewall operation. 
The firewall continues forwarding packets exactly as before—it simply exports metadata describing those packets in the background. 
Step 2 Add the NetFlow Sensor in PRTG
With the firewall exporting flow information, I switched back to PRTG. 
Inside the DMT-FW-01 device, I selected: 
Add Sensor 
Searching for NetFlow presented several options. 
Since the firewall was exporting Version 9, I selected: 
NetFlow v9 
The sensor configuration required only a few settings. 
Setting 
UDP Listening Port 
Sender IP 
Receiving Interface 
Flow Timeout 
Value 
9996 
WatchGuard Firewall 
PRTG Server LAN Interface 
1 minute 
Matching these values with the firewall configuration is essential. 
If either the UDP port or sender IP address is incorrect, PRTG simply won't receive any flow information. 
For the initial setup, I left all filtering disabled. 
I wanted to collect every flow so I could understand the overall traffic profile of my network before narrowing my focus. 
After clicking Create, the sensor entered a waiting state while it waited for the first exported flows. 
Step 3 Verify That NetFlow Is Working
After roughly one minute, the sensor began receiving data. 
Watching it populate for the first time was genuinely satisfying because it was immediately obvious how much more information NetFlow provides 
compared to SNMP alone. 
Several useful views became available. 
Top Protocols
The first page showed the protocols consuming bandwidth. 
Page 30 of 52 
PRTG Network Monitoring  |  Home Lab Project Documentation 
In my lab environment, the majority of traffic consisted of: 
• 
HTTPS 
• 
HTTP 
• 
DNS 
This immediately made sense because most of the activity in my lab involved web browsing and software updates. 
Instead of simply seeing bandwidth usage, I now understood what was generating that bandwidth. 
Top Talkers 
The Top Talkers view ranked devices according to bandwidth consumption. 
Rather than manually investigating each workstation, PRTG automatically identified the busiest systems. 
This would be particularly useful if someone reported poor internet performance. 
Instead of asking every user what they were doing, I could immediately identify the largest consumers. 
Top Connections 
Another useful section displayed individual conversations. 
For every active flow, I could see: 
• 
source IP address 
• 
destination IP address 
• 
protocol 
• 
bandwidth usage 
This effectively creates a live summary of network communication across the monitored interface. 
Live Data 
Finally, I explored the Live Data graph. 
Unlike SNMP graphs, which simply show interface utilisation, the NetFlow graphs break traffic down by protocol and update continuously. 
Watching different protocols rise and fall throughout the day gave me a much clearer picture of normal network behaviour. 
That baseline becomes extremely valuable because unusual traffic patterns become immediately obvious. 
A Practical Troubleshooting Scenario
While exploring the graphs, I imagined a realistic support request. 
A user phones the IT department and says: 
"The internet is really slow today." 
Without NetFlow, I might begin checking: 
• 
firewall CPU 
• 
interface utilisation 
• 
ISP connectivity 
• 
switch performance 
Eventually I might discover the cause. 
With NetFlow, I can often answer the question in less than a minute. 
If one workstation is downloading a 200 GB backup to cloud storage, it immediately appears at the top of the Top Talkers list. 
If most WAN bandwidth is being consumed by HTTPS traffic, I can drill down even further to identify the specific conversations responsible. 
Rather than troubleshooting blindly, I'm following objective evidence. 
That's one of the biggest strengths of flow analysis. 
NetFlow Best Practices
While working through this lab, I noted several practices that would be important in a production environment. 
Choose an appropriate export interval. 
I configured an active flow timeout of one minute. 
This provides near real-time visibility without overwhelming the collector with unnecessary updates. 
Exporting every thirty seconds produces more detailed data but also doubles the volume of exported flow records. 
Monitor the correct interfaces. 
For internet analysis, exporting flows from the WAN interface makes the most sense. 
If my objective were analysing communication between internal departments, monitoring the uplinks on a core Layer 3 switch would provide more 
useful information. 
Selecting the correct observation point is just as important as enabling NetFlow itself. 
Protect flow data. 
Traditional NetFlow exports are not encrypted. 
Although the flow summaries do not contain packet payloads, they still reveal valuable information about communication patterns within the 
network. 
For that reason, flow exports should remain on trusted internal networks and should never be transmitted across the public internet without 
additional protection. 
Review trends regularly. 
NetFlow is most valuable when used proactively. 

PRTG Network Monitoring  |  Home Lab Project Documentation 
Rather than waiting for performance complaints, reviewing Top Talkers and protocol distributions each week helps establish a baseline of normal 
behaviour. 
Once that baseline is understood, unusual traffic becomes much easier to recognise. 
What I Learned From This Section
Before completing this lab, I thought monitoring bandwidth simply meant measuring how much traffic was flowing across an interface. NetFlow 
showed me that bandwidth alone tells only part of the story. 
By exporting flow information to PRTG, I gained visibility into the conversations happening across my network rather than just the volume of 
traffic they generated. I could identify which devices were consuming the most bandwidth, which applications were responsible, and how those 
patterns changed throughout the day. That additional context transforms troubleshooting from reactive investigation into informed analysis. 
Perhaps the biggest lesson I took away is that SNMP and NetFlow complement one another rather than compete. SNMP tells me how busy a device 
or interface is, while NetFlow explains why it is busy. Together, they provide a much more complete picture of network health, enabling me to 
detect performance issues, investigate bandwidth consumption, and make evidence-based decisions far more quickly than either technology could on 
its own.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/e96c3485-030b-4559-be7b-818384a0d748" />
Section 8: Monitoring Windows Servers with WMI and SNMP 
Monitoring Windows servers is where PRTG truly demonstrates its value. Unlike network devices such as switches, routers, and firewalls that are 
primarily monitored through SNMP, Windows servers provide multiple methods of exposing performance and health information. The two most 
common are Windows Management Instrumentation (WMI) and Simple Network Management Protocol (SNMP). 
Each serves a different purpose. 
WMI is Microsoft's native management framework and provides deep insight into the operating system itself. Through WMI, PRTG can monitor 
Windows services, Active Directory, Event Logs, Windows Update status, memory utilisation, disk performance, page file usage, and many other 
operating system components that simply aren't available through SNMP. 
SNMP, on the other hand, is platform independent. It works on almost every network device regardless of manufacturer and provides standard 
hardware statistics such as uptime, CPU usage, interface traffic and memory utilisation. While it isn't as detailed as WMI on Windows systems, it 
provides an excellent secondary monitoring method and allows Windows servers to be monitored in exactly the same way as routers, switches and 
storage appliances. 
Rather than choosing one or the other, I decided to configure both. WMI provides rich operating system monitoring, while SNMP gives me a 
vendor-neutral backup source of information. If one monitoring method experiences issues, the other continues collecting valuable data. 
Understanding WMI vs SNMP 
Before configuring the server, it's worth understanding when each protocol is most appropriate. 
Feature 
Authentication 
Installation 
Level of Detail 
Windows Services 
Event Logs 
Windows Updates 
CPU & Memory 
Network Traffic 
Best Use 
The key takeaway is simple: 
• 
WMI 
Windows or Active Directory credentials 
Built into Windows 
Extremely detailed 
Yes 
Yes 
Yes 
Yes 
Yes 
Windows Servers 
Use WMI whenever monitoring Windows-specific functionality. 
• 
SNMP 
Community String (v2c) or Username/Password (v3) 
Requires SNMP feature installation 
General system statistics 
No 
No 
No 
Yes 
Yes 
Network Devices & Baseline Monitoring 
Use SNMP for standard hardware metrics or when monitoring devices that don't support WMI. 
In production environments, most administrators use WMI as the primary monitoring method for Windows servers and SNMP as a supplementary 
source of information. 
Step 1 Add the Windows Server to PRTG 
Open Devices. 
With the monitoring groups already created in Section 3, I begin by adding my Active Directory server. 
1. 
2. 
Navigate to the Servers group. 
3. 
Right-click Servers and choose Add Device. 
I complete the configuration as follows. 
Device Name 
DMT-AD-01 
My naming convention follows a simple structure: 
• 
DMT – organisation abbreviation 
• 
AD – device role (Active Directory) 
• 
01 – first server of this type 
Using a consistent naming convention becomes increasingly important as environments grow. In organisations with hundreds or thousands of 
monitored devices, clear naming makes searching, reporting and troubleshooting significantly easier. 
IPv4 Address 
192.168.1.222 
(or whichever static IP address has been assigned to the server) 
Because monitoring systems rely on predictable connectivity, servers should always use static IP addresses. 
Device Icon 
PRTG includes several Windows Server icons. 
Although purely cosmetic, assigning the correct icon makes the device tree much easier to scan visually. 
Configure Windows Credentials 
Scrolling further down the device settings, I reach the Credentials for Windows Systems section. 
By default, devices inherit credentials from their parent group. 
For this server I disable inheritance because I want to specify the exact credentials PRTG should use. 
Domain: 
corp.danieltraining.com 
Username: 
Page 34 of 52 
PRTG Network Monitoring  |  Home Lab Project Documentation 
administrator 
Password: 
******** 
Once entered, I click OK. 
Why does PRTG need Windows credentials? 
Unlike SNMP, WMI performs authenticated remote management. 
When PRTG queries CPU utilisation or Windows Update status, it is effectively asking Windows itself for that information. 
Windows therefore requires an account with sufficient permissions. 
For this home lab I use the built-in Administrator account because it already has the required privileges. 
In a production environment this would not be considered best practice. 
Instead, I would create a dedicated service account such as: 
svc_prtg 
and grant it only the permissions necessary for monitoring. 
This follows the Principle of Least Privilege, reducing the impact should the account ever become compromised. 
Step 2 Add a WMI CPU Sensor 
Select DMT-AD-01 
With the device added, I begin monitoring the server's processor utilisation. 
1. 
2. 
Click Add Sensor 
3. 
Filter by: 
Target System: 
Windows 
Technology: 
WMI 
4. 
Select: 
WMI CPU Load 
5. 
Click Create 
Within approximately one polling interval (60 seconds by default), the sensor begins displaying processor utilisation. 
Internally, PRTG queries the Windows Win32_Processor WMI class and retrieves the current CPU usage. 
The sensor displays: 
• 
Current CPU percentage 
• 
Historical graphs 
• 
Minimum and maximum values 
• 
Average utilisation 
• 
Sensor availability 
Over time this graph becomes extremely valuable. 
If users report the server was slow yesterday afternoon, I can immediately review the CPU graph for that exact period instead of relying on 
guesswork. 
Step 3 Use Recommended Sensors 
One feature I particularly appreciate in PRTG is Recommended Sensors. 
Rather than manually searching through hundreds of available sensors, PRTG analyses the device and suggests the most appropriate monitoring 
options. 
I select: 
Add Sensor 
↓ 
Recommended Sensors 
PRTG performs a quick scan of the server before presenting a recommended list. 
For my Active Directory server the suggestions include: 
• 
Ping 
• 
WMI Memory 
• 
WMI Disk Free 
• 
WMI Page File 
• 
WMI Uptime 
• 
WMI Network Adapter 
• 
WMI Physical Disk 
• 
DNS (WMI) 
• 
Windows Update Status 
I simply tick each recommended sensor and click: 
Add Selected 
Within a few minutes the server begins building a complete health profile. 
Why each sensor matters 
Ping 
Page 35 of 52 
PRTG Network Monitoring  |  Home Lab Project Documentation 
The most basic health check. 
If Ping fails, every other monitoring protocol will almost certainly fail too. 
It is usually the very first indication that something has gone wrong. 
WMI Memory 
Memory shortages often cause performance problems long before CPU becomes overloaded. 
As RAM fills, Windows begins moving memory pages to disk. 
This process—known as paging—is dramatically slower than physical memory and causes applications to respond sluggishly. 
I typically configure: 
Warning: 
85% 
Critical: 
95% 
WMI Disk Free 
Windows requires free space for: 
• 
updates 
• 
temporary files 
• 
page file growth 
• 
event logs 
• 
Active Directory database operations 
Allowing the system drive to become full can render the server unstable. 
Typical thresholds are: 
Warning: 
20% free 
Critical: 
10% free 
WMI Page File 
This sensor reveals whether Windows is relying heavily on virtual memory. 
High page file usage combined with high RAM utilisation usually indicates that additional physical memory is required. 
WMI Physical Disk 
Rather than measuring capacity, this sensor measures storage performance. 
Metrics include: 
• 
Read latency 
• 
Write latency 
• 
Queue length 
• 
Disk utilisation 
High values may indicate: 
• 
overloaded storage 
• 
failing disks 
• 
excessive application activity 
WMI Network Adapter 
Monitors: 
• 
throughput 
• 
errors 
• 
discarded packets 
These values help identify network congestion or failing network adapters. 
DNS Sensor 
Because this server hosts DNS, monitoring the service itself is essential. 
If DNS fails: 
• 
users cannot resolve hostnames 
• 
Active Directory authentication begins failing 
• 
many applications stop functioning 
A healthy server with a failed DNS service is still a major outage. 
Windows Update Status 
This is arguably one of the most valuable sensors from a security perspective. 
PRTG continuously checks whether important Windows updates remain uninstalled. 
Missing updates often mean missing security patches. 
Instead of relying on monthly manual audits, PRTG automatically alerts me whenever update compliance drops below my desired standard. 
Step 4 Install the SNMP Service 
Although WMI provides excellent monitoring, I also want SNMP enabled. 
Unlike routers and switches, Windows does not install SNMP by default. 
Page 36 of 52 
PRTG Network Monitoring  |  Home Lab Project Documentation 
On the server: 
Server Manager 
↓ 
Manage 
↓ 
Add Roles and Features 
Navigate to: 
Features 
↓ 
SNMP Service 
Tick the feature. 
Click: 
Install 
After installation completes, open: 
services.msc 
Locate: 
SNMP Service 
Open Properties. 
Configure the Agent Tab 
The Agent tab allows optional documentation. 
Contact 
lab-admin@corp.local 
Location 
Home Lab – Primary Server 
While these values have no impact on monitoring, they provide useful inventory information. 
Configure the Traps Tab 
Although PRTG primarily polls devices, Windows can also send SNMP traps. 
Community: 
danmilltraining 
Trap Destination: 
192.168.1.100 
(the PRTG server) 
Configure the Security Tab 
Under Accepted Community Names: 
Community 
danmilltraining 
Rights 
READ ONLY 
Read-only access ensures PRTG cannot modify server configuration through SNMP. 
Finally, restrict access by selecting: 
Accept SNMP packets from these hosts 
Add only: 
192.168.1.100 
This prevents any other device from querying the server via SNMP. 
Step 5 Configure SNMP Credentials in PRTG 
Returning to PRTG: 
Open: 
DMT-AD-01 
↓ 
Edit 
Navigate to: 
Credentials for SNMP Devices 
Disable inheritance. 
Configure: 
Version: 
Page 37 of 52 
PRTG Network Monitoring  |  Home Lab Project Documentation 
SNMP v2c 
Community: 
danmilltraining 
Click OK. 
Step 6 Add an SNMP System Uptime Sensor 
Next I add an additional sensor. 
Add Sensor 
↓ 
SNMP System Uptime 
Create the sensor. 
Within the next polling cycle PRTG begins reporting the server's uptime independently of WMI. 
This provides a second verification source. 
If the server unexpectedly restarts overnight, the uptime immediately drops and can trigger an alert. 
Many administrators configure notifications whenever uptime falls below two hours outside scheduled maintenance windows, allowing them to 
detect unplanned reboots almost immediately. 
Step 7 Understanding the Most Important Windows Sensors 
Sensor 
Ping 
Why It Matters 
Confirms the server is reachable. Usually the first indication of an outage. 
WMI CPU Load 
WMI Memory 
WMI Disk Free 
WMI Physical Disk 
Sustained utilisation above 80–85% indicates the processor may be overloaded. 
Detects RAM exhaustion before applications begin slowing dramatically. 
Prevents operating system instability caused by full system drives. 
Reveals storage bottlenecks through latency and queue length measurements. 
WMI Network Adapter 
WMI Page File 
Identifies packet loss, interface errors and abnormal network utilisation. 
Highlights excessive paging, usually indicating insufficient physical memory. 
WMI Windows Update 
WMI Uptime 
Ensures security updates remain current and compliance is maintained. 
Detects unexpected server reboots and confirms system stability over time. 
SNMP System Uptime 
Independent validation of uptime using SNMP, providing redundancy if WMI becomes unavailable. 
What I Learned From This Section 
This lab reinforced an important lesson: effective server monitoring goes far beyond simply checking whether a machine is online. A server can 
respond to pings while still experiencing high CPU usage, memory exhaustion, failing disks, stalled services, or missing security updates. By 
combining WMI's deep visibility into the Windows operating system with SNMP's standardised hardware monitoring, I built a more complete and 
resilient monitoring solution. I also learned the importance of using dedicated service accounts, restricting SNMP access to trusted hosts, and setting 
meaningful alert thresholds. These practices help transform monitoring from a reactive process into a proactive one, allowing issues to be identified 
and addressed before they impact users.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/26073b6c-29ad-45a4-8baa-23c2f5c83266" />
Section 9: Monitoring Windows Server Roles and Services 
Up to this point in the lab, I've focused on monitoring the health of the server itself—its CPU utilisation, memory consumption, disk capacity, 
network traffic, and overall availability. Those metrics are essential because they tell me whether the operating system is functioning correctly. 
However, a healthy operating system does not necessarily mean the services running on that operating system are healthy. 
This is where many new administrators make a critical mistake. 
They build monitoring that tells them whether a server is alive, but not whether the business applications hosted on that server are actually working. 
In most organisations, users don't care whether a server's CPU is sitting at 18% utilisation. 
They care whether they can: 
• 
log into their computers, 
• 
access shared folders, 
• 
browse company websites, 
• 
send emails, 
• 
connect to databases, 
• 
or authenticate to business applications. 
Those capabilities are provided by server roles and services, not by the operating system itself. 
That's why monitoring server roles is just as important—if not more important—than monitoring hardware performance. 
Why Role Monitoring Matters 
To understand why, consider a real-world scenario. 
Imagine it's 8:47 on Monday morning. 
Your company's domain controller is running perfectly. 
PRTG shows: 
• 
CPU usage: 12% 
• 
Memory usage: 48% 
• 
Disk space: 74% free 
• 
Network adapter: Healthy 
• 
Ping: Successful 
Every infrastructure metric is green. 
Yet something catastrophic has happened. 
The Active Directory Domain Services (AD DS) service has unexpectedly crashed. 
Within minutes: 
• 
Employees arrive at work. 
• 
They attempt to log in. 
• 
Windows cannot authenticate their credentials. 
• 
Group Policy no longer applies. 
• 
File shares become inaccessible. 
• 
Applications that depend on Active Directory stop working. 
• 
Helpdesk phones begin ringing continuously. 
From the operating system's perspective, nothing appears wrong. 
From the users' perspective, the entire organisation is offline. 
Without service monitoring, IT discovers the outage only after users report it. 
With service monitoring, PRTG detects the failure the moment the service stops and immediately sends an alert. 
Instead of learning about the problem from frustrated users, the engineer learns about it directly from the monitoring platform. 
That difference is what separates reactive IT from proactive IT. 
Understanding Windows Server Roles 
Windows Server is modular. 
Instead of installing one large operating system with every feature enabled, administrators install only the server roles they need. 
Some of the most common roles include: 
Server Role 
Active Directory Domain Services (AD DS) 
DNS Server 
DHCP Server 
File Services 
Print Services 
IIS (Internet Information Services) 
Hyper-V 
Certificate Services 
Remote Desktop Services 
Windows Deployment Services 
Purpose 
Authenticates users and computers 
Resolves computer names into IP addresses 
Automatically assigns IP addresses 
Provides network file shares 
Hosts shared printers 
Hosts websites and web applications 
Runs virtual machines 
Issues digital certificates 
Provides remote application access 
Deploys Windows operating systems 
Each role consists of one or more Windows services. 
If those services stop running, the role stops functioning—even if Windows itself continues operating normally. 
PRTG can monitor each of these services individually. 
Page 40 of 52 
PRTG Network Monitoring  |  Home Lab Project Documentation 
Step 1 Add an LDAP Sensor for Active Directory 
Because my server is functioning as a Domain Controller, the first role I want to monitor is Active Directory. 
PRTG provides an LDAP Directory Services sensor specifically for this purpose. 
Rather than simply checking whether TCP port 389 is open, the sensor performs an actual LDAP query against the directory. 
This verifies that Active Directory is functioning correctly and capable of responding to authentication requests. 
To add the sensor: 
1. 
Select DMT-AD-01. 
2. 
Click Add Sensor. 
3. 
Search for: 
LDAP 
4. 
Select: 
LDAP Directory Services 
5. 
Click Add. 
Configure the Sensor 
Complete the configuration using the details from your domain. 
Distinguished Name 
DC=corp,DC=danieltraining,DC=com 
The Distinguished Name (DN) identifies the root of the Active Directory directory tree. 
Think of it as the starting point where LDAP begins searching. 
Username 
administrator@corp.danieltraining.com 
Password 
Enter the domain administrator password. 
Port 
389 
Port 389 is the default LDAP port. 
If the environment uses secure LDAP (LDAPS), port 636 would be used instead. 
Timeout 
60 seconds 
Finally, click Create. 
What Happens During Polling? 
Every polling interval (typically once every minute), PRTG performs an LDAP query against Active Directory. 
The sensor verifies that: 
• 
LDAP is accepting connections. 
• 
The supplied credentials authenticate successfully. 
• 
Active Directory responds correctly. 
• 
Directory queries complete within the configured timeout. 
If any of these steps fail, the sensor immediately changes to a warning or error state. 
This provides much greater confidence than simply checking whether the server responds to ping. 
Step 2 Monitor Active Directory Replication 
Most enterprise environments do not rely on a single domain controller. 
Instead, they deploy multiple domain controllers across different buildings, offices, or geographic regions. 
All of those controllers must continuously replicate information between one another. 
Whenever a user: 
• 
changes a password, 
• 
creates a new user, 
• 
joins a computer to the domain, 
• 
or updates a Group Policy, 
those changes must replicate successfully. 
If replication fails, different domain controllers begin holding different information. 
This can lead to confusing problems such as: 
• 
users can log in from one office but not another, 
• 
password changes only work intermittently, 
• 
Group Policy updates never apply, 
• 
new accounts disappear, 
• 
DNS records become inconsistent. 
These issues can remain hidden for weeks before anyone notices. 
Monitoring replication prevents that. 
To configure replication monitoring: 
1. 
Click Add Sensor. 
Page 41 of 52 
PRTG Network Monitoring  |  Home Lab Project Documentation 
2. 
Search: 
Active Directory 
3. 
Select: 
WMI Active Directory Replication Errors 
4. 
Click Create. 
In my home lab, which contains only one omain controller, this sensor has little activity because there are no replication partners. 
However, in production environments with multiple domain controllers, this is one of the most valuable sensors available. 
Step 3 Monitor Critical Windows Services 
Roles are built from Windows services. 
PRTG allows individual monitoring of those services. 
This means I can receive an alert the instant an important service stops. 
To add service monitoring: 
1. 
Click Add Sensor. 
2. 
Search: 
Windows Service 
3. 
Select: 
WMI Service Monitor 
After connecting to the server, PRTG retrieves a complete list of installed services. 
From that list, I select the services that are essential to my environment. 
DNS Server 
The DNS service converts hostnames into IP addresses. 
Without DNS: 
• 
websites fail, 
• 
applications cannot locate servers, 
• 
Active Directory begins experiencing authentication problems. 
Even though the server remains online, users perceive the network as unavailable. 
Netlogon 
Netlogon handles communication between computers and domain controllers. 
It is responsible for: 
• 
authenticating users, 
• 
locating domain controllers, 
• 
establishing secure channels, 
• 
processing domain logons. 
If Netlogon stops, users cannot authenticate successfully. 
Windows Time (W32Time) 
Time synchronisation is far more important than many people realise. 
Kerberos—the authentication protocol used by Active Directory—requires clocks to remain closely synchronised. 
Even a five-minute difference between systems can cause authentication failures. 
Monitoring the Windows Time service helps prevent these subtle but serious issues. 
DFS Replication (DFSR) 
The DFS Replication service keeps the SYSVOL folder synchronised between domain controllers. 
SYSVOL contains: 
• 
Group Policies 
• 
login scripts 
• 
domain-wide configuration files 
If replication stops, different domain controllers may deliver different Group Policies to users. 
After selecting these services, click Create. 
Each service becomes an independent sensor. 
The sensors report either: 
• 
Running 
• 
Stopped 
• 
Warning 
• 
Error 
If one service stops unexpectedly, PRTG immediately generates an alert identifying: 
• 
the affected server, 
• 
the exact service, 
• 
the time the failure occurred. 
Rather than investigating an entire server, the engineer immediately knows where to begin troubleshooting. 
Page 42 of 52 
PRTG Network Monitoring  |  Home Lab Project Documentation 
Step 4 Explore Other Role-Specific Sensors 
One aspect of PRTG I particularly appreciate is the enormous range of built-in sensors. 
As I browse the sensor library, I discover specialised monitoring options for many common Microsoft workloads. 
DNS Sensor 
Rather than simply checking whether the DNS service is running, this sensor performs actual DNS lookups. 
It can verify: 
• 
forward lookups, 
• 
reverse lookups, 
• 
response times, 
• 
specific DNS records. 
This confirms DNS is functioning correctly from an end-user perspective. 
Microsoft Exchange Sensors 
For organisations running on-premises Exchange, PRTG includes dedicated sensors that monitor: 
• 
mailbox databases, 
• 
database mount status, 
• 
replication health, 
• 
mail queues, 
• 
Outlook Web Access, 
• 
transport services. 
IIS Web Server Sensors 
If the server hosts websites, IIS sensors monitor: 
• 
HTTP response codes, 
• 
page response times, 
• 
concurrent connections, 
• 
application pools, 
• 
website availability. 
This allows administrators to detect web application issues long before customers begin reporting them. 
Microsoft SQL Server 
Database servers are often business-critical. 
PRTG can monitor: 
• 
database availability, 
• 
backup status, 
• 
replication, 
• 
query performance, 
• 
transaction log growth, 
• 
storage utilisation. 
Hyper-V 
For virtualisation hosts, PRTG provides sensors covering: 
• 
virtual machine health, 
• 
replication status, 
• 
checkpoint age, 
• 
CPU allocation, 
• 
memory allocation, 
• 
RDP 
storage usage. 
This sensor verifies that Remote Desktop connections can actually be established. 
Simply responding to ping does not guarantee administrators can remotely access the server. 
RADIUS 
Authentication servers can be monitored by measuring: 
• 
authentication response times, 
• 
request failures, 
• 
server availability. 
This is especially valuable in wireless and VPN environments. 
Step 5 Creating Custom Monitoring with PowerShell 
Eventually every administrator encounters something that no built-in sensor can monitor. 
Fortunately, PRTG allows custom PowerShell sensors. 
This is one of its most powerful capabilities. 
Page 43 of 52 
PRTG Network Monitoring  |  Home Lab Project Documentation 
The workflow is straightforward: 
1. 
Write a PowerShell script. 
2. 
The script checks something specific. 
3. 
Return an exit value. 
4. 
PRTG reads that result. 
5. 
Generate graphs and alerts automatically. 
For example, a script could verify: 
• 
Hyper-V replication status 
• 
VPN tunnel availability 
• 
Azure AD synchronisation 
• 
Microsoft 365 connectivity 
• 
SharePoint availability 
• 
SQL backup completion 
• 
Application log errors 
• 
Disk encryption status 
• 
Certificate expiry 
• 
Third-party application health 
If PowerShell can retrieve the information, PRTG can usually monitor it. 
A simple script might return: 
• 
0 = Healthy 
• 
1 = Warning 
• 
2 = Critical 
PRTG then treats the result exactly like any other built-in sensor, complete with graphs, historical reports, and alert notifications. 
This flexibility means the monitoring platform can grow alongside the organisation instead of being limited to predefined sensor types. 
What I Learned From This Section 
This lab fundamentally changed the way I think about server monitoring. I realised that monitoring hardware resources alone is only one part of 
maintaining a healthy environment. A server can have low CPU usage, plenty of free memory, and ample disk space while still failing to deliver the 
services users depend on every day. By monitoring Active Directory, DNS, Windows services, replication health, and other server roles, I shifted 
from monitoring infrastructure to monitoring business functionality. I also discovered how powerful PRTG's specialised sensors and custom 
PowerShell integration are, allowing virtually any operational check to become an automated health monitor. This proactive approach helps detect 
problems at the service level—often before users experience any disruption—making monitoring a tool for preventing outages rather than simply 
reacting to them.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/3b789231-4d2d-4fa2-8dc1-775dbafabff8" />
Section 10: Maps, Reports, Ticketing, and Backup 
By this point in the lab, my monitoring environment is fully operational. PRTG is collecting data from network switches, Windows servers, the 
firewall, and the NAS. I can already see graphs, receive alerts, and monitor the health of my infrastructure. 
However, simply collecting data isn't enough. A monitoring platform becomes genuinely valuable when it helps IT teams quickly understand what is 
happening, communicate that information to others, respond to incidents efficiently, and recover if something goes wrong. 
This final section explores four features that transform PRTG from a collection of sensors into a complete monitoring platform: 
• 
Network Maps for visual monitoring 
• 
Reports for documentation and long-term planning 
• 
Ticketing for incident management 
• 
Configuration Backups for disaster recovery 
These capabilities are used every day by network operations centres (NOCs), managed service providers (MSPs), and enterprise IT departments 
because they improve visibility, accountability, and resilience. 
Network Maps 
One of the first things I noticed after adding multiple devices was that the Devices tree gradually became larger and more difficult to visualise 
mentally. 
Although the device hierarchy is organised, it doesn't immediately answer questions like: 
• 
Which switch connects to which firewall? 
• 
Which server is connected to which network segment? 
• 
Which device is actually failing? 
This is where PRTG Maps become incredibly valuable. 
A network map is a live dashboard that combines monitoring information with a visual representation of the network. 
Instead of scrolling through hundreds of sensors, I can look at a diagram where every device updates its status automatically. 
Green means healthy. 
Yellow means warning. 
Red means something requires immediate attention. 
Because the colours update in real time, I can identify a problem within seconds without opening individual sensor pages. 
Large organisations often display these dashboards continuously on large wall-mounted monitors inside Network Operations Centres so engineers 
can immediately notice when something changes. 
Creating My First Map 
From the PRTG web interface: 
1. 
Click Maps. 
2. 
Select Add Map. 
3. 
Name the map: 
Home Lab Network Topology 
4. 
Open the Map Designer. 
The editor works using drag-and-drop components. 
I begin dragging my monitored devices onto the canvas. 
My layout roughly follows the physical network: 
Internet 
│ 
WatchGuard Firewall 
│ ----------------------- 
│                     
Cisco Switch         
│ 
Core Switch 
│                     
│ 
Windows Server        
Synology NAS 
Once a monitored device is added, its status becomes dynamic. 
I don't need to manually update colours or labels. 
If the firewall goes offline, it immediately changes from green to red. 
If CPU utilisation exceeds a threshold, the icon changes colour automatically. 
This makes the map much more than a static diagram—it becomes a live operational dashboard. 
Customising the Dashboard 
PRTG maps are highly customisable. 
Besides device icons, I can add: 
• 
company logos 
• 
text labels 
• 
bandwidth graphs 
• 
sensor gauges 
• 
live traffic charts 
• 
status tables 
• 
uptime counters 
• 
custom HTML widgets 
Instead of displaying only devices, I can also show key business information. 
Page 46 of 52 
PRTG Network Monitoring  |  Home Lab Project Documentation 
For example: 
Internet Status 
✓
Firewall Online 
✓
VPN Connected 
✓
Domain Controller Healthy 
✓
DNS Available 
Current WAN Usage: 
148 Mbps 
This allows the dashboard to communicate useful operational information to anyone walking into the room, even if they aren't familiar with PRTG. 
Enterprise Use Cases 
In a large organisation, one map is rarely enough. 
Operations teams often build multiple dashboards for different audiences. 
Examples include: 
Site Maps 
London Office 
Sydney Office 
Melbourne Office 
Singapore Office 
Each map focuses on a particular location. 
Department Maps 
• 
Network Infrastructure 
• 
Servers 
• 
Security 
• 
Cloud Services 
Each team can focus only on the equipment they manage. 
Executive Dashboards 
Executives generally don't need interface utilisation graphs. 
Instead, they care about overall availability. 
An executive dashboard might simply display: 
• 
Services Available 
• 
Active Incidents 
• 
SLA Compliance 
• 
Internet Availability 
without exposing low-level technical information. 
Reports 
Real-time monitoring tells me what is happening now. 
Reports tell me what happened yesterday, last month, or last year. 
This historical perspective is one of the biggest advantages of continuously collecting monitoring data. 
Instead of relying on memory, I can produce objective evidence. 
Why Reports Matter 
Imagine someone asks: 
"Has the internet connection really been unstable this month?" 
Without monitoring, the answer is based on opinion. 
With PRTG, I simply generate an availability report. 
It might show: 
Internet Availability 
99.98% 
Downtime 
11 minutes 
Average Latency 
7 ms 
Now the discussion is based on data rather than assumptions. 
Creating a Report 
To generate one: 
1. 
Open Reports. 
Page 47 of 52 
PRTG Network Monitoring  |  Home Lab Project Documentation 
2. 
Select Add Report. 
3. 
Choose a template. 
Examples include: 
• 
Sensor Overview 
• 
Availability Report 
• 
Historic Data 
• 
SLA Report 
Next, I select which devices or sensors should be included. 
For example: 
• 
Firewall 
• 
Core Switch 
• 
Domain Controller 
• 
NAS 
I then choose a reporting period: 
• 
Last 24 Hours 
• 
Last Week 
• 
Last Month 
• 
Last 90 Days 
• 
Custom Range 
Finally, I choose how often the report should run automatically. 
Available schedules include: 
• 
Daily 
• 
Weekly 
• 
Monthly 
PRTG can automatically email the finished PDF or HTML report to administrators without requiring any manual work. 
Practical Uses 
Reports are useful for far more than troubleshooting. 
Availability Reporting 
Many organisations promise customers a certain uptime. 
For example: 
99.9% SLA 
PRTG reports provide evidence that these service level agreements were met. 
Capacity Planning 
Monitoring trends over several months reveals how infrastructure is growing. 
Examples include: 
• 
storage gradually filling 
• 
WAN utilisation increasing 
• 
memory usage steadily rising 
Instead of waiting until a disk becomes full, I can predict when additional capacity will be required. 
Security Auditing 
Historical reports also support compliance reviews. 
Examples include: 
• 
missing Windows updates 
• 
repeated authentication failures 
• 
firewall interface downtime 
This historical record is often required during security audits. 
Ticketing 
Monitoring only becomes useful when someone acts on the information. 
Ticketing provides that operational workflow. 
Instead of simply displaying a red sensor, PRTG records the issue, tracks progress, and documents how it was resolved. 
Built-In Ticketing 
PRTG includes a lightweight ticketing system. 
Whenever a sensor enters a Down state, it can automatically generate a ticket. 
Each ticket records information such as: 
• 
affected device 
• 
sensor name 
• 
alert time 
• 
priority 
• 
current status 
Engineers can then: 
• 
acknowledge the issue 
2 
PRTG Network Monitoring  |  Home Lab Project Documentation 
• 
add notes 
• 
assign ownership 
• 
close the ticket after resolution 
For a home lab, this is more than sufficient. 
Enterprise Integrations 
Most enterprises already use dedicated IT Service Management platforms. 
Rather than replacing them, PRTG integrates with them. 
Popular examples include: 
• 
ServiceNow 
• 
Jira Service Management 
• 
ConnectWise 
• 
HaloPSA 
In these environments, the workflow becomes almost entirely automated. 
For example: 
Firewall Interface Down 
↓ 
PRTG detects failure 
↓ 
Ticket automatically created 
↓ 
Assigned to Network Team 
↓ 
Engineer investigates 
↓ 
Problem resolved 
↓ 
Ticket closed 
No manual intervention is required to notify the helpdesk. 
Configuration Backup Strategy 
Monitoring software becomes mission-critical very quickly. 
Once dozens—or even hundreds—of devices have been configured, rebuilding everything from scratch would take many hours. 
For that reason, protecting the monitoring system itself becomes just as important as monitoring the rest of the infrastructure. 
I use two complementary backup methods. 
1. PRTG Configuration Backup 
This protects the application's configuration. 
It includes: 
• 
devices 
• 
groups 
• 
sensors 
• 
notification templates 
• 
credentials 
• 
user accounts 
• 
monitoring settings 
To create one: 
1. 
Open Setup. 
2. 
Select Administrative Tools. 
3. 
Click Save Configuration to File. 
PRTG generates a ZIP archive containing the current configuration. 
I store copies on both my NAS and cloud storage for redundancy. 
2. Virtual Machine Backup 
A configuration backup alone is not enough. 
If Windows becomes corrupted or the virtual disk fails, restoring only the configuration still requires reinstalling the operating system and PRTG. 
To avoid this, I also back up the complete VMware virtual machine using Veeam Backup & Replication. 
A VM backup captures: 
Page 49 of 52 
PRTG Network Monitoring  |  Home Lab Project Documentation 
• 
Windows Server 
• 
PRTG installation 
• 
historical monitoring data 
• 
application settings 
• 
operating system configuration 
If disaster strikes, I simply restore the VM and resume monitoring with minimal downtime. 
My Backup Schedule 
To minimise the risk of data loss, I follow a layered backup routine: 
Backup 
PRTG Configuration 
Frequency 
Daily and before major changes 
VMware VM Backup 
Every night 
NAS Replication 
Restore Testing 
Continuous 
Quarterly 
Purpose 
Protects monitoring configuration 
Protects the complete monitoring server 
Protects backup storage 
Verifies backups can actually be restored 
This layered approach ensures that no single failure can completely eliminate my monitoring platform. 
Testing the Backups 
Creating backups is only half the process. 
A backup that has never been restored is simply an assumption. 
At least once every quarter, I perform a test recovery by restoring the PRTG configuration to a separate test virtual machine. 
During the test, I confirm that: 
• 
all devices reappear correctly 
• 
sensors resume polling 
• 
notification templates remain intact 
• 
historical data is accessible where applicable 
• 
dashboards and maps load without errors 
Only after a successful restore test can I be confident that the backup strategy will work during a real disaster. 
What I Learned From This Section 
This final section reinforced that effective monitoring extends far beyond collecting performance metrics. Maps transform raw sensor data into an 
intuitive visual dashboard that speeds up fault identification, reports provide historical evidence for troubleshooting and capacity planning, ticketing 
ensures alerts become structured incidents rather than forgotten notifications, and a layered backup strategy protects the monitoring platform itself. 
Perhaps the most important lesson was recognising that a monitoring solution is itself a critical service. If the monitoring server fails during a 
network outage, visibility into every other system is lost at exactly the moment it is needed most. By combining regular configuration backups, full 
virtual machine backups, and routine restore testing, I can ensure that the monitoring environment remains as resilient as the infrastructure it is 
designed to protect.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/4ee7f85a-352b-4675-ba0c-7d7b21d17eac" />








