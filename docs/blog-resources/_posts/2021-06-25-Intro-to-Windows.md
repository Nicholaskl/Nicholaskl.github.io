---
layout: post
title:  "Intro To Windows | Try Hack Me"
date:   2021-06-25 16:45:00 +0800
categories: Res THM
---

## Task 1 - A little history

### Questions

#### When was Windows announced?

November 20 1985

#### Which is the latest version of Windows?

Windows 10

#### Which is the latest version of Windows Server?

Windows Server 2019


## Task 2 - Windows file system and permissions explained

There is a critical folder structure on the C drive, such as the following:

1. PerfLogs - Stores the system issues and other reports regarding performance
2. Program Files and Program Files (x86) - Is the location where programs install unless you change their path (Ex: Choosing to install software on D drive)
3. Users - In this folder are stored the users created. It also stores users generated data (Ex: Saving a file on your Desktop)
4. Windows - It's the folder which basically contains the code to run the operating system and some utility tools (we'll talk about them later)

File permissions can be set by admins or priveleged accounts, applying to either users or groups:

1. Full control - allows the user/users/group/groups to set the ownership of the folder, set permission for others, modify, read, write, and execute files.
2. Modify - allows the user/users/group/groups to modify, read, write, and execute files.
3. Read & execute - allows the user/users/group/groups to read and execute files.
4. List folder contents - allows the user/users/group/groups to list the contents (files, subfolders, etc) of a folder.
5. Read - only allows the user/users/group/groups to read files.
6. Write - allows the user/users/group/groups to write data to the specified folder (automatically set when "Modify" right is checked).

Note: You can allow or deny permissions for users or groups.

There is a tool called **'icacls'** which can check files/folder permissions. You can use it to check permissions, or set ownership. The symbols are the following:

I - permission inherited from the parent container

F - full access (full control)

M - Modify right/access

OI - object inherit

IO - inherit only

CI - container inherit

RX - read and execute

AD - append data (add subdirectories)

WD - write data and add files

### Questions

#### In which folder are users profiles stored?
Users


## Task 3 - Understanding the authentication process

Local authentication is done by the Local Security Authority (LSA), a protected subsysytem that maintains the security policies and accounts on a system.

There are two types of active directory:

* On-Premise Active Directory (AD)
* Azure Active Directory (AAD)

### On-Premise Active Directory

These have a record of all users as well as any PCs and servers, and also authenticates any users that are trying to sign in. They also govern what users have, or don't and what they can do and have access to.

These types of servers can have authenticate with the following protocols:

* NTLM
* LDAP / LDAPS
* KERBEROS

#### NTML / NTML 2

This uses a challenge-response sequence of messages between client and a server. It provides authentication based on the challenge-response. 

It can't provide data integrity or confidentiality protection.

#### LDAP / LDAPS

LDAPS supports encryption and thereofre crendentials are not sent in plain text.

The domain controller (DC) can be considered a form of database of users, groups and computers (they contain information about 'objects').

Using this protocol, a user's workstation sends crendentials using an API to the DC to validate and authenticate what they can do.

#### Kerberos

This protocol uses symmetric-key cryptography; requiring a **trusted** third-party to authenitcate and verify user identities.

### Azure Active Directory

This is a secure online authentication store, containing users and groups. User have username and password when logging into an applicatio using Azure Active Directory. This is true for Microsoft cloud servicesx such as Office 365.

Azure AD supports the following authentication method:

* SAML (Security Assertion Markup Language)
* OAUTH 2.0
* OpenID Connect

#### SAML (Security Assertion Markup Language)

SAML is a type of Single-Sign-On (SSO) standard. It defines the rules/protocols that allow a user to access web apps with a single logon. These types of applications (service providers) all trust the systems that verify user identities (Identity Providers)

So:

* Service Providers - Systems and applications that a user can access throughout the day
* Identity Providers - Systems that peform user authentication

#### OAuth 2.0

This is a standard that allows applications to provide a client application with access.

It has four important roles:

1. Authorisation Server - Server that issues an access token
2. Resource Owner - Application's end user that grants permission to access resource server with an access token.
3. Client - Application that requests access token, then passes to resource server.
4. Resource Server - Attempts using access token, must verify it is valid, which is the application.

#### OpenID Connect

A simple authentication standard built above OAUTH 2.0, adding additional token called ID token.

This is done through the use of simple JSON Web Tokens (JWT), being more about user authentication as compared to OAuth which is about resource access and sharing.

### Questions

#### Which Active Directory is cloud based?

Azure Active Directory

#### Which authentication method does not provide data integrity?

NTLM

#### Which authentication method assigns a ticket in order for a user to login?

kerberos

#### Which authentication method allow users to access applications with a single login (short name)?

SAML

#### Authentication method that uses JSON Web Tokens?

OpenID Connect


## Utility Tools

Windows comes with some important utility tools, some are:
* Computer Management
* Local Security Policy
* Disk Cleanup
* Registry Editor
* Command-line tools

### Computer Management

This contains more subtools such as:

* Task Scheduler
    * Allows predefined actions to be automatically executed due to certain set of permissions
    * Ie. set time and date for software to be installed, or script to be run
