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

This reveals some much more interesting information. It tells us that "lighttpd 1.4.35" is running on port 80 and that port 2222 is actually running SSH (away from the default port of 22) with a version called OpenSHH 6.6.1.

With this information, the first thing I did was check out the webpage. Navigating to it reveals a pfsense login page - confirming this is a router. It could possible be open to brute force if it has a weak password. However, before bruteforcing it and going wild, maybe they didn't change the default credentials? Googling for pfsense default credentials reveals that pfsense:pfsense is the username and password. 

![](/images/pfsensedefault.png)

However, this will fail. Next, we can try a bruteforce attack using Hydra. Hydra needs 3 things before it starts to crack - the URL, the type of HTTP request, location of username/password forms. To get these, we first intercept a request to login using any credentials - I used test and test

![](/images/4.5%20-%20pfsense%20burp%20intercept.png)

The highlighted text at the bottom we will need so keep a hold of it. First, the URL we need is simply the IP address followed by the request in the HTTP request - in this case, it is simply "/index.php". 

Next, we need the actual information being sent (the highlighted part) which includes a username field called "usernamefld" and a password field called "passwordfld. Looking carefully, you should see the fake credentials you entered in these fields. 

Finally, we need an error message so Hydra knows if the password hasn't worked. This varies from login page to login page. In this case, the error message is "Username or Password incorrect"

![](/images/incorrect.png)

Putting these parts all together in the Hydra command should look like the following:

![](/images/5%20-%20pfsense%20bruteforce.png)

This breaks down into the following parts:

* hydra -l admin specifies the username to use as "admin"
* -P /usr/share/wordlists/rockyou.txt specifies the wordlist to use
* http-post-form specifies the HTTP method this request uses - as found in Burp Suite titled "POST"
* "/index.php is the login page itself
* __csrf_magic...login=Login - this long string indicates the actual data being sent
    * Worth noting here that the username and password fields have been replaced by ^USER^ and &PASS^ which tells Hydra which parts to replace - ^USER^ gets replaced with admin and ^PASS^ gets replaced with every password in the wordlist we chose
* Username or Password incorrect" specifies the error message for a failed login - helps Hydra know if the password worked or not. If this message is in the HTTP response, the login failed. If not, the password worked.


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