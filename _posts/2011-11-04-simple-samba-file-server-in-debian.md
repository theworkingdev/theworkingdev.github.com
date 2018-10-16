---
title: Simple Samba File Server setup in Debian Linux
layout: post
categories: linux
permalink: /2011/11/simple-samba-file-server-in-debian.html
date:   2011-11-04 
---

CIFS is short for Common Internet File System, it's also known as SMB which is short for Server Message Block.

_Example 1. How to get smb_

```bash
sudo apt-get update
sudo apt-get install smb smbfs
```

Edit the samba configuration file which is located in /etc/smb.conf you will need elevated privileges when you edit it. You need to use sudo, like this
`sudo vim /etc/smb.conf`

Don't forget to backup the original configuration file before you do any edits. It might be best to delete all contents of the existing smb.conf and just start with a blank config.

Listing 1 shows a minimal samba configuration

_Listing 1. minimal /etc/smb.conf_
```
security=user                     # (1)
username map=/etc/samba/smbusers  # (2)

[homes]
comment=Home Directories
browseable=yes
writable=yes
valid=%S ; so that only the defined users can view it
```

**(1)** allow only members of /etc/samba/smbusers

**(2)** restrict access only members of smbusers who also have a shell account. A shell account lets you login to linux as a user

The lines under [home] defines which folders Windows users can see, access, and write to, the rest is self explanatory.

Next, edit the Samba users file

_Listing 2./etc/samba/smbusers_
```
ted=ted
johndoe=johndoe
adrianne=dennis
```

The left hand side is the Samba users name and the right hand side is the shell account. In the example, the Samba user name is "ted" which is mapped to shell account "ted"; the Samba user name is exactly the same as the shell user name.
Next, set the Samba password for the users, note that the a user's Samba password is distinct from his shell account's password.

```bash
sudo smbpasswd -a ted
```

The `-a` flag is used to create a password entry in /etc/samba/smbpasswd in case it doesn't exist yet. If the password for the user already exist, the existing password entry will be updated. The shell account for the user must already exist before running this command.

Reboot the box.

The Finder in macOS should be able to see the Samba share, it should also appear to Windows users in Network Places. When the Samba server asks for your password, it's asking for your Samba password and not your shell account password.
 