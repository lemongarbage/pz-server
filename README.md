# Project Zomboid Server on Vultr VPS Runbook

## This document will outline common practices and procedures used to operate and maintain the PZ server that exists on a Vultr instance.

### Architecture
The droplet instance is currently a basic type running a regular Intel processor. It has 16 Gb of memory, 80 Gb of disk space (SSD), 4 vCPUs, and is running Ubuntu 22.10. The software powering the server is the PZ dedicated server application that is downloaded directly from Steam. Steam is installed on the box. OpenSSH (Secure Shell) is installed. Firewall rules (UFW) restrict access to the box and only allow certain IP addresses to remotely access the server for either SSH or PZ connection. TMUX is used for terminal multiplexing to facilitate administration of the PZ server config.

### Accessing the Server
The server can be accessed remotely using any lcoal SSH client. A private key is needed to access the server. This will be provided to whoever requires access. To access the server from a Windows instance, do the following:
1. Download the Key file in this repository. Place it somewhere safe on your computer
2. Open up Command Prompt via `Win+R` and type `cmd`
3. Type `ssh root@198.199.121.112 -i <path_to_key_file>`
4. Connection to the server is established

### Accessing the PZ Server Console
Once logged into the VPS, you should be greeted by the Ubuntu Welcome message. You will be logged in as `root`. In order to access the TMUX session setup for administration of the PZ server, do the following:
 1. Type `tmux attach -t 0` (alternatively, you can hit the `up` arrow key two times)
 2. You will enter the TMUX session which has two panes open. The pane on the left is where the configuration files for the server are. The pane on the right is the active PZ Server console which can be used to save, quit, execute events, or interact with the PZ game world in any number of ways.
 3. To switch between these windows, hit `CTRL+b` simultaenously and use either the `left` or `right` arrow key after hitting `CTRL+b`

### Common Server Operations
#### Rebooting the Server
To reboot the PZ server, do the following:
 1. Enter the PZ Server console
 2. Type `save` 
 3. Type `quit`
 4. Hit the `up` arrow key, or type `./start-server.sh -servername two_frank`
 5. Hit enter
 6. The server will initiate the startup sequence
 7. Once you see `DISCORD DISABLED` and/or `SERVER STARTED` the server startup is complete

#### Editing the Modlist or Other Server Settings
Most of the server settings can be found in the `two_frank.ini` file located at `/home/pzuser/Zomboid/Server`. You will need to open the file with a text editor and manually edit the values contained within. For a list of server options, see the [PZ Wiki](https://pzwiki.net/wiki/Server_Settings). To edit the settings and mods used by the server, do the following:
 1. If not already, switch to the left window pane using `CTRL+b` and the `left` arrow key
 2. Type `nano two_frank.ini` and hit `enter` or hit the `up` arrow key a couple of times
 3. You are now in the Nano text editor. Use the arrow keys to navigate. You can edit text like you normally would in Microsoft Word for example. Use `CTRL+up or down arrow` to navigate faster.
 4. For editing mods:
    1. Locate the `Mods=` line
    2. Append the ***MOD NAME*** to the end of the list, with a semi-colon
    3. Locate the `WorkshopItems=` line
    4. Append the ***WORKSHOP ID*** to the end of the list, with a semi-colon
    5. Once complete, hit `CTRL+x` and hit `Y` to save changes
    6. Restart the server
 5. For Editing any other game settings:
    1. Locate the setting you want to change
    2. Change the value accordingly
    3. Hit `CTRL+x` and `Y` to save changes
    4. Restart the server

### Exiting the Server
To exit the server and close your remote session, do the following:
 1. Hit `CTRL+b` and then `d` to detach from the TMUX session
 2. type `exit` 
 3. You have closed connection to the server

### Miscellaneous 
Digital Ocean uses a `loopback` address on the network interface. For some reason, this causes a conflict with the PZ server networking setup, preventing players from succesfully establishing connection to the server. I have manually deleted the interface entry from the OS. However, in the event of a VPS restart, the interface will come back online and must be manually deleted again. To delete it, do the following:
 1. Type `ip a sh` to view and confirm presence of loopback (`10.10.0.6/16`)
 2. Type `ip a delete 10.10.0.6/16 dev eth0`
 3. Type `ip a sh` to confirm deletion




