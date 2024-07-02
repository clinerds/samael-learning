Recently blackhat hackers approached us saying they could takeover and are asking us for a big ransom. Please help us to find what they can takeover.

Our website is located at [https://futurevera.thm](https://futurevera.thm/)
# Difficulty = Easy

**Challenges: Sub-Domain Enumeration, Sub-Domain Takeover.**

TryHackMe Room Link: [https://tryhackme.com/room/takeover](https://tryhackme.com/room/takeover)

The beginning starts the machine and grub the IP. Then add the $IP in /etc/hosts for futurevera.thm Use the bellow command to do that.


```sudo echo <THM-IP> futurevera.thm >> /etc/hosts```

![TakeOver TryHackMe Writeup](https://miro.medium.com/v2/resize:fit:700/1*n44zCgJjwvbYIVVUdsl1xg.png)

First, visit the website and let check what’s there…

So, there is nothing interesting. I tried Rustscan to check the open port but found nothing and there is nothing in the source code.

Now we begin our subdomain enumeration stuff. For that, I’m using the Gobuster tool.

```gobuster vhost -u futurevera.thm -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -t 50 --append-domain```

![TakeOver TryHackMe Writeup](https://miro.medium.com/v2/resize:fit:700/1*4rVn0_XVfST7JccOW3N2Bg.png)

Here we got “[https://portal.futurevera.thm](http://portal.futurevera.thm/)” as a subdomain.

Before access add that on /etc/hosts then visit that subdomain.

```sudo echo <THM-IP> portal.futurevera.thm >> /etc/hosts```

![TakeOver TryHackMe Writeup](https://miro.medium.com/v2/resize:fit:700/1*lYTeEEwUR38XwA_q-a5DSQ.png)

But we can’t access the site cause it’s a privet or internal subdomain.

If you read the description carefully then you notice that they write blogs and they are rebuilding their support. Umm… There might be [https://blog.futurevera.thm](https://blog.futurevera.thm/) and [https://support.futurevera.thm](https://suport.futurevera.thm/) subdomains.

So, let’s check this.

A blog….

![TakeOver TryHackMe Writeup](https://miro.medium.com/v2/resize:fit:700/1*rBtfCqbxtAvOZquwfK-tlg.png)

And suport website!!!!

![TakeOver TryHackMe Writeup](https://miro.medium.com/v2/resize:fit:700/1*WNlYhbHn-GV1p8U9aQLyMg.png)

Actually, I tried DNS enumeration but didn’t get any record except A record.

So, I checked all these subdomains' SSL Certificates. In [https://support.futurevera.thm](https://suport.futurevera.thm/) subdomains Certificates there is an alternative DNS record :)

![TakeOver TryHackMe Writeup](https://miro.medium.com/v2/resize:fit:700/1*Vyv_P5mEdd-S-S0543o1jg.png)

Now add that on /etc/hosts then visit that subdomain.

```
sudo echo <THM-IP> secrethelpdesk934752.support.futurevera.thm >> /etc/hosts
```

![TakeOver TryHackMe Writeup](https://miro.medium.com/v2/resize:fit:700/1*Xgi-Xx5lhvpSAF_4IhP-nw.png)

It actually shows the main website here… What if we remove ‘https//’ and add ‘http://’ before the subdomain? Let's check.

![TakeOver TryHackMe Writeup](https://miro.medium.com/v2/resize:fit:700/1*CKfRMokQdBz3wABQhfYfmg.png)

Bang we got our flag….!!!!!!! :)

# Actually what happened here and What is a subdomain takeover?

> _Subdomain takeover vulnerabilities occur when a subdomain (subdomain.example.com) is pointing to a service (e.g. GitHub pages, Heroku, etc.) that has been removed or deleted. This allows an attacker to set up a page on the service that was being used and point their page to that subdomain. For example, if subdomain.example.com was pointing to a GitHub page and the user decided to delete their GitHub page, an attacker can now create a GitHub page, add a CNAME file containing subdomain.example.com, and claim subdomain.example.com._

You can read more about subdomain takeovers here:

- [https://labs.detectify.com/2014/10/21/hostile-subdomain-takeover-using-herokugithubdesk-more/](https://labs.detectify.com/2014/10/21/hostile-subdomain-takeover-using-herokugithubdesk-more/)
- [https://www.hackerone.com/blog/Guide-Subdomain-Takeovers](https://www.hackerone.com/blog/Guide-Subdomain-Takeovers)
- [https://0xpatrik.com/subdomain-takeover-ns/](https://0xpatrik.com/subdomain-takeover-ns/)