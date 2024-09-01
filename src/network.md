# Network stuffs
The usual thing with home networks is, that they are behind
[NAT](https://www.geeksforgeeks.org/network-address-translation-nat/) which
prevents new connections from internet. Addresses in the local net are not
visible to the internet. It is also normal for network computers to
receive their ip address from
[DHCP-server](https://www.geeksforgeeks.org/dynamic-host-configuration-protocol-dhcp/),
which physically is the router. Router keeps tabs on connections opened from
local network to outside world, and maps the return packets to correct ip
in the local net.

Because local net machines get their addresses from DHCP, the server should
be assigned static IP within the local net from the settings of dhcp server.
At least it is inconvenient that the server address might change. Static ip
can usually be configured by using the network interface's
[MAC](https://en.wikipedia.org/wiki/MAC_address) address. That is the
physical address of a computer, or rather the network card's. You can likely
inspect these from the router's interface. They look like this:
`0e:d9:fe:c9:92:d5`.

Port-forwarding makes it possible to connect from internet, and establish a
new connection to the local net, which is behind NAT. Port is a number in
network packet, that the operating system uses to to send the data to
correct program.
[Port forwards](https://en.wikipedia.org/wiki/Port_forwarding) should go to
the server ip. 
# Connecting to the router control interface
Home routers usually have a web-interface to change settings with. In Windows
PowerShell write `ipconfig` and you should get your network interfaces. Look
for default gateway. That's the router. Type the ip into your browser, and you
should probably get a web page which asks for login and password. Look these
from your router. These are often printed onto them.

# Ports used
Satisfactory uses the following ports if they are free:
- UDP/15777
- UDP/15000
- UDP/7777

More: [https://satisfactory.wiki.gg/wiki/Dedicated_servers](https://satisfactory.wiki.gg/wiki/Dedicated_servers)
