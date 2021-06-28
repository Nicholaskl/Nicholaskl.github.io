---
layout: post
title:  "Active Directory Basics | Try Hack Me"
date:   2021-06-28 15:01:24 +0800
categories: Res THM
---

# Active Directory basics

## Task 1 - Introduction

Active Directory is the directory service for Windows Domain Networks. 

It is a collection of machines and servers inside of domans, which are part of a collective part of bigger forest of domains. This makes up the Active Directory network.

### What is it exactly?

An AD contains many functioning bits and pieces coming together to make a large network of machines and servers:

* Domain Controllers
* Forests, trees and domains 
* Users and Groups
* Trusts
* Policies
* Domain Services

### Why use it?

Used majorly by large companies. It Allows for the control/monitoring of a number of computers through a single Domain Controller (DC). 

It allows a user to have the ability to sign in to any computer on the AD network as well as have access to any files or folders on the server - this also includes the local storage on that specific machine.

In all, this reduces the setup for machines in a company - the AD does it all.


## Task 2 - Physical Active Directory

The physical AD is the actual servers and machines themsevles. Thse can be DC's or storage servers or domain user machines. Anything the AD needs except for software.

### Domain Controllers

This is a Windows Server that has the Active Directory Domain Services (AD DS) installed on it, and has been promoted to a DC inside its current forest.

These machines are the focal point of the AD - controlling the rest of the domain. It performs the following tasks:

* Holds the AD DS **data store**
* Handles Authentication and authorisation services
* Replicate updates from other DCs in the forest
* Allow admin access - to manage domain resources

### AS DS Data Store

This machine holds the database and any processes that need to be stored to manage directory information - like users, groups, and services.

It contains the 'NTDS.dit' - A database that contains all information about an AD domain controller and any passowrd hashes for domain users.

* This is stored by default in '%SystemRoot%\NTDS'
* Should only accessible by the DC

### Questions

#### What database does the AD DS contain?

ntds.dit

#### Where is the NTDS.dit stored?

%SystemRoot%\NTDS

#### What type of machine can be a domain controller?

windows server


## Task 3 - The Forest

This is what defines the entire AD. It is a container that holds all the network pieces together - without it all other trees and domains wouldn't be able to interact. It is just a way of describing the connection created between trees and domains in the network.

### Overview

In its literal sense, a forest is a collection of one or more domain trees in an AD.

It contains of the following parts:

* Trees
    * Hierarchy of domains in AD Domain Services
* Domains
    * Used to group and manage the objects
* Organisational Units (OUs)
    * Containers for groups, computers, users, printers and other OUs
* Trusts
    * What allows users to access resources in other domains
* Objects
    * Users, groups, printers, computers and shares
* Domain Services
    * DNS Server, LLMNR, IPv6
* Domain Schema
    * The rules for object creation

### Questions

#### What is the term for a hierarchy of domains in a network?

Tree

#### What is the term for the rules for object creation?

Domain Schema

#### What is the term for containers for groups, computers, users, printers, and other OUs?

Organizational Units - American :)


## Task 4 - Users + Groups

The users and groups for an AD are controlled by the sysadmin. By default there are already default groups and two default users: Administrator and Guest.

### Users

Users are the endpoint accounts that determine access. There are four main types of users that are within an AD, however there may be more depending on the companies setup. These four main groups are the following:

* Domain Admins
* Service Accounts
* Local Administrators
* Domain users

#### Domain Admins

This is the big boss of the Active Directory. They have the ability to control any of the domains as well as they are the only user type that has access to the Domain Controller.

#### Service Accounts

These can sometimes be classified as Domain Admins as well.

Mostly these will never be used except in service maintenance circumstances where they are required by Windows for services like SQL for the ability to pair services with service accounts.

#### Local Administraotrs

These types of users have the ability to make changes to some local machines at an administrator level, they can also have the ability to control other 'normal' users. They are **not** able to access the Domain Controller.

#### Domain Users

These users are the ordinary, everyday users of the network. They have the ability to log into the machines that they have authorisation to acces.

They may even have local admin rights to machines in some cases.

### Groups

These are categories with specified permissions that make managing user permissions easier.

There are two main groups:

* Security Groups
* Distribution Groups

#### Security Groups

These are the types of groups that would specify the permissions that a large amount of users have.

#### Distribution Groups

These are the types of groups that can specify email distribution lists.

For attackers, these groups are must less beneficial but can still be beneficial when trying to enumerate.

### Default Security Groups

There a lot of default security groups that come with the AD. A brief description of them follow:

* **Domain Controllers**
* **Domain Guests**
* **Domain Users**
* **Domain Computers**
    * All the workstations and servers that are joined to the domain
* **Domain Admins**
    * The designated administrators of the domain
* **Enterprise Admins**
    * The designated adminsitrators of the schema
* **DNS Admins**
* **DNS Update Proxy**
    * The DNS clients that are permitted to perform dynamic updated on behalf of other clients. Ie. The DHCP Servers
* **Allowed RODC Password Replication Group**
    * Any members in this gruop have passwords replicated to **all** read-only domain controllers within the domain
* **Group Policy Creator Owners**
    * They have the ability to modify the group policy within the domain
* **Denied RODC Password Replication Group**
    * Cannot have passwords replicated to any read-only domain controllers in the domain
* **Protected Users**
    * Users within this group are afforded additional protections against authentication security threats.
    * http://go.microsoft.com/fwlink/?LinkId=298939
* **Cert Publishers**
* **Read-Only Domain Controllers**
* **Enterprise Read-Only Domain Controllers**
* **Key Admins**
    * Can perform administrative actions on the key objects within the **domain**
