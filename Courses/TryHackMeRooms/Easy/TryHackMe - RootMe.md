#CTF #TryHackMe #Easy #Linux 

The skills to be tested and needed to solve this room are: **nmap**, **GoBuster**, **privilege escalation**, **SUID**, **find**, **webshell**, and **gtfobins**.

This room was released today, 9/9/2020. Shout-out to the room creator, **@reddyyZ**. You can access the room at [https://tryhackme.com/room/rootme](https://tryhackme.com/room/rootme)

I was assigned a target IP address of **10.10.177.208**. You will be assigned a different IP address, so double check your entries when following this walk-through.

The requirements to complete this room are:

- How many ports are open?
    - *****
- What version of Apache is running?
    - **2.*.****
- What service is running on port 22?
    - **S****
- Find directories on the web server using web enumeration tool.
    - **No answer needed**
- What is the hidden directory?
    - **/p****/**
- user.txt
    - **THM{y**_***_*_****l}**
- Search for files with SUID permission, which file is weird?
    - **/u**/***/********
- Find a form to escalate your privilege
    - **No answer needed**
- root.txt
    - **THM{p********_*********n}**

**Steps:**

1. Deploy the target machine:

![](https://lightkunyagami.files.wordpress.com/2020/09/deploy.png?w=1024)

2. Scan the machine using **nmap**. Type **_nmap -sC -sV 10.10.177.208_**

- **_-sC_** – to scan using the default nmap scripts

- **_-sV_** – to pull version information of open ports found during the scan

![](https://lightkunyagami.files.wordpress.com/2020/09/nmap.png?w=813)

We have SSH and Web Server open

The nmap scan result above will answer the first three questions:

- How many ports are open? *****
- What version of Apache is running? **2.*.****
- What service is running on port 22? **S****

3. Run **gobuster** to look for hidden directories and files on the web server. Type **_gobuster dir -u [http://10.10.177.208](http://10.10.177.208/) -w /usr/share/wordlists/dirb/common.txt_**

- **_dir_** – to use directory/file brute-forcing mode
- **_-u_** – is the flag to tell gobuster that we are scanning a URL
- **_-w_** – is the flag to set the list of possible directory and file names

![](https://lightkunyagami.files.wordpress.com/2020/09/gobuster.png?w=691)

Directories were found

The gobuster scan result above will answer the next question:

- What is the hidden directory? **/p****/**

4. Visit the hidden directory through your preferred browser. Type **_10.10.177.208/p****/_**

![](https://lightkunyagami.files.wordpress.com/2020/09/site.png?w=934)

Hidden directory page

5. Looks like we can use this to upload a **web shell** and get a reverse shell. Download the php reverse shell script [here](http://pentestmonkey.net/tools/web-shells/php-reverse-shell). Make sure to change the values in the script with your own IP address and a port of your choice to get a reverse shell

![](https://lightkunyagami.files.wordpress.com/2020/09/php-reverse.png?w=356)

Here are the values you need to change in the **php-reverse-shell** script

6. Upon uploading the **php-reverse-shell.php** file, we get a “PHP not permitted” I supposed message

![](https://lightkunyagami.files.wordpress.com/2020/09/not-permitted.png?w=528)

It looks like there is a file extension filtering here

This reminded of a recent walk-through room called **Upload Vulnerabilities** in tryhackme.com.

7. I tried other extensions such as **jpg**, and that uploaded successfully. I tried changing the magic number from **PHP** to jpg of “**FF F8 FF DB**“. That uploaded the php reverse-shell script but it won’t execute. Then, I realized maybe it is just filtering the .php extension, so I renamed the script to a .php5 extension. And that uploaded successfully too

![](https://lightkunyagami.files.wordpress.com/2020/09/upload.png?w=529)

Successfully uploaded my php reverse-shell script

8. Start a netcat listener from your attack machine. Type **_nc -nlvp 9999_**

![](https://lightkunyagami.files.wordpress.com/2020/09/listener.png?w=250)

Netcat listener running on port 9999

9. Run the php reverse-shell script by using **curl**. Type **_curl [http://10.10.177.208/uploads/reverse-shell.php5](http://10.10.177.208/uploads/reverse-shell.php5)_**

![](https://lightkunyagami.files.wordpress.com/2020/09/curl.png?w=503)

10. Go back to check on your listener if you have a connection

![](https://lightkunyagami.files.wordpress.com/2020/09/sehll.png?w=897)

We got a shell!

11. Search for the user.txt using **find**. Type **_find / -type f -name user.txt 2> /dev/null_**

- **_-type f_** – you are telling find to look exclusively for files
- **_-name user.txt_** – instructing the find command to search for a file with the name “user.txt”
- **_2> /dev/null_** – so error messages do not show up as part of the search result

![](https://lightkunyagami.files.wordpress.com/2020/09/search-usertxt.png?w=397)

user flag location

12. Retrieve the content of user.txt. Type **_cat /var/www/user.txt_**

![](https://lightkunyagami.files.wordpress.com/2020/09/user-flag.png?w=233)

user flag

The screenshot above answers the question:

- What is user.txt? **THM{y**_***_*_****l}**

13. Search for files with SUID permission to escalate our privilege using **find**. Type **_find / type -f -user root -perm -u=s 2> /dev/null_**

![](https://lightkunyagami.files.wordpress.com/2020/09/suid.png?w=508)

I blurred the one that seems to be out of place

The screenshot above will answer the question:

- Search for files with SUID permissions, which file is weird? **/u**/***/********

14. Check **gtfobins** on how to exploit the suid above. Access gtfobins [here](https://gtfobins.github.io/). Then search for the specific binary you found above and study how you can exploit through SUID.

![](https://lightkunyagami.files.wordpress.com/2020/09/privesc.png?w=1024)

15. Escalate our privilege to root user. Type **_python -c ‘import os; os.execl(“/bin/sh”, “sh”, “-p”)’_**

![](https://lightkunyagami.files.wordpress.com/2020/09/root.png?w=493)

We are root user!

16. Search for root.txt. Type **_find / -type f -name root.txt_**

![](https://lightkunyagami.files.wordpress.com/2020/09/rootflag-location.png?w=287)

root flag location

17. Retrieve the content of root.txt. Type **_cat /root/root.txt_**

![](https://lightkunyagami.files.wordpress.com/2020/09/root-flag.png?w=229)

Found the root flag!

![](https://lightkunyagami.files.wordpress.com/2020/09/final.png?w=1024)

Room completed

I hope you had fun and learned something from following my walk-through. Please don’t forget to subscribe to my blog to get the latest update when I post new contents.

