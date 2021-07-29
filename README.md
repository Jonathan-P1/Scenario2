# Scenario2
 
This is a writeup of a CTF/Pentesting challenge

<details><summary>Information Gathering</summary>
<p>

First things first, I ran a pingsweep scan using Nmap to identify possible targets across the network

![](/images/1.png)

We know have five targets:

* 192.168.22.1
* 192.168.22.20
* 192.168.22.30
* 192.168.22.40
* 192.168.22.45

With this information we can start scanning each one:

<details><summary>192.168.22.1</summary>
<p>
The IP of this machine could indicate that it is the router/default gateway of the network. Most routers either end in .1 or .254 for convenience. The first scan we run is a simple all ports scan outputting to a "normal" file.

![](/images/2%20pfsense%20allports.png)

Looking at the results, we can gather a few things. First, that it is running DNS - almost confirming suspicions it is the router. It is also running a possible website or web interface on port 80 and another service running on port 2222 called "EtherNetIP-1. Researching port 2222 indicates some results telling us what it could possibly be. 

![](/images/port%202222.png)

Now that we have a number of open ports, a more detailed scan can be run on these three ports only - this is much faster than running it across all ports.

![](/images/3%20pfsense%20servscan.png)

</p>
</details>

<details><summary>192.168.22.20</summary>
<p>

</p>
</details>

<details><summary>192.168.22.30</summary>
<p>

</p>
</details>

<details><summary>192.168.22.40</summary>
<p>

</p>
</details>

<details><summary>192.168.22.45</summary>
<p>

</p>
</details>