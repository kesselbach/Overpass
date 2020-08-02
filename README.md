# 'Overpass' box writeup
## Overpass is a CTF box written by NinjaJc01 and available on the [TryHackMe](https://tryhackme.com/) platform.
## Read about
# ![bg](images/background.png?raw=true "Title")

## Foothold
+ **Let's start with a nmap scan for ports**

``nmap -sV -sC -oN scan1  10.10.135.227``

+ **We can clearly see 2 ports open, ssh and a http, configured on default ports**

# ![1](images/nmap_scan_ov(1).jpg?raw=true "nmap_scan")

+ **Visiting the http web-site, we are welcomed by a page which contains info about a specific password-manager app built by some developers. We also see a download link button**

# ![2](images/visit.png?raw=true "visit")

**Accessing the download page, we can look through the Source code of the password-manager app. It looks like it's written in go language which encrypt passwords with the ROT47 algorithm**