* Event Viewer
    * Very important tool; it logs events that happen on the device
        * This could include successful and failed login attempts, system errors etc.
    * Can also forward events to a SIEM (Security Information and Event Manager)
        * Hepls IT team determine possible malicious activities
* Shared Folders
    * Directory/Folder that is shared on a network an be accessed by multiple users
* Local Users and Computers - Can create users, add to built-in groups, and be given levels of access.
    * User 1 can access RDP, whilst User 2 cannot
* Performance Monitor
    * Monitors different activies on the device
    * Such as CPU usgae, Memory usage etc.
* Disk Management
    * Can shrink, expand, format, create and delete partitions on a device.
* Services and Applications
    * Checks running services on a system
    * Can start, stop or restart them.

### Local Security Policy

A group of settings that should be configured for a computer's security. With this you can set minimum password length, complextiy and disable accounts and more.

*Note: Without an AD, disabling local admin is a bad idea*

### Disk Cleanup

Allows the removal of files that are no longer needed by the system. Running this as Administrator will allow the cleanup of system files.

* Ie. Old update files that are no longer needed

### Registry Editor

Is a database that stores important OS settings. For example:

* Contains entries with information with what should happen when a file is double clicked
* Contains entries determining how wide the taskbar should be
* Contains entries about built-in and inserted hardware 

It is called whenever the system is booted up.

It contains low-level settings for settings and applications for Windows. They are structure as following:

* HKEY_CLASSES_ROOT
* HKEY_CURRENT_USER
* HKEY_LOCAL_MACHINE
* HKEY_USERS
* HKEY_CURRENT_CONFIG

Powershell has a tool called 'req' that can add, remove, query, import and export registry keys.

There is also a GUI available called 'Regedit'

### Command-line tools

There are 3 possible tools that can allow Windows to be used via command line:

* CMD
    * Can use to interpet the Windows OS    
    * Can automate various system-related tasks using scripts and batch files    
    * Can interact directly with OS   
    * Emulates most command line abilities from MS-DOS    
    * More modern and contains additional features and commands than Powershell
* Powershell    
    * Mainly used by sysadmins to manage network and domain
        * Also any computers and devices that are part of it
    * Is a scripting language
    * Can interpret bash and powershell commands
        * Command promt can only interpret bash commands...
    * Is more advanced than CMD
* Windows Terminal - Can be installed
    * Includes multiple tab support, themes and customisation


## Task 5 - Types of Servers

### Servers

A server is a piece of hardware or software equipment that allows the functionality for other software or devices.

The following types of servers are; Domain Controller, File server, Web server, FTP Server, Mail Server, Database Server, Proxy Server, Application Server.

#### Domain Controller

When used in an AD or AAD, allows for the management of users, groups, restrict actions, improve security and many more of other computers and servers.

#### File Server

Provides an abilitiy to share files accross a network

#### Web Server

Serves dynamic/static content to a web browser.

Done by a means of loading a file from a disk and serving it across a network to a web browser on the client's machine.

#### FTP Server

Allows for the transferring of one or many files securely (if SFTP) accross computers while providing file security, file organisation and transfer control.

#### Mail Server

Allows for the moving and storing of electronic mail over corportate networks and the internet.

#### Database Server

Provides computers with services that allow accessing, quering, adding and removal of data from one or multiple databases.

#### Proxy Server

Sits between a client program and external server.

This is to filter requests, improve performance and share connections.

#### Application Server

Usually connect to database server and users to provide software.

### Questions

#### Which can be considered the most important server?

Domain Controller

#### Which server can store emails?

Mail Server


## Task 6 - Users and Groups Management

Was using 'Parrot' OS for this one. Deployed the machine, ensuring I was connected to the THM network. 

Then using 'Remmina' connected to '10.10.129.56' Using 'Administrator' and 'tryhackme123!' as the user.

### Creating a User

In order to create a new user you have to do the following:

1. Within 'Server Manager' go to tools > Active Directory Users and Computers
2. Go view > advanced features
3. Now double click the 'thm.lab' to go in the Active Directory tree
4. Go right-click and new > Organistaional Unit
    1. This is so you can have Users and Groups
5. Can now right-click and go New > User 

A user has a bunch of options with its creation:

* Password Never Expires
    * By default passwords expire after 42 days
    * In production environment, may use 'User must change password at next logon' to force the user to reset the password after creation.
* User cannot change password
    * If a user had to change their password when it expires this is a bad idea...
* Disable account
    * This is useful if an employee is going on leave, allows for the user to log in.
* User Logon Name
    * This is the name that a user will use to sign in

### Creating a Group

In order to create a new group you have to do the following:

1. Same as group - Make an Organisational Group
2. Right click and go new > group
    1. Do all the steps for this

### Adding a User to a Group

Should have users and a group already:

1. Go into the LAB folder and find Users
2. Now right click a user and > add to group


## Task 7 - Creating a GPO

A Group Policy Object (GPO) is a feature of AD that allows additional controls to user accounts and computers.

It includes local, site-wide, organisational and domain level settings.

### How to create a GPO

1. Within 'Server Manager' go to tools > Group Policy Management
2. Right click on 'Group Policy Objects' and create a new object

To edit this, right click on it and go 'edit'. There should now be policies that can be added to this group policy.

In order to apply the GPO open CMD as admin and go `gpupdate /force` and wait.