# About
This guide is a sore attempt at making the the installation of Satisfactory
dedicated server more approachable for people new to Linux or networks. I
don't include Windows guide here, but if you would like to add that,
please contact me, and make a pull request. Same with any improvements you
would like to contribute.

# What this tutorial covers?
This tutorial focuses on installation of Satisfactory dedicated server software
 using steamcmd on and run it using a separate user created for this. We create
a systemd unit file for it, so it is easy to start and stop the service, and
make it start on boot. We also automate the server updates. Some of the
things are not so much related to setting up the Satisfactory server as
doing very some basic things to prevent bad Internet Goons from guessing
your password by disabling password-login and using ssh keys.

We also take a look how to make the server accessible from the internet. And
also how to have an easy to remember host name for your internet IP address
using a dyndns provider. Usually it is DHCP server which provides IP
addresses. Server having it's address change every once in a while is
inconvenient and your router may even have dyndns feature in itself, which
makes having a hostname for your ip really easy.

# Pre-requisites
This tutorial assumes you have [Debian](https://debian.org/) operating system
installed on your server. At the time of writing this, the newest version is
12 (Bookworm). Other distributions may do aswell, but as I have Debian, I
will write this guide using it.

It is also immensly helpful to know a few Linux-commands to move around the
filesystem, edit files etc. 
[Here](https://www.freecodecamp.org/news/the-linux-commands-handbook/) is a
tutorial, but you don't need to learn all of it. But at least learn these:
- ls
- mkdir
- pwd
- whoami
- rm
- mv
- nano
- cd
- sudo

And maybe also:
- journalctl
- tab auto-completes paths ;D
