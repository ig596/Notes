---
title: Old CTF Writeups
excerpt: "Writeups of CTF Challenges including TSJ, UT, Pico, and more..."
tags: ctf
last_modified_at: 2022-03-26T16:00:58-04:00
toc: true
toc_label: "CTF Writeups"
toc_icon: "cog"
categories:
  - CTF

---

## CTF 1: TSJ CTF
- [CTF Time Link](https://ctftime.org/event/1547)
- [Offical URL](https://ctf.tsj.tw/)

**Worked mostly independently but occasionally slacked with OSRIS which competes under NYUSEC.**

### Challenge - Nimja at Nantu(Web)  500 PTS 

This was a web based challenge that consisted of 2 Web servers( 1 Nim, 1 Node.js) and an Appache Traffic Server. They were deployed in containers and we had access to the deployment files and code.

The Challenge ended up having 2 parts. The first was to exploit the Nim server to receive a key which was not the flag but needed to be pased as part of the request to the Node.js server in order to get the flag. This key was behind endpoints that could only be reached by the local server. The Nim server was vulnerable to SSRF which allowed me to make a request like 
` POST http://34.81.54.62:5487/hello-from-the-world/get_hello?host=http://localhost:80/key`

To access the flag I attempted to perform HTTP Request Smuggling of a POST request to `/service-info/admin` on the NodeJS Service by switching the Transfer-Encoding to chunked and calling one of its responsive endpoints. `/service-info/` During the CTF I was unable to successfully accomplish this part, I believe likely due to issues counting the content length properly or not properly breaking the lines.

After the CTF someone showed me that the Apache Traffic Server had a vuln that allowed you to bypass the path restrictions by using `//` instead of `/` on the last portion of the path so the filtering and blocking it did would be bypassed by calling  `http://34.81.54.62:5487/hello-from-the-world//key` and this also worked for the `/service-info//admin` path 
## CTF 2: UT CTF
- [CTF Time Link](https://ctftime.org/event/1582)
- [Offical URL](https://utctf.live/)

**Worked mostly independently but occasionally sent discord messages with Dhyey Shah on from OSRIS which competes under NYUSEC.**
### Challenge - OSINT Full - Score kept changing but was over 500 at start
The objective of this challenge was to send the Named invidual (EddKing6) a phishing email containg his dog's name, his favorite video game, where he graduated from, his title at his company and his favorite food. When I first searched his [twitter](https://twitter.com/eddking6) came up fairly quickly. The twitter account included his favorite video game (FactorIO), The fact that he was a CISO and that he works for blob corp.  One of the tweets was a link to a [github repo](https://github.com/eddking6/vulnerable-web-app) which gave me his [github profile](https://github.com/eddking6) and that his favorite food was Cacio e Pepe. One of the [other repos](https://github.com/eddking6/DogFeedScheduler/blob/main/quickstart.go) on his account had to do with dogs and included an smtp request that had his email address(blobcorpciso@gmail.com) and the name of his dog(spot). While a linkedin profile didn't show up for him when googling during the challenge and searching his name in association with blob corp did not return any results in linkedin manually trying the profile url for https://www.linkedin.com/in/eddking6/, which was the given userid and used on both twitter and github, we found his linkedin profile which included that he went to Texas A&M University and confirmed the tweet mentioning CISO. 
At this point all I had to do was send those details in an email to the address and I received back the flag.  
**utflag{osint_is_fun}**
## CTF 3: [OFPPT-CTF]
- [CTF Time](https://ctftime.org/ctf/758)
- [Offical CTF Link](https://www.ofppt-ctf.com/)

### Challenge - Rome famous general - 200 PTS
Challenge:
```
 We received an anonymous encrypted message. Can you help us decrypt this text? LRMMS-PSR{d3x3h_p43q4o_p1my3o}
it says you have to use a key: cipherkey
```
My normal tool CyberChef didn't support this use of Caesar Ciphers so Googling Keyed Caesar Cipher returned [this site](https://www.boxentriq.com/code-breaking/keyed-caesar-cipher) that allowed me to crack the flag
**OFPPT-CTF{k3y3d_c43s4r_c1ph3r}**
### Challenge LFI - 400 PTS

As Titled this challenge was all about LFI. The Link in the challenge leads to a copy of a squarespace template hosted on an apache webserver. Exploring the website quickly lead to a LFI on the About Button/Section which was a link to  `/pages/page.php?f=about-us-bedford`. From  here I was able to easily  
to expand and confirm the scope of the lfi by trying common basics like `/pages/page.php?f=../index.php` and `/pages/page.php?f=/etc/passwd/`. I spent quite a bit of time trying to find the flag file until I eventually caved and used a scanner that retrieved the robots.txt
```
User-agent: *
Disallow: /config
Disallow: /commerce/
Disallow: /checkout$
Disallow: /checkout/
Disallow: /cart$
Disallow: /cart/
Disallow: /account$
Disallow: /account/
Disallow: /api/
Disallow: /static/
Disallow:/*?author=*
Disallow:/*&author=*
Disallow:/*?tag=*
Disallow:/*&tag=*
Disallow:/*?category=*
Disallow:/*&category=*
Disallow:/*?month=*
Disallow:/*&month=*
Disallow:/*?view=*
Disallow:/*&view=*
Disallow: /somerandomtext/flag.php
Disallow:/*?format=json
Disallow:/*&format=json
Disallow:/*?format=page-context
Disallow:/*&format=page-context
Disallow:/*?format=main-content
Disallow:/*&format=main-content
Disallow:/*?format=json-pretty
Disallow:/*&format=json-pretty
Disallow:/*?format=ical
Disallow:/*&format=ical
Disallow:/*?reversePaginate=*
Disallow:/*&reversePaginate=*
```
That list included the disallow of scanning a flag.php found at `/somerandomtext/flag.php` Combining this information I tried `http://143.198.224.219:3333/somerandomtext/flag.php` which resulted in a 200 but no data returned. I then returned to trying the query param LFI which also returned a 200 with no data. I was able to extract the flag.php file after adding the php base64 wrapper. `http://143.198.224.219:3333/pages/page.php?f=php://filter/convert.base64-encode/resource=../somerandomtext/flag.php`
and then decoded the base64 with CyberChef to get the flag.
**OFPPT-CTF{th1s_1s_an_34sy_lf1_ch4lleng3}**
### Challenge PHP - 491(Originally 500)
This is another web challenge and this one was fairly straightforward. It provided a link to a page that showed you its source code.
```
<?php

if (isset($_GET['hash'])) {
    if ($_GET['hash'] === "10932435112") {
        die('Not so easy mate.');
    }

    $hash = sha1($_GET['hash']);
    $target = sha1(10932435112);
    if($hash == $target) {
        include('flag.php');
        print $flag;
    } else {
        print "OFPPT-CTF{not-the-one}";
    }
} else {
    show_source(__FILE__);
}

?>
```
As we can see we need to find a value that will have a loose sha1 hash comparison match. Using the resource [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Type%20Juggling) I was able to find a suitable input of _aaK1STfY_. So just calling `http://143.198.224.219:20000/?hash=aaK1STfY` returned the flag **OFPPT-CTF{typ3_juggl1ng_1n_php}**

## CTF 4: PicoCTF 2022 (Two-Week CTF)
- [CTFTime Link](https://ctftime.org/event/1578)
- [CTF Link](https://play.picoctf.org/events/70/)
### Challenge SQLiLite  - 300 PTS (Easy)
This was an extremely easy Web Login Challenge with a simple form that had injection point on the username but had form validation requiring password to be included.
Submitting the form with the following returned an html page with a success message and when inspecting the source there was a hidden paragraph `Your flag is: picoCTF{L00k5_l1k3_y0u_solv3d_it_8dac17f1}`
```
username: admin' OR 1-1 -- - ;
password: test
```

