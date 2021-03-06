# netwox
Netwox是一款非常强大和易用的开源工具包，可以创造任意的TCP/UDP/IP数据报文。Netwox工具包中包含了超过200个不同功能的网络报文生成工具，每个工具都拥有一个特定的编号。

## Introduction
Toolbox netwox helps to find and solve network problems :
  - sniff, spoof
  - clients, servers
  - DNS, FTP, HTTP, IRC, NNTP, SMTP, SNMP, SYSLOG, TELNET, TFTP
  - scan, ping, traceroute
  - etc.

Netwox contains 222 tools using network library netwib. Some tools are only a simplified implementation, while others are very sophisticated.

Netwag is a graphical front-end to netwox.

## DownLoad
http://www.laurentconstantin.com/en/netw/netwox/

## License
Netwox is available under the GNU GPL License.

## Synopsis
    netwox [ number [ parameters... ] ]

    number : number of the tool to use
    parameters : parameters for the chosen tool number


## Descrīption
When using netwox without number and parameters, it enters help mode. In this mode, the user has to select a category by pressing a key. Then by choosing a tool number, its corresponding usage is displayed.  Or the user can enter "netwox [number] --help" or "netwox [number] --help2" to display the detail usage.
```sh
The meaning of number
   1 : Display network configuration
   2 : Display debugging information
   3 : Display information about an IP address or a hostname
   4 : Display information about an Ethernet address
   5 : Obtain Ethernet addresses of computers in an IP list
   6 : Display how to reach an IP address
   7 : Sniff
   8 : Sniff and display open ports
   9 : Sniff and display Ethernet addresses
  10 : Sniff and display network statistics
  11 : Sniff and verify checksums
  12 : Display which values to use for netwox parameters
  13 : Obtain DLT type for sniff and spoof for each device
  14 : Spoof a record
  15 : Display content of a record
  16 : Convert a record
  17 : Recompute checksums of packets in a record
  18 : Reassemble IP packets of a record, and reorder TCP flow
  19 : Extract a range of packets from a record
  20 : Search for strings in packets from a record
  21 : Convert a number
  22 : Convert a string
  23 : Display ASCII table
  24 : Convert IP addresses ranges
  25 : Test if a directory is secure
  26 : Dump a file
  27 : Compute MD5 of a file
  28 : Convert a binary file to readable and editable file
  29 : Convert a readable and editable file to a binary file
  30 : Convert a file from unix to dos
  31 : Convert a file from dos to unix
  32 : Spoof Ethernet packet
  33 : Spoof EthernetArp packet
  34 : Spoof EthernetIp4 packet
  35 : Spoof EthernetIp4Udp packet
  36 : Spoof EthernetIp4Tcp packet
  37 : Spoof EthernetIp4Icmp4 packet
  38 : Spoof Ip4 packet
  39 : Spoof Ip4Udp packet
  40 : Spoof Ip4Tcp packet
  41 : Spoof Ip4Icmp4 packet
  42 : Spoof of packet samples : fragment
  43 : Spoof of packet samples : fragment, ip4opt:noop
  44 : Spoof of packet samples : fragment, ip4opt:rr
  45 : Spoof of packet samples : fragment, ip4opt:lsrr
  46 : Spoof of packet samples : fragment, ip4opt:ts
  47 : Spoof of packet samples : fragment, ip4opt:ipts
  48 : Spoof of packet samples : fragment, ip4opt:ippts
  49 : Ping ICMP
  50 : Ping ICMP (EthIP spoof)
  51 : Ping TCP
  52 : Ping TCP (EthIp spoof)
  53 : Ping UDP
  54 : Ping UDP (EthIp spoof)
  55 : Ping ARP
  56 : Ping ARP (EthIp spoof)
  57 : Traceroute ICMP
  58 : Traceroute ICMP (EthIP spoof)
  59 : Traceroute TCP
  60 : Traceroute TCP (EthIp spoof)
  61 : Traceroute UDP
  62 : Traceroute UDP (EthIp spoof)
  63 : Traceroute on a specified IP protocol
  64 : Traceroute on a specified IP protocol (EthIp spoof)
  65 : Scan ICMP
  66 : Scan ICMP (EthIP spoof)
  67 : Scan TCP
  68 : Scan TCP (EthIp spoof)
  69 : Scan UDP
  70 : Scan UDP (EthIp spoof)
  71 : Scan ARP
  72 : Scan ARP (EthIp spoof)
  73 : Simulate presence of a/several computer/s (arp and ping)
  74 : Flood a host with random fragments
  75 : Fill table of a switch using a flood of Ethernet packets
  76 : Synflood
  77 : Check if seqnum are predictible
  78 : Reset every TCP packet
  79 : Acknowledge every TCP SYN
  80 : Periodically send ARP replies
  81 : Send an ICMP4 timestamp
  82 : Sniff and send ICMP4/ICMP6 destination unreachable
  83 : Sniff and send ICMP4/ICMP6 time exceeded
  84 : Sniff and send ICMP4/ICMP6 parameter problem
  85 : Sniff and send ICMP4 source quench
  86 : Sniff and send ICMP4/ICMP6 redirect
  87 : TCP client
  88 : UDP client
  89 : TCP server
  90 : UDP server
  91 : TCP server multiclients
  92 : UDP server multiclients
  93 : TCP remote administration server
  94 : TCP remote administration client (exec)
  95 : TCP remote administration client (get file)
  96 : TCP remote administration client (put file)
  97 : SYSLOG client
  98 : Flood a host with syslog messages
  99 : TELNET client
 100 : TELNET client executing one or several commands
 101 : Brute force telnet client
 102 : Query a DNS server
 103 : Obtain version of a Bind DNS server
 104 : DNS server always answering same values
 105 : Sniff and send DNS answers
 106 : Send an email
 107 : Post a newsgroup message
 108 : List newsgroups available on a server
 109 : Download one, or more, newsgroup messages
 110 : Ethernet bridge limiting flow
 111 : FTP listing a directory
 112 : FTP client : get a file
 113 : FTP client : put a file
 114 : FTP client : del a file
 115 : FTP client : get a directory recursively
 116 : FTP client : put a directory recursively
 117 : FTP client : del a directory recursively
 118 : HTTP GET
 119 : HTTP HEAD
 120 : HTTP POST
 121 : HTTP PUT
 122 : HTTP DELETE
 123 : HTTP TRACE
 124 : HTTP OPTIONS
 125 : HTTP server
 126 : HTTP remote administration server
 127 : Cypher/decypher a file using a xor
 128 : Split a file in smaller chunks
 129 : Reassemble chunks of a file
 130 : Brute force ftp client
 131 : Brute force http client (site password)
 132 : Brute force http client (proxy password)
 133 : Convert an url/uri
 134 : Obtain urls/uris in a HMTL file
 135 : Convert urls/uris in a HMTL file to absolute urls
 136 : Web download (http://... or ftp://...)
 137 : Create a sample configuration file for tool 138
 138 : Web spider (use configuration file created by tool 137)
 139 : Web spider on command line (fully recursive)
 140 : Spoof EthernetIp6 packet
 141 : Spoof EthernetIp6Udp packet
 142 : Spoof EthernetIp6Tcp packet
 143 : Spoof EthernetIp6Icmp6 packet
 144 : Spoof Ip6 packet
 145 : Spoof Ip6Udp packet
 146 : Spoof Ip6Tcp packet
 147 : Spoof Ip6Icmp6 packet
 148 : Ping ICMP6 Neighbor Discovery
 149 : Ping ICMP6 Neighbor Discovery (EthIp spoof)
 150 : Scan ICMP6 Neighbor Discovery
 151 : Scan ICMP6 Neighbor Discovery (EthIp spoof)
 152 : Interactive IRC client
 153 : IRC client listing channels
 154 : IRC client listening on a channel
 155 : Network performance measurement : TCP server
 156 : Network performance measurement : TCP client
 157 : Network performance measurement : UDP server
 158 : Network performance measurement : UDP client
 159 : SNMP Get
 160 : SNMP Walk
 161 : SNMP Trap
 162 : SNMP Trap2
 163 : SNMP Inform
 164 : SNMP Set
 165 : TFTP client : get a file
 166 : TFTP client : put a file
 167 : TFTP server
 168 : FTP server
 169 : Display simple network configuration easy to parse
 170 : TELNET server
 171 : DHCP client
 172 : List articles range of a newsgroup
 173 : Download overview of one, or more, newsgroup messages
 174 : FTP client : get a file and check its MD5
 175 : Web download (http://... or ftp://...) and check its MD5
 176 : TFTP client : get a file and check its MD5
 177 : Check if a SMTP server is up
 178 : Check if an IRC server is up
 179 : DHCP client requesting an INFORM
 180 : SNTP client obtaining time
 181 : SNTP server
 182 : Obtain size of a web file (http://... or ftp://...)
 183 : TCP relay
 184 : UDP relay
 185 : TCP multiclient relay
 186 : Millisecond sleep
 187 : Display date and time
 188 : SYSLOG server
 189 : SMTP server
 190 : Make coffee
 191 : Generate a password (English, French, Spanish)
 192 : Spoof of packet samples : fragment, ip4opt:ssrr
 193 : IDENT client requesting info about an open session
 194 : IDENT client creating a session and requesting its info
 195 : IDENT server
 196 : WHOIS client
 197 : WHOIS client guessing server
 198 : SMB/CIFS client: list shares
 199 : SMB/CIFS client: create a directory
 200 : SMB/CIFS client: delete a directory
 201 : SMB/CIFS client: rename a directory
 202 : SMB/CIFS client: list contents of a directory
 203 : SMB/CIFS client: delete a file
 204 : SMB/CIFS client: rename a file
 205 : SMB/CIFS client: get a file
 206 : SMB/CIFS client: put a file
 207 : SMB/CIFS client: recursively get a directory
 208 : SMB/CIFS client: recursively put a directory
 209 : SMB/CIFS client: recursively delete a directory
 210 : Web spider on command line (stay in same directory)
 211 : Web spider : converts a local downloaded filename to its original url
 212 : Web spider : converts an url to its local downloaded filename
 213 : Display a list of IP addresses
 214 : Traceroute discovery: graph of network topology
 215 : Traceroute discovery (EthIp spoof)
 216 : Beep
 217 : SMB/CIFS server
 218 : Netwox internal validation suite
 219 : Compute cryptographic hash of a file (md5, sha, etc.)
 220 : Convert a binary file to a base64 encoded file
 221 : Convert a base64 encoded file to a binary file
 222 : In a HMTL file, suppress links pointing to local urls
```

## 常用命令
```sh
# 
netwox
```