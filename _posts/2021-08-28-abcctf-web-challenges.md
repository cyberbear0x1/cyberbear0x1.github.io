---
layout: post
title: "ABC Conference CyberHackathon CTF Write-Up(EASY PEASY)"
date: 2021-08-28
tags: Web
author: Cyber Bear
avatar: assets/img/common/bestprofile2.jpeg
category: CTF
---


## Dear Reader,
Today, you are going  be reading how my team and I solved most of **The ABC's Conference CyberHackathon** web-based challenges.  

[Link](http://abc.ctf.ng/)  

___

## 1. Easy Peasy:  
![Challenge-Photo](assets/abcctf/web/Screenshot_2021-08-29_01-50-54.png)

On clicking the link provided for the challenge, A Login Page is displayed.   

![Challenge-Login-Page](assets/abcctf/web/Screenshot_2021-08-29_14-52-37.png)

Naturally, seeing a login page in a CTF, three things came to mind SQL Injection, Bruteforcing for default credentials and  IDOR(Insecure Direct Object Reference).

But Firstly before delving into all the good stuff, I needed to gather surface level knowlege on what this website runs on. To do this I use the [Whatweb tool](https://installlion.com/kali/kali/main/w/whatweb/install/index.html) preinstalled in Kali Linux.

![Challenge-Whatweb](assets/abcctf/web/Screenshot_2021-08-29_15-04-50.png)

Now I know:
1. It runs on a Apache(2.4.48) server.
2. The Host is a Linux(Debian) Machine.
3. It runs on PHP(7.4.22). 

Although this information was irrelvant to solving this challenge, it is good practice to always enumerate as much as possible because:

> Enumeration is the most critical part of all. The art, the difficulty, and the goal are not to gain access to our target computer. Instead, it is identifying all of the ways we could attack a target we must find.  

While running quick tests on the vulnerabilities I highlighted previously, I ran [FFUF](https://github.com/ffuf/ffuf) in the backround. [FFUF](https://github.com/ffuf/ffuf) is a Fast Web Fuzzer written in go.

```
ffuf -u "http://185.203.119.50:4200/FUZZ" -w ~/wordlist/common.txt
```
From the result below, all other directories returned either status codes **403,** **200,** **301** or **404.**

![Challenge-FFUF](assets/abcctf/web/Screenshot_2021-08-29_15-35-35.png)

Of all other status codes, The directory with the status code **301** i.e **/dev** stood out.  

From experience, I decided to run a recursive scan on that directory using FFUF again!

```
ffuf -u "http://185.203.119.50:4200/dev/FUZZ" -w ~/wordlist/common.txt
```

With the result came a new web page.

![Challenge-FFUF-Recrusive](assets/abcctf/web/Screenshot_2021-08-29_15-51-11.png)

On visiting this new web page in the browser.

An almost blank page with inscription: **Hey champ, there is nothing funny here!**

![Challenge-DevPage](assets/abcctf/web/Screenshot_2021-08-29_16-07-51.png)

Was that really all there is?, Have I been trolled? well, Let's take a quick look at the page's source code.

Viewing the page source code we get this:

![Challenge-DevPage-Source](assets/abcctf/web/Screenshot_2021-08-29_16-18-06.png)

1. A hint(to be used later)
2. A variable **_0x26a3** with values that match Hex.

Quickly converting this to it's ASCII eqivalent, we get:

![Challenge-PythonHex](assets/abcctf/web/Screenshot_2021-08-29_16-35-04.png)

Now we have this text, How do we use it, well we have a hint.  

> By the way, we have created a nice **login page** in **PHP.** Go find it out

Let's use FFUF to find files with the **.php** extention.  

```
ffuf -u "http://185.203.119.50:4200/dev/FUZZ" -w ~/wordlist/common.txt -e .php
```

We get a new page:

![Challenge-FFUF-Login](assets/abcctf/web/Screenshot_2021-08-29_16-43-36.png)

Viewing this page in a browser, Another Login page is displayed with just a password field.

![Challenge-LoginPage](assets/abcctf/web/Screenshot_2021-08-29_16-46-12.png)

Let's try slapping in that string we found earlier; We get redirected to another page **/about_to_win_page.html**

![challenge-Page](assets/abcctf/web/Screenshot_2021-08-29_16-48-54.png)

After waiting a few seconds for some sort of grand reveal of the FLAG, it dawned on us that there was still more to do.  

Viewing the page source code we attempted to download the image on the web page and decided to view the EXIF data of the image using Exiftool.

```
wget http://185.203.119.50:4200/AboutToWin.jpg
```

Afterwards running the Exiftool:

```
exiftool AboutToWin.jpg
```

We found this:

![exif](assets/abcctf/web/Screenshot_2021-08-29_16-57-16.png)

Looks like an html file? Then it is an html file;  

This must be it.....  

![Flag](assets/abcctf/web/Screenshot_2021-08-29_17-01-46.png)

## FLAG
```
abcctf{H4cKiNG_i$_FuN_K33P_L34RN1NG}
```