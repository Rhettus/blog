---
layout: post
title: "Check password strength with hashcat"
date: 2019-12-01 13:32:20 +0300
description: # Add post description (optional)
img: hashcat.jpeg # Add image post (optional)
categories: Tech
---

At my day job we have spent the past year or more going over, and automating our whole infrastructure. A part of this process was also increasing our security controls. On all our linux machines we are now at CIS and OpenSCAP compliance levels. 

Now back at home I have been building a new Ryzen 9 3900x based machine and thought a good experiement would be to try and crack some of the passwords we require our staff to generate.

Hashcat to the rescue. Install it on my Debian based system was the standard

`sudo apt-get install hashcat`

Next I had to download a dictionary file. The one I downloaded via a torrent was crackstation list.

`https://crackstation.net/crackstation-wordlist-password-cracking-dictionary.htm`

I then grabbed one of the `/etc/shadow` hashes one of my staff had generated. I know this is a SHA512 as we generate them with some python code and push them out via ansible. This example has been modified.

`echo $6$UbAY9/dOfT.JSlNR$cm18pOdW5/SJvxcO1VXXXXXWrNs8/L3qVqesJhpIpF8pj1XXXXXG57Twlv8/hkQj9janCxwFKh6dwxxzLXXXXX > hash.txt`

Before starting the process I decided to throw another Nvidia GTX 1080 in my rig to speed things up since hashcat uses GPU's to speed things up. 

![dualgpu](https://i.imgur.com/ATeDTlO.jpg)

To kick things off I used the following command:

`hashcat -m 1800 -a 0 --session session1 -o cracked.txt hash.txt ./crackstation.txt -O -w 3`

The `-m 1800` is the hash type, in this case 1800 is the SHA512 in the standard linux format

`-a 0` is the attack type

`--session session1` will allow you to restart from a checkpoint if something happens

`-o cracked.txt` is the output file, or the file which will be used to store the discovered password if it can

`hash.txt` is the input file containing our single hash

`./crackstation.txt` is our dictionary file

`-w 3` puts a high priority on the job, and makes using the system almost impossible when it's running

Once running I gathered the following information

`Speed.#*.........:   267.9 kH/s` kiloHashes a second

`Hardware.Mon.#1..: Temp: 82c Fan: 54% Util:100% Core:1645MHz Mem:4513MHz Bus:4` GPU 0 

`Hardware.Mon.#2..: Temp: 83c Fan: 55% Util: 99% Core:1607MHz Mem:4513MHz Bus:16` GPU 1


The job finished in about 1.5 hours and could not crack the password which I was very happy about.

