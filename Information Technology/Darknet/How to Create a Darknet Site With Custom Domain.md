#Darknet #InformationTechnology 
This tutorial consists of three steps

0. Preparing your computer (Not a step)
1. Installing nginx
2. Installing Tor
3. Setting up the tor server
4. Getting custom domain for your website

## Ingredients

- A Good Performance computer (For Domain Hashing) 💻
- A stable internet connection 🌐
- 2 hours ⌚
- Comfort using terminal ⌨

## Instructions

Find a spare computer that can be used as a server. Unlike the normal internet, you do not need a static IP. You will need to install a Linux based OS on the server, I have tested the below steps on **Ubuntu 18.04 Server LTS** and recommend that you also use the same.

It is also recommended to use SSH to connect to your server since there will be no GUI, you will not be able to open this article on the server and copy-paste the commands. Typing the commands out will be time taking & also very error-prone.

Also, make sure you are running as root throughout the tutorial.

```
sudo su
```

Let's get our hands dark 😎

## Installing nginx

`nginx` will serve the HTML files and assets (act as a web server).

```
apt update
apt install nginx
```

The above commands will update & install `nginx`. To start the `nginx` server

```
service nginx start
```

To check the status of the `nginx` server

```
service nginx status
```

To confirm if the `nginx` server is working. We will make a `GET` request to the server using `curl`. Before that, you'll need to know what is your IP address.

```
ifconfig
```

The output will be similar to this

```
lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 134  bytes 21230 (21.2 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 134  bytes 21230 (21.2 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlp9s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet your-ip-address  netmask 255.255.255.0  broadcast ###.###.#.###
        inet6 ####::####:####:####:####  prefixlen 64  scopeid 0x20<link>
        ether ##:##:##:##:##:##  txqueuelen 1000  (Ethernet)
        RX packets 6379  bytes 8574482 (8.5 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 3518  bytes 506008 (506.0 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

Your IP address will be the `inet your-IP-address` one. Note it down, using that now make a `curl` request.

```
curl your-IP-address:80
```

The console should print out the HTML code of the default `nginx` page. To add your custom page, follow the steps from their official documentation. You can also check if `nginx` is working by typing the IP address of the server in your browser.


### Installing Tor

Installing Tor (not just the browser) allows your computer to communicate with the Tor network. Before installing Tor, we will have to install `apt-transport-https`, so that we can use source lines with `https://`

```
apt install apt-transport-https
```

