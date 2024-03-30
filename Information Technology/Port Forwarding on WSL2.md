 #InformationTechnology #Linux #WSL2

I want to share with you a pro tip to make programming on Windows a little easier with the help of the [WSL 2](https://docs.microsoft.com/en-us/windows/wsl/about).

Imagine you are developing an API, web app, website, etc. on your Windows machine under WSL2. For example, in my case, [my own website](https://aalonso.dev/). I use the React framework [Next.js](https://nextjs.org/), and yarn as package manager, so if I want to start the development server to check my site I just do:

```bash
yarn dev
```

With the above command, the development server will start and listen on port `3000`. It will serve the page to every device that can reach it, as it's serving the site on the `0.0.0.0` address by default.

![Starting the dev server of my website](https://aalonso.dev/_next/image?url=%2Fimages%2Farticles%2Faccessing-network-apps-running-inside-wsl2-from-other-devices-in-your-lan-1e1p%2F4uzme7peiaxhkbvykuuk.png&w=1920&q=75)

Then, if I visit `localhost:3000` on my Windows machine, everything works. The server returns my live website. However, what happens when I try to open my live site from my mobile phone? I navigate to `<my_windows_local_ip>:3000` and then

![The site can't be reached](https://aalonso.dev/_next/image?url=%2Fimages%2Farticles%2Faccessing-network-apps-running-inside-wsl2-from-other-devices-in-your-lan-1e1p%2Flppaoyo9copyqc1x1cki.jpg&w=1200&q=75)

## The solution: forwarding ports in WSL 2 NAT

WSL 2 creates a [virtualized Ethernet adapter with its own unique IP address](https://docs.microsoft.com/en-us/windows/wsl/compare-versions#accessing-a-wsl-2-distribution-from-your-local-area-network-lan), so in fact it creates a NAT between your WSL instance and your Windows host computer. So to be able to access applications running in your WSL from within your Local Area Network (LAN), you must port forward the desired ports.

The solution is to add a port proxy in the Windows Firewall, so we open a PowerShell terminal with administrator rights and execute the following command:

```powershell
netsh interface portproxy add v4tov4 listenport=3000 listenaddress=0.0.0.0 connectport=3000 connectaddress=<the_wsl_ip>
```

You can get the WSL virtual machine IP right from PowerShell, by executing bash:

```powershell
bash.exe -c "ip -4 addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}'"
```

Or the same from within the WSL instance:

```bash
ip -4 addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}'
```

After executing the `netsh interface` command in an administrator PowerShell, the server can now be reached without further configuration needed, nor restarting the server.

![Now it works!](https://aalonso.dev/_next/image?url=%2Fimages%2Farticles%2Faccessing-network-apps-running-inside-wsl2-from-other-devices-in-your-lan-1e1p%2Fqg9fd6gj3v9xo1hv1nzo.jpg&w=1200&q=75)

## Bonus: a PowerShell script to simplify this task

The IP address of the WSL VM changes every time it reboots (like when you power off your Windows), so if you have already added a port proxy for an IP address, it might not work if you restart your computer.

I have created a PowerShell script to simplify this task when you need it to be done. You could also configure a Windows task to execute this PowerShell script on every reboot, if you also start WSL on boot (because WSL needs to be launched to get its IP address). Or could be a way to executing it from WSL once it starts. I'm not explaining here how to achieve this, as I don't have configured it that way. I don't usually access apps running inside WSL from my LAN, so when I need to do this, I just execute the following script in an administrator PowerShell terminal **(explanation below)**:

```powershell
$remoteport = bash.exe -c "ip -4 addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}'"
$found = $remoteport -match '\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}';

if( $found ){
  $remoteport = $matches[0];
} else{
  echo "The Script Exited, the ip address of WSL 2 cannot be found";
  exit;
}

#[Ports]

#All the ports you want to forward separated by coma
$ports=@(80,443,3000,3001,3002,3333,5000,5432,6000,8080,8081,19000,19001);


#[Static ip]
#You can change the addr to your ip config to listen to a specific address
$addr='0.0.0.0';
$ports_a = $ports -join ",";


#Remove Firewall Exception Rules
iex "Remove-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' ";

#adding Exception Rules for inbound and outbound Rules
iex "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Outbound -LocalPort $ports_a -Action Allow -Protocol TCP";
iex "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Inbound -LocalPort $ports_a -Action Allow -Protocol TCP";

for( $i = 0; $i -lt $ports.length; $i++ ){
  $port = $ports[$i];
  iex "netsh interface portproxy delete v4tov4 listenport=$port listenaddress=$addr";
  iex "netsh interface portproxy add v4tov4 listenport=$port listenaddress=$addr connectport=$port connectaddress=$remoteport";
}
```

What this script does is:

- Gets the IP address of the WSL VM.
- Defines the ports you want to proxy/forward. You can therefore edit the `$ports` array as you wish. Here I have written all the ports I could potentially use, so the script will be valid for multiple apps/technologies.
- Adds Windows Firewall rules to allow all inbound/outbound traffic to the WSL. Also removes old rules from previous script executions (with different IP addresses).
- Adds the required port proxies, at the same times that removes old ones from old script executions (with different IP addresses).

If this is the first time that you execute this script, or you have added a new port, you can get a lot of _not found_ messages. These are normal warnings that you can safely ignore, as these occur because the script is trying to delete non-existent firewall rules (first time execution) or port proxies (when you add a new port).

I hope this serves you to improve your developer experience in WSL and make you more productive. If you want to continue improving your WSL experience, check [how to run GUI applications on WSL2](https://aalonso.dev/blog/how-to-use-gui-apps-in-wsl2-forwarding-x-server-cdj).

---

I hope my article has helped you, or at least, that you have enjoyed reading it. I do this for fun and I don't need money to keep the blog running. However, if you'd like to show your gratitude, you can pay for my next coffee(s) with a one-time donation of just $1.00. Thank you!

---

### Remove Port Forwarding Rules

```netsh interface portproxy delete v4tov4 listenport=80 listenaddress=0.0.0.0```