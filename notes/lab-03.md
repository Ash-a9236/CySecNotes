<!--@ash-a9236 2025 : please see licence for -->

<!--VARIABLES-->

<style>

  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&family=Fira+Code&display=swap');

  :root {
    --text: #DED9E2;
    --title: #80A1D4;
    --highlight: #75C9C8;
    --link: #b99eea;
  }

  body {
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  }
</style>

[//]: # (<span style="color: var&#40;--text&#41;">)

# <span style="color: var(--title)">CYSEC 00</span>

## <span style="color: var(--title)">PASSWORD CRACKING</span>

<br>
<hr>

### <a name="base-concepts"><span style="color: var(--title)">00.00 INTRODUCTION</span></a>

<hr>

<br>


base configuration in virtualbox for both machines : 

Adapter 1 :
    Attached to = NAT
    Cable connected = true

Adapter 2 : 
    Attached to = Internal Network
    Name = HOME-LAN-NETWORK
    Cable connected = true


in the kali machine : 

```bash
# KALI MACHINE  
sudo su
ifconfig eth1 10.10.10.10 netmask 255.255.255.0 up
sudo echo "
127.0.0.1       localhost
127.0.1.1       kali
10.10.10.20     myhomelab.local

::1             localhost ip6-localhost ip6-loopback
ff02::1         ip6-allnodes
ff02::2         ip6-allrouters
" > /etc/hosts


cp /usr/share/wordlists/rockyou.txt.gz /home/$USER/Desktop 

# IF XHYDRA IS NOT INSTALLED 
sudo apt update && sudo apt install hydra-gtk -y
```

```bash
# META MACHINE  
sudo su
ifconfig eth1 10.10.10.20 netmask 255.255.255.0 up
```



return : 

```bash
Hydra v9.7 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2026-06-04 22:48:29
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344402 login tries (l:1/p:14344402), ~896526 tries per task
[DATA] attacking http-get://10.10.10.20:80/foo/bar/protected.html
[80][http-get] host: 10.10.10.20   misc: /foo/bar/protected.html   login: admin   password: iloveyou
[80][http-get] host: 10.10.10.20   misc: /foo/bar/protected.html   login: admin   password: daniel
[80][http-get] host: 10.10.10.20   misc: /foo/bar/protected.html   login: admin   password: 123456
in   password: nicole
<finished>
```


<br><br>

### <a id="previous"><span style="color: var(--link)">[<- Previous](../README.md)</a></span> <a id="next"><span style="color: var(--link)">[Next ->](./01_BASE_COMPONENTS.md)</a></span>

<hr><br></span>