**Important**: The below commands are for Ubuntu 18.04 only If you are running other OS, please find the commands [here, from Tor's official, site](https://2019.www.torproject.org/docs/debian.html.en). We will now add the Tor sources to the sources file.

```
touch /etc/apt/sources.list.d/
nano /etc/apt/sources.list.d/
```

Once the editor is open, add the following to the file

```
deb https://deb.torproject.org/torproject.org bionic main
deb-src https://deb.torproject.org/torproject.org bionic main
```

After exit & saving, type the following in the terminal. This is to add the gpg key used to sign the Tor packages.

```
curl https://deb.torproject.org/torproject.org/A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89.asc | gpg --import
gpg --export A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89 | apt-key add -
```

Finally, now we install Tor and also a package which will help to keep the signing key current.

```
apt update
apt install tor deb.torproject.org-keyring
```

Similar to `nginx`, Tor can be started & checked by the following commands

```
service nginx start
service nginx status
```

### Setting up the tor server

Now that we have `nginx` & Tor up and running, we will have to configure Tor so that our server acts as a Tor server **(Your server will not be used as a relay node)**

We will have to edit the `torrc` file.

```
nano /etc/tor/torrc
```

In the `torrc` file, Go to the middle section and look for the line

```
############### This section is just for location-hidden services ###
```

And uncomment the following lines.

```
HiddenServiceDir /var/lib/tor/hidden_service/
HiddenServicePort 80 127.0.0.1:80
```

In these lines

- `HiddenServiceDir` will tell Tor where to save the private_key & hostname of your Tor website (They are information about your dark website). The private key stored is very important & could be used to impersonate you.
- `HiddenServicePort` lets you specify a virtual port (that is, what port people accessing the website will think they're using) and an IP address and port for redirecting connections to this virtual port.

To apply this new configuration, Stop the Tor service and start it again by typing the following commands.

```
service tor stop
service tor start
```

Now check the status of the tor service to see if the changes are working & valid.

```
service tor status
```

If things are looking good, proceed to the next step, otherwise, you might have made a mistake in editing the `.torrc` file.

At this point, your dark website must be running. But we don't know what is the URL, to get it, run the following command

```
cat /var/lib/tor/hidden_service/hostname
```

The URL to your all-new dark website will be printed in the console. To test if it's working

```
curl -v --socks5-hostname localhost:9050 http://your-onion-domain.onion
```

The URL is actually `your-public-RSA-key.onion`. Tor has a different way of identifying websites via their public RSA key.

Now let's celebrate on your first dark website 🎉.

## Getting custom domain for your website

### mkp224o

mkp224o is a vanity address generator for Tor Onion v3 Hidden Services, created by [cathugger](https://github.com/cathugger/mkp224o) and available [on Github](https://github.com/cathugger/mkp224o).

_The mkp224o repository on cathugger's GitHub account._

[Tor Proposal 224](https://gitweb.torproject.org/torspec.git/tree/proposals/224-rend-spec-ng.txt) is the proposal for the new Onion v3 specification, so that's what mkp224o was named after.

mkp224o is very easy to install and run. Download and extract the latest version from GitHub, then on Debian-based systems, you can use:

#Install dependencies if required
sudo apt-get install autoconf libsodium-dev

#Build
./autogen
./configure
make

You can run "./mkp224o -h" to view the available options:

js@node0:~/onionv3/mkp224o$ ./mkp224o
Usage: ./mkp224o filter [filter...] [options]
       ./mkp224o -f filterfile [options]
Options:
	-h  - print help
	-f  - instead of specifying filter(s) via commandline, specify filter file which contains filters separated by newlines
	-q  - do not print diagnostic output to stderr
	-x  - do not print onion names
	-o filename  - output onion names to specified file
	-F  - include directory names in onion names output
	-d dirname  - output directory
	-t numthreads  - specify number of threads (default - auto)
	-j numthreads  - same as -t
	-n numkeys  - specify number of keys (default - 0 - unlimited)
	-N numwords  - specify number of words per key (default - 1)
	-z  - use faster key generation method. this is now default
	-Z  - use slower key generation method
	-s  - print statistics each 10 seconds
	-S t  - print statistics every specified amount of seconds
	-T  - do not reset statistics counters when printing

You can then run mkp224o to search for an address. The "-S" argument will make mkp224o output statistics every 5 seconds, and the "-d" argument allows you to specify a directory to store the generated keys.

js@node0:~/onionv3/mkp224o$ ./mkp224o -S 5 -d onions jamie
set workdir: onions/
sorting filters... done.
filters:
	jamie
in total, 1 filter
using 4 threads
>calc/sec:17035.009192, succ/sec:0.000000, rest/sec:39.964831, elapsed:0.100088sec
>calc/sec:22250.236672, succ/sec:0.000000, rest/sec:0.000000, elapsed:5.103855sec
>calc/sec:22272.487630, succ/sec:0.000000, rest/sec:0.000000, elapsed:10.107517sec
>calc/sec:22256.077578, succ/sec:0.000000, rest/sec:0.000000, elapsed:15.111813sec
>calc/sec:22278.089855, succ/sec:0.000000, rest/sec:0.000000, elapsed:20.116102sec
>calc/sec:22253.435622, succ/sec:0.000000, rest/sec:0.000000, elapsed:25.120363sec
>calc/sec:21762.771552, succ/sec:0.000000, rest/sec:0.000000, elapsed:30.142012sec
>calc/sec:22258.870726, succ/sec:0.000000, rest/sec:0.000000, elapsed:35.146309sec
>calc/sec:22282.575137, succ/sec:0.000000, rest/sec:0.000000, elapsed:40.150578sec
>calc/sec:22258.577166, succ/sec:0.000000, rest/sec:0.000000, elapsed:45.154941sec
>calc/sec:22198.079226, succ/sec:0.000000, rest/sec:0.000000, elapsed:50.159383sec
>calc/sec:22253.661568, succ/sec:0.000000, rest/sec:0.000000, elapsed:55.163728sec
>calc/sec:22276.712822, succ/sec:0.000000, rest/sec:0.000000, elapsed:60.168057sec
>calc/sec:22254.261335, succ/sec:0.000000, rest/sec:0.000000, elapsed:65.172357sec
>calc/sec:22277.632325, succ/sec:0.000000, rest/sec:0.000000, elapsed:70.176659sec
>calc/sec:22256.428313, succ/sec:0.000000, rest/sec:0.000000, elapsed:75.180966sec
^Cwaiting for threads to finish... done.

In the example above, I exited mkp224o after 75 seconds using Ctrl+C.

Matching addresses are automatically saved into either the directory where mkp224o is running or the one specified using the "-d" switch.

js@node0:~/onionv3/mkp224o$ ls onions/ | head -n 5
jamie22ezawwi5r3o7lrgsno43jj7vq5en74czuw6wfmjzkhjjryxnid.onion
jamie22jd4c7g7osiio7jnqnsqj4w7dpqmit32easwp2igjge67nktid.onion
jamie233pm4t6zkbkdyiu7yknnqccirtkcwne2h5nc73mykvckg76sqd.onion
jamie23kp7n6xgk3lvy6wnse4cuk4dawfpwl52img7za35tiyex2mvyd.onion
jamie24hjpe7ia2usa6odvoi3s77j4uegeytk7c3syfyve2t33curbyd.onion

js@node0:~/onionv3/mkp224o$ ls jamie22ezawwi5r3o7lrgsno43jj7vq5en74czuw6wfmjzkhjjryxnid.onion/
hostname  hs_ed25519_public_key  hs_ed25519_secret_key

You could also set up a cronjob to run mkp224o at boot in a detached screen session:

@reboot cd /path/to/mkp224o && screen -dmS onionv3 ./mkp224o -S 600 -d onions jamie

You can then check up on mkp224o using "screen -r onionv3". Use Ctrl+A followed by Ctrl+D to detach from a screen session without terminating it.

## Performance

Running on a Raspberry Pi 2B, mkp224o achieved a pretty consistent 22,200 calculations per second, with 1 filter running on 4 threads.

The 4 Raspberry Pi Zeros all achieved around 4,180 calculations per second, with 1 filter running on 1 thread.

mkp224o ran 24 hours a day for 21 days. The total number of addresses generated by each device is shown below:

Master (RPi 2B)   : 1014
Node 1 (RPi Zero) : 222
Node 2 (RPi Zero) : 192
Node 3 (RPi Zero) : 212
Node 4 (RPi Zero) : 235

This makes for a total of 1,875 addresses, which is roughly 1 matching address found every 16 minutes and 8 seconds, or 967.68 seconds.

## Address Generation Times

In order to get a very rough estimate for the address generation time for each vanity length, I used the total generation time for the 1,875 addresses that I generated over 21 days, and extrapolated that using the total number of possible characters (32, a-z, 2-7) in order to get values for other vanity address lengths.

These values are estimated using combined data from mkp224o running on 1 Raspberry Pi 2B and 4 Raspberry Pi Zeros, simultaneously and 24/7 for 21 days.

Vanity Characters : Approximate Generation Time
1  : <1 second
2  : <1 second
3  : 1 second
4  : 30 seconds
5  : 16 minutes
6  : 8.5 hours
7  : 11.5 days
8  : 1 year
9  : 32 years
10 : 1,024 years
11 : 32,768 years
12 : 1 million years
13 : 32 million years
14 : 1 billion years
15 : 32 billion years
16 : 1 trillion years
17 : 32 trillion years
18 : 1 quadrillion years
19 : 32 quadrillion years
20 : 1 quintillion years
21 : 32 quintillion years
22 : 1 sextillion years
23 : 32 sextillion years
24 : 1 septillion years
25 : 32 septillion years
26 : 1 octillion years
27 : 32 octillion years
28 : 1 nonillion years
29 : 32 nonillion years
30 : 1 decillion years
31 : 32 decillion years
32 : 1 undecillion years
33 : 32 undecillion years
34 : 1 duodecillion years
35 : 32 duodecillion years
36 : 1 tredecillion years
37 : 32 tredecillion years
38 : 1 quattuordecillion years
39 : 32 quattuordecillion years
40 : 1 quindecillion years
41 : 32 quindecillion years
42 : 1 sexdecillion years
43 : 32 sexdecillion years
44 : 1 septendecillion years
45 : 32 septendecillion years
46 : 1 octodecillion years
47 : 32 octodecillion years
48 : 1 novemdecillion years
49 : 32 novemdecillion years
50 : 1 vigintillion years
51 : 32 vigintillion years
52 : 10^66 years
53 : 10^69 years
54 : 10^72 years
55 : 10^75 years
56 : 10^78 years (1,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000)

I think that 11 vanity characters is realistically achievable given enough computing power, for example when/if Facebook upgrade their hidden service to Onion v3. The Raspberry Pi cluster that I used is a very low performance device when compared to a dedicated GPU crypto rig.

## Filtering Generated Addresses

After I had been running mkp224o for 21 days, I needed to use the cluster for something else so it was time to choose an address. Glancing through the results, there wasn't anything that stood out, so I searched through it with grep for various keywords and acronyms (eg: sec, pgp, php, etc), however there wasn't anything particularly good.

I wanted to search for English words in the output, so I used the /usr/share/dict/british-english file that is included with the package "wbritish" on Debian-based systems.

At first I tried the following, but it turned out to be **extremely inefficient** and could **cause a system crash**, so I do not recommend trying this yourself!

Install wbritish if required
sudo apt-get install wbritish

Make a copy of the british-english dictionary for searching
cp /usr/share/dict/british-english wordlist.txt

Remove one character words
grep -v "^[A-Za-z]$" wordlist.txt > wordlist2.txt

Remove two character words
grep -v "^[A-Za-z][A-Za-z]$" wordlist2.txt > wordlist.txt

Remove "onion", "Onion", "Jamie", "ion", "Jami", "jam" & "Amie"
egrep -v "(^onion$|^Onion$|^ion$|^Jamie$|^Amie$|^Jami$|^jam$)" wordlist.txt > wordlist2.txt

Find words from wordlist2.txt in onion-v3-export.txt
**This could crash your system, be careful!**
grep -iFf wordlist2.txt onion-v3-export.txt

The reason that this technique is so inefficient is that it uses just one instance of grep for the entire search, and grep isn't designed to handle that. I had this running for around 5 minutes and it slowly built up 16 GB RAM usage and started eating into my swap partition, which isn't good! Grep isn't designed for handling this sort of processing, so it's unfair to discount it for this memory-handling behaviour.

![](https://www.jamieweb.net/blog/onionv3-vanity-address/grep-system-monitor.png)

The technique that worked well was using a separate instance of grep for each word to search for, instead of trying to search for them all at once. This was a slower searching method, however it uses minimal memory.

I also decided to cut the addresses to search down to just the first 12 characters, as words that aren't at the start of the address aren't of much value in most cases.

cut -c 1-12 onion-v3-export.txt > onion-cut.txt

Then use a simple bash for loop in order to check each of the dictionary words against the address list individually:

for word in `cat wordlist2.txt`; do grep --colour=always -i "$word" onion-cut.txt; done

I set this running on a Raspberry Pi 2B and it took around 5 hours to search 1875 addresses for around 90,000 dictionary words.

The output can be seen below:

jamieiabe7a4
jamie6r6zala
jamiearkjlxm
jamiecq7kaug
jamiefzubmwi
jamieshbikol
jamieyvh7chi
jamielwsclem
jamied4coltx
jamied4coltx
jamieigdelcr
jamie35cjdoe
jamieolrdowa
jamieastrwc5
jamiecojx25o
jamieli4bdva
jamieli5eiua
jamieliozi7k
jamiemmelinl
jamiengdpoqe
jamiengzlpbx
jamiem7erinn
jamie6wesqby
jamiedesqkt7
jamiesf7besq
jamiehuetnao
jamieva5stck
jamievaa3tz7
jamievai4mxi
jamievavnyxz
jamievawy4s3
jamieve3ay5l
jamievegkrdr
jamievendyi3
jamieveut4pl
jamieus7fdry
jamieklfoxy6
jamieoygeov7
jamiess73geo
jamieh3qgodd
jamieigodyok
jamieogodx2p
jamiegusqcma
jamieygusgz2
jamierguy22t
jamieylguysw
jamiehaysabt
jamieochay7v
jamiehaysabt
jamie4mianru
jamiezayfian
jamiezce5ida
jamie52vinae
jamieiincyvq
jamieaiveslc
jamieza5jayl
jamie5joec2a
jamiejon7tqe
jamiepjondzx
jamiejovebbc
jamie4kentlb
jamie4kentlb
jamiekongx4v
jamie3leegvc
jamieruezlen
jamieg7xrleo
jamiemiaaleo
jamiejjj5les
jamiewles7cq
jamieflewzbs
jamiemmelinl
jamieltdwsfu
jamieqpltdvr
jamieqwmciwy
jamierfnmhzf
jamie7kmarhc
jamienmaycho
jamiemmelinl
jamiez72melu
jamie4mianru
jamiemiaaleo
jamienoem2kg
jamiemynohox
jamiealoctkj
jamiei6rkono
jamieoslok3j
jamiecqommgv
jamiex5iqomc
jamiefrcayod
jamieircaxtn
jamiesraybwe
jamiesamxrep
jamiepysaptu
jamiesamxrep
jamie4dssgtt
jamieoossols
jamiellhsuex
jamie3osunkf
jamiefwsuzyt
jamiekftwax2
jamietwau3l2
jamiecgtadb2
jamieoftaogv
jamiented7pn
jamieiup3tex
jamietexsrhi
jamieenbmvic
jamiee45wisj
jamie5zenznc
jamie7zoeb5u
jamie63uado4
jamiekadz6r3
jamie7yailju
jamieel2iail
jamieb3c6aim
jamiekaime7x
jamierair7zf
jamiefoalemu
jamiekoale5v
jamiemiaaleo
jamieou5wale
jamievwuiall
jamiepysaptu
jamiearkjlxm
jamiekawlt2h
jamiepbagzbw
jamieqkr3ban
jamie3fbetfj
jamieybogdzn
jamie3dmbooj
jamiet4bugok
jamiebunquil
jamieqfburu7
jamie2yvbut2
jamiegcap4bx
jamiecarxx2z
jamieyvh7chi
jamieclapvft
jamiepcodwwz
jamied4coltx
jamie3icoo5o
jamieqcrudor
jamieztmdabf
jamie4debrkh
jamiedewp2l4
jamiedigjmx5
jamie2fidip4
jamies4fdipl
jamie35cjdoe
jamiexiidogt
jamieus7fdry
jamief3adubg
jamielhidubk
jamiehxduet6
jamiehxduet6
jamiearkjlxm
jamiearxrrxz
jamieastrwc5
jamieatksy64
jamiebbazgwr
jamiebbe4onn
jamiebbk4civ
jamiebblfkwp
jamiebboqkaq
jamiejovebbc
jamieel2iail
jamieelc6o6e
jamieelvm27e
jamiegg4cmuc
jamieggmw7r4
jamieggqukg4
jamieggr35m5
jamieggx5omp
jamieke6czqs
jamielfccgrt
jamielfh3f6h
jamielfzwd2l
jamie4zelkks
jamielk2btz3
jamielklik6v
jamiell45bdv
jamiellhsuex
jamieems2vzv
jamiems3bz5b
jamiemsavbsj
jamiemslb7wv
jamiemsty6yk
jamiefoalemu
jamiemub2jhx
jamiemukqyol
jamienddpbfp
jamiepend5mx
jamievendyi3
jamieeonwydp
jamiera2wgtx
jamiera4jhrl
jamierac2xhr
jamierair7zf
jamiere4xdh5
jamiereiwyin
jamieremhidk
jamiereza4ph
jamierezh3pg
jamierguy22t
jamierr2jah7
jamievferrat
jamietap7o7p
jamieve3ay5l
jamievegkrdr
jamievendyi3
jamieveut4pl
jamievendyi3
jamiewe5ihs6
jamiexamjhrr
jamieyeik2sq
jamieyek5q45
jamiewhafaxb
jamievferrat
jamiefig7jeu
jamieflewzbs
jamiebqfflud
jamiefoalemu
jamiewmcfob4
jamiefoe7puf
jamiefogksuv
jamienpfopab
jamieklfoxy6
jamieklfoxy6
jamiezsfuntu
jamiecxgel4m
jamiegq3sgem
jamieufiugob
jamieh3qgodd
jamieigodyok
jamieogodx2p
jamiedgraphf
jamieegunxiy
jamierguy22t
jamieylguysw
jamieylguysw
jamie4hat4o7
jamieprwhawx
jamiehaysabt
jamieochay7v
jamiehaysabt
jamiefhazyvn
jamielhidubk
jamieremhidk
jamienrjdhip
jamiensqihot
jamiepjdhotn
jamiehuetnao
jamieal2hugy
jamieifsuoia
jamiem7erinn
jamieijxjawp
jamieza5jayl
jamiemkhz2zm
jamie4kentlb
jamie5ikidcw
jamieigwkin4
jamie3vkiwib
jamieclapvft
jamienlazyb4
jamieleduvsh
jamie3leegvc
jamieeaqlibc
jamieasl3lit
jamiejlivevk
jamie5ibllow
jamieqcwlowg
jamiehmadnlg
jamie7kmarhc
jamiepdmasxf
jamie44mawsj
jamie44mawsj
jamienmaycho
jamien3qp5cw
jamien42romd
jamien4khbab
jamien56czjy
jamien6iiq47
jamien6vzoon
jamien766udo
jamien7evqfh
jamien7hxhqq
jamien7rrxmx
jamiena25tw3
jamiena2m4ad
jamienb5srqt
jamiencevt24
jamienddpbfp
jamieneoqsde
jamienfcnozu
jamienfrzm5t
jamienfulha7
jamiengdpoqe
jamiengzlpbx
jamieni4wpem
jamienigx3xw
jamieniox2od
jamienje2ohv
jamienjsyflg
jamienkdtpy7
jamienkvdjty
jamienlazyb4
jamienlwvpxl
jamienm27ex6
jamienmafk4n
jamienmaycho
jamienntzth2
jamienoem2kg
jamienoyixsl
jamienp7rstj
jamienpfopab
jamienptccfs
jamienrhpwj3
jamienrjdhip
jamiensf3icf
jamiensfldni
jamiensndk5t
jamiensogx5u
jamiensqihot
jamiented7pn
jamienup4blr
jamienwulrkn
jamienwwuixd
jamienx6zpgq
jamienxcz23c
jamienxdmjtp
jamienyjmqc6
jamienyrheg6
jamienz2zsnp
jamienz6du67
jamienzbapkd
jamienzhu36p
jamienzv45tc
jamiensf3icf
jamiensfldni
jamiensndk5t
jamiensogx5u
jamiensqihot
jamiegdtmixa
jamieymommno
jamie7nixgpq
jamiee4notf2
jamieod2nubq
jamie6zuodd5
jamieh3qgodd
jamieoftaogv
jamiemynohox
jamielrpnopt
jamiesoptrhw
jamiegpadbjk
jamiekwbpadx
jamie7pawj52
jamiebtpawpi
jamieb6paygd
jamiepend5mx
jamieypepj3i
jamie2mvpitg
jamieplyw6bm
jamiepodl6ka
jamiepbzupol
jamie6vdpotq
jamieprytdwt
jamiedgraphf
jamiewp2jrap
jamievferrat
jamiesraybwe
jamiesamxrep
jamiejribrux
jamieugwlroe
jamiezu4mrow
jamiej4truew
jamieruezlen
jamiepysaptu
jamiesaysnbd
jamiesaysnbd
jamiewqs7sex
jamie2zzysly
jamieeslyftw
jamieoossols
jamieoossols
jamiesoptrhw
jamiebcspyd2
jamiemsty6yk
jamiellhsuex
jamiesumk3iw
jamie3osunkf
jamie3osunkf
jamiecgtadb2
jamietap7o7p
jamiey4lvtea
jamieqryitie
jamiewtitd5q
jamietop4unx
jamiej4truew
jamie5xtryfc
jamieagtubqw
jamieitunyoy
jamievendyi3
jamie6jjvia2
jamieou5wale
jamiegwas2k5
jamiekftwax2
jamiel6nwen3
jamiewigoswr
jamieoqpwowi
jamieyapnyqg
jamieod6yawh
jamien6vzoon

On a side note, in order to generate HTML output of the ANSI-encoded terminal emulator colours, you can use the "aha" command:

for word in `cat wordlist4.txt`; do grep --colour=always -i "$word" onion-cut.txt; done | aha

...which will convert the input into HTML with inline colour styling and the following header:

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<!-- This file was created with the aha Ansi HTML Adapter. https://github.com/theZiz/aha -->
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="application/xml+xhtml; charset=UTF-8" />
<title>stdin</title>
</head>
<body>
...

I then edited out the inline styling and used my CSS file instead in order to be compliant with my Content-Security-Policy.

A significantly better way of filtering the vanity addresses would have been putting them all into a database and querying it, as database software is highly optimised for handling this sort of computation.

I was really hoping to get a ^jamieweb address to go along with my Onion v2 service jamiewebgbelqfno.onion, however with Onion v3 and mkp224o, my estimates show that would take around a year. With Onion v2 and [Shallot](https://github.com/katmagic/Shallot), that would (and did) take around 25 days.

Honourable Mentions:

**jamie4deb**rkhllykiyiztbpg6fokecmzhxvj4buig5qss5m4wasn5yyd.onion - Debian?
**jamiedgraph**fwrf2mfhc4iprjjpgzapiqikjxz4yseodq2bvnqcicoad.onion - Fast Graph Database?
**jamiedig**jmx56qpxfwht6pniy2jibt6lpled4juja3wmyuk7bfeaczad.onion - DNS Lookup Utility?
**jamieoslo**k3jxyg223xdm2fftsdd33cio2p5hozkipj27wcygomy4yid.onion - Capital of Norway?
**jamietap**7o7pux6fxpnxrbqvto6dypr3tx3befc7bpon6gn5m4tsftqd.onion - Network Tap?

The full list of generated addresses is available on my Pastebin: [https://pastebin.com/eiFYaCHG](https://pastebin.com/eiFYaCHG)

## Implementation

As of writing this, Onion v3 support is only available alpha versions of Tor, however a Tor build with Onion v3 functionality is currently at the second release candidate, so it should be reaching the stable branch very soon.

For as long as Tor Onion v3 functionality remains in alpha only, I will be keeping the current hidden service address (http://32zzibxmqi2ybxpqyggwwuwz7a3lbvtzoloti7cxoevyvijexvgsfeid.onion) as well as running the Tor alpha instance on a separate, isolated server.

When Onion v3 hits Tor stable, I will move it across to a main server as well as start using the jamie3vkiwi vanity address. I will also continue hosting my [Onion v2 hidden service](https://www.jamieweb.net/blog/tor-hidden-service/) for as long as it is safe to do so.

Luckily it is very easy to host multiple hidden services using the same Tor instance. In your torrc, just add multiple hidden service configurations:

HiddenServiceDir /desired/path/to/v3/hidden/service/config
HiddenServiceVersion 3
HiddenServicePort <localport> <server>

HiddenServiceDir /desired/path/to/v2/hidden/service/config
HiddenServiceVersion 2
HiddenServicePort <localport> <server>

## Other Onion v3 Vanity Address Generation Programs

In addition to [mkp224o](https://github.com/cathugger/mkp224o) by [cathugger](https://github.com/cathugger) (which is what I used), there are also other Onion v3 vanity address generation programs out there:

- horse25519: [https://github.com/Yawning/horse25519](https://github.com/Yawning/horse25519)
- oniongen-go: [https://github.com/rdkr/oniongen-go](https://github.com/rdkr/oniongen-go)
- oniongen-c: [https://github.com/rdkr/oniongen-c](https://github.com/rdkr/oniongen-c)

I personally tried out oniongen-go and oniongen-c in addition to mkp224o.