* **Enterprise Key Admins**
    * Cam perform administrative actions on the key objects within the **forest**
* **Cloneable Domain Controllers**
* **RAS and IAS Servers**
    * Servers that have remote access properties of users

### Questions

#### Which type of groups specify user permissions?

security groups

#### Which group contains all workstations and servers joined to the domain?

domain computers

#### Which group can publish certificates to the directory?

cert publishers

#### Which user can make changes to a local machine but not to a domain controller?

local administrator

#### Which group has their passwords replicated to read-only domain controllers?

Allowed RODC Password Replication Group


## Task 5 - Trusts + Policies

These are what helps the domains/trees communicate with each other whilst maintaining security inside of the network. They put rules in place:

* how domains in a forest may communicate with each other
* how external forests can interact with a forest
* overall domain rules/policies that a domain must follow

### Domain Trusts

These are mechanisms that are in place to allow users to gain access to other resrouces in the domain. They outline the way in which domains inside a forest can interact with one another.

In some environments these trusts may be able to expand out to external domains and sometimes even forests in some cases.

There are two main types of trusts:
* **Directional**
    * The direction of trusts flows from a trusting domain to another trusting domain
* **Transitive**
    * Trust relationship expands beyond just two domains to include other trusted domains.

When attacking an AD, these trusts determines how forests/domains communicate and send data, so these can be abused to move laterally throughout the network.

### Domain Policies

These are very important as they dictate how the server operates and whatever rules it may or may not follow. They act almost like domain groups except they contain rules, and apply to a domain as a whole.

This includes a very long list of default domain policies, but domain admins can choose to add their own policies if they are not on the domain controller.

### Questions

#### What type of trust flows from a trusting domain to a trusted domain?

Directional

#### What type of trusts expands to include other trusted domains?

Transitive


## Task 6 - Active Directory Domain Services + Authentication

The AD Domain Services form the core functions of an AD network, allowing for management of the domain, security certificates, LDAPs, etc. The controller uses these services to decide what it wants to do and services that it will provide for the domain.

### Domain Services Overview

These are services that are provided by the DC for the rest of the domain.

Some of the default services that are included a Windows DC server are:

* **Lightweight Directory Access Protocol (LDAP)**
    * Provides communication between application and directory services
* **Certificate Services**
    * Allows a DC to create, validate or revoke public key certificates.
* **DNS, LLMNR, NBT-NS**
    * Domain Name services that provide identification for IP hostnames

### Domain Authentication Overview

This is the most crucial part of an AD. There are two main forms of authentication: NTML and Kerberos.

#### Kerberos

This is the default service that is used in Active Directories. Kerberos uses a ticket-granting system allowing users to authenticate and provides users to have the ability to access resources across the domain.

#### NTML

This is the default authentication protocol for Windows systems. It authenticates users by using an encrypted challenge/response between the server and client.

### Questions

#### What type of authentication uses tickets? 

Kerberos

#### What domain service can create, validate, and revoke public key certificates? 

Certificate Services


## Task 7 - AD in the Cloud

The modern methods of AD is using a cloud provider such as Azure AD. Their default settings are more secure than physical AD's but can still be insecure.

### Azure AD

Azure is the middle service between a phyical Active Directory and what the users use to sign in. Because of this, it allows for more secure transactions between domains. This does make lots of Active Directory attacks ineffective.

### Cloud Security Overview

| Windows Server AD  | Azure AD       |
|--------------------|----------------|
| LDAP               | Rest APIs      |
| NTLM               | OAuth/SAML     |
| Kerberos           | OpenID         |
| OU Tree            | Flat Structure |
| Domain and Forests | Tenants        |
| Trusts             | Guests         |

### Questions

#### What is the Azure AD equivalent of LDAP?

Rest APIs

####What is the Azure AD equivalent of Domains and Forests?

Tenants

#### What is the Windows Server AD equivalent of Guests?

Trusts


## Task 8 - Hands-On Lab

### Questions

#### What is the name of the Windows 10 operating system?

First of all `ssh Administrator@10.10.83.4` with password 'password123@'

Change directory to the 'Downloads' folder. `cd Downloads`

Then to load powershell with an execution policy bypassed go `powershell -ep bypass`

Now go `. .\PowerView.ps1` to import the PowerView module

For this question go `Get-NetComputer -fulldata | select operatingsystem`

Got the following output
```
operatingsystem
---------------
Windows Server 2019 Standard
Windows 10 Enterprise Evaluation      
Windows 10 Enterprise Evaluation 
```

So answer is 'Windows 10 Enterprise Evaluation'

#### What is the second "Admin" name?

Using PowerView as above, go `Get-NetUser | select cn`

And got the following output:
```
cn
--
Administrator
Guest
krbtgt
Machine-1
Admin2
Machine-2
SQL Service
POST{P0W3RV13W_FTW}
sshd
```

So 'Admin2' is the second.

#### Which group has a capital "V" in the group name?

`Get-NetLocalGroup | select cn` and then saved the output to a file on kali called 'file'

Used grep to find the group with a 'V' by going `cat file | grep 'V'`.

So answer is 'Hyper-V Administrators'

#### When was the password last set for the SQLService user?

Using the following PowerView command: `Get-NetUser | ?{$_.cn -match 'SQL Service'} | select pwdlastset`

`Get-NetUser` returns the user objects.

Then I filter so only the object of the user that has 'cn matches' what we are looking for is returned.

Then just print out when the password was last set.

Got '5/13/2020 8:26:58 PM'