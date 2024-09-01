# Adding new user for running the server
Linux has `useradd` command, and some distributions also have `adduser`
command. I will be using `adduser`, as it's more interactive and easy to use.
If your distro doesn't have one, you can see `man useradd` or `useradd --help`
for help. Command `sudo adduser sfactory` should result in prompts and output
like this:
```
Adding user `sfactory' ...
Adding new group `sfactory' (1001) ...
Adding new user `sfactory' (1001) with group `sfactory (1001)' ...
Creating home directory `/home/sfactory' ...
Copying files from `/etc/skel' ...
New password:
Retype new password:
passwd: password updated successfully
Changing the user information for sfactory
Enter the new value, or press ENTER for the default
        Full Name []:
        Room Number []:
        Work Phone []:
        Home Phone []:
        Other []:
Is the information correct? [Y/n]
Adding new user `sfactory' to supplemental / extra groups `users' ...
Adding user `sfactory' to group `users' ...
```

Which password you give the user doesn't matter much, as we will disable
login through normal means.

I'm not quite sure if Debian default installation has Nano-editor, but if it
doesn't, you can add it with `sudo apt install nano`. It is an easy to use
text editor for the console.

Edit _/etc/passwd_ by typing `sudo nano /etc/passwd`. Last line in file should
be like this:
```
sfactory:x:1001:1001:,,,:/home/sfactory:/bin/bash
```
Now change it to:
```
sfactory:x:1001:1001:,,,:/home/sfactory:/usr/sbin/nologin
```

# Installing steamcmd
The following command should install steamcmd on your Debian:
```bash
sudo apt update -y && \
  sudo apt install software-properties-common && \
  sudo apt-add-repository non-free && \
  sudo dpkg --add-architecture i386 && \
  sudo apt update && \
  sudo apt install steamcmd
```
Then switch to newly created user, and add steamcmd to $PATH if it isn't:
```
sudo -u sfactory bash -c "echo export PATH=\\\$PATH:/usr/games >> ~/.bashrc"
```
If you have a system other than Debian or you want to see more on how to use
steamcmd, take a look [here](https://developer.valvesoftware.com/wiki/SteamCMD#Downloading_SteamCMD)

# Installing Satisfactory dedicated server
Use the command below to change to install the server as user _sfactory_:
```bash
sudo -u sfactory steamcmd +force_install_dir ~/SatisfactoryDedicatedServer +login anonymous +app_update 1690800 validate +quit
```

# Setting up systemd unit file

Below are the contents of file _/etc/systemd/system/satisfactory.service_.
Use the command `nano /etc/systemd/system/satisfactory.service` to create
the file add the contents. Remember to write the changes to disk before
exiting. 
```systemd
# Contents of /etc/systemd/system/myservice.service
[Unit]
Description=Satisfactory server
After=network.target

[Service]
User=sfactory
Group=sfactory
Type=simple
Restart=always
RestartSec=5
WorkingDirectory=/home/sfactory/SatisfactoryDedicatedServer/

# !NOTICE To comment the line below if you don't want to update server every time it starts.
ExecStartPre=/usr/games/steamcmd +force_install_dir /home/sfactory/SatisfactoryDedicatedServer +login anonymous +app_update 1690800 validate +quit
ExecStart=/home/sfactory/SatisfactoryDedicatedServer/FactoryServer.sh -multihome=0.0.0.0

[Install]
WantedBy=multi-user.target
```

# Using systemctl to control the server
First go back to your normal user by pressing `CTRL-D` *once*. You can check
the username you're logged in as currently with the command `whoami`.

Then change to root user for not having to type sudo all the time: `sudo -s`.
*Beware*: Root user can do anything, and not all commands prompt you for
doing something dangerous. It is also easy to forget to go back to your
normal user, so remember to logout from root after using it.

Either as root user, or by using sudo, reload changes made to systemd unit
files using command `systemctl daemon-reload`. Start the server with command
`systemctl start satisfactory` and check the status with
`systemctl status satisfactory`. A running server should give you output
like:
```
● satisfactory.service - Satisfactory server
     Loaded: loaded (/etc/systemd/system/satisfactory.service; enabled; preset: enabled)
     Active: active (running) since Fri 2024-08-30 11:47:08 EEST; 2 days ago
   Main PID: 6149 (FactoryServer.s)
      Tasks: 46 (limit: 19029)
     Memory: 1.0G
        CPU: 2h 13min 28.094s
     CGroup: /system.slice/satisfactory.service
             ├─6149 /bin/sh /home/sfactory/SatisfactoryDedicatedServer/FactoryServer.sh -multihome=0.0.0.0
             └─6156 /home/sfactory/SatisfactoryDedicatedServer/Engine/Binaries/Linux/UnrealServer-Linux-Shipping FactoryGame -multihome=0.0.0.0
```
Stopping the server is done with `systemctl stop satisfactory`. To make
it start on boot, write `systemctl enable satisfactory`... as root.

