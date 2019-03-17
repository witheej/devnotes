---
layout: post
title: Create a Windows user for a service to run as
sub_heading: ''
date: 2019-03-16 04:00:00 +0000
tags:
- Windows Service
- User
- cmd
banner_image: ''
related_posts: []

---
Create a user in cmd

    net user <user_account> <password> /add

Add user to a group

    net localgroup <group> <user_account> /add

Grant read/write/execute permission to the user for the app's folder

    icacls "<appdirectory>" /grant <user_account>:<permission_flags> /t

<span style="font-size:24px; font-weight:bold;">`<user_account>`</span>

The user account (SID).

<span style="font-size:24px; font-weight:bold;">`<app_directory>`</span>

The path of the folder containing the app.

<span style="font-size:24px; font-weight:bold;">`<permission_flags>`</span>

Sets the access permissions.

* (OI): The Object Inherit flag propagates permissions to subordinate files
* (CI): The Container Inherit flag propagates permissions to subordinate folders
* (W): Write
* (R): Read
* (X): Execute
* (F): Full
* (M): Modify

<span style="font-size:24px; font-weight:bold;">`/t`</span>
<span style="font-size:24px;">**`/t`**</span>

Apply recursively to existing subordinate folders and files.

**Example:**

    icacls "C:/myAppFolder/" /grant AppUser:(OI)(CI)(F) /t

***

Create the service and assign the user to it (Note: the space after each "=" is required)

    sc create <service_name> binPath= "<executable_path>" obj= "<domain>\<user_account>" password= "<password>"

<span style="font-size:24px; font-weight:bold;">`sc`</span>

Execute the sc.exe command-line tool.

<span style="font-size:24px; font-weight:bold;">`<service_name>`</span>

The name to assign to the service in Service Control Manager.

<span style="font-size:24px; font-weight:bold;">`<executable_path>`</span>

The path of the service executable.

<span style="font-size:24px; font-weight:bold;">`<domain>`</span>

The domain of a domain-joined machine. If the machine isn't domain-joined, the local machine name.

<span style="font-size:24px; font-weight:bold;">`<user_account>`</span>

The user account under which the service runs.

<span style="font-size:24px; font-weight:bold;">`<password>`</span>

The user account password.