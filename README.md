# StreamDivert
StreamDivert is a tool to man-in-the-middle or relay in and outgoing network connections on a system. It has the ability to, for example, relay all incoming SMB connections to port 445 to another server, or only relay specific incoming SMB connections from a specific set of source IP's to another server. Summed up, StreamDivert is able to:


*  Relay all incoming connections to a specific port to another destination.
*  Relay incoming connections from a specific source IP to a port to another destination.
*  Relay all outgoing connections to a specific port to another destination.
*  Relay outgoing connections to a specific IP and port to another destination.
*  Handle IPv4 and IPv6 connections.
*  Handle TCP, UDP and ICMP connections

## Download Binaries
Pre-compiled binaries for StreamDivert can be downloaded [here](url).

## Usage
How do you use StreamDivert? Run the the tool:

```console
streamdivert.exe [config file]
```

The config file contains entries for streams you want to have diverted. En example config file:
```conf
//Divert all inbound TCP connections to port 445 (SMB) coming from 10.0.1.50 to 10.0.1.49 port 445
tcp < 445 10.0.1.50 -> 10.0.1.49 445

//Divert all inbound TCP connections to port 445 (SMB) coming from fe80::f477:846a:775d:d37 to fe80::20c:29ff:fe6f:88ff port 445
tcp < 445 fe80::f477:846a:775d:d37 -> fe80::20c:29ff:fe6f:88ff 445

//Divert all inbound TCP connections to port 445 (SMB) to 10.0.1.48 port 445
tcp < 445 0.0.0.0 -> 10.0.1.48 445

//Divert all inbound UDP connections to to port 53 (DNS) to  10.0.1.49 port 53
udp < 53 0.0.0.0 -> 10.0.1.49 53

//Divert all inbound ICMP packets coming from 10.0.1.50 to 10.0.1.49
icmp < 10.0.1.50 -> 10.0.1.49

//Divert all outbound TCP connections to 10.0.1.50, port 80 to 10.0.1.49 port 8080
tcp > 10.0.1.50 80 -> 10.0.1.49 8080

//Divert all outbound UDP connection to port 53 (DNS) to 10.0.1.49 port 53
udp > 0.0.0.0 53 -> 10.0.1.49 53
```

## Some Use Cases
*  Diverting C&C traffic to a local socket for dynamic malware analysis.
*  Diverting inbound SMB connections of a compromised host to Responder/ ntlmrelayx (usefull in penetration tests).
*  Routing C&C protocols over reserved ports. For example routing a meterpreter bind shell over port 445.
*  ...

## Help! My packets/ connections are not correctly diverted!
One thing to keep in mind when configuring diverted connections is that you don't have conflicting diverted streams. Given the following example config file:
```conf
icmp < 0.0.0.0 -> 10.0.1.50
icmp > 10.0.1.49 -> 10.0.1.48
```
Those two diverted streams will conflict with eachother, as packets for the first diverted stream will also be picked up by the second packet 'diverter'. Generally you will only run into these issues with UDP and ICMP and using wildcards. 

## Contributing to Streamdivert
Features wanted:
*  IP range support
*  ...