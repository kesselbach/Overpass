# 'Overpass' box writeup
## Overpass is a CTF box written by NinjaJc01 and available on the [TryHackMe](https://tryhackme.com/) platform.
## Read about [Broken Authentication](https://www.youtube.com/watch?v=mruO75ONWy8), [OWASP A2](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A2-Broken_Authentication)
# ![bg](images/background.png?raw=true "Title")

## Foothold
+ **Let's start with a nmap scan for ports**

``nmap -sV -sC -oN scan1 10.10.107.5``

+ **We can clearly see 2 ports open, ssh and a http, configured on default ports**

# ![1](images/nmap_scan_ov(1).jpg?raw=true "nmap_scan")

+ **Visiting the http web-site, we are welcomed by a page which contains info about a specific password-manager app built by some developers. We also see a download link button**

# ![2](images/visit.png?raw=true "visit")

**Accessing the download page, we can look through the Source code of the password-manager app. It looks like it's written in go language which encrypt passwords with the ROT47 algorithm**

+ **Let's do a gobuster scan of the web site**

``gobuster dir -u http://10.10.107.5/ -w /usr/share/wordlists/dirb/big.txt``

# ![3](images/admin.jpg?raw=true "admin")

+ **We can spot an admin page and accessing it, we can see an administrator login area. I tried, unsuccessfully, a SQL injection and then moved on the 2nd OWASP vuln. Let's look into the source code of the page and into some js code**

# ![4](images/scripts.jpg?raw=true "scripts")

**We can clearly see a login.js script so let's look into the source code. The login function seems to be a way to go**

```js
async function login() {
    const usernameBox = document.querySelector("#username");
    const passwordBox = document.querySelector("#password");
    const loginStatus = document.querySelector("#loginStatus");
    loginStatus.textContent = ""
    const creds = { username: usernameBox.value, password: passwordBox.value }
    const response = await postData("/api/login", creds)
    const statusOrCookie = await response.text()
    if (statusOrCookie === "Incorrect credentials") {
        loginStatus.textContent = "Incorrect Credentials"
        passwordBox.value=""
    } else {
        Cookies.set("SessionToken",statusOrCookie)
        window.location = "/admin"
    }
```

+ **The credentials of the login page are sent to a endpoint inside some DB of the box and a session token is used. It seems like the session token is set with the statusOrCookie parameter. If we change the SessionToken value, maybe we can bypass the login. Let's create the cookie with the name of *SessionToken* and any value you want inside it**

# ![5](images/session.jpg?raw=true "sess")

**Now, trying to refresh the page we can observe we bypassed the login auth and a RSA private key of some guy named James is on our screen. Let's save it in some txt file and use it to connect to ssh**

# ![6](images/RSA.jpg?raw=true "rsa")

``chmod 600 id_rsa``
``ssh -i id_rsa james@10.10.107.5``

+ **Trying to connect to ssh, a key is required for our file. Let's use ssh2john to bruteforce our way in: i'm gonna use ssh2john to get our first hash then crack it with john using the rockyou.txt wordlist**

``python ssh2john.py id_rsa > key_hash``
``sudo john key_hash -wordlist=/usr/share/wordlists/rockyou.txt``

# ![7](images/johned.jpg?raw=true "johnny")

**Using our password for the ssh we are in! There's two files into user's home directory**


