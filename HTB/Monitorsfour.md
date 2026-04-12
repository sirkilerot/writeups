## Monitorsfour HTB-Writeup
[!image](images/monitorsfour/1.png)
Monitors four is an easy difficulty machine that involves website enumeration then exploitation of cacti service.
For priviledge escalation docker.
## Initial access
[!image](images/monitorsfour/2.png)
As always we start with an nmap scan.
That reveals only http and wsman.
So nothing special and we can pay attention towards the website.
The website is very bland and doesn't offer any attack surface.
The only functionality is login field but that is not vulnerable and we don't have creds yet.
We can try to fuzz the website to reveal any parts that aren't straight up visible.
The fuzz discovers user endpoint which is very interesting.
[!image](images/monitorsfour/3.png)
It displays `{"error":"Missing token parameter"}`.
So we provide one.
[!image](images/monitorsfour/4.png)
Now we get `"Invalid or missing token"`.
So we look for valid value and after some trial and error we find: `token=0`
[!image](images/monitorsfour/5.png)
This dumps some creds.
They are hashed but its md5 so its not a big deal and the andmin password is:`wonderful1`.
[!image](images/monitorsfour/6.png)
But hold on they don't pass anywhere not the website nor the wsman.
## Digging deeeper
After doing some online research i discovered that every monitors machine was using cacti service.
This was the 4th one so odds are in our favour.
[!image](images/monitorsfour/8.png)
I search for `cacti.monitorfour.htb` and we get to a subdomain.
So we can now login with the password and username is actualy marcus -owners name not admin.
Now we have the admin panel.
We can lookup if the version has any vulnerabilities and this brings us to `CVE-2025-24367`.
There is actualy an exploit for this at https://github.com/TheCyberGeek/CVE-2025-24367-Cacti-PoC 
[!image](images/monitorsfour/9.png)p
We can download this run it and get a user flag.
[!image](images/monitorsfour/10.png)

## Root

