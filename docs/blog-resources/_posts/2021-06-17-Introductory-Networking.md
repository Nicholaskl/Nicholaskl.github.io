---
layout: post
title:  "Introductory Networking | Try Hack Me"
date:   2021-06-17 13:24:00 +0800
categories: Res THM
---

# Introductory Networking

## Task 2 - The OSI Model: An Overview

### Which layer would choose to send data over TCP or UDP?
4 - Transport

### Which layer checks received packets to make sure that they haven't been corrupted?
2 - Data Link

### In which layer would data be formatted in preparation for transmission?
2 - Data Link

### Which layer transmits and receives data?
1 - Physical

### Which layer encrypts, compresses, or otherwise transforms the initial data to give it a standardised format?
6 - Presentation

### Which layer tracks communications between the host and receiving computers?
5 - Session

### Which layer accepts communication requests from applications?
7 - Application

### Which layer handles logical addressing?
3 - Network

### When sending data over TCP, what would you call the "bite-sized" pieces of data?
Segments

### [Research] Which layer would the FTP protocol communicate with?
7 - Application

### Which transport layer protocol would be best suited to transmit a live video?
UDP


## Task 3 - Encapsulation

### How would you refer to data at layer 2 of the encapsulation process (with the OSI model)?
Frames

### How would you refer to data at layer 4 of the encapsulation process (with the OSI model), if the UDP protocol has been selected?
Datagram

### What process would a computer perform on a received message?
De-Encapsulation

### Which is the only layer of the OSI model to add a trailer during encapsulation?
Data Layer

### Does encapsulation provide an extra layer of security (Aye/Nay)?
Aye


## Task 4- The TCP/IP Model

### Which model was introduced first, OSI or TCP/IP?
TCP/IP

### Which layer of the TCP/IP model covers the functionality of the Transport layer of the OSI model (Full Name)?
Transport

### Which layer of the TCP/IP model covers the functionality of the Session layer of the OSI model (Full Name)?
Application

### The Network Interface layer of the TCP/IP model covers the functionality of two layers in the OSI model. These layers are Data Link, and?.. (Full Name)?
Physical

### Which layer of the TCP/IP model handles the functionality of the OSI network layer?
Internet

### What kind of protocol is TCP?
Connection-Based

### What is SYN short for?
Synchronise

### What is the second step of the three way handshake?
SYN/ACK

### What is the short name for the "Acknowledgement" segment in the three-way handshake?
ACK


## Task 5 - Ping

### What command would you use to ping the bbc.co.uk website?
Go `ping bbc.co.uk`

### Ping muirlandoracle.co.uk - What is the IPv4 address?
Go `ping muirlandoracle.co.uk` which gives `217.160.0.152` as the IPv4 address.

### What switch lets you change the interval of sent ping requests?
`-i`

### What switch would allow you to restrict requests to IPv4?
`-4`

### What switch would give you a more verbose output?
`-v`


## Task 6 - Traceroute

### What switch would you use to specify an interface when using Traceroute?
`-i `

### What switch would you use if you wanted to use TCP SYN requests when tracing the route?
`-T`

### [Lateral Thinking] Which layer of the TCP/IP model will traceroute run on by default (Windows)?
Internet... Network for the OSI model.


## Task 7 - WHOIS

### What is the registrant postal code for facebook.com?
Did `whois facebook.com | grep Postal` which gave us '94025'

### When was the facebook.com domain first registered?
First did `whois facebook.com | grep Date` and then gave us '29/03/1997'

### Which city is the registrant based in?
First did `whois microsoft.com | grep City` which gave us 'Redmond'

### [OSINT] What is the name of the golf course that is near the registrant address for microsoft.com?
First did `whois microsoft.com | Street` which found street name 'One Microsoft Way'.

Looked this up with [Google Maps](https://www.google.com/maps/place/Bellevue+Golf+Course/@47.6474332,-122.14584,14.75z/data=!4m12!1m6!3m5!1s0x54906d71b804151b:0x1a52969ee03899e!2sMicrosoft!8m2!3d47.6422308!4d-122.1369332!3m4!1s0x54906d441ff622d3:0x674fb4f6e6569f39!8m2!3d47.6557944!4d-122.1517643) and found the 'Bellevue Golf Course'

### What is the registered Tech Email for microsoft.com?
First did `whois microsoft.com | grep Email` which gave us 'msnhst@microsoft.com'


## Task 8 - Dig

### What is DNS short for?
Domain Name System

### What is the first type of DNS server your computer would query when you search for a domain?
Recursive 

### What type of DNS server contains records specific to domain extensions (i.e. .com, .co.uk*, etc)*? Use the long version of the name.
Top-Level Domain

### Where is the very first place your computer would look to find the IP address of a domain?
Local Cache

### [Research] Google runs two public DNS servers. One of them can be queried with the IP 8.8.8.8, what is the IP address of the other one?
Looking this up on google givws '8.8.4.4'

### If a DNS query has a TTL of 24 hours, what number would the dig query show?
It would be in seconds. So 24\*60\*60
86400