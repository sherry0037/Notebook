.. _computer_networking.rst:

####################
Computer Networking
####################

Reference:

- https://www.youtube.com/watch?v=QKfk7YFILws
- https://microchipdeveloper.com/tcpip:tcp-ip-five-layer-model

*************
Introduction
*************

**Protocal** is a defined set of standards that computers must follow in order to communicate properly.

Some of the applications we use requires us to move data across a network from point A to point B. The **Transmission Control Protocol/Internet Protocol** (TCP/IP) network provides a framework for transmitting this data.

================================
TCP/IP Five-Layer Nerwork Model
================================

== =========== ================== ========= ============
#  Layer       Protocol
== =========== ================== ========= ============
5  Application HTTP, SMTP, etc    Messages  n/a
4  Transport   TCP/UDP            Segment   Port #'s
3  Network     IP                 Datagram  IP address
2  Data Link   Ethernet, Wi-Fi    Frames    MAC Address
1  Physical    10 Base T, 801.11  Bits      n/a
== =========== ================== ========= ============

**Physical layer**

- represents the physical devices that interconnect computers.

- this includes specifications for the networking cables and the connectors that join the devices together, and specifications describing how signals are sent through these connectors.

**Data link layer**

- responsible for defining a common way of interpreting these signals so network devices can communicate.

- one of the protocol is the **Ethernet**. The Ethernet standards also define a protocol responsible for getting data to nodes on the same network or link.

**Network layer**

- allows different networks to communicate with each other through devices known as routers.

- a collection of networks connected together through routers is called **Internetwork**.

 - example: the Internet

- most common protocol is **IP** (Internet Protocol).

**Transport layer**

- sorts out which client and server programs are supposed to get the data.

  - clients send requests, and servers get requests

- most common protocal is **TCP/UDP** (Transmission Control Protocol/User Datagram Protocol)

  - TCP ensures data is reliably delivered, UDP does not.

**Application layer**

- protocol is specific to application.



******************
The Network Layer
******************

*************************************
The Transport and Application Layers
*************************************

********************
Networking Services
********************

***************************
Connecting to the Internet
***************************

*********************************************
Troubleshooting and the Future of Networking
*********************************************
