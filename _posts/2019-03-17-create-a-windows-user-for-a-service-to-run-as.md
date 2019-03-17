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

#### `<user_account>`

The user account (SID).

<span style="font-size:32px">`This is some code that I want to display large`</span>

<span style="font-size:32px; font-weight:bold">`This is some code that I want to display large`</span>

<span style="font-size:32px">`This is ``**some code**`` that I want to display large`</span>

<span style="font-size:32px">This is some `code` that I want to display large</span>

#### `<app_directory>`

The path of the folder containing the app.

#### `<permission_flags>`

# Sets th`e acc`ess permissions.

* (OI): The Object Inherit flag propagates permissions to subordinate files
* (CI): The Container Inherit flag propagates permissions to subordinate folders
* (W): Write
* (R): Read
* (X): Execute
* (F): Full
* (M): Modify

#### `/t`

Apply recursively to existing subordinate folders and files.

**Example:**

    icacls "C:/myAppFolder/" /grant AppUser:(OI)(CI)(F) /t

***

Create the service and assign the user to it (Note: the space after each "=" is required)

    sc create <service_name> binPath= "<executable_path>" obj= "<domain>\<user_account>" password= "<password>"

#### `sc`

Execute the sc.exe command-line tool.

#### `<service_name>`

The name to assign to the service in Service Control Manager.

#### `<executable_path>`

The path of the service executable.

#### `<domain>`

The domain of a domain-joined machine. If the machine isn't domain-joined, the local machine name.

#### `<user_account>`

The user account under which the service runs.

#### `<password>`

The user account password.