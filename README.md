# 'Overpass' box writeup
## Overpass is a CTF box written by NinjaJc01 and available on the [TryHackMe](https://tryhackme.com/) platform.
## Read about
# ![bg](images/background.png?raw=true "Title")

## Foothold
+ **Let's start with a nmap scan for ports**

``nmap -sV -sC -oN scan1 10.10.61.75``

+ **We can clearly see 2 ports open, ssh and a http, configured on default ports**

# ![1](images/nmap_scan_ov.jpg?raw=true "nmap_scan")
