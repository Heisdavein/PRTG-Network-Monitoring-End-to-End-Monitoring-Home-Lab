# PRTG Network Monitoring Home Lab
Full Documentation Attached as PDF
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

```text
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
